---
title: İşlemler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: ff12c4e7ace1f1b9e503cb2353bcdd53efd87cce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197901"
---
# <a name="using-transactions"></a><span data-ttu-id="9cb4f-102">İşlemleri Kullanma</span><span class="sxs-lookup"><span data-stu-id="9cb4f-102">Using Transactions</span></span>

<span data-ttu-id="9cb4f-103">İşlemler birkaç veritabanı işlemin atomik bir şekilde işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="9cb4f-104">İşlem yürütüolduysa, tüm işlemler veritabanına başarıyla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="9cb4f-105">İşlem geri alınırsa, hiçbir işlem veritabanına uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="9cb4f-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="9cb4f-107">Varsayılan işlem davranışı</span><span class="sxs-lookup"><span data-stu-id="9cb4f-107">Default transaction behavior</span></span>

<span data-ttu-id="9cb4f-108">Varsayılan olarak, veritabanı sağlayıcısı işlemleri destekliyorsa, tek bir çağrıdaki `SaveChanges()` tüm değişiklikler bir işlem içinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="9cb4f-109">Değişikliklerden herhangi biri başarısız olursa, işlem geri alınır ve veritabanına hiçbir değişiklik uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="9cb4f-110">Bu, tamamen `SaveChanges()` başarılı veya bir hata oluşursa veritabanını değiştirilmemiş olarak bırakma garantisi veren anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="9cb4f-111">Çoğu uygulama için bu varsayılan davranış yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="9cb4f-112">İşlemleri yalnızca, uygulama gereksinimleriniz gerekli değilse el ile kontrol etmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="9cb4f-113">İşlemleri denetleme</span><span class="sxs-lookup"><span data-stu-id="9cb4f-113">Controlling transactions</span></span>

<span data-ttu-id="9cb4f-114">İşlemleri başlatmak, yürütmek `DbContext.Database` ve geri almak için API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="9cb4f-115">Aşağıdaki örnekte, tek bir `SaveChanges()` işlemde yürütülen iki işlem ve bir LINQ sorgusu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="9cb4f-116">Tüm veritabanı sağlayıcıları işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-116">Not all database providers support transactions.</span></span> <span data-ttu-id="9cb4f-117">İşlem API 'Leri çağrıldığında bazı sağlayıcılar bu işlemleri atabilir veya gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="9cb4f-118">Çapraz bağlam işlemi (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="9cb4f-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="9cb4f-119">Ayrıca, bir işlemi birden çok bağlam örneği arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="9cb4f-120">Bu işlevsellik yalnızca, ilişkisel veritabanlarına özgü olan `DbTransaction` ve `DbConnection`kullanımını gerektirdiğinden ilişkisel bir veritabanı sağlayıcısı kullanılırken kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="9cb4f-121">Bir işlemi paylaşmak için bağlamların hem a `DbConnection` hem de bir `DbTransaction`paylaşılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="9cb4f-122">Bağlantının dışarıdan sağlanması için izin ver</span><span class="sxs-lookup"><span data-stu-id="9cb4f-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="9cb4f-123">' Nin `DbConnection` paylaşılması, bir bağlantıyı oluştururken bir bağlama geçirebilmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="9cb4f-124">Dışarıdan sağlanmanın en kolay `DbConnection` yolu, bağlamını yapılandırmak ve içeriği dışarıdan oluşturmak `DbContextOptions` ve bağlam `DbContext.OnConfiguring` oluşturucusuna geçirmek için yöntemini kullanmayı durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="9cb4f-125">`DbContextOptionsBuilder`, bağlamını yapılandırmak için ' de `DbContext.OnConfiguring` kullandığınız API 'dir, şimdi oluşturmak `DbContextOptions`için bunu dışarıdan kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="9cb4f-126">Diğer bir seçenek de kullanmaya `DbContext.OnConfiguring`devam etmesinin yanı sıra içinde `DbContext.OnConfiguring`kullanılan ve daha sonra kullanılan bir `DbConnection` .</span><span class="sxs-lookup"><span data-stu-id="9cb4f-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="9cb4f-127">Bağlantıyı ve işlemi paylaşma</span><span class="sxs-lookup"><span data-stu-id="9cb4f-127">Share connection and transaction</span></span>

<span data-ttu-id="9cb4f-128">Artık aynı bağlantıyı paylaşan birden çok bağlam örneği oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="9cb4f-129">Daha sonra aynı `DbContext.Database.UseTransaction(DbTransaction)` işlemde her iki bağlamı da listeleme için API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="9cb4f-130">Dış DbTransactions kullanma (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="9cb4f-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="9cb4f-131">İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojisi kullanıyorsanız, bu farklı teknolojiler tarafından gerçekleştirilen işlemler arasında bir işlem paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="9cb4f-132">Aşağıdaki örnek, aynı işlemde bir ADO.NET SqlClient işleminin ve Entity Framework Core işleminin nasıl gerçekleştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="9cb4f-133">System. Transactions kullanma</span><span class="sxs-lookup"><span data-stu-id="9cb4f-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="9cb4f-134">Bu özellik EF Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="9cb4f-135">Daha büyük bir kapsamda koordine etmeniz gerekiyorsa çevresel işlemler kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="9cb4f-136">Ayrıca, açık bir işlemde listeleme de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="9cb4f-137">System. Transactions sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="9cb4f-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="9cb4f-138">EF Core, System. Transactions desteğini uygulamak için veritabanı sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="9cb4f-139">Destek, .NET Framework için ADO.NET sağlayıcıları arasında oldukça yaygın olsa da, API yalnızca .NET Core 'a eklenmiştir ve bu nedenle destek yaygın olarak değildir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="9cb4f-140">Bir sağlayıcı System. Transactions için destek uygulamaz, bu API 'lere yapılan çağrılar tamamen yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="9cb4f-141">.NET Core için SqlClient, bunu 2,1 ve sonraki sürümlerde destekler.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="9cb4f-142">.NET Core 2,0 için SqlClient, özelliği kullanmayı denerseniz bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="9cb4f-143">İşlemleri yönetmek için kullanmadan önce API 'nin sağlayıcınızı doğru şekilde davrandığını test etmeniz önerilir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="9cb4f-144">Veri tabanı sağlayıcının bakımınonu ile iletişim kurmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="9cb4f-145">Sürüm 2,1 itibariyle, .NET Core 'daki System. Transactions uygulamasında dağıtılmış işlemler için destek yoktur, bu nedenle, birden çok kaynak yöneticisi arasında `TransactionScope` işlem `CommittableTransaction` koordine edilemez.</span><span class="sxs-lookup"><span data-stu-id="9cb4f-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
