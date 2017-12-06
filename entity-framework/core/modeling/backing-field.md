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
# <a name="backing-fields"></a>Destekleyen alanlar

> [!NOTE]  
> Bu özelliği de EF çekirdek 1.1 yeni bir özelliktir.

Yedekleme alanları okuyun ve/veya bir alan için bir özellik yerine yazma EF izin verir. Bu sınıf kapsülleme kullanılan kullanımını kısıtlamak ve/veya verilere erişimin geçici semantiğini geliştirmek için uygulama kodu tarafından ancak değer okuma ve/veya bu kısıtlamaları kullanmadan veritabanına yazılan kullanışlı olabilir / geliştirmeleri.

## <a name="conventions"></a>Kurallar

Kurala göre aşağıdaki alanlar alanları (öncelik sırasına göre listelenmiş) belirli bir özellik için yedekleme olarak bulunacaktır. Alanlar yalnızca modele dahil özellikleri bulunur. Üzerinde özellikleri modelde eklenir daha fazla bilgi için bkz: [dahil olmak üzere & özellikleri dışında](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Bir yedekleme alanını yapılandırıldığında EF varlık örnekleri veritabanı (özellik ayarlayıcısı kullanmak yerine) üzerinden gerçekleştirilmesini olduğunda bu alana doğrudan yazacaksınız. Diğer saatlerde değeri okunamıyor veya yazılamıyor EF ihtiyacı varsa, mümkünse özelliğini kullanır. Bir özellik için değer güncelleştirmek EF ihtiyacı varsa bir tanımlanmışsa, örneğin, bu özellik ayarlayıcısı kullanır. Özellik salt okunur ise alanına yazılır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Alanları yedekleme ile veri ek açıklamaları yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Bir özellik için bir yedekleme alanını yapılandırmak için Fluent API kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Alan kullanıldığında denetleme

EF alanı veya özelliği kullandığında yapılandırabilirsiniz. Bkz: [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) desteklenen seçenekler için.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Bir özellik olmadan alanları

Modelinizde karşılık gelen bir CLR özelliği varlık sınıfında yok, ancak bunun yerine varlıkta verileri depolamak için bir alanı kullanır, kavramsal özelliği de oluşturabilirsiniz. Bu farklıdır [gölge özellikleri](shadow-properties.md), veri değişikliği İzleyicisi depolandığı. Varlık sınıfı için değerleri get/set yöntemlerini kullanıyorsa bu tipik olarak kullanılır.

Alanın adını EF verebilirsiniz `Property(...)` API. Verilen ada sahip bir özellik varsa, EF bir alan için arar.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Özellik alan adı dışında bir ad vermek seçebilirsiniz. Bu ad, sonra modeli oluşturulurken kullanılır, özellikle bu veritabanında eşlenmiş sütun adı kullanılır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Varlık sınıfta bir özellik olduğunda kullanabileceğiniz `EF.Property(...)` kavramsal modelin parçası olan özelliğine başvurmak için bir LINQ Sorgu yöntemi.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
