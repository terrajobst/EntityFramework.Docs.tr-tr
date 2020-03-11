---
title: İşlem işleme başarısızlıklarını işleme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417372"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="c3b73-102">İşlem işleme başarısızlıklarını işleme</span><span class="sxs-lookup"><span data-stu-id="c3b73-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="c3b73-103">**EF 6.1 yalnızca** sonraki sürümler-bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="c3b73-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="c3b73-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="c3b73-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c3b73-105">6,1 kapsamında, EF için yeni bir bağlantı dayanıklılığı özelliği sunuyoruz: geçici bağlantı arızaları işlem yürütmelerinin bildirimini etkilediği zaman otomatik olarak algılama ve kurtarma özelliği.</span><span class="sxs-lookup"><span data-stu-id="c3b73-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="c3b73-106">Senaryonun tüm ayrıntıları en iyi şekilde [SQL veritabanı bağlantısı ve en etkili sorun sorunu](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)konusunda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c3b73-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="c3b73-107">Özet bölümünde senaryo, işlem sırasında bir özel durum ortaya çıktığında iki olası neden vardır:</span><span class="sxs-lookup"><span data-stu-id="c3b73-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="c3b73-108">Sunucuda işlem işleme başarısız oldu</span><span class="sxs-lookup"><span data-stu-id="c3b73-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="c3b73-109">İşlem, sunucuda başarılı bir şekilde tamamlandı, ancak bir bağlantı sorunu, Başarı bildiriminin istemciye ulaşmasını engelledi</span><span class="sxs-lookup"><span data-stu-id="c3b73-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="c3b73-110">İlk durum uygulama olduğunda veya Kullanıcı işlemi yeniden deneyebilir, ancak ikinci durum oluştuğunda yeniden denemeler önlenmiş olmalıdır ve uygulama otomatik olarak kurtarabilir.</span><span class="sxs-lookup"><span data-stu-id="c3b73-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="c3b73-111">Bu zorluk, kayıt sırasında bir özel durumun ne kadar gerçek olduğunu tespit etme yeteneği olmadan, uygulamanın doğru işlem planını seçemediğinden emin değildir.</span><span class="sxs-lookup"><span data-stu-id="c3b73-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="c3b73-112">EF 6,1 ' deki yeni özellik, işlem başarılı olursa ve doğru şekilde eyleme geçmek için EF 'in veritabanına iki kez işaret etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="c3b73-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="c3b73-113">Özelliğini kullanma</span><span class="sxs-lookup"><span data-stu-id="c3b73-113">Using the feature</span></span>  

<span data-ttu-id="c3b73-114">İhtiyacınız olan özelliği etkinleştirmek için, **DBConfiguration**' ın oluşturucusunda [settransactionhandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) ' a bir çağrı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c3b73-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="c3b73-115">**DBConfiguration**hakkında bilginiz yoksa bkz. [kod tabanlı yapılandırma](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="c3b73-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="c3b73-116">Bu özellik, EF6 ' de tanıtıldığımız otomatik yeniden denemeler ile birlikte kullanılabilir ve bu durum, geçici bir hata nedeniyle işlemin gerçekten sunucuda işleme amaması durumunda yardımcı olur:</span><span class="sxs-lookup"><span data-stu-id="c3b73-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="c3b73-117">İşlemler nasıl izlenir?</span><span class="sxs-lookup"><span data-stu-id="c3b73-117">How transactions are tracked</span></span>  

<span data-ttu-id="c3b73-118">Özellik etkinleştirildiğinde, EF otomatik olarak **__Transactions**adlı veritabanına yeni bir tablo ekler.</span><span class="sxs-lookup"><span data-stu-id="c3b73-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="c3b73-119">Her bir işlem EF tarafından oluşturulduğunda bu tabloya yeni bir satır eklenir ve kayıt sırasında bir işlem hatası oluşursa bu satır varlık için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="c3b73-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="c3b73-120">EF, artık gerek duyulmadığında tablodaki satırları ayıklamak için en iyi çaba yapabilse de, uygulama zamanından önce çıktığında tablo büyüyebilir ve bu nedenle, bazı durumlarda tabloyu el ile temizlemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="c3b73-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="c3b73-121">Önceki sürümlerle işleme başarısızlıklarını işleme</span><span class="sxs-lookup"><span data-stu-id="c3b73-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="c3b73-122">EF 6,1 ' den önce EF ürününde işleme başarısızlıklarını işlemeye yönelik bir mekanizma yoktu.</span><span class="sxs-lookup"><span data-stu-id="c3b73-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="c3b73-123">Önceki EF6 sürümlerine uygulanabilecek bu durumla ilgilenmenin birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="c3b73-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="c3b73-124">Seçenek 1-hiçbir şey yapma</span><span class="sxs-lookup"><span data-stu-id="c3b73-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="c3b73-125">İşlem işleme sırasında bağlantı hatası olasılığı düşük olduğundan, bu durum aslında gerçekleşirse uygulamanızın başarısız olması için kabul edilebilir hale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="c3b73-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="c3b73-126">Seçenek 2-durumu sıfırlamak için veritabanını kullanma</span><span class="sxs-lookup"><span data-stu-id="c3b73-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="c3b73-127">Geçerli DbContext 'i at</span><span class="sxs-lookup"><span data-stu-id="c3b73-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="c3b73-128">Yeni bir DbContext oluşturun ve uygulamanızın durumunu veritabanından geri yükleyin</span><span class="sxs-lookup"><span data-stu-id="c3b73-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="c3b73-129">Kullanıcıya son işlemin başarıyla tamamlanmamış olabileceğini bildirin</span><span class="sxs-lookup"><span data-stu-id="c3b73-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="c3b73-130">Seçenek 3-işlemi el Ile izleme</span><span class="sxs-lookup"><span data-stu-id="c3b73-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="c3b73-131">İşlemlerin durumunu izlemek için kullanılan veritabanına izlenmeyen bir tablo ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c3b73-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="c3b73-132">Her bir işlemin başındaki tabloya bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c3b73-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="c3b73-133">Kayıt sırasında bağlantı başarısız olursa, veritabanında karşılık gelen satırın varolup olmadığını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="c3b73-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="c3b73-134">Satır mevcutsa, işlem başarıyla yürütüldüğü için normal olarak devam edin</span><span class="sxs-lookup"><span data-stu-id="c3b73-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="c3b73-135">Satır yoksa, geçerli işlemi yeniden denemek için bir yürütme stratejisi kullanın.</span><span class="sxs-lookup"><span data-stu-id="c3b73-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="c3b73-136">Kayıt başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.</span><span class="sxs-lookup"><span data-stu-id="c3b73-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="c3b73-137">[Bu blog gönderisi](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) , SQL Azure için bu kodu yerine getirmeye yönelik örnek kod içerir.</span><span class="sxs-lookup"><span data-stu-id="c3b73-137">[This blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
