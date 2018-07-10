---
title: İşlemler - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
caps.latest.revision: 3
ms.openlocfilehash: 4f5408951f14d70a6f49818985f57c6ee88bfb5d
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914268"
---
# <a name="working-with-transactions"></a><span data-ttu-id="3ec34-102">İşlemleri ile çalışma</span><span class="sxs-lookup"><span data-stu-id="3ec34-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="3ec34-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="3ec34-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="3ec34-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="3ec34-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="3ec34-105">Bu belgede EF6 işlemleri ile çalışmayı kolaylaştırmak için bu yana EF5 ekledik geliştirmeler dahil olmak üzere işlemlerde kullanarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="3ec34-106">Varsayılan olarak EF yapar</span><span class="sxs-lookup"><span data-stu-id="3ec34-106">What EF does by default</span></span>  

<span data-ttu-id="3ec34-107">Entity Framework tüm sürümlerinde yürüttüğünüz her **SaveChanges()** eklemek, güncelleştirmek veya framework veritabanını silmek için bu işlem, bir işlem içinde kaydırılır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="3ec34-108">Bu işlem işlemi yürütmek için yalnızca yeteri kadar sürer ve ardından tamamlar.</span><span class="sxs-lookup"><span data-stu-id="3ec34-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="3ec34-109">Başka bir işlem yürüttüğünüzde, yeni bir işlem başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="3ec34-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="3ec34-110">EF6'ile başlayan **Database.ExecuteSqlCommand()** zaten mevcut değilse varsayılan olarak bir işlem içinde komut kaydırılır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="3ec34-111">Bu yöntemin istiyorsanız bu davranışı geçersiz kılma olanak tanıyan aşırı vardır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="3ec34-112">API'ler aracılığıyla modeline gibi dahil saklı yordamların EF6 yürütmesinde ayrıca **ObjectContext.ExecuteFunction()** (varsayılan davranış şu anda değiştirilemiyor, dışında) aynı yapar.</span><span class="sxs-lookup"><span data-stu-id="3ec34-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="3ec34-113">Her iki durumda da, yalıtım düzeyi veritabanı sağlayıcısı varsayılan ayar olarak değerlendirir. işlemin yalıtım düzeyini olduğu.</span><span class="sxs-lookup"><span data-stu-id="3ec34-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="3ec34-114">Varsayılan olarak, örneğin, SQL Server'da READ COMMITTED budur.</span><span class="sxs-lookup"><span data-stu-id="3ec34-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="3ec34-115">Entity Framework, bir işlemde sorguları sarmalamanız değil.</span><span class="sxs-lookup"><span data-stu-id="3ec34-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="3ec34-116">Bu varsayılan işlevi, çok fazla kullanıcılar ve bu nedenle olup olmadığını EF6 içinde farklı bir şey yapmanıza gerek yoktur uygundur; her zaman yaptığınız gibi kod yazmanız yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="3ec34-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="3ec34-117">Ancak bazı kullanıcılar işlemler üzerinde daha fazla denetim gerektiren – bu aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="3ec34-118">API'leri nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="3ec34-118">How the APIs work</span></span>  

