---
title: Bağlantı dayanıklılığı - EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 34ca1908257ed5544f2e134fa7686c9802fcebea
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949304"
---
# <a name="connection-resiliency"></a>Bağlantı dayanıklılığı

Bağlantı dayanıklılığı, başarısız olan veritabanı komutları otomatik olarak yeniden dener. Bu özellik ile herhangi bir veritabanı "hatalarını algılamak ve komutlar yeniden denemek gerekli mantığı kapsülleyen bir yürütme stratejisi", sağlanarak kullanılabilir. EF Core sağlayıcıları, belirli veritabanı hata koşulları ve en iyi bir yeniden deneme ilkeleri uyarlanmış yürütme stratejileri sağlayabilirsiniz.

Örneğin, SQL Server (SQL Azure dahil) için özel olarak uyarlanmış bir yürütme stratejisi SQL Server sağlayıcısı içerir. Denenebilecek özel durum türlerini farkında olduğundan ve maksimum yeniden deneme sayısı, vb. yeniden denemeler arasındaki gecikme için mantıklı varsayılanlar sahiptir.

Bir yürütme stratejisi, bağlamınızın seçeneklerini yapılandırırken belirtilir. Bu genellikle kullanımda `OnConfiguring` yöntemi veya türetilmiş bağlamınızın `Startup.cs` bir ASP.NET Core uygulaması.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Özel yürütme stratejisi

Bir özel yürütme stratejisi, varsayılan değerleri değiştirmek isterseniz kendi kaydetmek için bir mekanizma yoktur.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Yürütme stratejileri ve işlemler

Otomatik olarak hataları yeniden deneme bir yürütme stratejisi, her işlem başarısız bir yeniden deneme bloğunda oynatmak mümkün olması gerekiyor. Yeniden deneme etkinleştirildiğinde, EF Core ile gerçekleştirdiğiniz her işlem yeniden denenebilir kendi işlem olur. Diğer bir deyişle, her sorgu ve her çağrı `SaveChanges()` geçici bir hata oluşursa bir birim olarak yeniden denenecek.

Ancak, kodunuzu kullanarak bir işlem başlatırsa `BeginTransaction()` tanımladığınız bir birim olarak kabul edilmesi için gereken işlemleri kendi grubu ve işlem içinde her şeyi bir arıza tekrarlanmasını gerekir. Bu yürütme stratejisi kullanılırken yapmayı denerseniz, aşağıdaki gibi bir özel durum alırsınız:

> InvalidOperationException: 'SqlServerRetryingExecutionStrategy' yapılandırılmış yürütme stratejisi, kullanıcı tarafından başlatılan işlemleri desteklemez. İşlem yeniden denenebilir bir birim olarak tüm işlemleri yürütmek için 'DbContext.Database.CreateExecutionStrategy()' tarafından döndürülen yürütme stratejisi kullanın.

El ile yürütülmesi gereken her şeyi temsil eden bir temsilci ile yürütme stratejisi çağırmak için kullanılan çözümüdür. Geçici bir hata oluşursa yürütme stratejisi temsilciyi yeniden çağırır.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>İşlem yürütme hatası ve Teklik sorunu

Genel olarak, bir bağlantı hatası olduğunda geçerli işlem geri alınır. İşlem devam ederken bağlantı kesilirse ancak durdurulmasını ortaya çıkan kaydedilen işlem durumu bilinmiyor. Bkz. Bu [blog gönderisi](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) daha fazla ayrıntı için.

Varsayılan olarak, yürütme stratejisi işlemin işlem geri alındı, ancak böyle değilse yeni bir veritabanı durumu uyumsuz ya da yol açabilir bunu bir özel durum neden olur, olarak yeniden **veri bozulması** varsa işlem otomatik olarak oluşturulan anahtar değerleri içeren yeni bir satır eklendiğinde örneğin belirli bir durumdaki bağımlı kalmayacak.

Bu ayarı kullanarak çıkılacağını birkaç yolu vardır.

### <a name="option-1---do-almost-nothing"></a>Seçenek 1 - (yaklaşık) hiçbir şey yok

İşlem işleme sırasında bağlantı hatası olasılığını düşük olduğundan aslında bu durum ortaya çıkarsa, yalnızca başarısız olmasına, uygulamanız için kabul edilebilir olabilir.

Ancak, yinelenen bir satır eklemek yerine bir özel durum emin olmak için depoda üretilmiş anahtarlar kullanmaktan kaçınmanız gerekir. Bir istemci tarafından oluşturulan GUID değeri veya bir istemci-tarafı değeri oluşturucuyu kullanmayı düşünün.

### <a name="option-2---rebuild-application-state"></a>Seçenek 2 - yeniden uygulama durumu

1. Geçerli at `DbContext`.
2. Yeni bir `DbContext` ve uygulamanızın durumunu veritabanından geri yükleyin.
3. Kullanıcının son işlemi başarıyla tamamlanmamış olduğunu bildirir.

### <a name="option-3---add-state-verification"></a>Seçenek 3 - durumu doğrulama ekleme

Veritabanı durumunu değiştiren işlemlerinin çoğu için başarılı olup olmadığını denetleyen bir kod eklemeniz mümkündür. EF sağlar - kolaylaştırmak için bir genişletme yöntemi `IExecutionStrategy.ExecuteInTransaction`.

Bu yöntem başlar ve bir işlem yürütmeleri ve ayrıca bir işlevde kabul `verifySucceeded` hareket işleme sırasında geçici bir hata ortaya çıktığında çağrılan bir parametre.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Burada `SaveChanges` çağrıldı `acceptAllChangesOnSuccess` kümesine `false` durumunu değiştirmekten kaçınmak için `Blog` varlığa `Unchanged` varsa `SaveChanges` başarılı olur. Bu işleme başarısız olursa ve işlem geri alındı aynı işlemi yeniden denemek için sağlar.

### <a name="option-4---manually-track-the-transaction"></a>4. seçenek - el ile işlem izleme

Depoda üretilmiş anahtarlar kullanın veya yapılan işleme bağlı olmayan işleme hataları işleme bir genel çalışmanız gerekiyorsa, her işlem işleme başarısız olduğunda işaretli bir kimliği atanabilir.

1. Tablo işlemleri durumunu izlemek için kullanılan veritabanı ekleyin.
2. Her işlem başındaki tabloya bir satır ekleyin.
3. Yürütme sırasında bağlantı başarısız olursa, veritabanı karşılık gelen satırı varlığını denetleyin.
4. Kaydetme başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Doğrulama için kullanılan bağlamı bağlantı yeniden doğrulama sırasında işlem yürütme sırasında başarısız olduysa başarısız olma olasılığı yüksek olarak tanımlanan bir yürütme stratejisi olduğundan emin olun.
