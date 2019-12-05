---
title: Bağlantı dayanıklılığı ve yeniden deneme mantığı-EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824844"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="edd83-102">Bağlantı dayanıklılığı ve yeniden deneme mantığı</span><span class="sxs-lookup"><span data-stu-id="edd83-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="edd83-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="edd83-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="edd83-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="edd83-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="edd83-105">Bir veritabanı sunucusuna bağlanan uygulamalar, arka uç arızalarına ve ağ kararsızlığına neden olması nedeniyle her zaman bağlantı kesilmelerinden etkilenmektedir.</span><span class="sxs-lookup"><span data-stu-id="edd83-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="edd83-106">Ancak, adanmış veritabanı sunucularında çalışan bir LAN tabanlı ortamda bu hatalar, bu hataları işlemek için ek mantık genellikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="edd83-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="edd83-107">Windows Azure SQL veritabanı gibi bulut tabanlı veritabanı sunucularının ve daha az güvenilir ağlarla kurulan bağlantıların artmasıyla bağlantı sonlarının gerçekleşmesi için artık daha yaygın hale gelir.</span><span class="sxs-lookup"><span data-stu-id="edd83-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="edd83-108">Bunun nedeni, bulut veritabanlarının bağlantı azaltma veya ağdaki kararsızlığa yol açan zaman aşımları ve diğer geçici hatalara neden olması gibi hizmet eşitliği güvence altına almak için kullandığı savunma tekniklerinden kaynaklanıyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="edd83-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="edd83-109">Bağlantı dayanıklılığı, bu bağlantı molaları nedeniyle başarısız olan komutları otomatik olarak yeniden denemeye yönelik bir özellik anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="edd83-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="edd83-110">Yürütme stratejileri</span><span class="sxs-lookup"><span data-stu-id="edd83-110">Execution Strategies</span></span>  

<span data-ttu-id="edd83-111">Bağlantı yeniden denemesi, ıdbexecutionstrateji arabiriminin bir uygulamasıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="edd83-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="edd83-112">Bir işlemi kabul etmeden ve bir özel durum oluşursa, bir yeniden denenmenin uygun olup olmadığını belirlemek ve varsa yeniden denemek, ıdbexecutionstratejisinin uygulamalarına sorumludur.</span><span class="sxs-lookup"><span data-stu-id="edd83-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="edd83-113">EF ile birlikte gelen dört yürütme stratejisi vardır:</span><span class="sxs-lookup"><span data-stu-id="edd83-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="edd83-114">**Defaultexecutionstrateji**: Bu yürütme stratejisi herhangi bir işlemi yeniden denemez, SQL Server dışındaki veritabanları için varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="edd83-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="edd83-115">**Defaultsqlexecutionstrateji**: Bu, varsayılan olarak kullanılan bir iç yürütme stratejisidir.</span><span class="sxs-lookup"><span data-stu-id="edd83-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="edd83-116">Bu strateji hiç denenmez, ancak kullanıcılara bağlantı dayanıklılığı sağlamak istedikleri kullanıcıları bilgilendirmek için geçici olabilecek tüm özel durumları saracaktır.</span><span class="sxs-lookup"><span data-stu-id="edd83-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="edd83-117">**Dbexecutionstrateji**: Bu sınıf, kendi özel sahipleriniz de dahil olmak üzere diğer yürütme stratejileri için temel sınıf olarak uygundur.</span><span class="sxs-lookup"><span data-stu-id="edd83-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="edd83-118">İlk yeniden deneme sıfır gecikmeyle gerçekleştiği ve gecikme en fazla yeniden deneme sayısına ulaşana kadar üstel olarak arttığı bir üstel yeniden deneme ilkesi uygular.</span><span class="sxs-lookup"><span data-stu-id="edd83-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="edd83-119">Bu sınıfın, hangi özel durumların yeniden deneneceği denetlemek için türetilmiş yürütme stratejilerinde uygulanabilen bir soyut ShouldRetryOn yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="edd83-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="edd83-120">**SqlAzureExecutionStrategy**: Bu yürütme stratejisi dbexecutionstratejinizi devralır ve Azure SQL veritabanı ile çalışırken belki de geçici olarak bilinen özel durumlar üzerinde yeniden denenecek.</span><span class="sxs-lookup"><span data-stu-id="edd83-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="edd83-121">2 ve 4. yürütme stratejileri, EntityFramework. SqlServer derlemesinde yer alan EF ile birlikte gelen SQL Server sağlayıcısına dahildir ve SQL Server çalışmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="edd83-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="edd83-122">Yürütme stratejisini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="edd83-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="edd83-123">Bir yürütme stratejisi kullanmak için EF 'in en kolay yolu, [DBConfiguration](~/ef6/fundamentals/configuring/code-based.md) sınıfının Setexecutionstrateji yöntemidir:</span><span class="sxs-lookup"><span data-stu-id="edd83-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="edd83-124">Bu kod, SQL Server bağlanırken SqlAzureExecutionStrategy 'ı kullanmasını söyler.</span><span class="sxs-lookup"><span data-stu-id="edd83-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="edd83-125">Yürütme stratejisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="edd83-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="edd83-126">SqlAzureExecutionStrategy Oluşturucusu iki parametreyi kabul edebilir, MaxRetryCount ve MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="edd83-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="edd83-127">MaxRetry sayısı, stratejinin en fazla kaç kez yeniden deneneceği sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="edd83-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="edd83-128">MaxDelay, yürütme stratejisinin kullanacağı yeniden denemeler arasındaki en fazla gecikmeyi temsil eden bir TimeSpan değeri.</span><span class="sxs-lookup"><span data-stu-id="edd83-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="edd83-129">En fazla yeniden deneme sayısını 1 ' e ve en fazla 30 saniyeye gecikme süresini ayarlamak için şunları yürütebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="edd83-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

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

