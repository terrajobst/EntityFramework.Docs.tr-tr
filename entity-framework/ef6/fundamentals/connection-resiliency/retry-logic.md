---
title: Bağlantı dayanıklılığı ve yeniden deneme mantığı - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
caps.latest.revision: 3
ms.openlocfilehash: 4c6e296ecb8b43468408d359f44fd1d1a6edf864
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914223"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="be20a-102">Bağlantı dayanıklılığı ve yeniden deneme mantığı</span><span class="sxs-lookup"><span data-stu-id="be20a-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="be20a-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="be20a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="be20a-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="be20a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="be20a-105">Bir veritabanı sunucusuna bağlanan uygulamalar her zaman bağlantı sonu nedeniyle arka uç hataları ve ağ kararsızlığı savunmasız olmuştur.</span><span class="sxs-lookup"><span data-stu-id="be20a-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="be20a-106">Ancak, adanmış veritabanı sunucularına karşı çalışan bir LAN tabanlı ortamda bu ek mantık hataları işlemek için genellikle gerekli olmadığını yeterince nadir hatalardır.</span><span class="sxs-lookup"><span data-stu-id="be20a-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="be20a-107">Bulut Yükselişi artık bağlantı sonlarını gerçekleşmesi daha yaygın daha az güvenilir ağ üzerinden Windows Azure SQL veritabanı gibi veritabanı sunucuları ve bağlantıları temel.</span><span class="sxs-lookup"><span data-stu-id="be20a-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="be20a-108">Bu, veritabanlarını kullanım hizmeti aralıklı zaman aşımları ve diğer geçici hataları neden ağ kararsızlığı veya bu bağlantı azaltma gibi eşitliği emin olmak için bulut savunma teknikleri nedeniyle olabilir.</span><span class="sxs-lookup"><span data-stu-id="be20a-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="be20a-109">Bağlantı dayanıklılığı EF otomatik olarak bu bağlantı sonu nedeniyle başarısız olan tüm komutları yeniden denemek özelliği ifade eder.</span><span class="sxs-lookup"><span data-stu-id="be20a-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="be20a-110">Yürütme stratejileri</span><span class="sxs-lookup"><span data-stu-id="be20a-110">Execution Strategies</span></span>  

<span data-ttu-id="be20a-111">Bağlantı yeniden deneme Idbexecutionstrategy arabirimi uygulaması tarafından dikkate.</span><span class="sxs-lookup"><span data-stu-id="be20a-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="be20a-112">Idbexecutionstrategy uygulamaları bir işlem kabul ederek ve bir özel durum oluşursa, bir yeniden denemenin uygun olup olmadığını belirlemek ve varsa, yeniden deneniyor, sorumlu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="be20a-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="be20a-113">EF ile birlikte dört yürütme strateji vardır:</span><span class="sxs-lookup"><span data-stu-id="be20a-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="be20a-114">**DefaultExecutionStrategy**: sql server dışındaki veritabanları için varsayılan değer, bu yürütme stratejisi herhangi bir işlemi yeniden denemez.</span><span class="sxs-lookup"><span data-stu-id="be20a-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="be20a-115">**DefaultSqlExecutionStrategy**: varsayılan olarak kullanılan bir iç yürütme stratejisi budur.</span><span class="sxs-lookup"><span data-stu-id="be20a-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="be20a-116">Bu tüm yeniden deneme stratejisi değil, ancak bağlantı dayanıklılığı etkinleştirmek isteyebileceğiniz kullanıcılara bildirmek için geçici özel durumları kaydırılır.</span><span class="sxs-lookup"><span data-stu-id="be20a-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="be20a-117">**DbExecutionStrategy**: Bu sınıf temel sınıf olarak kendi özel olanları dahil olmak üzere diğer yürütme stratejileri için uygundur.</span><span class="sxs-lookup"><span data-stu-id="be20a-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="be20a-118">Maksimum yeniden deneme sayısına ulaşılır kadar burada ilk yeniden deneme gecikmesi ve sıfır gecikme ile artar katlanarak olmaz bir üstel yeniden deneme ilkesi uygular.</span><span class="sxs-lookup"><span data-stu-id="be20a-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="be20a-119">Bu sınıfın türetilmiş yürütme stratejileri hangi özel durumları yeniden denenmesi gerektiğini denetlemek için uygulanabilir bir soyut ShouldRetryOn yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="be20a-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="be20a-120">**SqlAzureExecutionStrategy**: Bu yürütme stratejisi DbExecutionStrategy devralır ve Azure SQL veritabanı ile çalışırken büyük olasılıkla geçici olduğu bilinen özel durumlar yeniden denenecek.</span><span class="sxs-lookup"><span data-stu-id="be20a-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="be20a-121">Yürütme stratejileri 2 ve 4 EntityFramework.SqlServer derlemede olan EF ile birlikte gelen Sql Server sağlayıcısı eklenir ve SQL Server ile çalışacak şekilde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="be20a-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="be20a-122">Bir yürütme stratejisi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="be20a-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="be20a-123">Bir yürütme stratejisi kullanmayı EF bildirmek için en kolay yolu SetExecutionStrategy ile yöntemidir [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) sınıfı:</span><span class="sxs-lookup"><span data-stu-id="be20a-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="be20a-124">Bu kod SqlAzureExecutionStrategy SQL Server'a bağlanırken kullanılacak EF söyler.</span><span class="sxs-lookup"><span data-stu-id="be20a-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="be20a-125">Yürütme stratejisi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="be20a-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="be20a-126">SqlAzureExecutionStrategy oluşturucusunun MaxRetryCount ve MaxDelay iki parametre kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="be20a-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="be20a-127">MaxRetry, strateji yeniden denenme sayısı sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="be20a-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="be20a-128">Yürütme stratejisi kullanacağı yeniden denemeler arasındaki en büyük gecikme temsil eden bir TimeSpan MaxDelay olur.</span><span class="sxs-lookup"><span data-stu-id="be20a-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="be20a-129">Yeniden deneme sayısı 1 ve 30 saniyeye kadar en büyük gecikme ayarlamak için aşağıdaki execue gerekir:</span><span class="sxs-lookup"><span data-stu-id="be20a-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

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

