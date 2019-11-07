---
title: Gölge özellikleri-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655961"
---
# <a name="shadow-properties"></a><span data-ttu-id="b9dbf-102">Gölge Özellikler</span><span class="sxs-lookup"><span data-stu-id="b9dbf-102">Shadow Properties</span></span>

<span data-ttu-id="b9dbf-103">Gölge özellikleri, .NET varlık sınıfınızda tanımlanmayan ancak EF Core modelinde bu varlık türü için tanımlanan özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="b9dbf-104">Bu özelliklerin değeri ve durumu yalnızca değişiklik Izleyicide korunur.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="b9dbf-105">Veritabanında, eşlenmiş varlık türlerinde gösterilmemesi gereken veriler olduğunda, gölge özellikleri faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="b9dbf-106">Bunlar, iki varlık arasındaki ilişkinin veritabanındaki bir yabancı anahtar değeri ile temsil edildiği, ancak ilişki varlık türleri arasında, varlık türleri arasında yönetildiğinde, yabancı anahtar özellikleri için en sık kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="b9dbf-107">Gölge özellik değerleri `ChangeTracker` API aracılığıyla elde edilebilir ve değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="b9dbf-108">`EF.Property` static yöntemi aracılığıyla, LINQ sorgularında gölge özelliklerine başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="b9dbf-109">Kurallar</span><span class="sxs-lookup"><span data-stu-id="b9dbf-109">Conventions</span></span>

<span data-ttu-id="b9dbf-110">Bir ilişki keşfedildiğinde, ancak bağımlı varlık sınıfında yabancı anahtar özelliği bulunamadığında, bu kural tarafından gölge özellikleri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="b9dbf-111">Bu durumda, bir gölge yabancı anahtar özelliği tanıtılacaktır.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="b9dbf-112">Gölge yabancı anahtar özelliği `<navigation property name><principal key property name>` olarak adlandırılır (birincil varlığa işaret eden, bu, adlandırma için kullanılır).</span><span class="sxs-lookup"><span data-stu-id="b9dbf-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="b9dbf-113">Asıl anahtar özellik adı Gezinti özelliğinin adını içeriyorsa, ad yalnızca `<principal key property name>`olur.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="b9dbf-114">Bağımlı varlık üzerinde hiçbir gezinti özelliği yoksa, asıl tür adı bunun yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="b9dbf-115">Örneğin, aşağıdaki kod listesi, `Post` varlığa `BlogId` bir gölge özelliği tanıtılmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a><span data-ttu-id="b9dbf-116">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="b9dbf-116">Data Annotations</span></span>

<span data-ttu-id="b9dbf-117">Gölge özellikleri, veri ek açıklamalarıyla oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b9dbf-118">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="b9dbf-118">Fluent API</span></span>

<span data-ttu-id="b9dbf-119">Gölge özelliklerini yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="b9dbf-120">`Property` dize aşırı yüklemesini çağırdıktan sonra, diğer özellikler için yaptığınız yapılandırma çağrılarının herhangi birini zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="b9dbf-121">`Property` yöntemi için sağlanan ad, var olan bir özelliğin adı (bir gölge özellik veya varlık sınıfında tanımlanmış bir) ile eşleşiyorsa, kod yeni bir gölge özellik tanıtımı yerine mevcut özelliği yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b9dbf-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