<span data-ttu-id="edd83-130">Geçici bir hata oluşması durumunda SqlAzureExecutionStrategy anında yeniden dener, ancak en fazla yeniden deneme sınırı aşılana veya toplam süre en fazla gecikme süresine rastlana kadar her yeniden denemeye karşı daha fazla gecikme olur.</span><span class="sxs-lookup"><span data-stu-id="edd83-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="edd83-131">Yürütme stratejileri genellikle geçici olan sınırlı sayıda özel durumu yeniden dener, ancak hata geçici olmayan veya çözülmesi çok uzun süren bir durum için diğer hataları da işlemeniz ve Retrylimitexcenersel özel durumunu yakalamak gerekir ilişkilendirilemez.</span><span class="sxs-lookup"><span data-stu-id="edd83-131">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="edd83-132">Yeniden deneme yürütme stratejisi kullanırken bazı sınırlamalar bilinmektedir:</span><span class="sxs-lookup"><span data-stu-id="edd83-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="edd83-133">Akış sorguları desteklenmez</span><span class="sxs-lookup"><span data-stu-id="edd83-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="edd83-134">Varsayılan olarak, EF6 ve sonraki sürümleri, bunları akışa almak yerine sorgu sonuçlarını arabelleğe alacak.</span><span class="sxs-lookup"><span data-stu-id="edd83-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="edd83-135">Sonuçların akışını yapmak istiyorsanız, bir LINQ to Entities sorgusunu akışa dönüştürmek için AsStreaming yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edd83-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="edd83-136">Yeniden deneme yürütme stratejisi kaydedildiğinde akış desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="edd83-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="edd83-137">Bu sınırlama, bağlantı, döndürülmekte olan sonuçlar aracılığıyla bir parça yolunu düşürülemediğinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="edd83-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="edd83-138">Bu gerçekleştiğinde, AŞV 'nin tüm sorguyu yeniden çalıştırması gerekir, ancak hangi sonuçların daha önce döndürüldüğünü bilmenin güvenilir bir yolu yoktur (ilk sorgu gönderildikten sonra veriler değişmiş olabilir, sonuçlar farklı bir sıraya dönebilir ve sonuçlar benzersiz bir tanımlayıcıya sahip olamaz , vb.).</span><span class="sxs-lookup"><span data-stu-id="edd83-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="edd83-139">Kullanıcı tarafından başlatılan işlemler desteklenmez</span><span class="sxs-lookup"><span data-stu-id="edd83-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="edd83-140">Yeniden denemeler sonucu veren bir yürütme stratejisi yapılandırdığınızda, işlem kullanımı etrafında bazı sınırlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="edd83-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="edd83-141">Varsayılan olarak, EF bir işlem içinde herhangi bir veritabanı güncelleştirmesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="edd83-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="edd83-142">Bunu etkinleştirmek için herhangi bir şey yapmanız gerekmez, EF her zaman otomatik olarak bunu yapar.</span><span class="sxs-lookup"><span data-stu-id="edd83-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="edd83-143">Örneğin, aşağıdaki kod SaveChanges bir işlem içinde otomatik olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="edd83-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="edd83-144">SaveChanges, yeni siteden birini ekledikten sonra başarısız olursa işlem geri alınır ve veritabanına hiçbir değişiklik uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="edd83-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="edd83-145">Bağlam Ayrıca, SaveChanges 'ın değişiklikleri uygulamayı yeniden denemek için yeniden çağrılmasına izin veren bir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="edd83-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="edd83-146">Yeniden deneme yürütme stratejisi kullanmadığınız durumlarda, birden çok işlemi tek bir işlemde kaydırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edd83-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="edd83-147">Örneğin, aşağıdaki kod tek bir işlemde iki SaveChanges çağrısı sarmalanmış.</span><span class="sxs-lookup"><span data-stu-id="edd83-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="edd83-148">Her iki işlemin bir parçası başarısız olursa, değişikliklerden hiçbiri uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="edd83-148">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="edd83-149">Bu, bir yürütme stratejisi kullanılırken, EF önceki işlemlerden haberdar olmadığından ve bunların nasıl yeniden denenmediğinden desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="edd83-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="edd83-150">Örneğin, ikinci SaveChanges başarısız olduysa EF, ilk SaveChanges çağrısını yeniden denemek için gereken bilgileri artık yok.</span><span class="sxs-lookup"><span data-stu-id="edd83-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="solution-manually-call-execution-strategy"></a><span data-ttu-id="edd83-151">Çözüm: El Ile çağrı yürütme stratejisi</span><span class="sxs-lookup"><span data-stu-id="edd83-151">Solution: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="edd83-152">Çözüm, yürütme stratejisini el ile kullanmak ve bir işlem başarısız olursa her şeyi yeniden denemek için bu uygulamayı çalıştırılacak mantığı kümesinin tamamına vermektir.</span><span class="sxs-lookup"><span data-stu-id="edd83-152">The solution is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="edd83-153">Dbexecutionstratejisinden türetilen bir yürütme stratejisi çalıştırıldığında, SaveChanges 'da kullanılan örtük yürütme stratejisini askıya alır.</span><span class="sxs-lookup"><span data-stu-id="edd83-153">When an execution strategy derived from DbExecutionStrategy is running it will suspend the implicit execution strategy used in SaveChanges.</span></span>  

<span data-ttu-id="edd83-154">Yeniden denenmek üzere kod bloğu içinde herhangi bir bağlam oluşturulması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="edd83-154">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="edd83-155">Bu, her yeniden deneme için temiz bir durumla başlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="edd83-155">This ensures that we are starting with a clean state for each retry.</span></span>  

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
