---
title: Bağlantı dayanıklılığı-EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416643"
---
# <a name="connection-resiliency"></a>Bağlantı Dayanıklılığı

Bağlantı dayanıklılığı başarısız veritabanı komutlarını otomatik olarak yeniden dener. Özelliği, hataların algılanması ve yeniden denenmesinin gerekli mantığını kapsülleyen bir "yürütme stratejisi" sağlayarak herhangi bir veritabanı ile birlikte kullanılabilir. EF Core sağlayıcılar, belirli veritabanı hata koşulları ve en iyi yeniden deneme ilkelerine uyarlanmış yürütme stratejileri sağlayabilir.

Örnek olarak, SQL Server sağlayıcısı, SQL Server (SQL Azure dahil) özel olarak uyarlanmış bir yürütme stratejisi içerir. Yeniden denenebilecek ve en fazla yeniden deneme sayısı, yeniden denemeler arasındaki gecikme (vb.) için izin verilen özel durum türlerinden haberdar değildir.

İçeriğiniz için seçenekler yapılandırılırken bir yürütme stratejisi belirtildi. Bu, genellikle türetilmiş bağlamınızın `OnConfiguring` yöntemidir:

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

ASP.NET Core bir uygulama için `Startup.cs`:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a>Özel yürütme stratejisi

Varsayılandan herhangi birini değiştirmek istiyorsanız, kendi özel bir yürütme stratejisini kaydetme mekanizması vardır.

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

Hatalarda otomatik olarak yeniden deneme yapan bir yürütme stratejisi, başarısız olan bir yeniden deneme bloğunda her işlemi kayıttan yürütmeyi gerektirir. Yeniden denemeler etkinleştirildiğinde, EF Core aracılığıyla gerçekleştirdiğiniz her işlem kendi yeniden kullanılabilir işlem haline gelir. Diğer bir deyişle, her sorgu ve her bir `SaveChanges()` çağrısı, geçici bir hata oluşursa birim olarak yeniden denenir.

Ancak, kodunuz `BeginTransaction()` kullanarak bir işlem başlatırsa, bir birim olarak değerlendirilmesi gereken kendi işlem grubunuzu tanımlamanız gerekir ve işlem içindeki her şeyin geri yürütülmesi bir hata oluşmasına neden olur. Bir yürütme stratejisi kullanırken bunu yapmayı denerseniz, aşağıdaki gibi bir özel durum alırsınız:

> InvalidOperationException: yapılandırılan ' Sqlserverretryingexecutionstrateji ' yürütme stratejisi Kullanıcı tarafından başlatılan işlemleri desteklemiyor. İşlemdeki tüm işlemleri yeniden kullanılabilir bir birim olarak yürütmek için ' DbContext. Database. Createexecutionstrateji () ' tarafından döndürülen yürütme stratejisini kullanın.

Çözüm, yürütülmesi gereken her şeyi temsil eden bir temsilciyle yürütme stratejisini el ile çağırmalıdır. Geçici bir hata oluşursa yürütme stratejisi temsilciyi yeniden çağırır.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

Bu yaklaşım, Ambient işlemler ile de kullanılabilir.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>İşlem işleme hatası ve teklik sorunu

Genel olarak, bir bağlantı hatası olduğunda geçerli işlem geri alınır. Ancak, işlem yürütülürken bağlantı kesildiğinde işlemin sonuç durumu bilinmez. 

Varsayılan olarak, yürütme stratejisi işlem geri alınmış gibi işlemi yeniden dener, ancak böyle bir durum yoksa, yeni veritabanı durumu uyumsuzsa ya da işlem belirli bir duruma bağlı değilse **veri bozulmasına** yol açacağından, otomatik olarak oluşturulan anahtar değerleriyle yeni bir satır eklenirken bir özel durumla sonuçlanacaktır.

Bu konuyla başa çıkmak için birkaç yol vardır.

### <a name="option-1---do-almost-nothing"></a>Seçenek 1-do (neredeyse) Nothing

İşlem işleme sırasında bağlantı hatası olasılığı düşük olduğundan, bu durum aslında gerçekleşirse uygulamanızın başarısız olması için kabul edilebilir hale gelebilir.

Ancak, yinelenen bir satır eklemek yerine bir özel durumun yapıldığından emin olmak için mağaza tarafından oluşturulan anahtarları kullanmaktan kaçının. İstemci tarafından oluşturulan bir GUID değeri veya bir istemci tarafı değer Oluşturucu kullanmayı düşünün.

### <a name="option-2---rebuild-application-state"></a>Seçenek 2-uygulama durumunu yeniden derle

1. Geçerli `DbContext`atın.
2. Yeni bir `DbContext` oluşturun ve uygulamanızın durumunu veritabanından geri yükleyin.
3. Kullanıcıya son işlemin başarıyla tamamlanmamış olabileceğini bildirin.

### <a name="option-3---add-state-verification"></a>Seçenek 3-durum doğrulama ekleme

Veritabanı durumunu değiştiren işlemlerin çoğu için başarılı olup olmadığını denetleyen kodu eklemek mümkündür. EF bu, daha kolay `IExecutionStrategy.ExecuteInTransaction`yapmak için bir genişletme yöntemi sağlar.

Bu yöntem başlar ve bir işlemi tamamlar ve ayrıca işlem işlemesi sırasında geçici bir hata oluştuğunda çağrılan `verifySucceeded` parametresinde bir işlevi kabul eder.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Burada `SaveChanges`, `Unchanged` başarılı olursa `Blog` varlığının durumunu `SaveChanges` olarak değiştirmeyi önlemek için `acceptAllChangesOnSuccess` `false` olarak ayarlanır. Bu, işleme başarısız olursa ve işlem geri alınırsa aynı işlemi yeniden denemeye izin verir.

### <a name="option-4---manually-track-the-transaction"></a>Seçenek 4-işlemi el Ile izleme

Mağaza tarafından oluşturulan anahtarları kullanmanız gerekiyorsa veya işleme bağlı olmayan işlem başarısızlıklarını işlemek için genel bir yönteme ihtiyaç duyuyorsanız, işleme başarısız olduğunda denetlenen bir KIMLIK atanabilir.

1. İşlemin durumunu izlemek için kullanılan veritabanına tablo ekleyin.
2. Her bir işlemin başındaki tabloya bir satır ekleyin.
3. Kayıt sırasında bağlantı başarısız olursa, veritabanında karşılık gelen satırın varolup olmadığını kontrol edin.
4. Kayıt başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Doğrulama için kullanılan bağlamın bağlantı, işlem kaydı sırasında başarısız olduysa doğrulama sırasında yeniden başarısız olması nedeniyle tanımlanmış bir yürütme stratejisi olduğundan emin olun.
