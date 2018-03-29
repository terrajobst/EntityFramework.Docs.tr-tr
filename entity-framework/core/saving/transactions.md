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
# <a name="using-transactions"></a><span data-ttu-id="66fac-102">İşlemleri kullanma</span><span class="sxs-lookup"><span data-stu-id="66fac-102">Using Transactions</span></span>

<span data-ttu-id="66fac-103">İşlemler birkaç veritabanı işlemlerinin atomik bir şekilde işlenmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="66fac-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="66fac-104">İşlem, tüm işlemlerin başarılı bir şekilde veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="66fac-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="66fac-105">İşlem geri alındı, işlemleri hiçbiri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="66fac-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="66fac-106">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) github'da.</span><span class="sxs-lookup"><span data-stu-id="66fac-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="66fac-107">Varsayılan işlem davranışı</span><span class="sxs-lookup"><span data-stu-id="66fac-107">Default transaction behavior</span></span>

<span data-ttu-id="66fac-108">Varsayılan olarak, veritabanı sağlayıcısı işlemleri, destekliyorsa tüm değişiklikler tek bir çağrı `SaveChanges()` bir işlem içinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="66fac-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="66fac-109">Değişiklikleri başarısız olursa, işlem geri alındı ve değişikliklerin hiçbiri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="66fac-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="66fac-110">Bunun anlamı `SaveChanges()` tamamen başarılı ya da bir hata oluşursa değiştirilmemiş veritabanını bırak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="66fac-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="66fac-111">Çoğu uygulama için bu varsayılan davranış yeterli olur.</span><span class="sxs-lookup"><span data-stu-id="66fac-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="66fac-112">Uygulamanızın gereksinimleri gerekli bulduğunuz varsa işlemleri yalnızca el ile denetlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="66fac-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="66fac-113">İşlemleri denetleme</span><span class="sxs-lookup"><span data-stu-id="66fac-113">Controlling transactions</span></span>

<span data-ttu-id="66fac-114">Kullanabileceğiniz `DbContext.Database` başlamak için Yürüt API ve geri alma işlemleri.</span><span class="sxs-lookup"><span data-stu-id="66fac-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="66fac-115">Aşağıdaki örnekte iki gösterir `SaveChanges()` işlemler ve bir LINQ Sorgu tek bir işlemde yürütülmekte.</span><span class="sxs-lookup"><span data-stu-id="66fac-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="66fac-116">Tüm veritabanı sağlayıcıları işlemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="66fac-116">Not all database providers support transactions.</span></span> <span data-ttu-id="66fac-117">Bazı sağlayıcılar atabilir veya no-op işlem API'leri çağrıldığında.</span><span class="sxs-lookup"><span data-stu-id="66fac-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="66fac-118">Çapraz bağlam işlemi (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="66fac-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="66fac-119">Bu gibi durumlarda, bir işlem aynı zamanda birden fazla bağlam örneklerinde paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66fac-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="66fac-120">Bu işlevsellik yalnızca kullanımını gerektirdiğinden bir ilişkisel veritabanı sağlayıcısı kullanırken kullanılabilir `DbTransaction` ve `DbConnection`, ilişkisel veritabanları için belirli olduğu.</span><span class="sxs-lookup"><span data-stu-id="66fac-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="66fac-121">Bir işlem paylaşmak için bağlamları her ikisi de paylaşmalıdır bir `DbConnection` ve `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="66fac-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="66fac-122">Dışarıdan sağlanması bağlantıya izin ver</span><span class="sxs-lookup"><span data-stu-id="66fac-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="66fac-123">Paylaşımı bir `DbConnection` , oluşturma sırasında bir bağlamına bağlantı geçmesine becerisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="66fac-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="66fac-124">İzin vermek için en kolay yolu `DbConnection` dışarıdan sağlanması için kullanarak durdurmaktır `DbContext.OnConfiguring` bağlam yapılandırmak ve harici olarak oluşturmak için yöntem `DbContextOptions` ve bağlam oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="66fac-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="66fac-125">`DbContextOptionsBuilder` içinde kullanılan API `DbContext.OnConfiguring` bağlam yapılandırmak için şimdi onu dışarıdan oluşturmak için kullanacaksanız `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="66fac-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="66fac-126">Kullanmaya devam etmek için bir alternatif olan `DbContext.OnConfiguring`, ancak kabul bir `DbConnection` kaydedilir ve ardından kullanılan `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="66fac-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="66fac-127">Paylaşım bağlantı ve işlem</span><span class="sxs-lookup"><span data-stu-id="66fac-127">Share connection and transaction</span></span>

<span data-ttu-id="66fac-128">Artık aynı bağlantıyı paylaşan birden fazla bağlam örneği oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66fac-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="66fac-129">Ardından `DbContext.Database.UseTransaction(DbTransaction)` aynı işlem içinde her iki bağlamları listeleme API.</span><span class="sxs-lookup"><span data-stu-id="66fac-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="66fac-130">Dış DbTransactions (yalnızca ilişkisel veritabanları) kullanma</span><span class="sxs-lookup"><span data-stu-id="66fac-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="66fac-131">İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojileri kullanıyorsanız, bir işlem farklı teknolojiler tarafından gerçekleştirilen işlemler arasında paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66fac-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="66fac-132">Aşağıdaki örnek, bir ADO.NET SqlClient işlem ve bir Entity Framework Çekirdek işlem aynı işlemde gerçekleştirme gösterir.</span><span class="sxs-lookup"><span data-stu-id="66fac-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="66fac-133">System.Transactions kullanma</span><span class="sxs-lookup"><span data-stu-id="66fac-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="66fac-134">Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="66fac-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="66fac-135">Ortam işlemleri arasında daha büyük bir kapsama koordine etmek gerekiyorsa kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="66fac-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="66fac-136">Bir açık işleminde listelenmeyi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="66fac-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="66fac-137">System.Transactions sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="66fac-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="66fac-138">EF çekirdek System.Transactions desteği uygulamak için veritabanı sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="66fac-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="66fac-139">ADO.NET sağlayıcıda .NET Framework, API yalnızca en son .NET Core eklenmiş durumda ve bu nedenle desteklemek için destek oldukça yaygındır rağmen olduğu olmaması gibi yaygın.</span><span class="sxs-lookup"><span data-stu-id="66fac-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="66fac-140">Bir sağlayıcı System.Transactions desteği uygulamaz varsa bu API çağrıları tam yoksayılmasına mümkündür.</span><span class="sxs-lookup"><span data-stu-id="66fac-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="66fac-141">.NET Core SqlClient başlayarak 2.1 desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="66fac-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="66fac-142">SqlClient .NET Core 2.0 için bir özel durum oluşturur, özelliğini kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="66fac-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="66fac-143">Buna işlemleri yönetmek için bağlı önce API sağlayıcınız ile düzgün şekilde davranan test önerilir.</span><span class="sxs-lookup"><span data-stu-id="66fac-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="66fac-144">Mevcut değilse veritabanı sağlayıcısı Bakımcı başvurmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="66fac-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="66fac-145">Sürüm 2.1 itibariyle .NET Core System.Transactions uygulamasında dağıtılmış işlem desteği dahil değildir; bu nedenle kullanamazsınız `TransactionScope` veya `CommitableTransaction` arasında birden çok kaynak yöneticileri işlemleri koordine etmek için.</span><span class="sxs-lookup"><span data-stu-id="66fac-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
