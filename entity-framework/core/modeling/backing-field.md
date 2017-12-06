---
title: "EF çekirdek alanlar - yedekleme"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a><span data-ttu-id="ede5d-102">Destekleyen alanlar</span><span class="sxs-lookup"><span data-stu-id="ede5d-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="ede5d-103">Bu özelliği de EF çekirdek 1.1 yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="ede5d-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="ede5d-104">Yedekleme alanları okuyun ve/veya bir alan için bir özellik yerine yazma EF izin verir.</span><span class="sxs-lookup"><span data-stu-id="ede5d-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="ede5d-105">Bu sınıf kapsülleme kullanılan kullanımını kısıtlamak ve/veya verilere erişimin geçici semantiğini geliştirmek için uygulama kodu tarafından ancak değer okuma ve/veya bu kısıtlamaları kullanmadan veritabanına yazılan kullanışlı olabilir / geliştirmeleri.</span><span class="sxs-lookup"><span data-stu-id="ede5d-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="ede5d-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="ede5d-106">Conventions</span></span>

<span data-ttu-id="ede5d-107">Kurala göre aşağıdaki alanlar alanları (öncelik sırasına göre listelenmiş) belirli bir özellik için yedekleme olarak bulunacaktır.</span><span class="sxs-lookup"><span data-stu-id="ede5d-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="ede5d-108">Alanlar yalnızca modele dahil özellikleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="ede5d-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="ede5d-109">Üzerinde özellikleri modelde eklenir daha fazla bilgi için bkz: [dahil olmak üzere & özellikleri dışında](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="ede5d-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="ede5d-110">Bir yedekleme alanını yapılandırıldığında EF varlık örnekleri veritabanı (özellik ayarlayıcısı kullanmak yerine) üzerinden gerçekleştirilmesini olduğunda bu alana doğrudan yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ede5d-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="ede5d-111">Diğer saatlerde değeri okunamıyor veya yazılamıyor EF ihtiyacı varsa, mümkünse özelliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="ede5d-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="ede5d-112">Bir özellik için değer güncelleştirmek EF ihtiyacı varsa bir tanımlanmışsa, örneğin, bu özellik ayarlayıcısı kullanır.</span><span class="sxs-lookup"><span data-stu-id="ede5d-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="ede5d-113">Özellik salt okunur ise alanına yazılır.</span><span class="sxs-lookup"><span data-stu-id="ede5d-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ede5d-114">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ede5d-114">Data Annotations</span></span>

<span data-ttu-id="ede5d-115">Alanları yedekleme ile veri ek açıklamaları yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="ede5d-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ede5d-116">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="ede5d-116">Fluent API</span></span>

<span data-ttu-id="ede5d-117">Bir özellik için bir yedekleme alanını yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ede5d-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="ede5d-118">Alan kullanıldığında denetleme</span><span class="sxs-lookup"><span data-stu-id="ede5d-118">Controlling when the field is used</span></span>

<span data-ttu-id="ede5d-119">EF alanı veya özelliği kullandığında yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ede5d-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="ede5d-120">Bkz: [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) desteklenen seçenekler için.</span><span class="sxs-lookup"><span data-stu-id="ede5d-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="ede5d-121">Bir özellik olmadan alanları</span><span class="sxs-lookup"><span data-stu-id="ede5d-121">Fields without a property</span></span>

<span data-ttu-id="ede5d-122">Modelinizde karşılık gelen bir CLR özelliği varlık sınıfında yok, ancak bunun yerine varlıkta verileri depolamak için bir alanı kullanır, kavramsal özelliği de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ede5d-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="ede5d-123">Bu farklıdır [gölge özellikleri](shadow-properties.md), veri değişikliği İzleyicisi depolandığı.</span><span class="sxs-lookup"><span data-stu-id="ede5d-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="ede5d-124">Varlık sınıfı için değerleri get/set yöntemlerini kullanıyorsa bu tipik olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ede5d-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="ede5d-125">Alanın adını EF verebilirsiniz `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="ede5d-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="ede5d-126">Verilen ada sahip bir özellik varsa, EF bir alan için arar.</span><span class="sxs-lookup"><span data-stu-id="ede5d-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="ede5d-127">Özellik alan adı dışında bir ad vermek seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ede5d-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="ede5d-128">Bu ad, sonra modeli oluşturulurken kullanılır, özellikle bu veritabanında eşlenmiş sütun adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ede5d-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="ede5d-129">Varlık sınıfta bir özellik olduğunda kullanabileceğiniz `EF.Property(...)` kavramsal modelin parçası olan özelliğine başvurmak için bir LINQ Sorgu yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ede5d-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
