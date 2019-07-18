---
title: Işlemlerle çalışma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306518"
---
# <a name="working-with-transactions"></a>Işlemlerle çalışma
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Bu belge, EF6 ' deki işlemler kullanılarak, işlemler ile çalışmayı kolaylaştırmak için EF5 bu yana eklediğimiz iyileştirmeler dahil olmak üzere açıklanır.  

## <a name="what-ef-does-by-default"></a>Varsayılan olarak ne kadar EF  

Tüm Entity Framework sürümlerinde, her **SaveChanges ()** işlemini veritabanı üzerinde eklemek, güncelleştirmek veya silmek için çalıştırdığınızda Framework bu işlemi bir işlem içinde saracaktır. Bu işlem yalnızca işlemi yürütmek için yeterince uzun sürer ve sonra tamamlanır. Bu gibi başka bir işlem yürüttüğünüzde yeni bir işlem başlatılır.  

EF6 veritabanı ile başlangıç **. ExecuteSqlCommand ()** varsayılan olarak, bir işlem zaten mevcut değilse komutu bir işlem içinde saracaktır. İsterseniz bu davranışı geçersiz kılmanıza izin veren bu yöntemin aşırı yüklemeleri vardır. Ayrıca, EF6 **. ExecuteFunction ()** gibi API 'ler aracılığıyla modelde bulunan saklı yordamların yürütülmesi de aynı olur (varsayılan davranışın Şu anda geçersiz kılınamaması hariç).  

Her iki durumda da, işlemin yalıtım düzeyi, veritabanı sağlayıcının varsayılan ayarını düşündüğü her türlü yalıtım düzeyidir. Varsayılan olarak, örneğin SQL Server, bu okundu olarak kaydedilir.  

Entity Framework bir işlemdeki sorguları sarmaz.  

Bu varsayılan işlevsellik çok sayıda kullanıcı için uygundur ve EF6 içinde farklı bir şey yapmanız gerekmez; her zaman yaptığınız gibi kodu yazmanız yeterlidir.  

Ancak bazı kullanıcılar işlemleri üzerinde daha fazla denetim gerektirir. Bu, aşağıdaki bölümlerde ele alınmıştır.  

## <a name="how-the-apis-work"></a>API 'Ler nasıl çalışır?  

EF6 ' dan önce, veritabanı bağlantısının kendisini açmaya yönelik olarak Entity Framework, (zaten açık olan bir bağlantıyı geçirmişse bir özel durum oluşturdu). Bir işlem yalnızca açık bir bağlantı üzerinde başlatılacağından, bu, bir kullanıcının birkaç işlemi tek bir işlemde kaydıracağından, bir [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) kullanmanın veya **ObjectContext. Connection** özelliğini kullanırken ve Başlarken doğrudan döndürülen **EntityConnection** nesnesinde **Open ()** ve **BeginTransaction ()** çağrılıyor. Buna ek olarak, kendi üzerinde temel alınan veritabanı bağlantısında bir işlem başladıysanız veritabanıyla iletişim kurulan API çağrıları başarısız olur.  

> [!NOTE]
> Yalnızca kapalı bağlantıları kabul etme sınırlaması Entity Framework 6 ' da kaldırılmıştır. Ayrıntılar için bkz. [bağlantı yönetimi](~/ef6/fundamentals/connection-management.md).  

EF6 ile başlayarak Framework artık şunları sağlar:  

1. **Database. BeginTransaction ()** : Bir kullanıcının mevcut bir DbContext içinde kendi işlemlerini başlatması ve tamamlaması için daha kolay bir yöntem vardır. birkaç işlemin aynı işlem içinde birleştirilmesi ve bu nedenle tümü kaydedilmiş ya da tümünün bir tane olarak geri alınması sağlanır. Ayrıca, kullanıcının işlem için yalıtım düzeyini daha kolay belirlemesine izin verir.  
2. **Database. UseTransaction ()** : dbcontext 'in Entity Framework dışında başlatılan bir işlem kullanmasına izin verir.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Birkaç işlemi aynı bağlam içindeki tek bir işlemde birleştirme  

