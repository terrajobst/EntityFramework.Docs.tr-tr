---
title: İşlemler - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: fe4c0d6ad7ccb2e97dc94fbf2eb26a41e7fbcb19
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="using-transactions"></a>İşlemleri kullanma

İşlemler birkaç veritabanı işlemlerinin atomik bir şekilde işlenmesine izin verin. İşlem, tüm işlemlerin başarılı bir şekilde veritabanına uygulanır. İşlem geri alındı, işlemleri hiçbiri veritabanına uygulanır.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) github'da.

## <a name="default-transaction-behavior"></a>Varsayılan işlem davranışı

Varsayılan olarak, veritabanı sağlayıcısı işlemleri, destekliyorsa tüm değişiklikler tek bir çağrı `SaveChanges()` bir işlem içinde uygulanır. Değişiklikleri başarısız olursa, işlem geri alındı ve değişikliklerin hiçbiri veritabanına uygulanır. Bunun anlamı `SaveChanges()` tamamen başarılı ya da bir hata oluşursa değiştirilmemiş veritabanını bırak sağlanır.

Çoğu uygulama için bu varsayılan davranış yeterli olur. Uygulamanızın gereksinimleri gerekli bulduğunuz varsa işlemleri yalnızca el ile denetlemesi gerekir.

## <a name="controlling-transactions"></a>İşlemleri denetleme

Kullanabileceğiniz `DbContext.Database` başlamak için Yürüt API ve geri alma işlemleri. Aşağıdaki örnekte iki gösterir `SaveChanges()` işlemler ve bir LINQ Sorgu tek bir işlemde yürütülmekte.

Tüm veritabanı sağlayıcıları işlemleri destekler. Bazı sağlayıcılar atabilir veya no-op işlem API'leri çağrıldığında.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>Çapraz bağlam işlemi (yalnızca ilişkisel veritabanları)

Bu gibi durumlarda, bir işlem aynı zamanda birden fazla bağlam örneklerinde paylaşabilirsiniz. Bu işlevsellik yalnızca kullanımını gerektirdiğinden bir ilişkisel veritabanı sağlayıcısı kullanırken kullanılabilir `DbTransaction` ve `DbConnection`, ilişkisel veritabanları için belirli olduğu.

Bir işlem paylaşmak için bağlamları her ikisi de paylaşmalıdır bir `DbConnection` ve `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Dışarıdan sağlanması bağlantıya izin ver

Paylaşımı bir `DbConnection` , oluşturma sırasında bir bağlamına bağlantı geçmesine becerisi gerektirir.

İzin vermek için en kolay yolu `DbConnection` dışarıdan sağlanması için kullanarak durdurmaktır `DbContext.OnConfiguring` bağlam yapılandırmak ve harici olarak oluşturmak için yöntem `DbContextOptions` ve bağlam oluşturucuya geçirin.

> [!TIP]  
> `DbContextOptionsBuilder` içinde kullanılan API `DbContext.OnConfiguring` bağlam yapılandırmak için şimdi onu dışarıdan oluşturmak için kullanacaksanız `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Kullanmaya devam etmek için bir alternatif olan `DbContext.OnConfiguring`, ancak kabul bir `DbConnection` kaydedilir ve ardından kullanılan `DbContext.OnConfiguring`.

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a>Paylaşım bağlantı ve işlem

Artık aynı bağlantıyı paylaşan birden fazla bağlam örneği oluşturabilirsiniz. Ardından `DbContext.Database.UseTransaction(DbTransaction)` aynı işlem içinde her iki bağlamları listeleme API.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Dış DbTransactions (yalnızca ilişkisel veritabanları) kullanma

İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojileri kullanıyorsanız, bir işlem farklı teknolojiler tarafından gerçekleştirilen işlemler arasında paylaşmak isteyebilirsiniz.

Aşağıdaki örnek, bir ADO.NET SqlClient işlem ve bir Entity Framework Çekirdek işlem aynı işlemde gerçekleştirme gösterir.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>System.Transactions kullanma

> [!NOTE]  
> Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.

Ortam işlemleri arasında daha büyük bir kapsama koordine etmek gerekiyorsa kullanmak da mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

Bir açık işleminde listelenmeyi mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a>System.Transactions sınırlamaları  

1. EF çekirdek System.Transactions desteği uygulamak için veritabanı sağlayıcıları kullanır. ADO.NET sağlayıcıda .NET Framework, API yalnızca en son .NET Core eklenmiş durumda ve bu nedenle desteklemek için destek oldukça yaygındır rağmen olduğu olmaması gibi yaygın. Bir sağlayıcı System.Transactions desteği uygulamaz varsa bu API çağrıları tam yoksayılmasına mümkündür. .NET Core SqlClient başlayarak 2.1 desteklemiyor. SqlClient .NET Core 2.0 için bir özel durum oluşturur, özelliğini kullanmayı dener. 

   > [!IMPORTANT]  
   > Buna işlemleri yönetmek için bağlı önce API sağlayıcınız ile düzgün şekilde davranan test önerilir. Mevcut değilse veritabanı sağlayıcısı Bakımcı başvurmanız önerilir. 

2. Sürüm 2.1 itibariyle .NET Core System.Transactions uygulamasında dağıtılmış işlem desteği dahil değildir; bu nedenle kullanamazsınız `TransactionScope` veya `CommitableTransaction` arasında birden çok kaynak yöneticileri işlemleri koordine etmek için. 
