---
title: Destek alanları - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994099"
---
# <a name="backing-fields"></a><span data-ttu-id="5a452-102">Destek alanları</span><span class="sxs-lookup"><span data-stu-id="5a452-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="5a452-103">Bu özellik, EF Core 1.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="5a452-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="5a452-104">Destek alanları okuyun ve/veya bir özelliği yerine bir alan yazma EF izin verir.</span><span class="sxs-lookup"><span data-stu-id="5a452-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="5a452-105">Bu sınıf kapsülleme kullanılan kısıtlama ve/veya verilere erişim semantiğini geliştirmek için uygulama kodu tarafından ancak değeri okuma ve/veya bu kısıtlamaları kullanmadan veritabanına yazılan kullanışlı olabilir / geliştirmeleri.</span><span class="sxs-lookup"><span data-stu-id="5a452-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="5a452-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="5a452-106">Conventions</span></span>

<span data-ttu-id="5a452-107">Kural gereği, alanlar (öncelik sırasına göre listelenmiş) verilen bir özellik için yedekleme olarak aşağıdaki alanları bulunur.</span><span class="sxs-lookup"><span data-stu-id="5a452-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="5a452-108">Alanlar yalnızca modele dahil edilen özellikleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="5a452-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="5a452-109">Üzerinde özellikler dahil edilecek model içinde daha fazla bilgi için bkz. [dahil olan ve dışlanan Özellikler](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="5a452-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="5a452-110">Destek alanı yapılandırıldığında EF düzeniyle varlık örneklerden veritabanı (özellik ayarlayıcısını kullanmak yerine), bu alana doğrudan yazılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5a452-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="5a452-111">EF okumak veya diğer zamanlarda değeri yazmak gerekiyorsa, mümkünse özelliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5a452-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="5a452-112">EF bir özelliğinin değerini güncelleştirme yapması gerekiyorsa, bir tanımlanmışsa, örneğin, bu özellik ayarlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="5a452-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="5a452-113">Özellik salt okunur ise, alana yazar.</span><span class="sxs-lookup"><span data-stu-id="5a452-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5a452-114">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5a452-114">Data Annotations</span></span>

<span data-ttu-id="5a452-115">Destek alanları veri açıklamalarla yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="5a452-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5a452-116">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="5a452-116">Fluent API</span></span>

<span data-ttu-id="5a452-117">Fluent API'si, bir özellik için destek alanı yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a452-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="5a452-118">Alan kullanıldığında denetleme</span><span class="sxs-lookup"><span data-stu-id="5a452-118">Controlling when the field is used</span></span>

<span data-ttu-id="5a452-119">EF alanı veya özelliği kullandığında yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a452-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="5a452-120">Bkz: [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) desteklenen seçenekler için.</span><span class="sxs-lookup"><span data-stu-id="5a452-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="5a452-121">Bir özellik olmadan alanları</span><span class="sxs-lookup"><span data-stu-id="5a452-121">Fields without a property</span></span>

<span data-ttu-id="5a452-122">Modelinizdeki karşılık gelen bir CLR özelliği varlık sınıfında yok, ancak bunun yerine varlıktaki verileri depolamak için bir alan kullanır, kavramsal bir özelliği de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a452-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="5a452-123">Bu farklıdır [gölge Özellikler](shadow-properties.md), verilerin depolandığı değişiklik İzleyici '.</span><span class="sxs-lookup"><span data-stu-id="5a452-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="5a452-124">Varlık sınıfı için değerleri get/set yöntemleri kullanıyorsa, bu genellikle kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="5a452-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="5a452-125">EF alanın adını verebilirsiniz `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="5a452-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="5a452-126">Belirtilen ada sahip özellik varsa EF bir alan için arar.</span><span class="sxs-lookup"><span data-stu-id="5a452-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="5a452-127">Özellik alanı adı dışında bir ad vermek seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a452-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="5a452-128">Bu ad, sonra modeli oluşturulurken kullanılır, en önemlisi, veritabanında eşlenen sütun adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a452-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="5a452-129">Varlık sınıfta bir özellik olduğunda kullanabileceğiniz `EF.Property(...)` kavramsal modelin bir parçası olan özelliğine başvurmak için bir LINQ sorgusundaki yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5a452-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
