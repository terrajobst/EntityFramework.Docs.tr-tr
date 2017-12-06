---
title: "Eşzamanlılık belirteçleri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri

Ardından bir özelliği bir eşzamanlılık belirteci olarak yapılandırılmışsa, EF başka bir kullanıcı o değerini veritabanında değişiklikler kayda kaydedilirken değiştirdi denetleyin. EF iyimser eşzamanlılık desen kullanır, yani değeri değiştirilmediği varsayar ve verileri kaydetmek, ancak değeri değiştirildi bulursa throw deneyin.

Örneğin biz yapılandırmak isteyebilirsiniz `LastName` üzerinde `Person` bir eşzamanlılık belirteci olmalıdır. Bazı yapılan değişiklikleri kaydetmek bir kullanıcı çalışırsa, yani bir `Person`, ancak başka bir kullanıcı değiştirdi `LastName` bir özel durum sonra. Böylece uygulamanız bu kaydı hala aynı gerçek kişi yaptıkları değişiklikleri kaydetmeden önce temsil eden emin olmak için kullanıcı isteyebilir bu tercih edilebilir.

> [!NOTE]
> Bu sayfayı eşzamanlılık belirteçleri yapılandırma konusunda belgelenmiştir. Bkz: [eşzamanlılık işleme](../saving/concurrency.md) uygulamanızda iyimser eşzamanlılık kullanma örnekleri için.

## <a name="how-concurrency-tokens-work-in-ef"></a>Eşzamanlılık belirteçleri EF içinde nasıl çalışır?

Veri depoları, güncelleştirilmiş veya hala silinmiş herhangi bir kayıt bağlam veritabanından veri ilk yüklendiğinde atandı eşzamanlılık belirteci için aynı değere sahip denetleyerek eşzamanlılık belirteçleri zorunlu kılabilir.

Örneğin, ilişkisel veritabanları eşzamanlılık belirteci dahil ederek bunun `WHERE` yan tümcesi herhangi `UPDATE` veya `DELETE` komutlar ve etkilenen satırların sayısı denetleniyor. Eşzamanlılık belirteci hala eşleşmesi durumunda bir satır güncelleştirilir. Değeri veritabanındaki değer değiştiyse, hiçbir satır güncelleştirilir.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a>Kurallar

Kurala göre hiçbir zaman eşzamanlılık belirteçleri yapılandırılır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özelliği bir eşzamanlılık belirteci olarak yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent API'si

Bir özelliği bir eşzamanlılık belirteci olarak yapılandırmak için Fluent API kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Zaman damgası/satır sürümü

Bir zaman damgası, yeni bir değer satır eklenmesi veya güncelleştirilmesi her zaman veritabanı tarafından oluşturulduğu bir özelliktir. Özelliği, ayrıca bir eşzamanlılık belirteci olarak kabul edilir. Bu, başka birisi için verileri sorgulanan beri güncelleştirmeye çalıştığınız bir satır değiştirmişse bir özel durum alırsınız sağlar.

Bu, nasıl sağlanır kullanılan veritabanı kadar sağlayıcıdır. SQL Server için zaman damgası genellikle kullanılan bir *byte []* olacaktır özelliği kurulumunu olarak bir *ROWVERSION* veritabanındaki sütun.

### <a name="conventions"></a>Kurallar

Kurala göre hiçbir zaman zaman damgaları yapılandırılır.

### <a name="data-annotations"></a>Veri ek açıklamaları

Bir özelliği bir zaman damgası yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent API'si

Fluent API bir zaman damgası bir özelliğini yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
