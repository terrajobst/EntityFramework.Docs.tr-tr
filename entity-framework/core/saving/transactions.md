---
title: İşlemler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 7083a1228420416a1b60d9744ca2dad2339be53f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993611"
---
# <a name="using-transactions"></a><span data-ttu-id="e0f04-102">İşlemleri kullanma</span><span class="sxs-lookup"><span data-stu-id="e0f04-102">Using Transactions</span></span>

<span data-ttu-id="e0f04-103">İşlemler atomik bir şekilde işlenecek çeşitli veritabanı işlemlerini izin verin.</span><span class="sxs-lookup"><span data-stu-id="e0f04-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="e0f04-104">İşlem, varsa, tüm işlemleri veritabanına başarıyla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e0f04-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="e0f04-105">İşlem geri alınır, hiçbiri işlemleri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e0f04-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="e0f04-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="e0f04-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="e0f04-107">Varsayılan işlem davranışı</span><span class="sxs-lookup"><span data-stu-id="e0f04-107">Default transaction behavior</span></span>

<span data-ttu-id="e0f04-108">Varsayılan olarak, veritabanı sağlayıcısı işlemleri, destekliyorsa yapılan tüm değişiklikler tek bir çağrıda `SaveChanges()` bir işlemde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e0f04-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="e0f04-109">Değişikliklerden herhangi birini başarısız olursa, işlem geri alınır ve değişikliklerin hiçbiri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e0f04-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="e0f04-110">Diğer bir deyişle `SaveChanges()` tamamen başarılı ya da bir hata oluşursa, üzerinde değişiklik yapılmadan veritabanını bırak garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="e0f04-111">Bu varsayılan davranışı, çoğu uygulama için yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="e0f04-112">Uygulama gereksinimlerinize gerekli bulduğunuz, yalnızca el ile işlem denetlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="e0f04-113">İşlemleri denetleme</span><span class="sxs-lookup"><span data-stu-id="e0f04-113">Controlling transactions</span></span>

<span data-ttu-id="e0f04-114">Kullanabileceğiniz `DbContext.Database` işleme başlamak için API ve geri alma işlemleri.</span><span class="sxs-lookup"><span data-stu-id="e0f04-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="e0f04-115">Aşağıdaki örnek iki gösterir `SaveChanges()` işlemler ve bir LINQ Sorgu tek bir işlemde yürütülmekte.</span><span class="sxs-lookup"><span data-stu-id="e0f04-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="e0f04-116">Veritabanı sağlayıcıları tüm işlemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="e0f04-116">Not all database providers support transactions.</span></span> <span data-ttu-id="e0f04-117">Bazı sağlayıcıları atabilir veya İşlemsiz işlem API'ler çağrıldığında kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="e0f04-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="e0f04-118">Bağlam içi işlem (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="e0f04-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="e0f04-119">Ayrıca, bir işlem birden çok içerik örneğine paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0f04-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="e0f04-120">Bu işlev yalnızca kullanımı gerektirdiğinden, ilişkisel veritabanı sağlayıcısı kullanırken kullanılabilir `DbTransaction` ve `DbConnection`, ilişkisel veritabanlarına belirli olduğu.</span><span class="sxs-lookup"><span data-stu-id="e0f04-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="e0f04-121">Bir işlem paylaşmak için bağlamı hem de paylaşmanız gerekir bir `DbConnection` ve `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="e0f04-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="e0f04-122">Harici olarak sağlanan bağlantıya izin ver</span><span class="sxs-lookup"><span data-stu-id="e0f04-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="e0f04-123">Paylaşımı bir `DbConnection` oluştururken bu bağlantı bir bağlamına geçirme özelliği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="e0f04-124">İzin vermek için en kolay yolu `DbConnection` durdurmak için dışarıdan sağlanması için olan `DbContext.OnConfiguring` bağlam yapılandırmak ve harici olarak oluşturmak için gereken yöntemini `DbContextOptions` ve bunları bağlam oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="e0f04-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="e0f04-125">`DbContextOptionsBuilder` içinde kullanılan API `DbContext.OnConfiguring` bağlam yapılandırmak için artık bunu harici olarak oluşturmak için kullanacaksanız `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="e0f04-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="e0f04-126">Kullanmaya devam etmek için bir alternatifidir `DbContext.OnConfiguring`, ancak kabul bir `DbConnection` kaydedildi ve kullanılan `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="e0f04-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="e0f04-127">Paylaşım bağlantı ve işlem</span><span class="sxs-lookup"><span data-stu-id="e0f04-127">Share connection and transaction</span></span>

<span data-ttu-id="e0f04-128">Artık aynı bağlantıyı paylaşan birden çok içerik örneği oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0f04-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="e0f04-129">Ardından `DbContext.Database.UseTransaction(DbTransaction)` kaydolunamadı aynı işlem içinde her iki bağlamları API.</span><span class="sxs-lookup"><span data-stu-id="e0f04-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="e0f04-130">Dış DbTransactions (yalnızca ilişkisel veritabanları) kullanma</span><span class="sxs-lookup"><span data-stu-id="e0f04-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="e0f04-131">İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojileri kullanıyorsanız, bir işlem bu farklı teknolojiler tarafından gerçekleştirilen işlemler arasında paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0f04-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="e0f04-132">Aşağıdaki örnek, aynı işlemde bir ADO.NET SqlClient işlem ve Entity Framework Core işlemi gerçekleştirmek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="e0f04-133">System.Transactions kullanma</span><span class="sxs-lookup"><span data-stu-id="e0f04-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="e0f04-134">Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e0f04-135">Daha büyük bir kapsama arasında koordine etmeniz gerekiyorsa, ortam işlemleri kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e0f04-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="e0f04-136">Açık bir işleme mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e0f04-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="e0f04-137">System.Transactions sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="e0f04-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="e0f04-138">EF Core System.Transactions için destek uygulamak için veritabanı sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="e0f04-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="e0f04-139">Destek .NET Framework için ADO.NET sağlayıcıları arasında oldukça sık kullanılan olsa da, API .NET Core için yalnızca bir son eklendi ve bu nedenle desteği gibi yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="e0f04-140">Bir sağlayıcı System.Transactions desteği uygulamaz, bu API'lere giden çağrıların tamamen yoksayılacak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e0f04-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="e0f04-141">.NET Core için SqlClient 2.1 ve üzeri desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="e0f04-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="e0f04-142">.NET Core 2.0 için SqlClient bir özel durum oluşturur, bu özellik kullanmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="e0f04-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="e0f04-143">Üzerinde işlemleri yönetmek için kullanan önce API sağlayıcınız ile düzgün şekilde davranan test önerilir.</span><span class="sxs-lookup"><span data-stu-id="e0f04-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="e0f04-144">Veritabanı sağlayıcısı Bakımcı kullanmıyorsa başvurmaları.</span><span class="sxs-lookup"><span data-stu-id="e0f04-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="e0f04-145">2.1 sürümü itibarıyla, .NET Core System.Transactions uygulamasında dağıtılmış işlemler için destek içermez; bu nedenle kullanamazsınız `TransactionScope` veya `CommittableTransaction` işlemler arasında birden çok kaynak yöneticileri koordine etmek için.</span><span class="sxs-lookup"><span data-stu-id="e0f04-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