**Database. BeginTransaction ()** iki geçersiz kılmaya sahiptir: bir açık [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) ve bağımsız değişken alan ve temel alınan veritabanı sağlayıcısından varsayılan IsolationLevel 'ı kullanan bir tane. Her iki geçersiz kılma de, temel alınan depolama işleminde COMMIT ve Rollback gerçekleştiren **COMMIT ()** ve **Rollback ()** yöntemlerini sağlayan bir **dbcontexttransaction** nesnesi döndürür.  

**Dbcontexttransaction** , kaydedildikten veya geri alındıktan sonra atılmalıdır. Bunu yapmanın kolay bir yolu, **using (...) {...}** using bloğu tamamlandığında **Dispose ()** öğesini otomatik olarak çağıran sözdizimi:  

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
> Bir işlemin başlangıcında, temeldeki depo bağlantısının açık olması gerekir. Bu nedenle, zaten açılmadıysa Database. BeginTransaction () çağrılırken bağlantı açılır. DbContextTransaction bağlantıyı açtıysa, Dispose () çağrıldığında onu kapatır.  

### <a name="passing-an-existing-transaction-to-the-context"></a>Varolan bir işlemi bağlama geçirme  

Bazen kapsamda daha geniş olan ve aynı veritabanı üzerinde işlemler içeren, ancak EF 'in tamamen dışında bir işlem istersiniz. Bunu gerçekleştirmek için bağlantıyı açmanız ve işlemi kendiniz başlatmanız ve sonra da bu bağlantı üzerinde var olan işlemi kullanmak üzere zaten açık olan veritabanı bağlantısını (b) kullanmak için EF 'e söylemeniz gerekir.  

Bunu yapmak için, bağlam sınıfınız üzerinde, var olan bir bağlantı parametresi ve II) contextOwnsConnection Boole değeri olan DbContext oluşturucularından birinden devralan bir Oluşturucu tanımlamanız ve kullanmanız gerekir.  

