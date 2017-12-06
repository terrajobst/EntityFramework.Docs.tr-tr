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
# <a name="connection-resiliency"></a><span data-ttu-id="36c50-102">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="36c50-102">Connection Resiliency</span></span>

<span data-ttu-id="36c50-103">Bağlantı dayanıklılığı başarısız veritabanı komutları otomatik olarak yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="36c50-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="36c50-104">Bu özellik "hatalarını algılayacak ve komutları yeniden denemek gerekli mantığı kapsülleyen bir yürütme stratejisi", sağlayarak herhangi bir veritabanı ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="36c50-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="36c50-105">EF çekirdek sağlayıcılar, belirli bir veritabanını hata koşulları ve en iyi yeniden deneme ilkelerini uyarlanmış yürütme stratejileri sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="36c50-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="36c50-106">Örnek olarak, SQL Server sağlayıcısı SQL Server (SQL Azure dahil) için özellikle tasarlanmış bir yürütme stratejisi içerir.</span><span class="sxs-lookup"><span data-stu-id="36c50-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="36c50-107">Yeniden denenebilir özel durum türlerini farkındadır ve maksimum yeniden deneme sayısı, vb. yeniden denemeler arasındaki gecikme için duyarlı Varsayılanları olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="36c50-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="36c50-108">Bir yürütme stratejisi içeriğiniz için seçenekleri yapılandırma sırasında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="36c50-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="36c50-109">Bu genellikle kullanımda `OnConfiguring` yöntemi türetilmiş içeriğiniz, ya da `Startup.cs` ASP.NET Core uygulamanın.</span><span class="sxs-lookup"><span data-stu-id="36c50-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="36c50-110">Özel yürütme stratejisi</span><span class="sxs-lookup"><span data-stu-id="36c50-110">Custom execution strategy</span></span>

<span data-ttu-id="36c50-111">Varsayılanları değiştirmek isterseniz, kendi özel yürütme stratejisi kaydetmek için bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="36c50-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="36c50-112">Yürütme stratejilerini ve işlemler</span><span class="sxs-lookup"><span data-stu-id="36c50-112">Execution strategies and transactions</span></span>

