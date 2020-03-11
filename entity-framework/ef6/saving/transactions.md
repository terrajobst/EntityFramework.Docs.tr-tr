---
title: Işlemlerle çalışma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419687"
---
# <a name="working-with-transactions"></a><span data-ttu-id="efe2a-102">Işlemlerle çalışma</span><span class="sxs-lookup"><span data-stu-id="efe2a-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="efe2a-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="efe2a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="efe2a-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="efe2a-105">Bu belge, EF6 ' deki işlemler kullanılarak, işlemler ile çalışmayı kolaylaştırmak için EF5 bu yana eklediğimiz iyileştirmeler dahil olmak üzere açıklanır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="efe2a-106">Varsayılan olarak ne kadar EF</span><span class="sxs-lookup"><span data-stu-id="efe2a-106">What EF does by default</span></span>  

<span data-ttu-id="efe2a-107">Tüm Entity Framework sürümlerinde, her **SaveChanges ()** işlemini veritabanı üzerinde eklemek, güncelleştirmek veya silmek için çalıştırdığınızda Framework bu işlemi bir işlem içinde saracaktır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="efe2a-108">Bu işlem yalnızca işlemi yürütmek için yeterince uzun sürer ve sonra tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="efe2a-109">Bu gibi başka bir işlem yürüttüğünüzde yeni bir işlem başlatılır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="efe2a-110">EF6 veritabanı ile başlangıç **. ExecuteSqlCommand ()** varsayılan olarak, bir işlem zaten mevcut değilse komutu bir işlem içinde saracaktır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="efe2a-111">İsterseniz bu davranışı geçersiz kılmanıza izin veren bu yöntemin aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="efe2a-112">Ayrıca, EF6 **. ExecuteFunction ()** gibi API 'ler aracılığıyla modelde bulunan saklı yordamların yürütülmesi de aynı olur (varsayılan davranışın Şu anda geçersiz kılınamaması hariç).</span><span class="sxs-lookup"><span data-stu-id="efe2a-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="efe2a-113">Her iki durumda da, işlemin yalıtım düzeyi, veritabanı sağlayıcının varsayılan ayarını düşündüğü her türlü yalıtım düzeyidir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="efe2a-114">Varsayılan olarak, örneğin SQL Server, bu okundu olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="efe2a-115">Entity Framework bir işlemdeki sorguları sarmaz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="efe2a-116">Bu varsayılan işlevsellik çok sayıda kullanıcı için uygundur ve EF6 içinde farklı bir şey yapmanız gerekmez; her zaman yaptığınız gibi kodu yazmanız yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="efe2a-117">Ancak bazı kullanıcılar işlemleri üzerinde daha fazla denetim gerektirir. Bu, aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="efe2a-118">API 'Ler nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="efe2a-118">How the APIs work</span></span>  

