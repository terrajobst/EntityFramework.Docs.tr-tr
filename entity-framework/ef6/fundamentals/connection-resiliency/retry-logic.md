---
title: Bağlantı dayanıklılığı ve yeniden deneme mantığı-EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402161"
---
# <a name="connection-resiliency-and-retry-logic"></a>Bağlantı dayanıklılığı ve yeniden deneme mantığı
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Bir veritabanı sunucusuna bağlanan uygulamalar, arka uç arızalarına ve ağ kararsızlığına neden olması nedeniyle her zaman bağlantı kesilmelerinden etkilenmektedir. Ancak, adanmış veritabanı sunucularında çalışan bir LAN tabanlı ortamda bu hatalar, bu hataları işlemek için ek mantık genellikle gerekli değildir. Windows Azure SQL veritabanı gibi bulut tabanlı veritabanı sunucularının ve daha az güvenilir ağlarla kurulan bağlantıların artmasıyla bağlantı sonlarının gerçekleşmesi için artık daha yaygın hale gelir. Bunun nedeni, bulut veritabanlarının bağlantı azaltma veya ağdaki kararsızlığa yol açan zaman aşımları ve diğer geçici hatalara neden olması gibi hizmet eşitliği güvence altına almak için kullandığı savunma tekniklerinden kaynaklanıyor olabilir.  

Bağlantı dayanıklılığı, bu bağlantı molaları nedeniyle başarısız olan komutları otomatik olarak yeniden denemeye yönelik bir özellik anlamına gelir.  

## <a name="execution-strategies"></a>Yürütme stratejileri  

Bağlantı yeniden denemesi, ıdbexecutionstrateji arabiriminin bir uygulamasıyla yapılır. Bir işlemi kabul etmeden ve bir özel durum oluşursa, bir yeniden denenmenin uygun olup olmadığını belirlemek ve varsa yeniden denemek, ıdbexecutionstratejisinin uygulamalarına sorumludur. EF ile birlikte gelen dört yürütme stratejisi vardır:  

1. **Defaultexecutionstrateji**: Bu yürütme stratejisi herhangi bir işlemi yeniden denemez, SQL Server dışındaki veritabanları için varsayılandır.  
2. **Defaultsqlexecutionstrateji**: Bu, varsayılan olarak kullanılan bir iç yürütme stratejisidir. Bu strateji hiç denenmez, ancak kullanıcılara bağlantı dayanıklılığı sağlamak istedikleri kullanıcıları bilgilendirmek için geçici olabilecek tüm özel durumları saracaktır.  
3. **Dbexecutionstrateji**: Bu sınıf, kendi özel sahipleriniz de dahil olmak üzere diğer yürütme stratejileri için temel sınıf olarak uygundur. İlk yeniden deneme sıfır gecikmeyle gerçekleştiği ve gecikme en fazla yeniden deneme sayısına ulaşana kadar üstel olarak arttığı bir üstel yeniden deneme ilkesi uygular. Bu sınıfın, hangi özel durumların yeniden deneneceği denetlemek için türetilmiş yürütme stratejilerinde uygulanabilen bir soyut ShouldRetryOn yöntemi vardır.  
4. **SqlAzureExecutionStrategy**: Bu yürütme stratejisi dbexecutionstratejinizi devralır ve Azure SQL veritabanı ile çalışırken belki de geçici olarak bilinen özel durumlar üzerinde yeniden denenecek.

> [!NOTE]
> 2 ve 4. yürütme stratejileri, EntityFramework. SqlServer derlemesinde yer alan EF ile birlikte gelen SQL Server sağlayıcısına dahildir ve SQL Server çalışmak üzere tasarlanmıştır.  

## <a name="enabling-an-execution-strategy"></a>Yürütme stratejisini etkinleştirme  

Bir yürütme stratejisi kullanmak için EF 'in en kolay yolu, [DBConfiguration](~/ef6/fundamentals/configuring/code-based.md) sınıfının Setexecutionstrateji yöntemidir:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Bu kod, SQL Server bağlanırken SqlAzureExecutionStrategy 'ı kullanmasını söyler.  

## <a name="configuring-the-execution-strategy"></a>Yürütme stratejisini yapılandırma  

SqlAzureExecutionStrategy Oluşturucusu iki parametreyi kabul edebilir, MaxRetryCount ve MaxDelay. MaxRetry sayısı, stratejinin en fazla kaç kez yeniden deneneceği sayısıdır. MaxDelay, yürütme stratejisinin kullanacağı yeniden denemeler arasındaki en fazla gecikmeyi temsil eden bir TimeSpan değeri.  

