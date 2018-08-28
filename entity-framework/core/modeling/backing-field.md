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
# <a name="backing-fields"></a>Destek alanları

> [!NOTE]  
> Bu özellik, EF Core 1.1 içinde yeni bir özelliktir.

Destek alanları okuyun ve/veya bir özelliği yerine bir alan yazma EF izin verir. Bu sınıf kapsülleme kullanılan kısıtlama ve/veya verilere erişim semantiğini geliştirmek için uygulama kodu tarafından ancak değeri okuma ve/veya bu kısıtlamaları kullanmadan veritabanına yazılan kullanışlı olabilir / geliştirmeleri.

## <a name="conventions"></a>Kurallar

Kural gereği, alanlar (öncelik sırasına göre listelenmiş) verilen bir özellik için yedekleme olarak aşağıdaki alanları bulunur. Alanlar yalnızca modele dahil edilen özellikleri bulunur. Üzerinde özellikler dahil edilecek model içinde daha fazla bilgi için bkz. [dahil olan ve dışlanan Özellikler](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Destek alanı yapılandırıldığında EF düzeniyle varlık örneklerden veritabanı (özellik ayarlayıcısını kullanmak yerine), bu alana doğrudan yazılacaktır. EF okumak veya diğer zamanlarda değeri yazmak gerekiyorsa, mümkünse özelliğini kullanır. EF bir özelliğinin değerini güncelleştirme yapması gerekiyorsa, bir tanımlanmışsa, örneğin, bu özellik ayarlayıcısını kullanır. Özellik salt okunur ise, alana yazar.

## <a name="data-annotations"></a>Veri ek açıklamaları

Destek alanları veri açıklamalarla yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir özellik için destek alanı yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Alan kullanıldığında denetleme

EF alanı veya özelliği kullandığında yapılandırabilirsiniz. Bkz: [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) desteklenen seçenekler için.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Bir özellik olmadan alanları

Modelinizdeki karşılık gelen bir CLR özelliği varlık sınıfında yok, ancak bunun yerine varlıktaki verileri depolamak için bir alan kullanır, kavramsal bir özelliği de oluşturabilirsiniz. Bu farklıdır [gölge Özellikler](shadow-properties.md), verilerin depolandığı değişiklik İzleyici '. Varlık sınıfı için değerleri get/set yöntemleri kullanıyorsa, bu genellikle kullanılmaz.

EF alanın adını verebilirsiniz `Property(...)` API. Belirtilen ada sahip özellik varsa EF bir alan için arar.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Özellik alanı adı dışında bir ad vermek seçebilirsiniz. Bu ad, sonra modeli oluşturulurken kullanılır, en önemlisi, veritabanında eşlenen sütun adı için kullanılır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Varlık sınıfta bir özellik olduğunda kullanabileceğiniz `EF.Property(...)` kavramsal modelin bir parçası olan özelliğine başvurmak için bir LINQ sorgusundaki yöntemi.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