<span data-ttu-id="be20a-130">SqlAzureExecutionStrategy geçici bir hata meydana gelir, ancak ya da en fazla yeniden deneme kadar sınırı denemeler arasında daha uzun süre gecikeceğini ilk kez aşıldığında veya toplam süreyi en uzun gecikme İsabetleri instantly yeniden deneyecek.</span><span class="sxs-lookup"><span data-stu-id="be20a-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="be20a-131">Yürütme stratejileri yalnızca sınırlı sayıda tansient genellikle olan özel durumlar deneyecek, diğer hataları yanı sıra RetryLimitExceeded istisnayı çözmek burada bir hata geçici değilse veya çok uzun sürüyor durumu işlemek yine de gerekir kendisi.</span><span class="sxs-lookup"><span data-stu-id="be20a-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

## <a name="limitations"></a><span data-ttu-id="be20a-132">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="be20a-132">Limitations</span></span>  

<span data-ttu-id="be20a-133">Yeniden denenirken bir yürütme stratejisi kullanarak oluşan bazı bilinen sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="be20a-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

### <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="be20a-134">Akış sorgular desteklenmez</span><span class="sxs-lookup"><span data-stu-id="be20a-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="be20a-135">Varsayılan olarak, EF6 ve sonraki bir sürümü bunları akış yerine sorgu sonuçları arabellekte tutar.</span><span class="sxs-lookup"><span data-stu-id="be20a-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="be20a-136">Sahip olmasını isterseniz sonuçları akış AsStreaming yöntemi bir LINQ to Entities sorgusunda akış değiştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="be20a-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="be20a-137">Yeniden denenirken bir yürütme stratejisi kaydedildiğinde akış desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="be20a-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="be20a-138">Bağlantı parçası şekilde döndürülen sonuç bırak çünkü bu sınırlama bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="be20a-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="be20a-139">Böyle bir durumda EF sorgunun tamamını yeniden çalıştırmak için gereken ancak hangi sonuçları zaten iade edilmiş olduğunu bilmesinin güvenilir bir yolu yoktur (veri değişmiş olabilir bu yana ilk sorgunun gönderildiği, sonuçları gelen geri farklı bir sırada, sonuçları benzersiz bir tanımlayıcı olmayabilir VS.).</span><span class="sxs-lookup"><span data-stu-id="be20a-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

### <a name="user-initiated-transactions-not-supported"></a><span data-ttu-id="be20a-140">Kullanıcı tarafından başlatılan işlemleri desteklenmiyor</span><span class="sxs-lookup"><span data-stu-id="be20a-140">User initiated transactions not supported</span></span>  

<span data-ttu-id="be20a-141">Yeniden denemeler sonuçları bir yürütme stratejisi yapılandırıldığında, işlem kullanımı geçici olarak bazı sınırlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="be20a-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

#### <a name="whats-supported-efs-default-transaction-behavior"></a><span data-ttu-id="be20a-142">Desteklenen özellikler: EF'ın varsayılan işlem davranışı</span><span class="sxs-lookup"><span data-stu-id="be20a-142">What's Supported: EF's default transaction behavior</span></span>  