<span data-ttu-id="3ec34-119">(Zaten açık bir bağlantı aktarılırsa bir özel durum oluşturuyordu) veritabanı bağlantısı kendisini açarken önce EF6 Entity Framework insisted.</span><span class="sxs-lookup"><span data-stu-id="3ec34-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="3ec34-120">Bir işlem açık bir bağlantı üzerinde yalnızca başlatılabilir olduğundan, bu tek yolu bir kullanıcı birden çok işlem tek bir işleme kaydırma ya da kullanılacak olduğunu geliyordu bir [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) veya  **ObjectContext.Connection** özelliği ve başlangıç çağırma **Open()** ve **BeginTransaction()** doğrudan üzerinde döndürülen **EntityConnection** nesne.</span><span class="sxs-lookup"><span data-stu-id="3ec34-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="3ec34-121">Ayrıca, kendi temel alınan veritabanı bağlantısında bir işlem başlatıldı, API çağrıları, bir veritabanı iletişim başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="3ec34-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="3ec34-122">Yalnızca kapalı bağlantılarını kabul sınırlaması, Entity Framework 6'da kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="3ec34-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="3ec34-123">Ayrıntılar için bkz [bağlantı yönetimi](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="3ec34-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="3ec34-124">Framework EF6 ile başlayarak, artık sağlar:</span><span class="sxs-lookup"><span data-stu-id="3ec34-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="3ec34-125">**Database.BeginTransaction()** : başlatmak ve işlemleri kendileri aynı işlem içinde birleştirilmek üzere çeşitli işlemlere izin verme, var olan bir DbContext içinde– tamamlamak bir kullanıcı için daha kolay bir yöntem ve bu nedenle tüm kaydedilmiş veya tüm biri olarak geri alındı.</span><span class="sxs-lookup"><span data-stu-id="3ec34-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="3ec34-126">Ayrıca, daha kolay işlemin yalıtım düzeyini belirtmek kullanıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3ec34-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="3ec34-127">**Database.UseTransaction()** : Entity Framework dışında başlatıldı, bir işlem kullanmak DbContext sağlar.</span><span class="sxs-lookup"><span data-stu-id="3ec34-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="3ec34-128">Çeşitli işlemler aynı bağlam içinde tek bir işleme birleştirme</span><span class="sxs-lookup"><span data-stu-id="3ec34-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="3ec34-129">**Database.BeginTransaction()** iki geçersiz kılmaları – bir açık bir alan olan [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) ve olan hiçbir bağımsız değişkeni alır ve temel alınan veritabanı sağlayıcısından IsolationLevel varsayılan kullanır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="3ec34-130">Her iki geçersiz kılmaları dönüş bir **DbContextTransaction** sağlayan nesne **Commit()** ve **Rollback()** kaydetme ve geri alma, temel alınan mağaza gerçekleştiren yöntemler işlem.</span><span class="sxs-lookup"><span data-stu-id="3ec34-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="3ec34-131">**DbContextTransaction** kaydedilmiş veya geri alınmış sonra çıkarılması için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="3ec34-132">Bir yolla bunu yapmanın kolay **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="3ec34-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="3ec34-133">otomatik olarak çağırma söz dizimi **Dispose()** kullanarak engelliyken tamamlar:</span><span class="sxs-lookup"><span data-stu-id="3ec34-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="3ec34-134">Bir işlem başlatılmasına izin gerektirir temel alınan depolama bağlantı açıktır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="3ec34-135">Önceden açılmış olursa Database.BeginTransaction() yöntemini çağırır; dolayısıyla bağlantıyı açın.</span><span class="sxs-lookup"><span data-stu-id="3ec34-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="3ec34-136">DbContextTransaction bağlantı açtıysanız Dispose() çağrıldığında ardından bunu, sona erecektir.</span><span class="sxs-lookup"><span data-stu-id="3ec34-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="3ec34-137">Var olan bir işlem bağlamına geçirme</span><span class="sxs-lookup"><span data-stu-id="3ec34-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="3ec34-138">Bazen bir işlem kapsam içinde bile daha geniş olduğu ve işlemleri aynı veritabanında ancak EF dışında tamamen içeren ister.</span><span class="sxs-lookup"><span data-stu-id="3ec34-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="3ec34-139">Bunu gerçekleştirmek için bağlantıyı açın ve kendiniz bir hareket başlatır ve gerekir sonra EF zaten açık veritabanı bağlantısı (a) kullanmayı ve mevcut işlem, bağlantıda b) kullanmaya söyleyin.</span><span class="sxs-lookup"><span data-stu-id="3ec34-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="3ec34-140">Bunu yapmak için tanımlayın ve mevcut i) bir bağlantı parametresi ve ii) contextOwnsConnection Boole alan DbContext oluşturucular birinden devralan bağlam sınıfınızın bir oluşturucu kullanın.</span><span class="sxs-lookup"><span data-stu-id="3ec34-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="3ec34-141">Bu senaryoda çağrıldığında false contextOwnsConnection bayrağı ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="3ec34-142">Entity Framework ile bittiğinde, bağlantı kapatmayın bildirir gibi önemli budur (örn: aşağıdaki 4 satır bakın):</span><span class="sxs-lookup"><span data-stu-id="3ec34-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (e.g. see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="3ec34-143">Ayrıca, işlem kendiniz (varsayılan ayar kaçınmak istiyorsanız IsolationLevel dahil) başlatmalı ve Entity Framework bağlantıda zaten başlatılmış olan bir işlem olduğunu bilmesini sağlar (satır 33 aşağıya bakın).</span><span class="sxs-lookup"><span data-stu-id="3ec34-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="3ec34-144">Ardından doğrudan SqlConnection veya DbContext üzerinde veritabanı işlemleri yürütmek ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="3ec34-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="3ec34-145">Tüm işlemler bir işlem içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3ec34-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="3ec34-146">Sorumluluk işleniyor veya işlemi geri alınıyor ve üzerinde Dispose() çağırma yanı sıra kapatma ve veritabanı bağlantısı kesilirken alır.</span><span class="sxs-lookup"><span data-stu-id="3ec34-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="3ec34-147">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3ec34-147">E.g.:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="3ec34-148">Temizleme işlemi ayarlama</span><span class="sxs-lookup"><span data-stu-id="3ec34-148">Clearing up the transaction</span></span>

