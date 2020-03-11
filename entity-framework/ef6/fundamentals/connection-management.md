---
title: Bağlantı yönetimi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417989"
---
# <a name="connection-management"></a><span data-ttu-id="a18cd-102">Bağlantı yönetimi</span><span class="sxs-lookup"><span data-stu-id="a18cd-102">Connection management</span></span>
<span data-ttu-id="a18cd-103">Bu sayfada, bağlama bağlantılarının ve **Database. Connection. Open ()** API 'sinin işlevselliğiyle ilgili olarak Entity Framework davranışı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a18cd-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="a18cd-104">Bağlama bağlantıları geçirme</span><span class="sxs-lookup"><span data-stu-id="a18cd-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="a18cd-105">EF5 ve önceki sürümler için davranış</span><span class="sxs-lookup"><span data-stu-id="a18cd-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="a18cd-106">Bağlantıları kabul eden iki Oluşturucu vardır:</span><span class="sxs-lookup"><span data-stu-id="a18cd-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="a18cd-107">Bunları kullanmak mümkündür ancak birkaç sınırlamaya geçici çözüm kullanmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="a18cd-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="a18cd-108">Bunlardan herhangi birine açık bir bağlantı geçirirseniz, çerçeve bunu ilk kez kullanmaya çalıştığında, zaten açık bir bağlantıyı yeniden açkullanılamadığını belirten bir InvalidOperationException oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a18cd-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="a18cd-109">ContextOwnsConnection bayrağı, bağlam atıldığı zaman temeldeki depo bağlantısının atılıp atılmayacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a18cd-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="a18cd-110">Ancak, bu ayardan bağımsız olarak, bağlam atıldığı zaman mağaza bağlantısı her zaman kapalıdır.</span><span class="sxs-lookup"><span data-stu-id="a18cd-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="a18cd-111">Bu nedenle, aynı bağlantıya sahip olan birden fazla DbContext varsa, ilk olarak bağlantıyı kapatır (benzer şekilde, bir DbContext ile var olan bir ADO.NET bağlantısı karmalırsa DbContext, her zaman bırakıldığında bağlantıyı kapatır) .</span><span class="sxs-lookup"><span data-stu-id="a18cd-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="a18cd-112">Kapalı bir bağlantıyı geçirerek ve yalnızca tüm bağlamlar oluşturulduktan sonra açılacak kodu yürüterek yukarıdaki ilk kısıtlamayı geçici olarak çözmek mümkündür:</span><span class="sxs-lookup"><span data-stu-id="a18cd-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

<span data-ttu-id="a18cd-113">İkinci sınırlama, bağlantının kapatılmasını sağlamak için hazırlanana kadar DbContext Nesnelerinizden herhangi birini elden atma konusunda sizi onaylama gereği duymaktan kaçınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a18cd-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="a18cd-114">EF6 ve gelecekteki sürümlerde davranış</span><span class="sxs-lookup"><span data-stu-id="a18cd-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="a18cd-115">EF6 ve gelecekteki sürümlerde, DbContext aynı iki oluşturucuya sahiptir ancak artık, oluşturucuya geçirilen bağlantının alındığı sırada kapatılmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="a18cd-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="a18cd-116">Bu nedenle şu anda mümkündür:</span><span class="sxs-lookup"><span data-stu-id="a18cd-116">So this is now possible:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

<span data-ttu-id="a18cd-117">Ayrıca, contextOwnsConnection bayrağı artık, DbContext atıldığı zaman bağlantının kapatılıp kapatılmayacağını denetler.</span><span class="sxs-lookup"><span data-stu-id="a18cd-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="a18cd-118">Yukarıdaki örnekte, bağlam bırakıldığında bağlantı kapanmaz (satır 32), EF 'in önceki sürümlerinde olduğu gibi, ancak bağlantının kendisi atıldığı zaman (satır 40).</span><span class="sxs-lookup"><span data-stu-id="a18cd-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="a18cd-119">Bu işlem, DbContext 'in bağlantı denetimini ele geçirmesine (yalnızca contextOwnsConnection ' i true olarak ayarlamanız veya diğer oluşturuculardan birini kullanması) mümkün olmaya devam etmektedir.</span><span class="sxs-lookup"><span data-stu-id="a18cd-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="a18cd-120">Bu yeni modelle işlemler kullanılırken bazı ek hususlar vardır.</span><span class="sxs-lookup"><span data-stu-id="a18cd-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="a18cd-121">Ayrıntılar için bkz. [Işlemlerle çalışma](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="a18cd-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="a18cd-122">Database. Connection. Open ()</span><span class="sxs-lookup"><span data-stu-id="a18cd-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="a18cd-123">EF5 ve önceki sürümler için davranış</span><span class="sxs-lookup"><span data-stu-id="a18cd-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="a18cd-124">EF5 ve önceki sürümlerde, **ObjectContext. Connection. State** öğesinin temel alınan depo bağlantısının gerçek durumunu yansıtacak şekilde güncellenmemiş gibi bir hata vardır.</span><span class="sxs-lookup"><span data-stu-id="a18cd-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="a18cd-125">Örneğin, aşağıdaki kodu yürütülürsünüz, aslında temeldeki depo bağlantısının **Açık**olmasına rağmen **Kapatılan** durum döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a18cd-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="a18cd-126">Ayrıca Database. Connection. Open () yöntemini çağırarak veritabanı bağlantısını açarsanız, bir sorgu yürütülene veya bir veritabanı bağlantısı gerektiren (örneğin, SaveChanges ()), ancak temel alınan mağazadan sonra bir şey çağırana kadar açık olacaktır. bağlantı kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="a18cd-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="a18cd-127">Bağlam daha sonra, başka bir veritabanı işlemi gerektiğinde bağlantıyı yeniden açıp yeniden kapatacak:</span><span class="sxs-lookup"><span data-stu-id="a18cd-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="a18cd-128">EF6 ve gelecekteki sürümlerde davranış</span><span class="sxs-lookup"><span data-stu-id="a18cd-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="a18cd-129">EF6 ve gelecekteki sürümlerde, çağıran kod, bağlamı çağırarak bağlantıyı açmayı seçerse bu yaklaşımı gerçekleştirdik. Database. Connection. Open (), bunu yapmak için iyi bir nedene sahiptir ve Framework, bağlantının açılmasını ve kapatılmasını denetlemek istediğini ve bağlantıyı otomatik olarak kapatmayacak olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="a18cd-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="a18cd-130">Bu, büyük olasılıkla açık olan bağlantılara neden olabilir ve bu da dikkatli olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a18cd-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="a18cd-131">Ayrıca, ObjectContext. Connection. State artık temel alınan bağlantının durumunu doğru bir şekilde izlemek için kodu güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="a18cd-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
