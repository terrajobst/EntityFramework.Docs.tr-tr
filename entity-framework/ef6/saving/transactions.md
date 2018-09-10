---
title: İşlemler - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 26473e1e52a6044babc717d5b158ad73aac5c738
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250615"
---
# <a name="working-with-transactions"></a>İşlemleri ile çalışma
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Bu belgede EF6 işlemleri ile çalışmayı kolaylaştırmak için bu yana EF5 ekledik geliştirmeler dahil olmak üzere işlemlerde kullanarak açıklanmıştır.  

## <a name="what-ef-does-by-default"></a>Varsayılan olarak EF yapar  

Entity Framework tüm sürümlerinde yürüttüğünüz her **SaveChanges()** eklemek, güncelleştirmek veya framework veritabanını silmek için bu işlem, bir işlem içinde kaydırılır. Bu işlem işlemi yürütmek için yalnızca yeteri kadar sürer ve ardından tamamlar. Başka bir işlem yürüttüğünüzde, yeni bir işlem başlatıldı.  

EF6'ile başlayan **Database.ExecuteSqlCommand()** zaten mevcut değilse varsayılan olarak bir işlem içinde komut kaydırılır. Bu yöntemin istiyorsanız bu davranışı geçersiz kılma olanak tanıyan aşırı vardır. API'ler aracılığıyla modeline gibi dahil saklı yordamların EF6 yürütmesinde ayrıca **ObjectContext.ExecuteFunction()** (varsayılan davranış şu anda değiştirilemiyor, dışında) aynı yapar.  

Her iki durumda da, yalıtım düzeyi veritabanı sağlayıcısı varsayılan ayar olarak değerlendirir. işlemin yalıtım düzeyini olduğu. Varsayılan olarak, örneğin, SQL Server'da READ COMMITTED budur.  

Entity Framework, bir işlemde sorguları sarmalamanız değil.  

Bu varsayılan işlevi, çok fazla kullanıcılar ve bu nedenle olup olmadığını EF6 içinde farklı bir şey yapmanıza gerek yoktur uygundur; her zaman yaptığınız gibi kod yazmanız yeterlidir.  

Ancak bazı kullanıcılar işlemler üzerinde daha fazla denetim gerektiren – bu aşağıdaki bölümlerde ele alınmıştır.  

## <a name="how-the-apis-work"></a>API'leri nasıl çalışır?  

(Zaten açık bir bağlantı aktarılırsa bir özel durum oluşturuyordu) veritabanı bağlantısı kendisini açarken önce EF6 Entity Framework insisted. Bir işlem açık bir bağlantı üzerinde yalnızca başlatılabilir olduğundan, bu tek yolu bir kullanıcı birden çok işlem tek bir işleme kaydırma ya da kullanılacak olduğunu geliyordu bir [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) veya  **ObjectContext.Connection** özelliği ve başlangıç çağırma **Open()** ve **BeginTransaction()** doğrudan üzerinde döndürülen **EntityConnection** nesne. Ayrıca, kendi temel alınan veritabanı bağlantısında bir işlem başlatıldı, API çağrıları, bir veritabanı iletişim başarısız olur.  

> [!NOTE]
> Yalnızca kapalı bağlantılarını kabul sınırlaması, Entity Framework 6'da kaldırıldı. Ayrıntılar için bkz [bağlantı yönetimi](~/ef6/fundamentals/connection-management.md).  

Framework EF6 ile başlayarak, artık sağlar:  

1. **Database.BeginTransaction()** : başlatmak ve işlemleri kendileri aynı işlem içinde birleştirilmek üzere çeşitli işlemlere izin verme, var olan bir DbContext içinde– tamamlamak bir kullanıcı için daha kolay bir yöntem ve bu nedenle tüm kaydedilmiş veya tüm biri olarak geri alındı. Ayrıca, daha kolay işlemin yalıtım düzeyini belirtmek kullanıcı sağlar.  
2. **Database.UseTransaction()** : Entity Framework dışında başlatıldı, bir işlem kullanmak DbContext sağlar.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Çeşitli işlemler aynı bağlam içinde tek bir işleme birleştirme  

**Database.BeginTransaction()** iki geçersiz kılmaları – bir açık bir alan olan [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) ve olan hiçbir bağımsız değişkeni alır ve temel alınan veritabanı sağlayıcısından IsolationLevel varsayılan kullanır. Her iki geçersiz kılmaları dönüş bir **DbContextTransaction** sağlayan nesne **Commit()** ve **Rollback()** kaydetme ve geri alma, temel alınan mağaza gerçekleştiren yöntemler işlem.  

**DbContextTransaction** kaydedilmiş veya geri alınmış sonra çıkarılması için tasarlanmıştır. Bir yolla bunu yapmanın kolay **using(...) {...}** otomatik olarak çağırma söz dizimi **Dispose()** kullanarak engelliyken tamamlar:  

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
> Bir işlem başlatılmasına izin gerektirir temel alınan depolama bağlantı açıktır. Önceden açılmış olursa Database.BeginTransaction() yöntemini çağırır; dolayısıyla bağlantıyı açın. DbContextTransaction bağlantı açtıysanız Dispose() çağrıldığında ardından bunu, sona erecektir.  

### <a name="passing-an-existing-transaction-to-the-context"></a>Var olan bir işlem bağlamına geçirme  

Bazen bir işlem kapsam içinde bile daha geniş olduğu ve işlemleri aynı veritabanında ancak EF dışında tamamen içeren ister. Bunu gerçekleştirmek için bağlantıyı açın ve kendiniz bir hareket başlatır ve gerekir sonra EF zaten açık veritabanı bağlantısı (a) kullanmayı ve mevcut işlem, bağlantıda b) kullanmaya söyleyin.  

