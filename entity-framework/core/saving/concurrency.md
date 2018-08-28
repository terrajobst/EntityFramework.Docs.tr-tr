---
title: Eşzamanlılık çakışmalarını - EF Core işleme
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: e050b17bfa31a4785161c700bc0355e83162b405
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993118"
---
# <a name="handling-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını işleme

> [!NOTE]
> Bu sayfa, eşzamanlılık EF Core nasıl çalıştığını ve uygulamanızdaki eşzamanlılık çakışmalarını nasıl ele alınacağını belgeler. Bkz: [eşzamanlılık belirteçleri](xref:core/modeling/concurrency) modelinizde eşzamanlılık belirteçleri yapılandırma hakkında ayrıntılar için.

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) GitHub üzerinde.

_Veritabanı eşzamanlılık_ , birden fazla işlem veya kullanıcıların erişim veya aynı verileri bir veritabanında aynı anda değiştirme durumları ifade eder. _Eşzamanlılık denetimi_ eşzamanlı değişiklikleri oradayken veri tutarlılığını sağlamak için kullanılan belirli mekanizmaları ifade eder.

EF Core uygulayan _iyimser eşzamanlılık denetimi_, birden çok işlem sağlar veya kullanıcıların yaptığı değişiklikleri eşitleme yükü olmadan bağımsız olarak anlamı veya kilitlenmesi. İdeal durumda, bu değişiklikler birbiriyle müdahale etmez ve bu nedenle başarılı olması mümkün olacaktır. En kötü durum senaryosunda çakışan değişiklikler yapmak iki veya daha fazla işlem dener ve bunlardan yalnızca biri başarılı olması gerekir.

## <a name="how-concurrency-control-works-in-ef-core"></a>EF Core eşzamanlılık denetimi nasıl çalışır?

Eşzamanlılık belirteçleri iyimser eşzamanlılık denetimi uygulamak için kullanılan yapılandırılmış özellikleri: her bir update veya delete işlemi gerçekleştirildi sırasında `SaveChanges`, veritabanında eşzamanlılık belirteci değeri özgün karşı karşılaştırılır. EF Core tarafından okunur değeri.

- Değerleri eşleşirse, işlemi tamamlayabilirsiniz.
- Değerler eşleşmiyorsa, EF Core başka bir kullanıcı çakışan bir işlem gerçekleştirdi ve geçerli işlemi iptal varsayar.

Başka bir kullanıcı geçerli işlem ile çakışan bir işlem gerçekleştirildiğinde durumu olarak da bilinen _eşzamanlılık çakışması_.

Veritabanı sağlayıcıları eşzamanlılık belirteci değerleri karşılaştırma uygulamak için sorumludur.

İlişkisel veritabanları, eşzamanlılık belirteci değeri için bir onay EF Core içeren `WHERE` herhangi yan tümcesi `UPDATE` veya `DELETE` deyimleri. EF Core deyimler yürütüldükten sonra etkilenen satır sayısını okur.

Hiçbir satır etkilenen bir eşzamanlılık çakışması algılandı ve EF Core oluşturur `DbUpdateConcurrencyException`.

Örneğin, biz yapılandırmak isteyebilirsiniz `LastName` üzerinde `Person` eşzamanlı bir simge olması. Kişi üzerinde herhangi bir güncelleştirme işlemi eşzamanlılık onay işareti içerecektir sonra `WHERE` yan tümcesi:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını çözümleme

Bazı değişiklikleri kaydetmek bir kullanıcı çalışırsa, önceki örneğe devam etmeden bir `Person`, ancak başka bir kullanıcı zaten değiştirdi `LastName`, sonra da bir özel durum oluşturulur.

Bu noktada, uygulamayı yalnızca güncelleştirme çakışan değişiklikler nedeniyle başarısız olduğunu kullanıcıya bildirmek ve geçin. Ancak, bu kaydı hala aynı gerçek kişi temsil emin olun ve işlemi yeniden denemek için kullanıcıdan istenebilir.

Bu işlem, örneğidir _bir eşzamanlılık çakışması çözümleme_.

Bir eşzamanlılık çakışması çözümleme içerir geçerli bekleyen değişiklikleri birleştirme `DbContext` veritabanı değerleri. Hangi değerlerin birleştirilir, uygulamaya göre değişir ve kullanıcı girişi tarafından yönlendirilebilir.

**Değer bir eşzamanlılık çakışması çözmenize yardımcı olması kullanılabilir üç kümeleri vardır:**

* **Geçerli değerler** uygulama veritabanına yazmaya denedi değerlerdir.

* **Özgün değerlerine** tüm düzenlemeleri yapılmadan önce ilk olarak veritabanından alınan değerlerdir.

* **Veritabanı değerleri** şu anda veritabanında depolanan değer.

Eşzamanlılık çakışmalarını işleme için genel yaklaşım şöyledir:

1. Catch `DbUpdateConcurrencyException` sırasında `SaveChanges`.
2. Kullanım `DbUpdateConcurrencyException.Entries` etkilenen varlıklar için yeni bir değişiklik kümesini hazırlamak için.
3. Veritabanı geçerli değerleri yansıtacak şekilde eşzamanlılık belirteci öğesinin özgün değerleri yenileyin.
4. Herhangi bir çakışma ortaya kadar işlemi yeniden deneyin.

Aşağıdaki örnekte, `Person.FirstName` ve `Person.LastName` Kurulum eşzamanlılık belirteçleri değiştirilebilir. Var olan bir `// TODO:` yorum kaydedilecek değer seçmesini belirli mantıksal uygulama dahil olduğu yerde.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
