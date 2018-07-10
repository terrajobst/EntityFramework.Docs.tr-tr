---
title: Bağlantı Yönetimi - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 361065def0e83a097d4bb0109d468983ce41cd86
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914048"
---
# <a name="connection-management"></a>Bağlantı Yönetimi
Bu sayfa bağlamı ve işlevselliği için bağlantıları geçirme onaylamaz Entity Framework davranışını açıklar **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>Bağlam bağlantılarını geçirme  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 ve önceki sürümleri için davranışı  

Bağlantıları kabul iki Oluşturucu vardır:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Bunlar kullanmak da mümkündür, ancak birkaç sınırlama çalışma gerekir:  

1. Diyor açık bir bağlantı bunlardan birisi sonra framework, bir InvalidOperationException durum kullanmayı dener ilk kez geçirirseniz zaten açık bir bağlantı yeniden açamazsınız.  
2. ContextOwnsConnection bayrağı, temel alınan depolama bağlantı bağlamı çıkarıldığından çıkarılması gereken olup olmadığını anlama olarak yorumlanır. Ancak bağlam çıkarıldığından, ayar ne olursa olsun, depolama bağlantı her zaman kapalı. Aynı bağlantı birden fazla DbContext varsa, hangi bağlam atıldı önce (bir DbContext ile var olan bir ADO.NET bağlantı karıştırıldığında çıkarıldığından, benzer şekilde DbContext her zaman bağlantısını kesecektir) bağlantı kapatılacak şekilde .  

İlk sınırlaması geçici olarak kapatılan bir bağlantı geçirme ve yalnızca tüm bağlamları oluşturduktan sonra açmak kodu yürüten çalışma mümkündür:  

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

İkinci sınırlama, yalnızca bağlantı kapatılacak hazır olana kadar herhangi bir DbContext nesnelerinizi disposing gelen engellemeye ihtiyacınız olduğu anlamına gelir.  

### <a name="behavior-in-ef6-and-future-versions"></a>EF6 ve gelecek sürümlerde davranışı  

EF6 ve gelecek sürümlerde DbContext aynı iki Oluşturucusu vardır, ancak artık, alındığında oluşturucuya geçirilen bağlantı kapalı gerektirir. Bu nedenle bu artık mümkündür:  

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

Ayrıca contextOwnsConnection bayrağı artık olsun veya olmasın, bağlantı hem kapatılır ve DbContext silinmediğinde denetler. Yukarıdaki örnekte bağlantı kapatıldığında, bu bağlam olduğunda değil için (satır 32) EF önceki sürümlerinde olabilirdi gibi ancak bunun yerine, bağlantı çıkarıldığından elden (satır 40).  

Elbette DbContext (doğru veya diğer oluşturucular birini yalnızca kümesi contextOwnsConnection) bağlantı denetimini almak hala mümkün şekilde istiyorsanız.  

> [!NOTE]
> İşlem, bu yeni modelde kullanırken bazı ek hususlar vardır. Ayrıntılar için bkz. [işlemleri çalışma](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>EF5 ve önceki sürümleri için davranışı  

EF5 ve önceki sürümlerde bir hata varsa şekilde **ObjectContext.Connection.State** true temel alınan depolama bağlantı durumunu yansıtacak şekilde güncelleştirilmedi. Aşağıdaki kod yürütüldüğünde, örneğin, durum döndürülebilir **kapalı** aslında temel alınan depolama olsa da bağlantı **açık**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Database.Connection.Open() çağırarak veritabanı bağlantısını açtığınıza başlatıldığında, bir sorgu yürütme veya bir veritabanı bağlantısı (temel alınan bağlantı depolamak örn SaveChanges()) sonra ancak gerektiren herhangi bir şey çağırmak kadar ayrı olarak, bu açık olacaktır kapatılacak. Bağlamı daha sonra yeniden açılacak ve bağlantıyı başka bir veritabanı işlemi gereklidir dilediğiniz zaman yeniden kapatın:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>EF6 ve gelecek sürümlerde davranışı  

Çağıran kod tarafından çağıran bağlamını bağlantı açmayı seçerse EF6 ve sonraki sürümleri için Biz bu yaklaşım, yönlendirdik. Database.Connection.Open() sonra bunu yapmak için geçerli bir nedeniniz var ve framework denetime açılış ve kapanış bağlantının istediği ve artık bağlantı otomatik olarak kapanacak varsayar.  

> [!NOTE]
> Bu olası dikkatli uzun şekilde kullanmak için açık olan bağlantılar açabilir.  

Böylece ObjectContext.Connection.State artık temel alınan bağlantı durumunu doğru şekilde izler ayrıca kod güncelleştirdik.  

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
