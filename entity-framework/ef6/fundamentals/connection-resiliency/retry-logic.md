---
title: Bağlantı dayanıklılığı ve yeniden deneme mantığı - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 09ebed18b43f864af36b6931f45638f3a3056229
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490811"
---
# <a name="connection-resiliency-and-retry-logic"></a>Bağlantı dayanıklılığı ve yeniden deneme mantığı
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Bir veritabanı sunucusuna bağlanan uygulamalar her zaman bağlantı sonu nedeniyle arka uç hataları ve ağ kararsızlığı savunmasız olmuştur. Ancak, adanmış veritabanı sunucularına karşı çalışan bir LAN tabanlı ortamda bu ek mantık hataları işlemek için genellikle gerekli olmadığını yeterince nadir hatalardır. Bulut Yükselişi artık bağlantı sonlarını gerçekleşmesi daha yaygın daha az güvenilir ağ üzerinden Windows Azure SQL veritabanı gibi veritabanı sunucuları ve bağlantıları temel. Bu, veritabanlarını kullanım hizmeti aralıklı zaman aşımları ve diğer geçici hataları neden ağ kararsızlığı veya bu bağlantı azaltma gibi eşitliği emin olmak için bulut savunma teknikleri nedeniyle olabilir.  

Bağlantı dayanıklılığı EF otomatik olarak bu bağlantı sonu nedeniyle başarısız olan tüm komutları yeniden denemek özelliği ifade eder.  

## <a name="execution-strategies"></a>Yürütme stratejileri  

Bağlantı yeniden deneme Idbexecutionstrategy arabirimi uygulaması tarafından dikkate. Idbexecutionstrategy uygulamaları bir işlem kabul ederek ve bir özel durum oluşursa, bir yeniden denemenin uygun olup olmadığını belirlemek ve varsa, yeniden deneniyor, sorumlu olacaktır. EF ile birlikte dört yürütme strateji vardır:  

1. **DefaultExecutionStrategy**: sql server dışındaki veritabanları için varsayılan değer, bu yürütme stratejisi herhangi bir işlemi yeniden denemez.  
2. **DefaultSqlExecutionStrategy**: varsayılan olarak kullanılan bir iç yürütme stratejisi budur. Bu tüm yeniden deneme stratejisi değil, ancak bağlantı dayanıklılığı etkinleştirmek isteyebileceğiniz kullanıcılara bildirmek için geçici özel durumları kaydırılır.  
3. **DbExecutionStrategy**: Bu sınıf temel sınıf olarak kendi özel olanları dahil olmak üzere diğer yürütme stratejileri için uygundur. Maksimum yeniden deneme sayısına ulaşılır kadar burada ilk yeniden deneme gecikmesi ve sıfır gecikme ile artar katlanarak olmaz bir üstel yeniden deneme ilkesi uygular. Bu sınıfın türetilmiş yürütme stratejileri hangi özel durumları yeniden denenmesi gerektiğini denetlemek için uygulanabilir bir soyut ShouldRetryOn yöntemi vardır.  
4. **SqlAzureExecutionStrategy**: Bu yürütme stratejisi DbExecutionStrategy devralır ve Azure SQL veritabanı ile çalışırken büyük olasılıkla geçici olduğu bilinen özel durumlar yeniden denenecek.

> [!NOTE]
> Yürütme stratejileri 2 ve 4 EntityFramework.SqlServer derlemede olan EF ile birlikte gelen Sql Server sağlayıcısı eklenir ve SQL Server ile çalışacak şekilde tasarlanmıştır.  

## <a name="enabling-an-execution-strategy"></a>Bir yürütme stratejisi etkinleştirme  

Bir yürütme stratejisi kullanmayı EF bildirmek için en kolay yolu SetExecutionStrategy ile yöntemidir [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) sınıfı:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Bu kod SqlAzureExecutionStrategy SQL Server'a bağlanırken kullanılacak EF söyler.  

## <a name="configuring-the-execution-strategy"></a>Yürütme stratejisi yapılandırma  

SqlAzureExecutionStrategy oluşturucusunun MaxRetryCount ve MaxDelay iki parametre kabul edebilir. MaxRetry, strateji yeniden denenme sayısı sayısıdır. Yürütme stratejisi kullanacağı yeniden denemeler arasındaki en büyük gecikme temsil eden bir TimeSpan MaxDelay olur.  

Yeniden deneme sayısı 1 ve 30 saniyeye kadar en büyük gecikme ayarlamak için aşağıdaki execue gerekir:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

SqlAzureExecutionStrategy geçici bir hata meydana gelir, ancak ya da en fazla yeniden deneme kadar sınırı denemeler arasında daha uzun süre gecikeceğini ilk kez aşıldığında veya toplam süreyi en uzun gecikme İsabetleri instantly yeniden deneyecek.  