<span data-ttu-id="3ec34-149">Geçerli işlem için Entity Framework'ün bilgisini temizlemek için Database.UseTransaction() için null değeri geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ec34-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="3ec34-150">Entity Framework ne yürütme veya geri alma mevcut işlem, bu nedenle dikkatli kullanın ve yalnızca eminseniz bu yapmak istediğiniz olur.</span><span class="sxs-lookup"><span data-stu-id="3ec34-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="3ec34-151">UseTransaction hataları</span><span class="sxs-lookup"><span data-stu-id="3ec34-151">Errors in UseTransaction</span></span>

<span data-ttu-id="3ec34-152">İşlem başarılı olursa Database.UseTransaction() adresinden bir özel durum görürsünüz olduğunda:</span><span class="sxs-lookup"><span data-stu-id="3ec34-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="3ec34-153">Entity Framework var olan bir işlem zaten var.</span><span class="sxs-lookup"><span data-stu-id="3ec34-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="3ec34-154">Entity Framework bir TransactionScope içinde zaten çalışıyor</span><span class="sxs-lookup"><span data-stu-id="3ec34-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="3ec34-155">Geçirilen işlemde bağlantı nesnesi null (yani bir tane olduğundan bu işlem zaten tamamlanmış bir oturum bağlantı – genellikle olan)</span><span class="sxs-lookup"><span data-stu-id="3ec34-155">The connection object in the transaction passed is null (i.e. one which has no connection – usually this is a sign that that transaction has already completed)</span></span>  
- <span data-ttu-id="3ec34-156">Geçirilen işlemde bağlantı nesnesi, Entity Framework'ün bağlantı eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="3ec34-156">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="3ec34-157">Diğer özelliklerle işlemleri kullanma</span><span class="sxs-lookup"><span data-stu-id="3ec34-157">Using transactions with other features</span></span>  

<span data-ttu-id="3ec34-158">Bu bölümde, yukarıdaki işlemleri nasıl etkileşim açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="3ec34-158">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="3ec34-159">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="3ec34-159">Connection resiliency</span></span>  
- <span data-ttu-id="3ec34-160">Zaman uyumsuz yöntemler</span><span class="sxs-lookup"><span data-stu-id="3ec34-160">Asynchronous methods</span></span>  
- <span data-ttu-id="3ec34-161">TransactionScope işlemleri</span><span class="sxs-lookup"><span data-stu-id="3ec34-161">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="3ec34-162">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="3ec34-162">Connection Resiliency</span></span>  

