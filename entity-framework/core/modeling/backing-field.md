---
title: Alanları yedekleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124386"
---
# <a name="backing-fields"></a>Destek Alanları

Yedekleme alanları, EF 'in bir özellik yerine bir alanı okumasına ve/veya yazmasına izin verir. Bu, sınıfın kapsüllenmesi ve/veya kullanımını kısıtlamak için kullanılırken bu yararlı olabilir, ancak bu kısıtlamalar/geliştirmeler kullanılmadan, bu değer veritabanına okunarak ve/veya veritabanına yazılması gerekir.

## <a name="basic-configuration"></a>Temel yapılandırma

Kurala göre, aşağıdaki alanlar belirli bir özellik için (öncelik sırasıyla listelenmiştir) yedekleme alanları olarak keşfedilir. 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

Aşağıdaki örnekte, `Url` özelliği, kendi destek alanı olarak `_url` olacak şekilde yapılandırılmıştır:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Yedekleme alanlarının yalnızca modele dahil olan özellikler için keşfedildiğini unutmayın. Modele dahil edilen özellikler hakkında daha fazla bilgi için bkz. [& özellikleri içerme](included-properties.md).

Ayrıca, alan adı Yukarıdaki kurallara karşılık gelmiyorsa, yedekleme alanlarını açık bir şekilde de yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a>Alan ve özellik erişimi

Varsayılan olarak, EF her zaman yedekleme alanına okur ve yazar. Bu, doğru bir şekilde yapılandırıldığından ve özelliğini hiçbir zaman kullanmayacaktır. Ancak EF, diğer erişim düzenlerini de destekler. Örneğin, aşağıdaki örnek, AŞV 'yi yalnızca bir test ederken ve diğer tüm durumlarda özelliğini kullanarak yedekleme alanına yazmasını söyler:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

Desteklenen seçeneklerin tamamı için bkz. [Propertyaccessmode sabit](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) listesi.

> [!NOTE]
> EF Core 3,0 ile varsayılan özellik erişim modu `PreferFieldDuringConstruction` `PreferField`olarak değiştirilmiştir.

## <a name="field-only-properties"></a>Yalnızca alan özellikleri

Ayrıca, modelinizde, varlık sınıfında karşılık gelen bir CLR özelliğine sahip olmayan bir kavramsal özellik oluşturabilirsiniz, ancak bunun yerine verileri varlıkta depolamak için bir alan kullanır. Bu, verilerin varlık CLR türü yerine değişiklik izleyicide depolandığı [Gölge özelliklerinden](shadow-properties.md)farklıdır. Yalnızca alan özellikleri, varlık sınıfı değerleri almak/ayarlamak için özellikler yerine Yöntemler kullandığında ya da alanların etki alanı modelinde (örn. birincil anahtarlar) gösterilmemesi gereken durumlarda kullanılır.

`Property(...)` API 'sinde bir ad sağlayarak yalnızca alan özelliğini yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

EF, verilen ada sahip bir CLR özelliği veya bir özellik bulunmazsa bir alan bulmaya çalışacaktır. Ne bir özellik ne de bir alan bulunmazsa, bunun yerine bir gölge özellik ayarlanır.

LINQ sorgularından yalnızca alan özelliğine başvurmanız gerekebilir, ancak bu tür alanlar genellikle özeldir. Alana başvurmak için bir LINQ sorgusunda `EF.Property(...)` yöntemini kullanabilirsiniz:

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
