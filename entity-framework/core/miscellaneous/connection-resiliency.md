---
title: "Bağlantı dayanıklılığı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="connection-resiliency"></a>Bağlantı dayanıklılığı

Bağlantı dayanıklılığı başarısız veritabanı komutları otomatik olarak yeniden dener. Bu özellik "hatalarını algılayacak ve komutları yeniden denemek gerekli mantığı kapsülleyen bir yürütme stratejisi", sağlayarak herhangi bir veritabanı ile kullanılabilir. EF çekirdek sağlayıcılar, belirli bir veritabanını hata koşulları ve en iyi yeniden deneme ilkelerini uyarlanmış yürütme stratejileri sağlayabilir.

Örnek olarak, SQL Server sağlayıcısı SQL Server (SQL Azure dahil) için özellikle tasarlanmış bir yürütme stratejisi içerir. Yeniden denenebilir özel durum türlerini farkındadır ve maksimum yeniden deneme sayısı, vb. yeniden denemeler arasındaki gecikme için duyarlı Varsayılanları olmasıdır.

Bir yürütme stratejisi içeriğiniz için seçenekleri yapılandırma sırasında belirtilir. Bu genellikle kullanımda `OnConfiguring` yöntemi türetilmiş içeriğiniz, ya da `Startup.cs` ASP.NET Core uygulamanın.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Özel yürütme stratejisi

Varsayılanları değiştirmek isterseniz, kendi özel yürütme stratejisi kaydetmek için bir mekanizma yoktur.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Yürütme stratejilerini ve işlemler

Hataları üzerinde otomatik olarak yeniden dener bir yürütme stratejisi her işlemi başarısız bir yeniden deneme bloğunda çalmak gerekir. Yeniden deneme etkinleştirildiğinde, EF çekirdek gerçekleştirdiğiniz her işlem yeniden denenebilir kendi işlemi, yani her sorgu ve her çağrı hale `SaveChanges()` geçici bir hata oluşursa, bir birim olarak yeniden denenecek.

Ancak, kodunuzu kullanarak bir işlem başlatıyorsa `BeginTransaction()` bir birim olarak kabul edilmesi için gereken işlemleri kendi grubunun tanımlıyorsanız, yani işlem içinde her şeyi geri bir hata meydana çalınacak gerekir. Bir yürütme stratejisi kullanırken bunu denerseniz aşağıdaki gibi bir özel durum alırsınız.

> InvalidOperationException: 'SqlServerRetryingExecutionStrategy' yapılandırılmış yürütme stratejisi kullanıcı tarafından başlatılan işlemleri desteklemez. İşlem yeniden denenebilir bir birim olarak tüm işlemleri yürütmek için 'DbContext.Database.CreateExecutionStrategy()' tarafından döndürülen yürütme stratejisi kullanın.

Çözüm, yürütülecek gereken her şeyi temsil eden bir temsilci ile yürütme stratejisi el ile çağırmaktır. Geçici bir hata oluşursa, yürütme stratejisi temsilci yeniden çağırır.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>İşlem yürütme hatası ve benzersizlik sorunu

Genel olarak, bir bağlantı hatası olduğunda geçerli işlem geri alınır. İşlem sırasında bağlantı kesilirse ancak olma elde edilen kaydedilen işlem durumu bilinmiyor. Bu bkz [blog gönderisi](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) daha fazla ayrıntı için.

Varsayılan olarak, yürütme stratejisi işlemi işlem geri alındı, ancak durum değilse yeni veritabanı durumu uyumsuz ya da neden olabilir, bu bir özel durumla sonuçlanır sanki deneyecek **veri bozulması** varsa işlem otomatik olarak oluşturulan anahtar değerlerini içeren yeni bir satır eklerken örneğin belirli bir durumdaki bağlı değildir.

Bu ile mücadele etmek için birkaç yolu vardır.

### <a name="option-1---do-almost-nothing"></a>Seçenek 1 - (neredeyse) hiçbir şey

İşlem yürütme sırasında bağlantı hatası olasılığını düşük olduğundan aslında bu durum oluşursa, yalnızca başarısız olmasına, uygulamanız için kabul edilebilir olabilir.

Ancak, yinelenen satır ekleme yerine bir özel durum emin olmak için depoda üretilmiş anahtarlar kullanmaktan kaçının gerekir. Bir istemci tarafından üretilen GUID değeri veya bir istemci-tarafı değeri oluşturucuyu kullanmayı düşünün.

### <a name="option-2---rebuild-application-state"></a>Seçenek 2 - yeniden uygulama durumu

1. Geçerli atmak `DbContext`.
2. Yeni bir `DbContext` ve veritabanından uygulamanızın durumunu geri yükle.
3. Kullanıcı son işlemi başarıyla tamamlanmamış gerektiğini bildirin.

### <a name="option-3---add-state-verification"></a>Seçenek 3 - durumu doğrulama ekleme

Veritabanı durumunu değiştirme işlemlerinin çoğu için başarılı olup olmadığını denetler kod eklemek mümkündür. EF sağlar - kolaylaştırmak için bir genişletme yöntemi `IExecutionStrategy.ExecuteInTransaction`.

Bu yöntem başlar ve bir işlem kaydeder ve ayrıca bir işlevde kabul `verifySucceeded` hareket kaydetme sırasında geçici bir hata ortaya çıktığında çağrılan parametresi.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Burada `SaveChanges` çağrılıyor `acceptAllChangesOnSuccess` kümesine `false` durumunu değiştirmekten kaçınmak için `Blog` varlığa `Unchanged` varsa `SaveChanges` başarılı olur. Yürütme başarısız olursa ve işlem geri alındı aynı işlemi yeniden denemek için bunu sağlar.

### <a name="option-4---manually-track-the-transaction"></a>4. seçenek - el ile işlem izleme

Depo tarafından üretilen anahtarlar kullanın veya gerçekleştirilen işlemi bağımlı değil genel şekilde yürütme hatalarının işleme gerek gerekiyorsa, her işlem yürütme başarısız olduğunda, denetlenir kimliği atanabilir.

1. Bir tablo işlemleri durumunu izlemek için kullanılan veritabanına ekleyin.
2. Her işlem başındaki tabloya bir satır ekler.
3. Kaydetme sırasında bağlantı başarısız olursa veritabanında karşılık gelen satırda varlığını kontrol edin.
4. Yürütme başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırda silin.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Doğrulama için kullanılan bağlam bağlantı büyük olasılıkla işlem yürütme sırasında başarısız olursa yeniden doğrulama sırasında başarısız olarak tanımlanan bir yürütme stratejisi sahip olduğundan emin olun.
