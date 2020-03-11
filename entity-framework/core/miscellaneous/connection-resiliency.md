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
# <a name="connection-resiliency"></a><span data-ttu-id="92b39-102">Bağlantı Dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="92b39-102">Connection Resiliency</span></span>

<span data-ttu-id="92b39-103">Bağlantı dayanıklılığı başarısız veritabanı komutlarını otomatik olarak yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="92b39-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="92b39-104">Özelliği, hataların algılanması ve yeniden denenmesinin gerekli mantığını kapsülleyen bir "yürütme stratejisi" sağlayarak herhangi bir veritabanı ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="92b39-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="92b39-105">EF Core sağlayıcılar, belirli veritabanı hata koşulları ve en iyi yeniden deneme ilkelerine uyarlanmış yürütme stratejileri sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="92b39-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="92b39-106">Örnek olarak, SQL Server sağlayıcısı, SQL Server (SQL Azure dahil) özel olarak uyarlanmış bir yürütme stratejisi içerir.</span><span class="sxs-lookup"><span data-stu-id="92b39-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="92b39-107">Yeniden denenebilecek ve en fazla yeniden deneme sayısı, yeniden denemeler arasındaki gecikme (vb.) için izin verilen özel durum türlerinden haberdar değildir.</span><span class="sxs-lookup"><span data-stu-id="92b39-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="92b39-108">İçeriğiniz için seçenekler yapılandırılırken bir yürütme stratejisi belirtildi.</span><span class="sxs-lookup"><span data-stu-id="92b39-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="92b39-109">Bu, genellikle türetilmiş bağlamınızın `OnConfiguring` yöntemidir:</span><span class="sxs-lookup"><span data-stu-id="92b39-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="92b39-110">ASP.NET Core bir uygulama için `Startup.cs`:</span><span class="sxs-lookup"><span data-stu-id="92b39-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="92b39-111">Özel yürütme stratejisi</span><span class="sxs-lookup"><span data-stu-id="92b39-111">Custom execution strategy</span></span>

<span data-ttu-id="92b39-112">Varsayılandan herhangi birini değiştirmek istiyorsanız, kendi özel bir yürütme stratejisini kaydetme mekanizması vardır.</span><span class="sxs-lookup"><span data-stu-id="92b39-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="92b39-113">Yürütme stratejileri ve işlemler</span><span class="sxs-lookup"><span data-stu-id="92b39-113">Execution strategies and transactions</span></span>

<span data-ttu-id="92b39-114">Hatalarda otomatik olarak yeniden deneme yapan bir yürütme stratejisi, başarısız olan bir yeniden deneme bloğunda her işlemi kayıttan yürütmeyi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="92b39-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="92b39-115">Yeniden denemeler etkinleştirildiğinde, EF Core aracılığıyla gerçekleştirdiğiniz her işlem kendi yeniden kullanılabilir işlem haline gelir.</span><span class="sxs-lookup"><span data-stu-id="92b39-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="92b39-116">Diğer bir deyişle, her sorgu ve her bir `SaveChanges()` çağrısı, geçici bir hata oluşursa birim olarak yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="92b39-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="92b39-117">Ancak, kodunuz `BeginTransaction()` kullanarak bir işlem başlatırsa, bir birim olarak değerlendirilmesi gereken kendi işlem grubunuzu tanımlamanız gerekir ve işlem içindeki her şeyin geri yürütülmesi bir hata oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="92b39-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="92b39-118">Bir yürütme stratejisi kullanırken bunu yapmayı denerseniz, aşağıdaki gibi bir özel durum alırsınız:</span><span class="sxs-lookup"><span data-stu-id="92b39-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="92b39-119">InvalidOperationException: yapılandırılan ' Sqlserverretryingexecutionstrateji ' yürütme stratejisi Kullanıcı tarafından başlatılan işlemleri desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="92b39-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="92b39-120">İşlemdeki tüm işlemleri yeniden kullanılabilir bir birim olarak yürütmek için ' DbContext. Database. Createexecutionstrateji () ' tarafından döndürülen yürütme stratejisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="92b39-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="92b39-121">Çözüm, yürütülmesi gereken her şeyi temsil eden bir temsilciyle yürütme stratejisini el ile çağırmalıdır.</span><span class="sxs-lookup"><span data-stu-id="92b39-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="92b39-122">Geçici bir hata oluşursa yürütme stratejisi temsilciyi yeniden çağırır.</span><span class="sxs-lookup"><span data-stu-id="92b39-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="92b39-123">Bu yaklaşım, Ambient işlemler ile de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="92b39-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="92b39-124">İşlem işleme hatası ve teklik sorunu</span><span class="sxs-lookup"><span data-stu-id="92b39-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="92b39-125">Genel olarak, bir bağlantı hatası olduğunda geçerli işlem geri alınır.</span><span class="sxs-lookup"><span data-stu-id="92b39-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="92b39-126">Ancak, işlem yürütülürken bağlantı kesildiğinde işlemin sonuç durumu bilinmez.</span><span class="sxs-lookup"><span data-stu-id="92b39-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> 