<span data-ttu-id="be20a-143">Varsayılan olarak EF, bir işlem içinde herhangi bir veritabanı güncelleştirme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="be20a-143">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="be20a-144">Bunu etkinleştirmek için herhangi bir şey yapmanız gerekmez, EF her zaman bunu otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="be20a-144">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="be20a-145">Örneğin, aşağıdaki kodda SaveChanges otomatik olarak bir işlem içinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="be20a-145">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="be20a-146">SaveChanges işlem geri alınamaz sonra yeni sitenin birini ekleyerek ve veritabanına uygulanan herhangi bir değişiklik sonra arızalanması durumunda.</span><span class="sxs-lookup"><span data-stu-id="be20a-146">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="be20a-147">Bağlam, ayrıca değişiklikleri uygulamadan yeniden denemek için yeniden çağrılması SaveChanges izin veren bir durumda bırakılır.</span><span class="sxs-lookup"><span data-stu-id="be20a-147">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a><span data-ttu-id="be20a-148">Neler desteklenmiyor: kullanıcı tarafından başlatılan işlemleri</span><span class="sxs-lookup"><span data-stu-id="be20a-148">What’s not supported: User initiated transactions</span></span>  

<span data-ttu-id="be20a-149">Birden çok işlem tek bir işlemde kayabilir yeniden denenirken bir yürütme stratejisi kullanılmadığında.</span><span class="sxs-lookup"><span data-stu-id="be20a-149">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="be20a-150">Örneğin, aşağıdaki kod, tek bir işlemde iki SaveChanges çağrılarını sarmalar.</span><span class="sxs-lookup"><span data-stu-id="be20a-150">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="be20a-151">Herhangi bir bölümünü ya da işlem değişikliklerin hiçbiri sonra başarısız olursa uygulanır.</span><span class="sxs-lookup"><span data-stu-id="be20a-151">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="be20a-152">Bu, EF önceki işlemleri ve bunları yeniden deneme işlemleri farkında olmadığından yeniden denenirken bir yürütme stratejisi kullanılırken desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="be20a-152">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="be20a-153">Örneğin, ikinci SaveChanges EF artık sonra başarısız olursa ilk SaveChanges çağrıyı yeniden denemesi için gerekli olan bilgileri vardır.</span><span class="sxs-lookup"><span data-stu-id="be20a-153">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

#### <a name="possible-workarounds"></a><span data-ttu-id="be20a-154">Olası geçici çözümler</span><span class="sxs-lookup"><span data-stu-id="be20a-154">Possible workarounds</span></span>  

##### <a name="suspend-execution-strategy"></a><span data-ttu-id="be20a-155">Yürütme stratejisini askıya alma</span><span class="sxs-lookup"><span data-stu-id="be20a-155">Suspend Execution Strategy</span></span>  

<span data-ttu-id="be20a-156">Olası bir geçici çözüm, bir kullanıcı kullanmak için gereken kod parçasını deneniyor yürütme stratejisini askıya alma işlemi tarafından başlatılan ' dir.</span><span class="sxs-lookup"><span data-stu-id="be20a-156">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="be20a-157">Bunu yapmanın en kolay yolu, kodunuza SuspendExecutionStrategy bayrak yapılandırma sınıfı tabanlı ve bayrak ayarlandığında, varsayılan (retying olmayan) yürütme stratejisi döndürmek için yürütme stratejisi lambda değiştirme eklemektir.</span><span class="sxs-lookup"><span data-stu-id="be20a-157">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

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

<span data-ttu-id="be20a-158">Unutmayın, biz CallContext bayrak değerini depolamak için kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="be20a-158">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="be20a-159">Bu iş parçacığı yerel depolama benzer bir işlevsellik sağlar, ancak zaman uyumsuz sorgu dahil olmak üzere zaman uyumsuz kodla - kullanın ve Entity Framework ile kaydetmek güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="be20a-159">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="be20a-160">Biz, artık bir kullanıcı tarafından başlatılan işlem kullanan kod bölümünün yürütme stratejisini askıya alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="be20a-160">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

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

##### <a name="manually-call-execution-strategy"></a><span data-ttu-id="be20a-161">El ile yürütme stratejisi çağırın</span><span class="sxs-lookup"><span data-stu-id="be20a-161">Manually Call Execution Strategy</span></span>  

<span data-ttu-id="be20a-162">Başka bir seçenek, el ile yürütme stratejisi kullanın ve böylece işlemlerden biri başarısız olursa her şeyi deneyebilirsiniz çalıştırılması için bir mantık kümesinin tamamını verin oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="be20a-162">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="be20a-163">Biz yine de tekniği kullanarak yürütme stratejisi - askıya gerekir, böylece yeniden denemek yeniden denenebilir kod bloğu içinde kullanılan bir bağlam çalışmayın - yukarıda gösterilen.</span><span class="sxs-lookup"><span data-stu-id="be20a-163">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="be20a-164">Bir bağlam denenmek üzere kod bloğu içinde oluşturulması gereken unutmayın.</span><span class="sxs-lookup"><span data-stu-id="be20a-164">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="be20a-165">Bu, her yeniden deneme için temiz bir durum ile başlıyoruz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="be20a-165">This ensures that we are starting with a clean state for each retry.</span></span>  

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