<span data-ttu-id="36c50-113">Hataları üzerinde otomatik olarak yeniden dener bir yürütme stratejisi her işlemi başarısız bir yeniden deneme bloğunda çalmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="36c50-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="36c50-114">Yeniden deneme etkinleştirildiğinde, EF çekirdek gerçekleştirdiğiniz her işlem yeniden denenebilir kendi işlemi, yani her sorgu ve her çağrı hale `SaveChanges()` geçici bir hata oluşursa, bir birim olarak yeniden denenecek.</span><span class="sxs-lookup"><span data-stu-id="36c50-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="36c50-115">Ancak, kodunuzu kullanarak bir işlem başlatıyorsa `BeginTransaction()` bir birim olarak kabul edilmesi için gereken işlemleri kendi grubunun tanımlıyorsanız, yani işlem içinde her şeyi geri bir hata meydana çalınacak gerekir.</span><span class="sxs-lookup"><span data-stu-id="36c50-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="36c50-116">Bir yürütme stratejisi kullanırken bunu denerseniz aşağıdaki gibi bir özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="36c50-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="36c50-117">InvalidOperationException: 'SqlServerRetryingExecutionStrategy' yapılandırılmış yürütme stratejisi kullanıcı tarafından başlatılan işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="36c50-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="36c50-118">İşlem yeniden denenebilir bir birim olarak tüm işlemleri yürütmek için 'DbContext.Database.CreateExecutionStrategy()' tarafından döndürülen yürütme stratejisi kullanın.</span><span class="sxs-lookup"><span data-stu-id="36c50-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="36c50-119">Çözüm, yürütülecek gereken her şeyi temsil eden bir temsilci ile yürütme stratejisi el ile çağırmaktır.</span><span class="sxs-lookup"><span data-stu-id="36c50-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="36c50-120">Geçici bir hata oluşursa, yürütme stratejisi temsilci yeniden çağırır.</span><span class="sxs-lookup"><span data-stu-id="36c50-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="36c50-121">İşlem yürütme hatası ve benzersizlik sorunu</span><span class="sxs-lookup"><span data-stu-id="36c50-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="36c50-122">Genel olarak, bir bağlantı hatası olduğunda geçerli işlem geri alınır.</span><span class="sxs-lookup"><span data-stu-id="36c50-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="36c50-123">İşlem sırasında bağlantı kesilirse ancak olma elde edilen kaydedilen işlem durumu bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="36c50-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="36c50-124">Bu bkz [blog gönderisi](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="36c50-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="36c50-125">Varsayılan olarak, yürütme stratejisi işlemi işlem geri alındı, ancak durum değilse yeni veritabanı durumu uyumsuz ya da neden olabilir, bu bir özel durumla sonuçlanır sanki deneyecek **veri bozulması** varsa işlem otomatik olarak oluşturulan anahtar değerlerini içeren yeni bir satır eklerken örneğin belirli bir durumdaki bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="36c50-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="36c50-126">Bu ile mücadele etmek için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="36c50-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="36c50-127">Seçenek 1 - (neredeyse) hiçbir şey</span><span class="sxs-lookup"><span data-stu-id="36c50-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="36c50-128">İşlem yürütme sırasında bağlantı hatası olasılığını düşük olduğundan aslında bu durum oluşursa, yalnızca başarısız olmasına, uygulamanız için kabul edilebilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="36c50-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="36c50-129">Ancak, yinelenen satır ekleme yerine bir özel durum emin olmak için depoda üretilmiş anahtarlar kullanmaktan kaçının gerekir.</span><span class="sxs-lookup"><span data-stu-id="36c50-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="36c50-130">Bir istemci tarafından üretilen GUID değeri veya bir istemci-tarafı değeri oluşturucuyu kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="36c50-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="36c50-131">Seçenek 2 - yeniden uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="36c50-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="36c50-132">Geçerli atmak `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="36c50-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="36c50-133">Yeni bir `DbContext` ve veritabanından uygulamanızın durumunu geri yükle.</span><span class="sxs-lookup"><span data-stu-id="36c50-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="36c50-134">Kullanıcı son işlemi başarıyla tamamlanmamış gerektiğini bildirin.</span><span class="sxs-lookup"><span data-stu-id="36c50-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="36c50-135">Seçenek 3 - durumu doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="36c50-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="36c50-136">Veritabanı durumunu değiştirme işlemlerinin çoğu için başarılı olup olmadığını denetler kod eklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="36c50-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="36c50-137">EF sağlar - kolaylaştırmak için bir genişletme yöntemi `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="36c50-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="36c50-138">Bu yöntem başlar ve bir işlem kaydeder ve ayrıca bir işlevde kabul `verifySucceeded` hareket kaydetme sırasında geçici bir hata ortaya çıktığında çağrılan parametresi.</span><span class="sxs-lookup"><span data-stu-id="36c50-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="36c50-139">Burada `SaveChanges` çağrılıyor `acceptAllChangesOnSuccess` kümesine `false` durumunu değiştirmekten kaçınmak için `Blog` varlığa `Unchanged` varsa `SaveChanges` başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="36c50-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="36c50-140">Yürütme başarısız olursa ve işlem geri alındı aynı işlemi yeniden denemek için bunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="36c50-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="36c50-141">4. seçenek - el ile işlem izleme</span><span class="sxs-lookup"><span data-stu-id="36c50-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="36c50-142">Depo tarafından üretilen anahtarlar kullanın veya gerçekleştirilen işlemi bağımlı değil genel şekilde yürütme hatalarının işleme gerek gerekiyorsa, her işlem yürütme başarısız olduğunda, denetlenir kimliği atanabilir.</span><span class="sxs-lookup"><span data-stu-id="36c50-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="36c50-143">Bir tablo işlemleri durumunu izlemek için kullanılan veritabanına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="36c50-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="36c50-144">Her işlem başındaki tabloya bir satır ekler.</span><span class="sxs-lookup"><span data-stu-id="36c50-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="36c50-145">Kaydetme sırasında bağlantı başarısız olursa veritabanında karşılık gelen satırda varlığını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="36c50-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="36c50-146">Yürütme başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırda silin.</span><span class="sxs-lookup"><span data-stu-id="36c50-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="36c50-147">Doğrulama için kullanılan bağlam bağlantı büyük olasılıkla işlem yürütme sırasında başarısız olursa yeniden doğrulama sırasında başarısız olarak tanımlanan bir yürütme stratejisi sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="36c50-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