<span data-ttu-id="92b39-127">Varsayılan olarak, yürütme stratejisi işlem geri alınmış gibi işlemi yeniden dener, ancak böyle bir durum yoksa, yeni veritabanı durumu uyumsuzsa ya da işlem belirli bir duruma bağlı değilse **veri bozulmasına** yol açacağından, otomatik olarak oluşturulan anahtar değerleriyle yeni bir satır eklenirken bir özel durumla sonuçlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="92b39-127">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="92b39-128">Bu konuyla başa çıkmak için birkaç yol vardır.</span><span class="sxs-lookup"><span data-stu-id="92b39-128">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="92b39-129">Seçenek 1-do (neredeyse) Nothing</span><span class="sxs-lookup"><span data-stu-id="92b39-129">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="92b39-130">İşlem işleme sırasında bağlantı hatası olasılığı düşük olduğundan, bu durum aslında gerçekleşirse uygulamanızın başarısız olması için kabul edilebilir hale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="92b39-130">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="92b39-131">Ancak, yinelenen bir satır eklemek yerine bir özel durumun yapıldığından emin olmak için mağaza tarafından oluşturulan anahtarları kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="92b39-131">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="92b39-132">İstemci tarafından oluşturulan bir GUID değeri veya bir istemci tarafı değer Oluşturucu kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="92b39-132">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="92b39-133">Seçenek 2-uygulama durumunu yeniden derle</span><span class="sxs-lookup"><span data-stu-id="92b39-133">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="92b39-134">Geçerli `DbContext`atın.</span><span class="sxs-lookup"><span data-stu-id="92b39-134">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="92b39-135">Yeni bir `DbContext` oluşturun ve uygulamanızın durumunu veritabanından geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="92b39-135">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="92b39-136">Kullanıcıya son işlemin başarıyla tamamlanmamış olabileceğini bildirin.</span><span class="sxs-lookup"><span data-stu-id="92b39-136">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="92b39-137">Seçenek 3-durum doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="92b39-137">Option 3 - Add state verification</span></span>

<span data-ttu-id="92b39-138">Veritabanı durumunu değiştiren işlemlerin çoğu için başarılı olup olmadığını denetleyen kodu eklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="92b39-138">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="92b39-139">EF bu, daha kolay `IExecutionStrategy.ExecuteInTransaction`yapmak için bir genişletme yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="92b39-139">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="92b39-140">Bu yöntem başlar ve bir işlemi tamamlar ve ayrıca işlem işlemesi sırasında geçici bir hata oluştuğunda çağrılan `verifySucceeded` parametresinde bir işlevi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="92b39-140">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="92b39-141">Burada `SaveChanges`, `Unchanged` başarılı olursa `Blog` varlığının durumunu `SaveChanges` olarak değiştirmeyi önlemek için `acceptAllChangesOnSuccess` `false` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="92b39-141">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="92b39-142">Bu, işleme başarısız olursa ve işlem geri alınırsa aynı işlemi yeniden denemeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="92b39-142">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="92b39-143">Seçenek 4-işlemi el Ile izleme</span><span class="sxs-lookup"><span data-stu-id="92b39-143">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="92b39-144">Mağaza tarafından oluşturulan anahtarları kullanmanız gerekiyorsa veya işleme bağlı olmayan işlem başarısızlıklarını işlemek için genel bir yönteme ihtiyaç duyuyorsanız, işleme başarısız olduğunda denetlenen bir KIMLIK atanabilir.</span><span class="sxs-lookup"><span data-stu-id="92b39-144">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="92b39-145">İşlemin durumunu izlemek için kullanılan veritabanına tablo ekleyin.</span><span class="sxs-lookup"><span data-stu-id="92b39-145">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="92b39-146">Her bir işlemin başındaki tabloya bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="92b39-146">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="92b39-147">Kayıt sırasında bağlantı başarısız olursa, veritabanında karşılık gelen satırın varolup olmadığını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="92b39-147">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="92b39-148">Kayıt başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.</span><span class="sxs-lookup"><span data-stu-id="92b39-148">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="92b39-149">Doğrulama için kullanılan bağlamın bağlantı, işlem kaydı sırasında başarısız olduysa doğrulama sırasında yeniden başarısız olması nedeniyle tanımlanmış bir yürütme stratejisi olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="92b39-149">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