<span data-ttu-id="efe2a-119">EF6 ' dan önce, veritabanı bağlantısının kendisini açmaya yönelik olarak Entity Framework, (zaten açık olan bir bağlantıyı geçirmişse bir özel durum oluşturdu).</span><span class="sxs-lookup"><span data-stu-id="efe2a-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="efe2a-120">Bir işlem yalnızca açık bir bağlantı üzerinde başlatılacağından, bu, bir kullanıcının birkaç işlemi tek bir işlemde kaydıracağından, bir [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) kullanmanın veya **ObjectContext. Connection** özelliğini kullanmanın ve doğrudan döndürülen **EntityConnection** nesnesinde **Open ()** ve **BeginTransaction ()** yöntemini çağırmaya başladığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="efe2a-121">Buna ek olarak, kendi üzerinde temel alınan veritabanı bağlantısında bir işlem başladıysanız veritabanıyla iletişim kurulan API çağrıları başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="efe2a-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="efe2a-122">Yalnızca kapalı bağlantıları kabul etme sınırlaması Entity Framework 6 ' da kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="efe2a-123">Ayrıntılar için bkz. [bağlantı yönetimi](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="efe2a-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="efe2a-124">EF6 ile başlayarak Framework artık şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="efe2a-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="efe2a-125">**Database. BeginTransaction ()** : bir kullanıcının var olan bir DbContext içinde işlem başlatması ve tamamlaması için daha kolay bir yöntem; aynı işlem içinde birden çok işlemin birleştirilmesi ve bu nedenle tümü kaydedilmiş ya da tümü bir tane olarak geri alındı.</span><span class="sxs-lookup"><span data-stu-id="efe2a-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="efe2a-126">Ayrıca, kullanıcının işlem için yalıtım düzeyini daha kolay belirlemesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="efe2a-127">**Database. UseTransaction ()** : dbcontext 'in Entity Framework dışında başlatılan bir işlem kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="efe2a-128">Birkaç işlemi aynı bağlam içindeki tek bir işlemde birleştirme</span><span class="sxs-lookup"><span data-stu-id="efe2a-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="efe2a-129">**Database. BeginTransaction ()** iki geçersiz kılmaya sahiptir: bir açık [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) ve bağımsız değişken alan ve temel alınan veritabanı sağlayıcısından varsayılan IsolationLevel 'ı kullanan bir tane.</span><span class="sxs-lookup"><span data-stu-id="efe2a-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="efe2a-130">Her iki geçersiz kılma de, temel alınan depolama işleminde COMMIT ve Rollback gerçekleştiren **COMMIT ()** ve **Rollback ()** yöntemlerini sağlayan bir **dbcontexttransaction** nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="efe2a-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="efe2a-131">**Dbcontexttransaction** , kaydedildikten veya geri alındıktan sonra atılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="efe2a-132">Bunu yapmanın kolay bir yolu **(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="efe2a-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="efe2a-133">using bloğu tamamlandığında **Dispose ()** öğesini otomatik olarak çağıran sözdizimi:</span><span class="sxs-lookup"><span data-stu-id="efe2a-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="efe2a-134">Bir işlemin başlangıcında, temeldeki depo bağlantısının açık olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="efe2a-135">Bu nedenle, zaten açılmadıysa Database. BeginTransaction () çağrılırken bağlantı açılır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="efe2a-136">DbContextTransaction bağlantıyı açtıysa, Dispose () çağrıldığında onu kapatır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="efe2a-137">Varolan bir işlemi bağlama geçirme</span><span class="sxs-lookup"><span data-stu-id="efe2a-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="efe2a-138">Bazen kapsamda daha geniş olan ve aynı veritabanı üzerinde işlemler içeren, ancak EF 'in tamamen dışında bir işlem istersiniz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="efe2a-139">Bunu gerçekleştirmek için bağlantıyı açmanız ve işlemi kendiniz başlatmanız ve sonra da bu bağlantı üzerinde var olan işlemi kullanmak üzere zaten açık olan veritabanı bağlantısını (b) kullanmak için EF 'e söylemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="efe2a-140">Bunu yapmak için, bağlam sınıfınız üzerinde, var olan bir bağlantı parametresi ve II) contextOwnsConnection Boole değeri olan DbContext oluşturucularından birinden devralan bir Oluşturucu tanımlamanız ve kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="efe2a-141">Bu senaryoda çağrıldığında contextOwnsConnection bayrağının false olarak ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="efe2a-142">Bu, ile işiniz bittiğinde bağlantıyı kapatmaması gerektiğini Entity Framework bilgilendirdiğinden önemlidir (örneğin, aşağıdaki satır 4 ' ü inceleyin):</span><span class="sxs-lookup"><span data-stu-id="efe2a-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="efe2a-143">Ayrıca, işlemi kendiniz başlatmanız gerekir (varsayılan ayarı önlemek istiyorsanız IsolationLevel dahil olmak üzere) ve bağlantı üzerinde zaten başlatılmış olan mevcut bir işlem olduğunu bildirmek Entity Framework (bkz. Line 33).</span><span class="sxs-lookup"><span data-stu-id="efe2a-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="efe2a-144">Daha sonra, veritabanı işlemlerini doğrudan SqlConnection üzerinde ya da DbContext üzerinde yürütebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="efe2a-145">Bu gibi tüm işlemler tek bir işlem içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="efe2a-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="efe2a-146">İşlemin uygulanması veya geri alınması ve üzerinde Dispose () çağrısı yapmak ve veritabanı bağlantısını kapatmak ve elden atmak için sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="efe2a-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="efe2a-147">Örnek:</span><span class="sxs-lookup"><span data-stu-id="efe2a-147">For example:</span></span>  

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
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
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
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="efe2a-148">İşlem temizleniyor</span><span class="sxs-lookup"><span data-stu-id="efe2a-148">Clearing up the transaction</span></span>

<span data-ttu-id="efe2a-149">Entity Framework geçerli işlem bilgisini temizlemek için Database. UseTransaction () öğesine null geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="efe2a-150">Entity Framework bunu yaptığınızda, mevcut işlemi ne yapmalı ne de geri almaz, bu nedenle dikkatli olun ve yalnızca bunun yapmak istediğiniz şeydir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="efe2a-151">UseTransaction hataları</span><span class="sxs-lookup"><span data-stu-id="efe2a-151">Errors in UseTransaction</span></span>

<span data-ttu-id="efe2a-152">Şu durumlarda bir işlem geçirirseniz Database. UseTransaction () için bir özel durum görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="efe2a-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="efe2a-153">Entity Framework zaten mevcut bir işlem var</span><span class="sxs-lookup"><span data-stu-id="efe2a-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="efe2a-154">Entity Framework zaten bir TransactionScope içinde çalışıyor</span><span class="sxs-lookup"><span data-stu-id="efe2a-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="efe2a-155">Geçirilen işlemdeki bağlantı nesnesi null.</span><span class="sxs-lookup"><span data-stu-id="efe2a-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="efe2a-156">Diğer bir deyişle, işlem bir bağlantıyla ilişkili değildir, genellikle bu işlemin zaten tamamlandığını belirten bir imzadır</span><span class="sxs-lookup"><span data-stu-id="efe2a-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="efe2a-157">Geçirilen işlemdeki bağlantı nesnesi Entity Framework bağlantısıyla eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="efe2a-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="efe2a-158">Diğer özelliklerle işlemleri kullanma</span><span class="sxs-lookup"><span data-stu-id="efe2a-158">Using transactions with other features</span></span>  

