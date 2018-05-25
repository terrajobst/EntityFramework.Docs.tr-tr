---
title: Eşzamanlılık çakışmalarını - EF çekirdek işleme
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını işleme

> [!NOTE]
> Bu sayfayı eşzamanlılık EF çekirdek nasıl çalıştığını ve uygulamanızda eşzamanlılık çakışmaları nasıl ele alınacağını belgeler. Bkz: [eşzamanlılık belirteçleri](xref:core/modeling/concurrency) eşzamanlılık belirteçleri modelinizde yapılandırma hakkında ayrıntılar için.

> [!TIP]
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) github'da.

_Veritabanı eşzamanlılık_ , çok sayıda işlem veya kullanıcıların erişmek veya aynı anda aynı verileri bir veritabanındaki değiştirmek durumlarda başvuruyor. _Eşzamanlılık denetimi_ eşzamanlı değişiklikleri bulunması veri tutarlılığını sağlamak için kullanılan belirli mekanizmaları ifade eder.

EF çekirdek uygulayan _iyimser eşzamanlılık denetimini_, birden çok işlem sağlayacaktır veya kullanıcılar bağımsız olarak eşitleme yükü olmadan değişiklikler yapmak anlamına veya kilitleme. İdeal durumda, bu değişiklikler birbirleri ile müdahale etmez ve bu nedenle başarılı mümkün olacaktır. En kötü Durum senaryosu iki veya daha fazla işlem çakışan değişiklik dener ve bunlardan yalnızca birini başarılı olması gerekir.

## <a name="how-concurrency-control-works-in-ef-core"></a>Eşzamanlılık denetimi EF çekirdek nasıl çalışır?

Eşzamanlılık belirteçleri iyimser eşzamanlılık denetimini uygulamak için kullanılan olarak yapılandırılan özellikler: her bir güncelleştirme veya silme işlemi gerçekleştirildi sırasında `SaveChanges`, Veritabanı eşzamanlılık belirteci değeri karşı özgün karşılaştırılır değer EF çekirdek tarafından okunur.

- Değerler eşleşiyorsa işlemi tamamlayabilir.
- Değerler eşleşmiyorsa EF çekirdek başka bir kullanıcı çakışan bir işlem gerçekleştirdi ve geçerli işlemi iptal varsayar.

Başka bir kullanıcı geçerli işlem ile çakışan bir işlem gerçekleştirdiğinde durum olarak bilinen _eşzamanlılık çakışması_.

Veritabanı sağlayıcıları eşzamanlılık belirteci değerleri karşılaştırma uygulamak için sorumlu.

İlişkisel veritabanları EF çekirdek eşzamanlılık belirteci değeri için bir denetimi içeren `WHERE` yan tümcesi herhangi `UPDATE` veya `DELETE` deyimleri. EF çekirdek deyimleri yürüttükten sonra etkilenen satır sayısını okur.

Hiçbir satır etkilenen bir eşzamanlılık çakışması algılandı ve EF çekirdek oluşturur `DbUpdateConcurrencyException`.

Örneğin, biz yapılandırmak isteyebilirsiniz `LastName` üzerinde `Person` bir eşzamanlılık belirteci olmalıdır. Kişi üzerinde herhangi bir güncelleştirme işlemi eşzamanlılık iade içerecektir sonra `WHERE` yan tümcesi:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını çözme

Bir kullanıcı için bazı değişiklikleri kaydetmek çalışırsa önceki örnekle devam etmeden bir `Person`, ancak başka bir kullanıcı zaten değiştirdi `LastName` bir özel durum.

Bu noktada, uygulama yalnızca kullanıcı güncelleştirme çakışan değişiklikler nedeniyle başarısız oldu ve bildirin geçin. Ancak bu kaydı hala aynı gerçek kişi temsil emin olun ve işlemi yeniden denemek için kullanıcıdan istenebilir.

Bu işlem, örneğidir _bir eşzamanlılık çakışması çözümleme_.

Bir eşzamanlılık çakışması çözümleme içerir geçerli bekleyen değişiklikleri birleştirme `DbContext` veritabanındaki değerlerle. Hangi değerlerin birleştirilir uygulamanın göre değişir ve kullanıcı girişi tarafından yönlendirilebilir.

**Değerleri bir eşzamanlılık çakışması çözümlenmesine yardımcı olmak kullanılabilir üç kümesi vardır:**

* **Geçerli değerler** uygulama veritabanına yazmaya çalışıyordu değerlerdir.

* **Özgün değerler** tüm düzenlemeleri yapılmadan önce ilk olarak veritabanından alınan değerlerdir.

* **Veritabanı değerleri** şu anda veritabanında depolanan değerler.

Bir eşzamanlılık çakışması işlemek için genel yaklaşım şöyledir:

1. Catch `DbUpdateConcurrencyException` sırasında `SaveChanges`.
2. Kullanım `DbUpdateConcurrencyException.Entries` etkilenen varlıklar için yeni bir değişiklik kümesini hazırlamak için.
3. Veritabanındaki geçerli değerleri yansıtacak şekilde eşzamanlılık belirteci özgün değerlerini yenileyin.
4. Hiçbir çakışmalar kadar işlemi yeniden deneyin.

Aşağıdaki örnekte, `Person.FirstName` ve `Person.LastName` Kurulum eşzamanlılık belirteçleri değiştirilebilir. Var olan bir `// TODO:` kaydedilecek değer seçmek için uygulama belirli mantığını dahil olduğu konumda açıklama.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
