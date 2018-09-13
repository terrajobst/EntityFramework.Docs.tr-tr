---
title: Bağlantı Yönetimi - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489342"
---
# <a name="connection-management"></a><span data-ttu-id="b5511-102">Bağlantı Yönetimi</span><span class="sxs-lookup"><span data-stu-id="b5511-102">Connection management</span></span>
<span data-ttu-id="b5511-103">Bu sayfa bağlamı ve işlevselliği için bağlantıları geçirme onaylamaz Entity Framework davranışını açıklar **Database.Connection.Open()** API.</span><span class="sxs-lookup"><span data-stu-id="b5511-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="b5511-104">Bağlam bağlantılarını geçirme</span><span class="sxs-lookup"><span data-stu-id="b5511-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="b5511-105">EF5 ve önceki sürümleri için davranışı</span><span class="sxs-lookup"><span data-stu-id="b5511-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="b5511-106">Bağlantıları kabul iki Oluşturucu vardır:</span><span class="sxs-lookup"><span data-stu-id="b5511-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="b5511-107">Bunlar kullanmak da mümkündür, ancak birkaç sınırlama çalışma gerekir:</span><span class="sxs-lookup"><span data-stu-id="b5511-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="b5511-108">Diyor açık bir bağlantı bunlardan birisi sonra framework, bir InvalidOperationException durum kullanmayı dener ilk kez geçirirseniz zaten açık bir bağlantı yeniden açamazsınız.</span><span class="sxs-lookup"><span data-stu-id="b5511-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="b5511-109">ContextOwnsConnection bayrağı, temel alınan depolama bağlantı bağlamı çıkarıldığından çıkarılması gereken olup olmadığını anlama olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="b5511-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="b5511-110">Ancak bağlam çıkarıldığından, ayar ne olursa olsun, depolama bağlantı her zaman kapalı.</span><span class="sxs-lookup"><span data-stu-id="b5511-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="b5511-111">Aynı bağlantı birden fazla DbContext varsa, hangi bağlam atıldı önce (bir DbContext ile var olan bir ADO.NET bağlantı karıştırıldığında çıkarıldığından, benzer şekilde DbContext her zaman bağlantısını kesecektir) bağlantı kapatılacak şekilde .</span><span class="sxs-lookup"><span data-stu-id="b5511-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="b5511-112">İlk sınırlaması geçici olarak kapatılan bir bağlantı geçirme ve yalnızca tüm bağlamları oluşturduktan sonra açmak kodu yürüten çalışma mümkündür:</span><span class="sxs-lookup"><span data-stu-id="b5511-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="b5511-113">İkinci sınırlama, yalnızca bağlantı kapatılacak hazır olana kadar herhangi bir DbContext nesnelerinizi disposing gelen engellemeye ihtiyacınız olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="b5511-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="b5511-114">EF6 ve gelecek sürümlerde davranışı</span><span class="sxs-lookup"><span data-stu-id="b5511-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="b5511-115">EF6 ve gelecek sürümlerde DbContext aynı iki Oluşturucusu vardır, ancak artık, alındığında oluşturucuya geçirilen bağlantı kapalı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="b5511-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="b5511-116">Bu nedenle bu artık mümkündür:</span><span class="sxs-lookup"><span data-stu-id="b5511-116">So this is now possible:</span></span>  

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

<span data-ttu-id="b5511-117">Ayrıca contextOwnsConnection bayrağı artık olsun veya olmasın, bağlantı hem kapatılır ve DbContext silinmediğinde denetler.</span><span class="sxs-lookup"><span data-stu-id="b5511-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="b5511-118">Yukarıdaki örnekte bağlantı kapatıldığında, bu bağlam olduğunda değil için (satır 32) EF önceki sürümlerinde olabilirdi gibi ancak bunun yerine, bağlantı çıkarıldığından elden (satır 40).</span><span class="sxs-lookup"><span data-stu-id="b5511-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="b5511-119">Elbette DbContext (doğru veya diğer oluşturucular birini yalnızca kümesi contextOwnsConnection) bağlantı denetimini almak hala mümkün şekilde istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="b5511-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="b5511-120">İşlem, bu yeni modelde kullanırken bazı ek hususlar vardır.</span><span class="sxs-lookup"><span data-stu-id="b5511-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="b5511-121">Ayrıntılar için bkz. [işlemleri çalışma](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="b5511-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="b5511-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="b5511-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="b5511-123">EF5 ve önceki sürümleri için davranışı</span><span class="sxs-lookup"><span data-stu-id="b5511-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="b5511-124">EF5 ve önceki sürümlerde bir hata varsa şekilde **ObjectContext.Connection.State** true temel alınan depolama bağlantı durumunu yansıtacak şekilde güncelleştirilmedi.</span><span class="sxs-lookup"><span data-stu-id="b5511-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="b5511-125">Aşağıdaki kod yürütüldüğünde, örneğin, durum döndürülebilir **kapalı** aslında temel alınan depolama olsa da bağlantı **açık**.</span><span class="sxs-lookup"><span data-stu-id="b5511-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="b5511-126">Database.Connection.Open() çağırarak veritabanı bağlantısını açtığınıza gerekirse kadar sonraki açışınızda, bir sorgu yürütme veya bir veritabanı bağlantısı gerektiren her şeyi arayın ayrı olarak açık olacak (örneğin, SaveChanges()) sonra ancak arka plandaki depolamanız bağlantı kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="b5511-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="b5511-127">Bağlamı daha sonra yeniden açılacak ve bağlantıyı başka bir veritabanı işlemi gereklidir dilediğiniz zaman yeniden kapatın:</span><span class="sxs-lookup"><span data-stu-id="b5511-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="b5511-128">EF6 ve gelecek sürümlerde davranışı</span><span class="sxs-lookup"><span data-stu-id="b5511-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="b5511-129">Çağıran kod tarafından çağıran bağlamını bağlantı açmayı seçerse EF6 ve sonraki sürümleri için Biz bu yaklaşım, yönlendirdik. Database.Connection.Open() sonra bunu yapmak için geçerli bir nedeniniz var ve framework denetime açılış ve kapanış bağlantının istediği ve artık bağlantı otomatik olarak kapanacak varsayar.</span><span class="sxs-lookup"><span data-stu-id="b5511-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="b5511-130">Bu olası dikkatli uzun şekilde kullanmak için açık olan bağlantılar açabilir.</span><span class="sxs-lookup"><span data-stu-id="b5511-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="b5511-131">Böylece ObjectContext.Connection.State artık temel alınan bağlantı durumunu doğru şekilde izler ayrıca kod güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="b5511-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