<span data-ttu-id="3ec34-163">Yeni bağlantı dayanıklılığı özelliğini kullanıcı tarafından başlatılan işlemleri ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="3ec34-163">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="3ec34-164">Ayrıntılar için bkz [yeniden deneme yürütme stratejileri ile sınırlamalar](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span><span class="sxs-lookup"><span data-stu-id="3ec34-164">For details, see [Limitations with Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="3ec34-165">Zaman uyumsuz programlama</span><span class="sxs-lookup"><span data-stu-id="3ec34-165">Asynchronous Programming</span></span>  

<span data-ttu-id="3ec34-166">Önceki bölümlerde açıklanan yaklaşımı hiçbir başka seçenekleri veya ayarları çalışmak için gereken [zaman uyumsuz yöntemleri kaydedin ve sorgu](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="3ec34-166">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="3ec34-167">Ancak, zaman uyumsuz yöntemler içinde neler bağlı olarak, bu sırayla, kilitlenmeleri veya genel uygulama performansını için hatalı olduğu engelleme neden olabilir. uzun süre çalışan işlemler – neden olabilir, unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3ec34-167">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="3ec34-168">TransactionScope işlemleri</span><span class="sxs-lookup"><span data-stu-id="3ec34-168">TransactionScope Transactions</span></span>  

<span data-ttu-id="3ec34-169">EF6 önce TransactionScope nesnesini kullanmak için önerilen yöntem, daha büyük kapsam işlemler sağlama şöyleydi:</span><span class="sxs-lookup"><span data-stu-id="3ec34-169">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="3ec34-170">SqlConnection ve Entity Framework hem ortam TransactionScope işlem kullanın ve bu nedenle birlikte yürütülmesi.</span><span class="sxs-lookup"><span data-stu-id="3ec34-170">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="3ec34-171">.NET 4.5.1 TransactionScope kullanımı aracılığıyla zaman uyumsuz yöntemler de çalışmak üzere güncelleştirilmiştir başlayarak [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) sabit listesi:</span><span class="sxs-lookup"><span data-stu-id="3ec34-171">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="3ec34-172">Yine de bazı sınırlamalar TransactionScope yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="3ec34-172">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="3ec34-173">.NET 4.5.1 gerektirir veya zaman uyumsuz yöntemler ile çalışmak daha büyük.</span><span class="sxs-lookup"><span data-stu-id="3ec34-173">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="3ec34-174">Bir ve yalnızca bir bağlantı olduğundan emin değilseniz bulut senaryolarda kullanılamaz (bulut senaryolarına dağıtılmış işlemler desteklemez).</span><span class="sxs-lookup"><span data-stu-id="3ec34-174">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="3ec34-175">Önceki bölümlerde Database.UseTransaction() yaklaşımıyla birleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="3ec34-175">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="3ec34-176">Bir DDL vermek ve MSDTC hizmeti aracılığıyla dağıtılmış işlemler etkinleştirmediniz özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="3ec34-176">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="3ec34-177">TransactionScope yaklaşımın avantajları:</span><span class="sxs-lookup"><span data-stu-id="3ec34-177">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="3ec34-178">Birden fazla belirli bir veritabanı bağlantısı veya aynı işlem içinde farklı bir veritabanı bağlantısı olan bir veritabanına bir bağlantı birleştirmek, otomatik olarak yerel bir işlem için Dağıtılmış bir işlem yükseltir (Not: olması gerekir Dağıtılmış işlemler için bunun çalışmasına izin verecek şekilde yapılandırılmış MSDTC hizmetinin).</span><span class="sxs-lookup"><span data-stu-id="3ec34-178">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="3ec34-179">Kodlama kolaylığı.</span><span class="sxs-lookup"><span data-stu-id="3ec34-179">Ease of coding.</span></span> <span data-ttu-id="3ec34-180">Tercih ettiğiniz ortam ve örtük olarak arka planda ile dağıtılmış işlem yerine, açıkça altında denetim ardından TransactionScope yaklaşımı, daha iyi uygun.</span><span class="sxs-lookup"><span data-stu-id="3ec34-180">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="3ec34-181">Özet olarak, yeni Database.BeginTransaction() ve yukarıdaki Database.UseTransaction() API'leri ile TransactionScope yaklaşım artık çoğu kullanıcı için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="3ec34-181">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="3ec34-182">TransactionScope kullanmaya devam ederseniz ardından yukarıdaki sınırlamaları unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3ec34-182">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="3ec34-183">Mümkün olduğunda bunun yerine önceki bölümlerde açıklanan yaklaşımı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="3ec34-183">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
