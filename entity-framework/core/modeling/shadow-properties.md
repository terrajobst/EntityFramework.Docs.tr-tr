---
title: Gölge özellikleri-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417414"
---
# <a name="shadow-properties"></a><span data-ttu-id="d27b8-102">Gölge Özellikler</span><span class="sxs-lookup"><span data-stu-id="d27b8-102">Shadow Properties</span></span>

<span data-ttu-id="d27b8-103">Gölge özellikleri, .NET varlık sınıfınızda tanımlanmayan ancak EF Core modelinde bu varlık türü için tanımlanan özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="d27b8-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="d27b8-104">Bu özelliklerin değeri ve durumu yalnızca değişiklik Izleyicide korunur.</span><span class="sxs-lookup"><span data-stu-id="d27b8-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="d27b8-105">Veritabanında, eşlenmiş varlık türlerinde gösterilmemesi gereken veriler olduğunda, gölge özellikleri faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="d27b8-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="d27b8-106">Yabancı anahtar gölge özellikleri</span><span class="sxs-lookup"><span data-stu-id="d27b8-106">Foreign key shadow properties</span></span>

<span data-ttu-id="d27b8-107">Gölge Özellikler çoğu zaman, iki varlık arasındaki ilişkinin veritabanındaki bir yabancı anahtar değeriyle temsil edildiği yabancı anahtar özellikleri için kullanılır, ancak ilişki varlık türlerinde şirket arasındaki gezinti özellikleri kullanılarak yönetilir türü.</span><span class="sxs-lookup"><span data-stu-id="d27b8-107">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="d27b8-108">Kurala göre, bir ilişki keşfedildiğinde, ancak bağımlı varlık sınıfında yabancı anahtar özelliği bulunamadığında EF bir Shadow özelliği ortaya çıkaracak.</span><span class="sxs-lookup"><span data-stu-id="d27b8-108">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="d27b8-109">Özellik `<navigation property name><principal key property name>` olarak adlandırılır (birincil varlığa işaret eden, bu, adlandırma için kullanılır).</span><span class="sxs-lookup"><span data-stu-id="d27b8-109">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="d27b8-110">Asıl anahtar özellik adı Gezinti özelliğinin adını içeriyorsa, ad yalnızca `<principal key property name>`olur.</span><span class="sxs-lookup"><span data-stu-id="d27b8-110">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="d27b8-111">Bağımlı varlık üzerinde hiçbir gezinti özelliği yoksa, asıl tür adı bunun yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d27b8-111">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="d27b8-112">Örneğin, aşağıdaki kod listesi, `Post` varlığa `BlogId` bir gölge özelliği tanıtılmasıyla sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="d27b8-112">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="d27b8-113">Gölge özelliklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d27b8-113">Configuring shadow properties</span></span>

<span data-ttu-id="d27b8-114">Gölge özelliklerini yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d27b8-114">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="d27b8-115">`Property`dize aşırı yüklemesini çağırdıktan sonra, diğer özellikler için yaptığınız yapılandırma çağrılarının herhangi birini zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d27b8-115">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="d27b8-116">Aşağıdaki örnekte, `Blog` `LastUpdated`adlı CLR özelliği olmadığından, bir Shadow özelliği oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="d27b8-116">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="d27b8-117">`Property` yöntemi için sağlanan ad, var olan bir özelliğin adı (bir gölge özellik veya varlık sınıfında tanımlanmış bir) ile eşleşiyorsa, kod yeni bir gölge özellik tanıtımı yerine mevcut özelliği yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d27b8-117">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="d27b8-118">Gölge özelliklerine erişme</span><span class="sxs-lookup"><span data-stu-id="d27b8-118">Accessing shadow properties</span></span>

<span data-ttu-id="d27b8-119">Gölge özellik değerleri `ChangeTracker` API aracılığıyla elde edilebilir ve değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="d27b8-119">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="d27b8-120">`EF.Property` statik yöntemi aracılığıyla, LINQ sorgularında gölge özelliklerine başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="d27b8-120">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
