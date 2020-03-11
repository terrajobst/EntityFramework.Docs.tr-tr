---
title: İşlemler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417557"
---
# <a name="using-transactions"></a><span data-ttu-id="e5428-102">İşlemleri Kullanma</span><span class="sxs-lookup"><span data-stu-id="e5428-102">Using Transactions</span></span>

<span data-ttu-id="e5428-103">İşlemler birkaç veritabanı işlemin atomik bir şekilde işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="e5428-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="e5428-104">İşlem yürütüolduysa, tüm işlemler veritabanına başarıyla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e5428-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="e5428-105">İşlem geri alınırsa, hiçbir işlem veritabanına uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="e5428-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="e5428-106">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5428-106">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="e5428-107">Varsayılan işlem davranışı</span><span class="sxs-lookup"><span data-stu-id="e5428-107">Default transaction behavior</span></span>

<span data-ttu-id="e5428-108">Varsayılan olarak, veritabanı sağlayıcısı işlemleri destekliyorsa, tek bir `SaveChanges()` çağrısıyla yapılan tüm değişiklikler bir işlem içinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e5428-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="e5428-109">Değişikliklerden herhangi biri başarısız olursa, işlem geri alınır ve veritabanına hiçbir değişiklik uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="e5428-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="e5428-110">Bu, `SaveChanges()` tamamen başarılı veya bir hata oluşursa veritabanını değiştirilmemiş olarak bırakma işleminin garanti olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e5428-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="e5428-111">Çoğu uygulama için bu varsayılan davranış yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="e5428-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="e5428-112">İşlemleri yalnızca, uygulama gereksinimleriniz gerekli değilse el ile kontrol etmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="e5428-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="e5428-113">İşlemleri denetleme</span><span class="sxs-lookup"><span data-stu-id="e5428-113">Controlling transactions</span></span>

<span data-ttu-id="e5428-114">İşlemleri başlatmak, yürütmek ve geri almak için `DbContext.Database` API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5428-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="e5428-115">Aşağıdaki örnekte, tek bir işlemde yürütülen iki `SaveChanges()` işlemi ve bir LINQ sorgusu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e5428-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="e5428-116">Tüm veritabanı sağlayıcıları işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="e5428-116">Not all database providers support transactions.</span></span> <span data-ttu-id="e5428-117">İşlem API 'Leri çağrıldığında bazı sağlayıcılar bu işlemleri atabilir veya gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="e5428-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="e5428-118">Çapraz bağlam işlemi (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="e5428-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="e5428-119">Ayrıca, bir işlemi birden çok bağlam örneği arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5428-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="e5428-120">Bu işlevsellik yalnızca, ilişkisel veritabanlarına özgü `DbTransaction` ve `DbConnection`kullanımını gerektirdiğinden ilişkisel bir veritabanı sağlayıcısı kullanılırken kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5428-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="e5428-121">Bir işlemi paylaşmak için bağlamlar hem `DbConnection` hem de bir `DbTransaction`paylaşmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e5428-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="e5428-122">Bağlantının dışarıdan sağlanması için izin ver</span><span class="sxs-lookup"><span data-stu-id="e5428-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="e5428-123">Bir `DbConnection` paylaşmak, bir bağlantıyı oluştururken bir bağlama geçirebilmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e5428-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="e5428-124">`DbConnection` dışarıdan sağlanmak için en kolay yol, bağlamı yapılandırmak ve `DbContextOptions` dışarıdan oluşturmak ve onları bağlam oluşturucusuna geçirmek için `DbContext.OnConfiguring` metodunu kullanmayı durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="e5428-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="e5428-125">`DbContextOptionsBuilder`, bağlamı yapılandırmak için `DbContext.OnConfiguring` kullandığınız API 'dir, artık `DbContextOptions`oluşturmak için bunu dışarıdan kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="e5428-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="e5428-126">Diğer bir seçenek de `DbContext.OnConfiguring`kullanmaya devam etmesinin yanı sıra, kaydedilen ve daha sonra `DbContext.OnConfiguring`kullanılan bir `DbConnection` kabul etmelidir.</span><span class="sxs-lookup"><span data-stu-id="e5428-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="e5428-127">Bağlantıyı ve işlemi paylaşma</span><span class="sxs-lookup"><span data-stu-id="e5428-127">Share connection and transaction</span></span>

<span data-ttu-id="e5428-128">Artık aynı bağlantıyı paylaşan birden çok bağlam örneği oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5428-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="e5428-129">Daha sonra aynı işlemde her iki bağlamı da listeleme `DbContext.Database.UseTransaction(DbTransaction)` API 'sini kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5428-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="e5428-130">Dış DbTransactions kullanma (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="e5428-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="e5428-131">İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojisi kullanıyorsanız, bu farklı teknolojiler tarafından gerçekleştirilen işlemler arasında bir işlem paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5428-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="e5428-132">Aşağıdaki örnek, aynı işlemde bir ADO.NET SqlClient işleminin ve Entity Framework Core işleminin nasıl gerçekleştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e5428-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="e5428-133">System. Transactions kullanma</span><span class="sxs-lookup"><span data-stu-id="e5428-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="e5428-134">Bu özellik EF Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="e5428-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e5428-135">Daha büyük bir kapsamda koordine etmeniz gerekiyorsa çevresel işlemler kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e5428-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="e5428-136">Ayrıca, açık bir işlemde listeleme de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e5428-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="e5428-137">System. Transactions sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="e5428-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="e5428-138">EF Core, System. Transactions desteğini uygulamak için veritabanı sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5428-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="e5428-139">Destek, .NET Framework için ADO.NET sağlayıcıları arasında oldukça yaygın olsa da, API yalnızca .NET Core 'a eklenmiştir ve bu nedenle destek yaygın olarak değildir.</span><span class="sxs-lookup"><span data-stu-id="e5428-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="e5428-140">Bir sağlayıcı System. Transactions için destek uygulamaz, bu API 'lere yapılan çağrılar tamamen yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="e5428-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="e5428-141">.NET Core için SqlClient, bunu 2,1 ve sonraki sürümlerde destekler.</span><span class="sxs-lookup"><span data-stu-id="e5428-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="e5428-142">.NET Core 2,0 için SqlClient, özelliği kullanmayı denerseniz bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5428-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span>

   > [!IMPORTANT]  
   > <span data-ttu-id="e5428-143">İşlemleri yönetmek için kullanmadan önce API 'nin sağlayıcınızı doğru şekilde davrandığını test etmeniz önerilir.</span><span class="sxs-lookup"><span data-stu-id="e5428-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="e5428-144">Veri tabanı sağlayıcının bakımınonu ile iletişim kurmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="e5428-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="e5428-145">Sürüm 2,1 itibariyle, .NET Core 'daki System. Transactions uygulamasında dağıtılmış işlemler için destek yoktur; bu nedenle, birden çok kaynak yöneticisi arasında işlem koordine etmek için `TransactionScope` veya `CommittableTransaction` kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="e5428-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
