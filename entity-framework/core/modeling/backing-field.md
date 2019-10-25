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
# <a name="backing-fields"></a>Destek Alanları

> [!NOTE]  
> Bu özellik EF Core 1,1 ' de yenidir.

Yedekleme alanları, EF 'in bir özellik yerine bir alanı okumasına ve/veya yazmasına izin verir. Bu, sınıfın kapsüllenmesi ve/veya kullanımını kısıtlamak için kullanılırken ve/veya, uygulama koduna göre verilere erişim semantiğinin geliştirilmesine yardımcı olabilir, ancak değer bu kısıtlamaları kullanmadan veritabanına okunmalı ve/veya yazılabilir olmalıdır/ gelişmeleri.

## <a name="conventions"></a>Kurallar

Kurala göre, aşağıdaki alanlar belirli bir özellik için (öncelik sırasıyla listelenmiştir) yedekleme alanları olarak keşfedilir. Alanlar yalnızca modelde bulunan özellikler için bulunur. Modele dahil edilen özellikler hakkında daha fazla bilgi için bkz. [& özellikleri içerme](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Bir yedekleme alanı yapılandırıldığında, veritabanından varlık örnekleri (Özellik ayarlayıcısı 'nı kullanmak yerine) çalıştırıldığında, EF bu alana doğrudan yazar. EF 'in değeri diğer zamanlarda okuması veya yazması gerekiyorsa, özelliği mümkünse özelliğini kullanacaktır. Örneğin, EF 'in bir özelliğin değerini güncelleştirmesi gerekiyorsa, bir özellik, tanımlanmışsa Özellik ayarlayıcısı 'nı kullanır. Özellik salt okunurdur, alana yazar.

## <a name="data-annotations"></a>Veri Açıklamaları

Yedekleme alanları, veri açıklamaları ile yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Bir özellik için bir destek alanı yapılandırmak üzere Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Alanın ne zaman kullanıldığını denetleme

EF 'in alanı veya özelliği ne zaman kullandığını yapılandırabilirsiniz. Desteklenen seçenekler için bkz. [Propertyaccessmode sabit listesi](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Özelliği olmayan alanlar

Ayrıca, modelinizde, varlık sınıfında karşılık gelen bir CLR özelliğine sahip olmayan bir kavramsal özellik oluşturabilirsiniz, ancak bunun yerine verileri varlıkta depolamak için bir alan kullanır. Bu, verilerin değişiklik izleyicide depolandığı [Gölge özelliklerinden](shadow-properties.md)farklıdır. Bu, genellikle varlık sınıfı değerleri almak/ayarlamak için yöntemler kullanıyorsa kullanılır.

`Property(...)` API 'sindeki alanın adını EF 'e verebilirsiniz. Verilen ada sahip bir özellik yoksa, EF bir alanı arayacaktır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

Varlık sınıfında özellik olmadığında, bir LINQ sorgusunda `EF.Property(...)` yöntemini kullanarak modelin kavramsal bir parçası olan özelliğe başvurabilirsiniz.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