Bunu yapmak için tanımlayın ve mevcut i) bir bağlantı parametresi ve ii) contextOwnsConnection Boole alan DbContext oluşturucular birinden devralan bağlam sınıfınızın bir oluşturucu kullanın.  

> [!NOTE]
> Bu senaryoda çağrıldığında false contextOwnsConnection bayrağı ayarlanmalıdır. Entity Framework ile bittiğinde, bağlantı kapatmayın bildirir gibi önemli budur (örneğin, aşağıdaki 4 satır bakın):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Ayrıca, işlem kendiniz (varsayılan ayar kaçınmak istiyorsanız IsolationLevel dahil) başlatmalı ve Entity Framework bağlantıda zaten başlatılmış olan bir işlem olduğunu bilmesini sağlar (satır 33 aşağıya bakın).  

Ardından doğrudan SqlConnection veya DbContext üzerinde veritabanı işlemleri yürütmek ücretsizdir. Tüm işlemler bir işlem içinde yürütülür. Sorumluluk işleniyor veya işlemi geri alınıyor ve üzerinde Dispose() çağırma yanı sıra kapatma ve veritabanı bağlantısı kesilirken alır. Örneğin:  

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

### <a name="clearing-up-the-transaction"></a>Temizleme işlemi ayarlama

Geçerli işlem için Entity Framework'ün bilgisini temizlemek için Database.UseTransaction() için null değeri geçirebilirsiniz. Entity Framework ne yürütme veya geri alma mevcut işlem, bu nedenle dikkatli kullanın ve yalnızca eminseniz bu yapmak istediğiniz olur.  

### <a name="errors-in-usetransaction"></a>UseTransaction hataları

İşlem başarılı olursa Database.UseTransaction() adresinden bir özel durum görürsünüz olduğunda:  
- Entity Framework var olan bir işlem zaten var.  
- Entity Framework bir TransactionScope içinde zaten çalışıyor  
- Geçirilen işlemde bağlantı nesnesi null olur. Diğer bir deyişle, işlem bir bağlantıyla ilişkili değil: genellikle bu, işlem zaten tamamlanmış bir oturum açma adıdır.  
- Geçirilen işlemde bağlantı nesnesi, Entity Framework'ün bağlantı eşleşmiyor.  

## <a name="using-transactions-with-other-features"></a>Diğer özelliklerle işlemleri kullanma  

Bu bölümde, yukarıdaki işlemleri nasıl etkileşim açıklanmaktadır:  

- Bağlantı dayanıklılığı  
- Zaman uyumsuz yöntemler  
- TransactionScope işlemleri  

### <a name="connection-resiliency"></a>Bağlantı Dayanıklılığı  

Yeni bağlantı dayanıklılığı özelliğini kullanıcı tarafından başlatılan işlemleri ile çalışmaz. Ayrıntılar için bkz [yeniden deneme yürütme stratejileri](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Zaman uyumsuz programlama  

Önceki bölümlerde açıklanan yaklaşımı hiçbir başka seçenekleri veya ayarları çalışmak için gereken [zaman uyumsuz yöntemleri kaydedin ve sorgu](~/ef6/fundamentals/async.md
). Ancak, zaman uyumsuz yöntemler içinde neler bağlı olarak, bu sırayla, kilitlenmeleri veya genel uygulama performansını için hatalı olduğu engelleme neden olabilir. uzun süre çalışan işlemler – neden olabilir, unutmayın.  

### <a name="transactionscope-transactions"></a>TransactionScope işlemleri  

EF6 önce TransactionScope nesnesini kullanmak için önerilen yöntem, daha büyük kapsam işlemler sağlama şöyleydi:  

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

SqlConnection ve Entity Framework hem ortam TransactionScope işlem kullanın ve bu nedenle birlikte yürütülmesi.  

.NET 4.5.1 TransactionScope kullanımı aracılığıyla zaman uyumsuz yöntemler de çalışmak üzere güncelleştirilmiştir başlayarak [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) sabit listesi:  

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

Yine de bazı sınırlamalar TransactionScope yaklaşım vardır:  

- .NET 4.5.1 gerektirir veya zaman uyumsuz yöntemler ile çalışmak daha büyük.  
- Bir ve yalnızca bir bağlantı olduğundan emin değilseniz bulut senaryolarda kullanılamaz (bulut senaryolarına dağıtılmış işlemler desteklemez).  
- Önceki bölümlerde Database.UseTransaction() yaklaşımıyla birleştirilemez.  
- Bir DDL vermek ve MSDTC hizmeti aracılığıyla dağıtılmış işlemler etkinleştirmediniz özel durumlar.  

TransactionScope yaklaşımın avantajları:  

- Birden fazla belirli bir veritabanı bağlantısı veya aynı işlem içinde farklı bir veritabanı bağlantısı olan bir veritabanına bir bağlantı birleştirmek, otomatik olarak yerel bir işlem için Dağıtılmış bir işlem yükseltir (Not: olması gerekir Dağıtılmış işlemler için bunun çalışmasına izin verecek şekilde yapılandırılmış MSDTC hizmetinin).  
- Kodlama kolaylığı. Tercih ettiğiniz ortam ve örtük olarak arka planda ile dağıtılmış işlem yerine, açıkça altında denetim ardından TransactionScope yaklaşımı, daha iyi uygun.  

Özet olarak, yeni Database.BeginTransaction() ve yukarıdaki Database.UseTransaction() API'leri ile TransactionScope yaklaşım artık çoğu kullanıcı için gerekli değildir. TransactionScope kullanmaya devam ederseniz ardından yukarıdaki sınırlamaları unutmayın. Mümkün olduğunda bunun yerine önceki bölümlerde açıklanan yaklaşımı kullanmanızı öneririz.  