> [!NOTE]
> Bu senaryoda çağrıldığında contextOwnsConnection bayrağının false olarak ayarlanması gerekir. Bu, ile işiniz bittiğinde bağlantıyı kapatmaması gerektiğini Entity Framework bilgilendirdiğinden önemlidir (örneğin, aşağıdaki satır 4 ' ü inceleyin):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Ayrıca, işlemi kendiniz başlatmanız gerekir (varsayılan ayarı önlemek istiyorsanız IsolationLevel dahil olmak üzere) ve bağlantı üzerinde zaten başlatılmış olan mevcut bir işlem olduğunu bildirmek Entity Framework (bkz. Line 33).  

Daha sonra, veritabanı işlemlerini doğrudan SqlConnection üzerinde ya da DbContext üzerinde yürütebilirsiniz. Bu gibi tüm işlemler tek bir işlem içinde yürütülür. İşlemin uygulanması veya geri alınması ve üzerinde Dispose () çağrısı yapmak ve veritabanı bağlantısını kapatmak ve elden atmak için sorumluluğu vardır. Örneğin:  

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

### <a name="clearing-up-the-transaction"></a>İşlem temizleniyor

Entity Framework geçerli işlem bilgisini temizlemek için Database. UseTransaction () öğesine null geçirebilirsiniz. Entity Framework bunu yaptığınızda, mevcut işlemi ne yapmalı ne de geri almaz, bu nedenle dikkatli olun ve yalnızca bunun yapmak istediğiniz şeydir.  

### <a name="errors-in-usetransaction"></a>UseTransaction hataları

Şu durumlarda bir işlem geçirirseniz Database. UseTransaction () için bir özel durum görürsünüz:  
- Entity Framework zaten mevcut bir işlem var  
- Entity Framework zaten bir TransactionScope içinde çalışıyor  
- Geçirilen işlemdeki bağlantı nesnesi null. Diğer bir deyişle, işlem bir bağlantıyla ilişkili değildir, genellikle bu işlemin zaten tamamlandığını belirten bir imzadır  
- Geçirilen işlemdeki bağlantı nesnesi Entity Framework bağlantısıyla eşleşmiyor.  

## <a name="using-transactions-with-other-features"></a>Diğer özelliklerle işlemleri kullanma  

Bu bölüm, yukarıdaki işlemlerin nasıl etkileşime gireceğini ayrıntılarıyla açıklamaktadır:  

- Bağlantı dayanıklılığı  
- Zaman uyumsuz yöntemler  
- TransactionScope işlemleri  

### <a name="connection-resiliency"></a>Bağlantı Dayanıklılığı  

Yeni bağlantı dayanıklılığı özelliği, Kullanıcı tarafından başlatılan işlemlerle çalışmaz. Ayrıntılar için bkz. [yürütme stratejilerini yeniden deneme](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Zaman Uyumsuz Programlama  

Önceki bölümlerde özetlenen yaklaşımın, [zaman uyumsuz sorgu ve kaydetme yöntemleriyle](~/ef6/fundamentals/async.md
)birlikte çalışması için başka seçenekler veya ayarlar olması gerekir. Ancak, zaman uyumsuz yöntemlerde yaptığınız işlemlere bağlı olarak, bu durum uzun süre çalışan işlemler oluşmasına neden olabilir. bu durum, genel uygulamanın performansı için bozuk olan kilitlenmeleri veya engellemeye yol açabilir.  

### <a name="transactionscope-transactions"></a>TransactionScope Işlemleri  

EF6 öncesinde, daha büyük kapsam işlemleri sağlamanın önerilen yolu bir TransactionScope nesnesi kullanmaktır:  

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

SqlConnection ve Entity Framework her ikisi de çevresel TransactionScope işlemini kullanır ve bu nedenle birlikte işlenir.  

.NET 4.5.1 TransactionScope ile çalışmaya başlamak, [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) numaralandırması kullanılarak zaman uyumsuz yöntemlerle de çalışacak şekilde güncelleştirilmiştir:  

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

TransactionScope yaklaşımına yönelik bazı sınırlamalar vardır:  

- Zaman uyumsuz yöntemlerle çalışmak için .NET 4.5.1 veya üstünü gerektirir.  
- Tek bir bağlantınızın olduğundan ve yalnızca bir bağlantınız olduğundan (bulut senaryoları dağıtılmış işlemleri desteklemediğinden), bulut senaryolarında kullanılamaz.  
- Önceki bölümlerin Database. UseTransaction () yaklaşımıyla birleştirilemez.  
- Herhangi bir DDL 'ye sahipseniz ve dağıtılmış işlemleri MSDTC hizmeti aracılığıyla etkinleştirmediyseniz, bu durum özel durumlar oluşturur.  

TransactionScope yaklaşımının avantajları:  

- Belirli bir veritabanına birden fazla bağlantı yaparsanız veya aynı işlem içindeki farklı bir veritabanıyla bağlantı ile bir bağlantıyı birleştiren bir yerel işlemi otomatik olarak dağıtılmış bir işlem olarak yükseltir (unutmayın: Bu işlemin çalışması için dağıtılmış işlemlere izin vermek üzere yapılandırılan MSDTC hizmeti.  
- Kodlama kolaylığı. İşlemin doğrudan kontrol altına almak yerine, bir hareketin çevresel hale ve arka planda bir şekilde karşılanmayı tercih ediyorsanız, TransactionScope yaklaşımı daha iyi bir şekilde gelebilir.  

Özet olarak, yukarıdaki yeni Database. BeginTransaction () ve Database. UseTransaction () API 'Lerinde, TransactionScope yaklaşımı çoğu kullanıcı için artık gerekli değildir. TransactionScope kullanmaya devam ederseniz yukarıdaki kısıtlamaların farkında olun. Mümkün olduğunda, önceki bölümlerde özetlenen yaklaşımı kullanmanızı öneririz.  
