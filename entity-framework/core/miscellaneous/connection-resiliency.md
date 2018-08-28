---
title: Bağlantı dayanıklılığı - EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: d6e31cf2b9b783ea503703536d159b34bf2e18c0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997196"
---
# <a name="connection-resiliency"></a><span data-ttu-id="e4c3b-102">Bağlantı Dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="e4c3b-102">Connection Resiliency</span></span>

<span data-ttu-id="e4c3b-103">Bağlantı dayanıklılığı, başarısız olan veritabanı komutları otomatik olarak yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="e4c3b-104">Bu özellik ile herhangi bir veritabanı "hatalarını algılamak ve komutlar yeniden denemek gerekli mantığı kapsülleyen bir yürütme stratejisi", sağlanarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="e4c3b-105">EF Core sağlayıcıları, belirli veritabanı hata koşulları ve en iyi bir yeniden deneme ilkeleri uyarlanmış yürütme stratejileri sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="e4c3b-106">Örneğin, SQL Server (SQL Azure dahil) için özel olarak uyarlanmış bir yürütme stratejisi SQL Server sağlayıcısı içerir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="e4c3b-107">Denenebilecek özel durum türlerini farkında olduğundan ve maksimum yeniden deneme sayısı, vb. yeniden denemeler arasındaki gecikme için mantıklı varsayılanlar sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="e4c3b-108">Bir yürütme stratejisi, bağlamınızın seçeneklerini yapılandırırken belirtilir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="e4c3b-109">Bu genellikle kullanımda `OnConfiguring` yöntemi veya türetilmiş bağlamınızın `Startup.cs` bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="e4c3b-110">Özel yürütme stratejisi</span><span class="sxs-lookup"><span data-stu-id="e4c3b-110">Custom execution strategy</span></span>

<span data-ttu-id="e4c3b-111">Bir özel yürütme stratejisi, varsayılan değerleri değiştirmek isterseniz kendi kaydetmek için bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="e4c3b-112">Yürütme stratejileri ve işlemler</span><span class="sxs-lookup"><span data-stu-id="e4c3b-112">Execution strategies and transactions</span></span>