En fazla yeniden deneme sayısını 1 ' e ve en fazla 30 saniyeye gecikme süresini ayarlamak için şunları yürütebilirsiniz:  

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

Geçici bir hata oluşması durumunda SqlAzureExecutionStrategy anında yeniden dener, ancak en fazla yeniden deneme sınırı aşılana veya toplam süre en fazla gecikme süresine rastlana kadar her yeniden denemeye karşı daha fazla gecikme olur.  

Yürütme stratejileri genellikle geçici olan sınırlı sayıda özel durumu yeniden dener, ancak hata geçici olmayan veya çözülmesi çok uzun süren bir durum için diğer hataları da işlemeniz ve Retrylimitexcenersel özel durumunu yakalamak gerekir ilişkilendirilemez.  

Yeniden deneme yürütme stratejisi kullanırken bazı sınırlamalar bilinmektedir:  

## <a name="streaming-queries-are-not-supported"></a>Akış sorguları desteklenmez  

Varsayılan olarak, EF6 ve sonraki sürümleri, bunları akışa almak yerine sorgu sonuçlarını arabelleğe alacak. Sonuçların akışını yapmak istiyorsanız, bir LINQ to Entities sorgusunu akışa dönüştürmek için AsStreaming yöntemini kullanabilirsiniz.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Yeniden deneme yürütme stratejisi kaydedildiğinde akış desteklenmez. Bu sınırlama, bağlantı, döndürülmekte olan sonuçlar aracılığıyla bir parça yolunu düşürülemediğinden oluşur. Bu gerçekleştiğinde, AŞV 'nin tüm sorguyu yeniden çalıştırması gerekir, ancak hangi sonuçların daha önce döndürüldüğünü bilmenin güvenilir bir yolu yoktur (ilk sorgu gönderildikten sonra veriler değişmiş olabilir, sonuçlar farklı bir sıraya dönebilir ve sonuçlar benzersiz bir tanımlayıcıya sahip olamaz , vb.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Kullanıcı tarafından başlatılan işlemler desteklenmez  

Yeniden denemeler sonucu veren bir yürütme stratejisi yapılandırdığınızda, işlem kullanımı etrafında bazı sınırlamalar vardır.  

Varsayılan olarak, EF bir işlem içinde herhangi bir veritabanı güncelleştirmesi gerçekleştirir. Bunu etkinleştirmek için herhangi bir şey yapmanız gerekmez, EF her zaman otomatik olarak bunu yapar.  

Örneğin, aşağıdaki kod SaveChanges bir işlem içinde otomatik olarak gerçekleştirilir. SaveChanges, yeni siteden birini ekledikten sonra başarısız olursa işlem geri alınır ve veritabanına hiçbir değişiklik uygulanmaz. Bağlam Ayrıca, SaveChanges 'ın değişiklikleri uygulamayı yeniden denemek için yeniden çağrılmasına izin veren bir durumda kalır.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Yeniden deneme yürütme stratejisi kullanmadığınız durumlarda, birden çok işlemi tek bir işlemde kaydırabilirsiniz. Örneğin, aşağıdaki kod tek bir işlemde iki SaveChanges çağrısı sarmalanmış. Her iki işlemin bir parçası başarısız olursa, değişikliklerden hiçbiri uygulanmaz.  

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

Bu, bir yürütme stratejisi kullanılırken, EF önceki işlemlerden haberdar olmadığından ve bunların nasıl yeniden denenmediğinden desteklenmez. Örneğin, ikinci SaveChanges başarısız olduysa EF, ilk SaveChanges çağrısını yeniden denemek için gereken bilgileri artık yok.  

### <a name="solution-manually-call-execution-strategy"></a>Çözüm: El Ile çağrı yürütme stratejisi  

Çözüm, yürütme stratejisini el ile kullanmak ve bir işlem başarısız olursa her şeyi yeniden denemek için bu uygulamayı çalıştırılacak mantığı kümesinin tamamına vermektir. Dbexecutionstratejisinden türetilen bir yürütme stratejisi çalıştırıldığında, SaveChanges 'da kullanılan örtük yürütme stratejisini askıya alır.  

Yeniden denenmek üzere kod bloğu içinde herhangi bir bağlam oluşturulması gerektiğini unutmayın. Bu, her yeniden deneme için temiz bir durumla başlıyoruz.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

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
```  
