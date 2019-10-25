---
title: Alanları yedekleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811883"
---
# <a name="backing-fields"></a><span data-ttu-id="e9a9d-102">Destek Alanları</span><span class="sxs-lookup"><span data-stu-id="e9a9d-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="e9a9d-103">Bu özellik EF Core 1,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="e9a9d-104">Yedekleme alanları, EF 'in bir özellik yerine bir alanı okumasına ve/veya yazmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="e9a9d-105">Bu, sınıfın kapsüllenmesi ve/veya kullanımını kısıtlamak için kullanılırken ve/veya, uygulama koduna göre verilere erişim semantiğinin geliştirilmesine yardımcı olabilir, ancak değer bu kısıtlamaları kullanmadan veritabanına okunmalı ve/veya yazılabilir olmalıdır/ gelişmeleri.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="e9a9d-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="e9a9d-106">Conventions</span></span>

<span data-ttu-id="e9a9d-107">Kurala göre, aşağıdaki alanlar belirli bir özellik için (öncelik sırasıyla listelenmiştir) yedekleme alanları olarak keşfedilir.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="e9a9d-108">Alanlar yalnızca modelde bulunan özellikler için bulunur.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="e9a9d-109">Modele dahil edilen özellikler hakkında daha fazla bilgi için bkz. [& özellikleri içerme](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="e9a9d-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="e9a9d-110">Bir yedekleme alanı yapılandırıldığında, veritabanından varlık örnekleri (Özellik ayarlayıcısı 'nı kullanmak yerine) çalıştırıldığında, EF bu alana doğrudan yazar.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="e9a9d-111">EF 'in değeri diğer zamanlarda okuması veya yazması gerekiyorsa, özelliği mümkünse özelliğini kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="e9a9d-112">Örneğin, EF 'in bir özelliğin değerini güncelleştirmesi gerekiyorsa, bir özellik, tanımlanmışsa Özellik ayarlayıcısı 'nı kullanır.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="e9a9d-113">Özellik salt okunurdur, alana yazar.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e9a9d-114">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="e9a9d-114">Data Annotations</span></span>

<span data-ttu-id="e9a9d-115">Yedekleme alanları, veri açıklamaları ile yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e9a9d-116">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="e9a9d-116">Fluent API</span></span>

<span data-ttu-id="e9a9d-117">Bir özellik için bir destek alanı yapılandırmak üzere Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="e9a9d-118">Alanın ne zaman kullanıldığını denetleme</span><span class="sxs-lookup"><span data-stu-id="e9a9d-118">Controlling when the field is used</span></span>

<span data-ttu-id="e9a9d-119">EF 'in alanı veya özelliği ne zaman kullandığını yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="e9a9d-120">Desteklenen seçenekler için bkz. [Propertyaccessmode sabit listesi](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="e9a9d-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="e9a9d-121">Özelliği olmayan alanlar</span><span class="sxs-lookup"><span data-stu-id="e9a9d-121">Fields without a property</span></span>

<span data-ttu-id="e9a9d-122">Ayrıca, modelinizde, varlık sınıfında karşılık gelen bir CLR özelliğine sahip olmayan bir kavramsal özellik oluşturabilirsiniz, ancak bunun yerine verileri varlıkta depolamak için bir alan kullanır.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="e9a9d-123">Bu, verilerin değişiklik izleyicide depolandığı [Gölge özelliklerinden](shadow-properties.md)farklıdır.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="e9a9d-124">Bu, genellikle varlık sınıfı değerleri almak/ayarlamak için yöntemler kullanıyorsa kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="e9a9d-125">`Property(...)` API 'sindeki alanın adını EF 'e verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="e9a9d-126">Verilen ada sahip bir özellik yoksa, EF bir alanı arayacaktır.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="e9a9d-127">Varlık sınıfında özellik olmadığında, bir LINQ sorgusunda `EF.Property(...)` yöntemini kullanarak modelin kavramsal bir parçası olan özelliğe başvurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a9d-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