<span data-ttu-id="efe2a-159">Bu bölüm, yukarıdaki işlemlerin nasıl etkileşime gireceğini ayrıntılarıyla açıklamaktadır:</span><span class="sxs-lookup"><span data-stu-id="efe2a-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="efe2a-160">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="efe2a-160">Connection resiliency</span></span>  
- <span data-ttu-id="efe2a-161">Zaman uyumsuz yöntemler</span><span class="sxs-lookup"><span data-stu-id="efe2a-161">Asynchronous methods</span></span>  
- <span data-ttu-id="efe2a-162">TransactionScope işlemleri</span><span class="sxs-lookup"><span data-stu-id="efe2a-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="efe2a-163">Bağlantı Dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="efe2a-163">Connection Resiliency</span></span>  

<span data-ttu-id="efe2a-164">Yeni bağlantı dayanıklılığı özelliği, Kullanıcı tarafından başlatılan işlemlerle çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="efe2a-165">Ayrıntılar için bkz. [yürütme stratejilerini yeniden deneme](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="efe2a-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="efe2a-166">Zaman Uyumsuz Programlama</span><span class="sxs-lookup"><span data-stu-id="efe2a-166">Asynchronous Programming</span></span>  

<span data-ttu-id="efe2a-167">Önceki bölümlerde özetlenen yaklaşımın, [zaman uyumsuz sorgu ve kaydetme yöntemleriyle](~/ef6/fundamentals/async.md
)birlikte çalışması için başka seçenekler veya ayarlar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="efe2a-168">Ancak, zaman uyumsuz yöntemlerde yaptığınız işlemlere bağlı olarak, bu durum uzun süre çalışan işlemler oluşmasına neden olabilir. bu durum, genel uygulamanın performansı için bozuk olan kilitlenmeleri veya engellemeye yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="efe2a-169">TransactionScope Işlemleri</span><span class="sxs-lookup"><span data-stu-id="efe2a-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="efe2a-170">EF6 öncesinde, daha büyük kapsam işlemleri sağlamanın önerilen yolu bir TransactionScope nesnesi kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="efe2a-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="efe2a-171">SqlConnection ve Entity Framework her ikisi de çevresel TransactionScope işlemini kullanır ve bu nedenle birlikte işlenir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="efe2a-172">.NET 4.5.1 TransactionScope ile çalışmaya başlamak, [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) numaralandırması kullanılarak zaman uyumsuz yöntemlerle de çalışacak şekilde güncelleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="efe2a-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="efe2a-173">TransactionScope yaklaşımına yönelik bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="efe2a-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="efe2a-174">Zaman uyumsuz yöntemlerle çalışmak için .NET 4.5.1 veya üstünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="efe2a-175">Tek bir bağlantınızın olduğundan ve yalnızca bir bağlantınız olduğundan (bulut senaryoları dağıtılmış işlemleri desteklemediğinden), bulut senaryolarında kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="efe2a-176">Önceki bölümlerin Database. UseTransaction () yaklaşımıyla birleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="efe2a-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="efe2a-177">Herhangi bir DDL 'ye sahipseniz ve dağıtılmış işlemleri MSDTC hizmeti aracılığıyla etkinleştirmediyseniz, bu durum özel durumlar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="efe2a-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="efe2a-178">TransactionScope yaklaşımının avantajları:</span><span class="sxs-lookup"><span data-stu-id="efe2a-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="efe2a-179">Belirli bir veritabanına birden fazla bağlantı yaparsanız veya aynı işlem içindeki farklı bir veritabanıyla bağlantı ile bir bağlantıyı birleştiren bir yerel işlemi otomatik olarak dağıtılmış bir işlem olarak yükseltir (unutmayın: Bu işlemin çalışması için dağıtılmış işlemlere izin vermek üzere yapılandırılan MSDTC hizmeti.</span><span class="sxs-lookup"><span data-stu-id="efe2a-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="efe2a-180">Kodlama kolaylığı.</span><span class="sxs-lookup"><span data-stu-id="efe2a-180">Ease of coding.</span></span> <span data-ttu-id="efe2a-181">İşlemin doğrudan kontrol altına almak yerine, bir hareketin çevresel hale ve arka planda bir şekilde karşılanmayı tercih ediyorsanız, TransactionScope yaklaşımı daha iyi bir şekilde gelebilir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="efe2a-182">Özet olarak, yukarıdaki yeni Database. BeginTransaction () ve Database. UseTransaction () API 'Lerinde, TransactionScope yaklaşımı çoğu kullanıcı için artık gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="efe2a-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="efe2a-183">TransactionScope kullanmaya devam ederseniz yukarıdaki kısıtlamaların farkında olun.</span><span class="sxs-lookup"><span data-stu-id="efe2a-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="efe2a-184">Mümkün olduğunda, önceki bölümlerde özetlenen yaklaşımı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="efe2a-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
