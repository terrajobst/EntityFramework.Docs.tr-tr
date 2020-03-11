---
title: Alanları yedekleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417311"
---
# <a name="backing-fields"></a><span data-ttu-id="45807-102">Destek Alanları</span><span class="sxs-lookup"><span data-stu-id="45807-102">Backing Fields</span></span>

<span data-ttu-id="45807-103">Yedekleme alanları, EF 'in bir özellik yerine bir alanı okumasına ve/veya yazmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="45807-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="45807-104">Bu, sınıfın kapsüllenmesi ve/veya kullanımını kısıtlamak için kullanılırken bu yararlı olabilir, ancak bu kısıtlamalar/geliştirmeler kullanılmadan, bu değer veritabanına okunarak ve/veya veritabanına yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="45807-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="45807-105">Temel yapılandırma</span><span class="sxs-lookup"><span data-stu-id="45807-105">Basic configuration</span></span>

<span data-ttu-id="45807-106">Kurala göre, aşağıdaki alanlar belirli bir özellik için (öncelik sırasıyla listelenmiştir) yedekleme alanları olarak keşfedilir.</span><span class="sxs-lookup"><span data-stu-id="45807-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="45807-107">Aşağıdaki örnekte, `Url` özelliği, kendi destek alanı olarak `_url` olacak şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="45807-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="45807-108">Yedekleme alanlarının yalnızca modele dahil olan özellikler için keşfedildiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="45807-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="45807-109">Modele dahil edilen özellikler hakkında daha fazla bilgi için bkz. [& özellikleri içerme](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="45807-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="45807-110">Ayrıca, alan adı Yukarıdaki kurallara karşılık gelmiyorsa, yedekleme alanlarını açık bir şekilde de yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45807-110">You can also configure backing fields explicitly, e.g. if the field name doesn't correspond to the above conventions:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="45807-111">Alan ve özellik erişimi</span><span class="sxs-lookup"><span data-stu-id="45807-111">Field and property access</span></span>

<span data-ttu-id="45807-112">Varsayılan olarak, EF her zaman yedekleme alanına okur ve yazar. Bu, doğru bir şekilde yapılandırıldığından ve özelliğini hiçbir zaman kullanmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="45807-112">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="45807-113">Ancak EF, diğer erişim düzenlerini de destekler.</span><span class="sxs-lookup"><span data-stu-id="45807-113">However, EF also supports other access patterns.</span></span> <span data-ttu-id="45807-114">Örneğin, aşağıdaki örnek, AŞV 'yi yalnızca bir test ederken ve diğer tüm durumlarda özelliğini kullanarak yedekleme alanına yazmasını söyler:</span><span class="sxs-lookup"><span data-stu-id="45807-114">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="45807-115">Desteklenen seçeneklerin tamamı için bkz. [Propertyaccessmode sabit](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) listesi.</span><span class="sxs-lookup"><span data-stu-id="45807-115">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="45807-116">EF Core 3,0 ile varsayılan özellik erişim modu `PreferFieldDuringConstruction` `PreferField`olarak değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="45807-116">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="45807-117">Yalnızca alan özellikleri</span><span class="sxs-lookup"><span data-stu-id="45807-117">Field-only properties</span></span>

<span data-ttu-id="45807-118">Ayrıca, modelinizde, varlık sınıfında karşılık gelen bir CLR özelliğine sahip olmayan bir kavramsal özellik oluşturabilirsiniz, ancak bunun yerine verileri varlıkta depolamak için bir alan kullanır.</span><span class="sxs-lookup"><span data-stu-id="45807-118">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="45807-119">Bu, verilerin varlık CLR türü yerine değişiklik izleyicide depolandığı [Gölge özelliklerinden](shadow-properties.md)farklıdır.</span><span class="sxs-lookup"><span data-stu-id="45807-119">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="45807-120">Yalnızca alan özellikleri, varlık sınıfı değerleri almak/ayarlamak için özellikler yerine Yöntemler kullandığında ya da alanların etki alanı modelinde (örn. birincil anahtarlar) gösterilmemesi gereken durumlarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="45807-120">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="45807-121">`Property(...)` API 'sinde bir ad sağlayarak yalnızca alan özelliğini yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45807-121">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="45807-122">EF, verilen ada sahip bir CLR özelliği veya bir özellik bulunmazsa bir alan bulmaya çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="45807-122">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="45807-123">Ne bir özellik ne de bir alan bulunmazsa, bunun yerine bir gölge özellik ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="45807-123">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="45807-124">LINQ sorgularından yalnızca alan özelliğine başvurmanız gerekebilir, ancak bu tür alanlar genellikle özeldir.</span><span class="sxs-lookup"><span data-stu-id="45807-124">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="45807-125">Alana başvurmak için bir LINQ sorgusunda `EF.Property(...)` yöntemini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45807-125">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
