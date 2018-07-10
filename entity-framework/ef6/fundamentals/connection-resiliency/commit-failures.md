---
title: İşlem işleme hataları - EF6 işleme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
caps.latest.revision: 3
ms.openlocfilehash: 95ac69a005a70f31086bd4d3c0088bd3e82d28e2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914090"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="e7524-102">İşlem işleme hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="e7524-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="e7524-103">**EF6.1 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 6.1 kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e7524-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="e7524-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="e7524-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="e7524-105">6.1 bir parçası olarak yeni bir bağlantı dayanıklılığı özelliği için EF sunuyoruz: algılamak ve geçici bağlantı hataları hareket işleme bildirim zaman etkileyen otomatik olarak kurtarmak yeteneği.</span><span class="sxs-lookup"><span data-stu-id="e7524-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="e7524-106">Senaryo tam ayrıntılarını en iyi blog gönderisinde açıklanan [SQL veritabanı bağlantısı ve Teklik sorunu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7524-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="e7524-107">Özet olarak, bir işlem kaydı sırasında bir özel durum harekete geçirildiğinde, iki olası nedeni vardır senaryodur:</span><span class="sxs-lookup"><span data-stu-id="e7524-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="e7524-108">İşlem yürütme sunucuda başarısız oldu</span><span class="sxs-lookup"><span data-stu-id="e7524-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="e7524-109">İşlem işleme sunucu üzerinde başarılı ancak bir bağlantı sorunu başarı bildirimi, istemciye ulaşmasını önleyen</span><span class="sxs-lookup"><span data-stu-id="e7524-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="e7524-110">İlk durum uygulama veya kullanıcıya durumda, işlemi yeniden deneyebilirsiniz, ancak ikinci durum oluştuğunda deneme kaçınılmalıdır ve uygulama otomatik olarak geri yüklenemiyor.</span><span class="sxs-lookup"><span data-stu-id="e7524-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="e7524-111">Sınama uygulama eyleminin doğru kurs seçemezsiniz bir özel durum işleme sırasında bildirildi gerçek nedeni neydi algılama özelliğini olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="e7524-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="e7524-112">İşlemin başarılı olduğunu ve doğru kurs eyleminin şeffaf bir şekilde ele, veritabanı ile bir kez daha denetleyin EF EF 6.1 yeni özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="e7524-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="e7524-113">Özelliğini kullanma</span><span class="sxs-lookup"><span data-stu-id="e7524-113">Using the feature</span></span>  

<span data-ttu-id="e7524-114">Özelliği etkinleştirmek için bir çağrı ekleyin [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) oluşturucusunun içinde **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="e7524-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="e7524-115">Alışık olduğunuz **DbConfiguration**, bkz: [kod tabanlı yapılandırma](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="e7524-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="e7524-116">Bu özellik, işlem gerçekten sunucuda geçici bir hata nedeniyle başarısız oldu durumda yardımcı otomatik deneme EF6 içinde kullanıma sunduk ile birlikte kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="e7524-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="e7524-117">İşlemler nasıl izlenir</span><span class="sxs-lookup"><span data-stu-id="e7524-117">How transactions are tracked</span></span>  

<span data-ttu-id="e7524-118">Bu özellik etkinleştirildiğinde, EF otomatik olarak yeni bir tablo adlı veritabanına ekler **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="e7524-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="e7524-119">Yeni bir satır, bir işlem tarafından EF oluşturulur ve işleme sırasında bir işlem hatası oluşursa, satır varlığını denetlenir her zaman bu tabloda eklenir.</span><span class="sxs-lookup"><span data-stu-id="e7524-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="e7524-120">EF artık ihtiyaç duyulmayan zaman çizelgesinden satırlar ayıklamak üzere en iyi çaba İlkesi ne yapacağını olsa da, uygulamanın erken ve tablo bazı durumlarda elle temizleme gerekebilir. Bu nedenle çıkılıyorsa tablo büyüyebilir.</span><span class="sxs-lookup"><span data-stu-id="e7524-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="e7524-121">Önceki sürümlerle işleme hatalarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="e7524-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="e7524-122">Önce EF 6.1 yoktu EF ürün hatalarını işleme mekanizması.</span><span class="sxs-lookup"><span data-stu-id="e7524-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="e7524-123">EF6 önceki sürümleri için uygulanabilir bu durumla ilgilenmek için çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="e7524-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

### <a name="option-1---do-nothing"></a><span data-ttu-id="e7524-124">1. seçenek - hiçbir şey yapma</span><span class="sxs-lookup"><span data-stu-id="e7524-124">Option 1 - Do nothing</span></span>  

<span data-ttu-id="e7524-125">İşlem işleme sırasında bağlantı hatası olasılığını düşük olduğundan aslında bu durum ortaya çıkarsa, yalnızca başarısız olmasına, uygulamanız için kabul edilebilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="e7524-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

## <a name="option-2---use-the-database-to-reset-state"></a><span data-ttu-id="e7524-126">2. seçenek - durumunu sıfırlamak için veritabanını kullan</span><span class="sxs-lookup"><span data-stu-id="e7524-126">Option 2 - Use the database to reset state</span></span>  

1. <span data-ttu-id="e7524-127">Geçerli DbContext AT</span><span class="sxs-lookup"><span data-stu-id="e7524-127">Discard the current DbContext</span></span>  
2. <span data-ttu-id="e7524-128">Yeni bir DbContext oluşturma ve uygulama durumunu veritabanından geri yükleme</span><span class="sxs-lookup"><span data-stu-id="e7524-128">Create a new DbContext and restore the state of your application from the database</span></span>  
3. <span data-ttu-id="e7524-129">Son işlemi başarıyla tamamlanmamış, kullanıcıyı bilgilendirmeniz</span><span class="sxs-lookup"><span data-stu-id="e7524-129">Inform the user that the last operation might not have been completed successfully</span></span>  

## <a name="option-3---manually-track-the-transaction"></a><span data-ttu-id="e7524-130">3. seçenek - el ile işlem izleme</span><span class="sxs-lookup"><span data-stu-id="e7524-130">Option 3 - Manually track the transaction</span></span>  

1. <span data-ttu-id="e7524-131">İzlenen olmayan tablo işlemlerin durumunu izlemek için kullanılan veritabanı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e7524-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
2. <span data-ttu-id="e7524-132">Her işlem başındaki tabloya bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e7524-132">Insert a row into the table at the beginning of each transaction.</span></span>  
3. <span data-ttu-id="e7524-133">Yürütme sırasında bağlantı başarısız olursa, veritabanı karşılık gelen satırı varlığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="e7524-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
    - <span data-ttu-id="e7524-134">Satır varsa, işlemin başarıyla yürütüldü gibi normal olarak devam eder</span><span class="sxs-lookup"><span data-stu-id="e7524-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
    - <span data-ttu-id="e7524-135">Satır yoksa, geçerli işlemi yeniden denemek için bir yürütme stratejisi kullanın.</span><span class="sxs-lookup"><span data-stu-id="e7524-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
4. <span data-ttu-id="e7524-136">Kaydetme başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.</span><span class="sxs-lookup"><span data-stu-id="e7524-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="e7524-137">[Bu blog gönderisini](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) SQL Azure üzerinde bu işlemi gerçekleştirmek için örnek kod içerir.</span><span class="sxs-lookup"><span data-stu-id="e7524-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
