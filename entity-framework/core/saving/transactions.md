---
title: "İşlemler - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a><span data-ttu-id="d1840-102">İşlemleri kullanma</span><span class="sxs-lookup"><span data-stu-id="d1840-102">Using Transactions</span></span>

<span data-ttu-id="d1840-103">İşlemler birkaç veritabanı işlemlerinin atomik bir şekilde işlenmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="d1840-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="d1840-104">İşlem, tüm işlemlerin başarılı bir şekilde veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d1840-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="d1840-105">İşlem geri alındı, işlemleri hiçbiri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d1840-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="d1840-106">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) github'da.</span><span class="sxs-lookup"><span data-stu-id="d1840-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="d1840-107">Varsayılan işlem davranışı</span><span class="sxs-lookup"><span data-stu-id="d1840-107">Default transaction behavior</span></span>

<span data-ttu-id="d1840-108">Varsayılan olarak, veritabanı sağlayıcısı işlemleri, destekliyorsa tüm değişiklikler tek bir çağrı `SaveChanges()` bir işlem içinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d1840-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="d1840-109">Değişiklikleri başarısız olursa, işlem geri alındı ve değişikliklerin hiçbiri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d1840-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="d1840-110">Bunun anlamı `SaveChanges()` tamamen başarılı ya da bir hata oluşursa değiştirilmemiş veritabanını bırak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d1840-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="d1840-111">Çoğu uygulama için bu varsayılan davranış yeterli olur.</span><span class="sxs-lookup"><span data-stu-id="d1840-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="d1840-112">Uygulamanızın gereksinimleri gerekli bulduğunuz varsa işlemleri yalnızca el ile denetlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d1840-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="d1840-113">İşlemleri denetleme</span><span class="sxs-lookup"><span data-stu-id="d1840-113">Controlling transactions</span></span>

<span data-ttu-id="d1840-114">Kullanabileceğiniz `DbContext.Database` başlamak için Yürüt API ve geri alma işlemleri.</span><span class="sxs-lookup"><span data-stu-id="d1840-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="d1840-115">Aşağıdaki örnekte iki gösterir `SaveChanges()` işlemler ve bir LINQ Sorgu tek bir işlemde yürütülmekte.</span><span class="sxs-lookup"><span data-stu-id="d1840-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="d1840-116">Tüm veritabanı sağlayıcıları işlemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="d1840-116">Not all database providers support transactions.</span></span> <span data-ttu-id="d1840-117">Bazı sağlayıcılar atabilir veya no-op işlem API'leri çağrıldığında.</span><span class="sxs-lookup"><span data-stu-id="d1840-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="d1840-118">Çapraz bağlam işlemi (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="d1840-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="d1840-119">Bu gibi durumlarda, bir işlem aynı zamanda birden fazla bağlam örneklerinde paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1840-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="d1840-120">Bu işlevsellik yalnızca kullanımını gerektirdiğinden bir ilişkisel veritabanı sağlayıcısı kullanırken kullanılabilir `DbTransaction` ve `DbConnection`, ilişkisel veritabanları için belirli olduğu.</span><span class="sxs-lookup"><span data-stu-id="d1840-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="d1840-121">Bir işlem paylaşmak için bağlamları her ikisi de paylaşmalıdır bir `DbConnection` ve `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="d1840-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="d1840-122">Dışarıdan sağlanması bağlantıya izin ver</span><span class="sxs-lookup"><span data-stu-id="d1840-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="d1840-123">Paylaşımı bir `DbConnection` , oluşturma sırasında bir bağlamına bağlantı geçmesine becerisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d1840-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="d1840-124">İzin vermek için en kolay yolu `DbConnection` dışarıdan sağlanması için kullanarak durdurmaktır `DbContext.OnConfiguring` bağlam yapılandırmak ve harici olarak oluşturmak için yöntem `DbContextOptions` ve bağlam oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="d1840-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="d1840-125">`DbContextOptionsBuilder`içinde kullanılan API `DbContext.OnConfiguring` bağlam yapılandırmak için şimdi onu dışarıdan oluşturmak için kullanacaksanız `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="d1840-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

<span data-ttu-id="d1840-126">Kullanmaya devam etmek için bir alternatif olan `DbContext.OnConfiguring`, ancak kabul bir `DbConnection` kaydedilir ve ardından kullanılan `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="d1840-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="d1840-127">Paylaşım bağlantı ve işlem</span><span class="sxs-lookup"><span data-stu-id="d1840-127">Share connection and transaction</span></span>

<span data-ttu-id="d1840-128">Artık aynı bağlantıyı paylaşan birden fazla bağlam örneği oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1840-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="d1840-129">Ardından `DbContext.Database.UseTransaction(DbTransaction)` aynı işlem içinde her iki bağlamları listeleme API.</span><span class="sxs-lookup"><span data-stu-id="d1840-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

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

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="d1840-130">Dış DbTransactions (yalnızca ilişkisel veritabanları) kullanma</span><span class="sxs-lookup"><span data-stu-id="d1840-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="d1840-131">İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojileri kullanıyorsanız, bir işlem farklı teknolojiler tarafından gerçekleştirilen işlemler arasında paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1840-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="d1840-132">Aşağıdaki örnek, bir ADO.NET SqlClient işlem ve bir Entity Framework Çekirdek işlem aynı işlemde gerçekleştirme gösterir.</span><span class="sxs-lookup"><span data-stu-id="d1840-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