Yürütme stratejileri yalnızca sınırlı sayıda tansient genellikle olan özel durumlar deneyecek, diğer hataları yanı sıra RetryLimitExceeded istisnayı çözmek burada bir hata geçici değilse veya çok uzun sürüyor durumu işlemek yine de gerekir kendisi.  

Yeniden denenirken bir yürütme stratejisi kullanarak oluşan bazı bilinen sınırlamalar vardır:  

## <a name="streaming-queries-are-not-supported"></a>Akış sorgular desteklenmez  

Varsayılan olarak, EF6 ve sonraki bir sürümü bunları akış yerine sorgu sonuçları arabellekte tutar. Sahip olmasını isterseniz sonuçları akış AsStreaming yöntemi bir LINQ to Entities sorgusunda akış değiştirmek için kullanabilirsiniz.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Yeniden denenirken bir yürütme stratejisi kaydedildiğinde akış desteklenmiyor. Bağlantı parçası şekilde döndürülen sonuç bırak çünkü bu sınırlama bulunmaktadır. Böyle bir durumda EF sorgunun tamamını yeniden çalıştırmak için gereken ancak hangi sonuçları zaten iade edilmiş olduğunu bilmesinin güvenilir bir yolu yoktur (veri değişmiş olabilir bu yana ilk sorgunun gönderildiği, sonuçları gelen geri farklı bir sırada, sonuçları benzersiz bir tanımlayıcı olmayabilir VS.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Kullanıcı tarafından başlatılan işlemleri desteklenmez.  

Yeniden denemeler sonuçları bir yürütme stratejisi yapılandırıldığında, işlem kullanımı geçici olarak bazı sınırlamalar vardır.  

Varsayılan olarak EF, bir işlem içinde herhangi bir veritabanı güncelleştirme gerçekleştirir. Bunu etkinleştirmek için herhangi bir şey yapmanız gerekmez, EF her zaman bunu otomatik olarak yapar.  

Örneğin, aşağıdaki kodda SaveChanges otomatik olarak bir işlem içinde gerçekleştirilir. SaveChanges işlem geri alınamaz sonra yeni sitenin birini ekleyerek ve veritabanına uygulanan herhangi bir değişiklik sonra arızalanması durumunda. Bağlam, ayrıca değişiklikleri uygulamadan yeniden denemek için yeniden çağrılması SaveChanges izin veren bir durumda bırakılır.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Birden çok işlem tek bir işlemde kayabilir yeniden denenirken bir yürütme stratejisi kullanılmadığında. Örneğin, aşağıdaki kod, tek bir işlemde iki SaveChanges çağrılarını sarmalar. Herhangi bir bölümünü ya da işlem değişikliklerin hiçbiri sonra başarısız olursa uygulanır.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

Bu, EF önceki işlemleri ve bunları yeniden deneme işlemleri farkında olmadığından yeniden denenirken bir yürütme stratejisi kullanılırken desteklenmez. Örneğin, ikinci SaveChanges EF artık sonra başarısız olursa ilk SaveChanges çağrıyı yeniden denemesi için gerekli olan bilgileri vardır.  

### <a name="workaround-suspend-execution-strategy"></a>Geçici çözüm: Yürütme stratejisini askıya alma  

Olası bir geçici çözüm, bir kullanıcı kullanmak için gereken kod parçasını deneniyor yürütme stratejisini askıya alma işlemi tarafından başlatılan ' dir. Bunu yapmanın en kolay yolu, kodunuza SuspendExecutionStrategy bayrak yapılandırma sınıfı tabanlı ve bayrak ayarlandığında, varsayılan (retying olmayan) yürütme stratejisi döndürmek için yürütme stratejisi lambda değiştirme eklemektir.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Unutmayın, biz CallContext bayrak değerini depolamak için kullanıyoruz. Bu iş parçacığı yerel depolama benzer bir işlevsellik sağlar, ancak zaman uyumsuz sorgu dahil olmak üzere zaman uyumsuz kodla - kullanın ve Entity Framework ile kaydetmek güvenlidir.  

Biz, artık bir kullanıcı tarafından başlatılan işlem kullanan kod bölümünün yürütme stratejisini askıya alabilirsiniz.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a>Geçici çözüm: El ile yürütme stratejisi çağırın  

Başka bir seçenek, el ile yürütme stratejisi kullanın ve böylece işlemlerden biri başarısız olursa her şeyi deneyebilirsiniz çalıştırılması için bir mantık kümesinin tamamını verin oluşturmaktır. Biz yine de tekniği kullanarak yürütme stratejisi - askıya gerekir, böylece yeniden denemek yeniden denenebilir kod bloğu içinde kullanılan bir bağlam çalışmayın - yukarıda gösterilen.  

Bir bağlam denenmek üzere kod bloğu içinde oluşturulması gereken unutmayın. Bu, her yeniden deneme için temiz bir durum ile başlıyoruz olmasını sağlar.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
