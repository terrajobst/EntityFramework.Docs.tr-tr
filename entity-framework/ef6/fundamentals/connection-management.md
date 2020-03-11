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
# <a name="connection-management"></a>Bağlantı yönetimi
Bu sayfada, bağlama bağlantılarının ve **Database. Connection. Open ()** API 'sinin işlevselliğiyle ilgili olarak Entity Framework davranışı açıklanmaktadır.  

## <a name="passing-connections-to-the-context"></a>Bağlama bağlantıları geçirme  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 ve önceki sürümler için davranış  

Bağlantıları kabul eden iki Oluşturucu vardır:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Bunları kullanmak mümkündür ancak birkaç sınırlamaya geçici çözüm kullanmanız gerekir:  

1. Bunlardan herhangi birine açık bir bağlantı geçirirseniz, çerçeve bunu ilk kez kullanmaya çalıştığında, zaten açık bir bağlantıyı yeniden açkullanılamadığını belirten bir InvalidOperationException oluşturulur.  
2. ContextOwnsConnection bayrağı, bağlam atıldığı zaman temeldeki depo bağlantısının atılıp atılmayacağı anlamına gelir. Ancak, bu ayardan bağımsız olarak, bağlam atıldığı zaman mağaza bağlantısı her zaman kapalıdır. Bu nedenle, aynı bağlantıya sahip olan birden fazla DbContext varsa, ilk olarak bağlantıyı kapatır (benzer şekilde, bir DbContext ile var olan bir ADO.NET bağlantısı karmalırsa DbContext, her zaman bırakıldığında bağlantıyı kapatır) .  

Kapalı bir bağlantıyı geçirerek ve yalnızca tüm bağlamlar oluşturulduktan sonra açılacak kodu yürüterek yukarıdaki ilk kısıtlamayı geçici olarak çözmek mümkündür:  

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

İkinci sınırlama, bağlantının kapatılmasını sağlamak için hazırlanana kadar DbContext Nesnelerinizden herhangi birini elden atma konusunda sizi onaylama gereği duymaktan kaçınabilirsiniz.  

### <a name="behavior-in-ef6-and-future-versions"></a>EF6 ve gelecekteki sürümlerde davranış  

EF6 ve gelecekteki sürümlerde, DbContext aynı iki oluşturucuya sahiptir ancak artık, oluşturucuya geçirilen bağlantının alındığı sırada kapatılmasını gerektirmez. Bu nedenle şu anda mümkündür:  

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

Ayrıca, contextOwnsConnection bayrağı artık, DbContext atıldığı zaman bağlantının kapatılıp kapatılmayacağını denetler. Yukarıdaki örnekte, bağlam bırakıldığında bağlantı kapanmaz (satır 32), EF 'in önceki sürümlerinde olduğu gibi, ancak bağlantının kendisi atıldığı zaman (satır 40).  

Bu işlem, DbContext 'in bağlantı denetimini ele geçirmesine (yalnızca contextOwnsConnection ' i true olarak ayarlamanız veya diğer oluşturuculardan birini kullanması) mümkün olmaya devam etmektedir.  

> [!NOTE]
> Bu yeni modelle işlemler kullanılırken bazı ek hususlar vardır. Ayrıntılar için bkz. [Işlemlerle çalışma](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database. Connection. Open ()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 ve önceki sürümler için davranış  

EF5 ve önceki sürümlerde, **ObjectContext. Connection. State** öğesinin temel alınan depo bağlantısının gerçek durumunu yansıtacak şekilde güncellenmemiş gibi bir hata vardır. Örneğin, aşağıdaki kodu yürütülürsünüz, aslında temeldeki depo bağlantısının **Açık**olmasına rağmen **Kapatılan** durum döndürülür.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Ayrıca Database. Connection. Open () yöntemini çağırarak veritabanı bağlantısını açarsanız, bir sorgu yürütülene veya bir veritabanı bağlantısı gerektiren (örneğin, SaveChanges ()), ancak temel alınan mağazadan sonra bir şey çağırana kadar açık olacaktır. bağlantı kapatılacak. Bağlam daha sonra, başka bir veritabanı işlemi gerektiğinde bağlantıyı yeniden açıp yeniden kapatacak:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>EF6 ve gelecekteki sürümlerde davranış  

EF6 ve gelecekteki sürümlerde, çağıran kod, bağlamı çağırarak bağlantıyı açmayı seçerse bu yaklaşımı gerçekleştirdik. Database. Connection. Open (), bunu yapmak için iyi bir nedene sahiptir ve Framework, bağlantının açılmasını ve kapatılmasını denetlemek istediğini ve bağlantıyı otomatik olarak kapatmayacak olduğunu varsayacaktır.  

> [!NOTE]
> Bu, büyük olasılıkla açık olan bağlantılara neden olabilir ve bu da dikkatli olarak kullanılır.  

Ayrıca, ObjectContext. Connection. State artık temel alınan bağlantının durumunu doğru bir şekilde izlemek için kodu güncelleştirdik.  

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