<span data-ttu-id="e4c3b-113">Otomatik olarak hataları yeniden deneme bir yürütme stratejisi, her işlem başarısız olursa yeniden deneme bloğunda oynatmak mümkün olması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="e4c3b-114">Yeniden deneme etkinleştirildiğinde, EF Core ile gerçekleştirdiğiniz her işlem yeniden denenebilir kendi işlem olur.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="e4c3b-115">Diğer bir deyişle, her sorgu ve her çağrı `SaveChanges()` geçici bir hata oluşursa bir birim olarak yeniden denenecek.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-115">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="e4c3b-116">Ancak, kodunuzu kullanarak bir işlem başlatırsa `BeginTransaction()` tanımladığınız bir birim olarak kabul edilmesi için gereken işlemleri kendi grubu ve işlem içinde her şeyi bir arıza tekrarlanmasını gerekir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-116">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="e4c3b-117">Bu yürütme stratejisi kullanılırken yapmayı denerseniz, aşağıdaki gibi bir özel durum alırsınız:</span><span class="sxs-lookup"><span data-stu-id="e4c3b-117">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="e4c3b-118">InvalidOperationException: 'SqlServerRetryingExecutionStrategy' yapılandırılmış yürütme stratejisi, kullanıcı tarafından başlatılan işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-118">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="e4c3b-119">İşlem yeniden denenebilir bir birim olarak tüm işlemleri yürütmek için 'DbContext.Database.CreateExecutionStrategy()' tarafından döndürülen yürütme stratejisi kullanın.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-119">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="e4c3b-120">El ile yürütülmesi gereken her şeyi temsil eden bir temsilci ile yürütme stratejisi çağırmak için kullanılan çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-120">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="e4c3b-121">Geçici bir hata oluşursa yürütme stratejisi temsilciyi yeniden çağırır.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-121">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="e4c3b-122">İşlem yürütme hatası ve Teklik sorunu</span><span class="sxs-lookup"><span data-stu-id="e4c3b-122">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="e4c3b-123">Genel olarak, bir bağlantı hatası olduğunda geçerli işlem geri alınır.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-123">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="e4c3b-124">İşlem devam ederken bağlantı kesilirse ancak durdurulmasını ortaya çıkan kaydedilen işlem durumu bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-124">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="e4c3b-125">Bkz. Bu [blog gönderisi](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-125">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="e4c3b-126">Varsayılan olarak, yürütme stratejisi işlemin işlem geri alındı, ancak böyle değilse yeni bir veritabanı durumu uyumsuz ya da yol açabilir bunu bir özel durum neden olur, olarak yeniden **veri bozulması** varsa işlem otomatik olarak oluşturulan anahtar değerleri içeren yeni bir satır eklendiğinde örneğin belirli bir durumdaki bağımlı kalmayacak.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-126">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="e4c3b-127">Bu ayarı kullanarak çıkılacağını birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-127">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="e4c3b-128">Seçenek 1 - (yaklaşık) hiçbir şey yok</span><span class="sxs-lookup"><span data-stu-id="e4c3b-128">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="e4c3b-129">İşlem işleme sırasında bağlantı hatası olasılığını düşük olduğundan aslında bu durum ortaya çıkarsa, yalnızca başarısız olmasına, uygulamanız için kabul edilebilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-129">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="e4c3b-130">Ancak, yinelenen bir satır eklemek yerine bir özel durum emin olmak için depoda üretilmiş anahtarlar kullanmaktan kaçınmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-130">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="e4c3b-131">Bir istemci tarafından oluşturulan GUID değeri veya bir istemci-tarafı değeri oluşturucuyu kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-131">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="e4c3b-132">Seçenek 2 - yeniden uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="e4c3b-132">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="e4c3b-133">Geçerli at `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-133">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="e4c3b-134">Yeni bir `DbContext` ve uygulamanızın durumunu veritabanından geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-134">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="e4c3b-135">Kullanıcının son işlemi başarıyla tamamlanmamış olduğunu bildirir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-135">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="e4c3b-136">Seçenek 3 - durumu doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="e4c3b-136">Option 3 - Add state verification</span></span>

<span data-ttu-id="e4c3b-137">Veritabanı durumunu değiştiren işlemlerinin çoğu için başarılı olup olmadığını denetleyen bir kod eklemeniz mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-137">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="e4c3b-138">EF sağlar - kolaylaştırmak için bir genişletme yöntemi `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-138">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="e4c3b-139">Bu yöntem başlar ve bir işlem yürütmeleri ve ayrıca bir işlevde kabul `verifySucceeded` hareket işleme sırasında geçici bir hata ortaya çıktığında çağrılan bir parametre.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-139">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="e4c3b-140">Burada `SaveChanges` çağrıldı `acceptAllChangesOnSuccess` kümesine `false` durumunu değiştirmekten kaçınmak için `Blog` varlığa `Unchanged` varsa `SaveChanges` başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-140">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="e4c3b-141">Bu işleme başarısız olursa ve işlem geri alındı aynı işlemi yeniden denemek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-141">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="e4c3b-142">4. seçenek - el ile işlem izleme</span><span class="sxs-lookup"><span data-stu-id="e4c3b-142">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="e4c3b-143">Depoda üretilmiş anahtarlar kullanın veya yapılan işleme bağlı olmayan işleme hataları işleme bir genel çalışmanız gerekiyorsa, her işlem işleme başarısız olduğunda işaretli bir kimliği atanabilir.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-143">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="e4c3b-144">Tablo işlemleri durumunu izlemek için kullanılan veritabanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-144">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="e4c3b-145">Her işlem başındaki tabloya bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-145">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="e4c3b-146">Yürütme sırasında bağlantı başarısız olursa, veritabanı karşılık gelen satırı varlığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-146">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="e4c3b-147">Kaydetme başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-147">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="e4c3b-148">Doğrulama için kullanılan bağlamı bağlantı yeniden doğrulama sırasında işlem yürütme sırasında başarısız olduysa başarısız olma olasılığı yüksek olarak tanımlanan bir yürütme stratejisi olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="e4c3b-148">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
