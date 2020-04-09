---
title: İşlemler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417557"
---
# <a name="using-transactions"></a><span data-ttu-id="72602-102">İşlemleri Kullanma</span><span class="sxs-lookup"><span data-stu-id="72602-102">Using Transactions</span></span>

<span data-ttu-id="72602-103">İşlemler, çeşitli veritabanı işlemlerinin atomik bir şekilde işlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="72602-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="72602-104">Hareket işlenirse, tüm işlemler veritabanına başarıyla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="72602-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="72602-105">Hareket geri alınırsa, işlemlerin hiçbiri veritabanına uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="72602-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="72602-106">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72602-106">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="72602-107">Varsayılan hareket davranışı</span><span class="sxs-lookup"><span data-stu-id="72602-107">Default transaction behavior</span></span>

<span data-ttu-id="72602-108">Varsayılan olarak, veritabanı sağlayıcısı hareketleri destekliyorsa, tek `SaveChanges()` bir çağrıdaki tüm değişiklikler bir harekette uygulanır.</span><span class="sxs-lookup"><span data-stu-id="72602-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="72602-109">Değişikliklerden herhangi biri başarısız olursa, hareket geri alınır ve değişikliklerin hiçbiri veritabanına uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="72602-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="72602-110">Bu, `SaveChanges()` bir hata oluşursa veritabanının tamamen başarılı olması veya veritabanını değiştirilmeden bırakması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="72602-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="72602-111">Çoğu uygulama için bu varsayılan davranış yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="72602-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="72602-112">Yalnızca başvuru gereksinimleriniz gerekli yse hareketleri el ile denetlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="72602-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="72602-113">Hareketleri denetleme</span><span class="sxs-lookup"><span data-stu-id="72602-113">Controlling transactions</span></span>

<span data-ttu-id="72602-114">`DbContext.Database` Hareketleri başlatmak, işlemek ve geri almak için API'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72602-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="72602-115">Aşağıdaki örnekte, `SaveChanges()` iki işlem ve tek bir işlemde yürütülmekte olan bir LINQ sorgusu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="72602-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="72602-116">Tüm veritabanı sağlayıcıları hareketleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="72602-116">Not all database providers support transactions.</span></span> <span data-ttu-id="72602-117">Bazı sağlayıcılar, işlem API'leri çağrıldığında atabilir veya işlem olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="72602-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="72602-118">Bağlamlar arası işlem (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="72602-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="72602-119">Bir hareketi birden çok bağlam örnekleri arasında da paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72602-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="72602-120">Bu işlevsellik yalnızca ilişkisel veritabanlarının `DbTransaction` kullanımını ve `DbConnection`ilişkisel veritabanlarına özgü olmasını gerektirdiğinden ilişkisel bir veritabanı sağlayıcısı kullanırken kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="72602-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="72602-121">Bir hareketi paylaşmak için bağlamların hem `DbConnection` a `DbTransaction`hem de bir' yi paylaşması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72602-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="72602-122">Bağlantının dışarıdan sağlanmasına izin ver</span><span class="sxs-lookup"><span data-stu-id="72602-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="72602-123">Bir `DbConnection` bağlantıyı oluşturmak için bir bağlama geçiş yeteneği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="72602-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="72602-124">Dışarıdan sağlanmanın `DbConnection` en kolay yolu, bağlamı `DbContext.OnConfiguring` yapılandırmak ve bunları dışsal olarak `DbContextOptions` oluşturmak ve bağlam oluşturucuya aktarmak için yöntemi kullanmayı durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="72602-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="72602-125">`DbContextOptionsBuilder`bağlamı `DbContext.OnConfiguring` yapılandırmak için kullandığınız API'dir, şimdi oluşturmak `DbContextOptions`için harici olarak kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="72602-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="72602-126">Alternatif olarak kullanmaya `DbContext.OnConfiguring`devam etmek, `DbConnection` ancak kaydedilmiş ve `DbContext.OnConfiguring`daha sonra kullanılan bir kabul etmektir.</span><span class="sxs-lookup"><span data-stu-id="72602-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="72602-127">Bağlantı ve hareketi paylaşma</span><span class="sxs-lookup"><span data-stu-id="72602-127">Share connection and transaction</span></span>

<span data-ttu-id="72602-128">Artık aynı bağlantıyı paylaşan birden çok bağlam örneği oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72602-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="72602-129">`DbContext.Database.UseTransaction(DbTransaction)` Ardından, her iki bağlamı da aynı işlemde kullanmak için API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="72602-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="72602-130">Harici DbTransactions kullanma (yalnızca ilişkisel veritabanları)</span><span class="sxs-lookup"><span data-stu-id="72602-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="72602-131">İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojisi kullanıyorsanız, bu farklı teknolojiler tarafından gerçekleştirilen işlemler arasında bir işlem paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72602-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="72602-132">Aşağıdaki örnek, aynı işlemde bir sqlclient işlemi ve bir Entity Framework Core işlemi ADO.NET nasıl gerçekleştirilini gösterir.</span><span class="sxs-lookup"><span data-stu-id="72602-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="72602-133">System.Transactions'ı Kullanma</span><span class="sxs-lookup"><span data-stu-id="72602-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="72602-134">Bu özellik EF Core 2.1'de yenidir.</span><span class="sxs-lookup"><span data-stu-id="72602-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="72602-135">Daha geniş bir kapsamda koordine olmanız gerekiyorsa ortam hareketlerini kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="72602-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="72602-136">Açık bir işlem için kaydolmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="72602-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="72602-137">System.Transactions Sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="72602-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="72602-138">EF Core, System.Transactions desteğini uygulamak için veritabanı sağlayıcılarına güvenir.</span><span class="sxs-lookup"><span data-stu-id="72602-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="72602-139">Destek .NET Framework için ADO.NET sağlayıcılar arasında oldukça yaygın olmasına rağmen, API sadece son zamanlarda .NET Core eklenmiştir ve bu nedenle destek kadar yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="72602-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="72602-140">Bir sağlayıcı System.Transactions için destek uygulamazsa, bu API'lere yapılan çağrıların tamamen yoksayılması mümkündür.</span><span class="sxs-lookup"><span data-stu-id="72602-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="72602-141">.NET Core için SqlClient 2.1'den itibaren destekler.</span><span class="sxs-lookup"><span data-stu-id="72602-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="72602-142">.NET Core 2.0 için SqlClient özelliğini kullanmaya çalışırsanız bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="72602-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span>

   > [!IMPORTANT]  
   > <span data-ttu-id="72602-143">Hareketleri yönetmek için güvenmeden önce API'nin sağlayıcınıza doğru şekilde hareket ettiğini test etmek önerilir.</span><span class="sxs-lookup"><span data-stu-id="72602-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="72602-144">Yoksa veritabanı sağlayıcısının bakıcısına başvurmanız tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="72602-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="72602-145">Sürüm 2.1 itibariyle, .NET Core'daki System.Transactions uygulaması dağıtılmış hareketler için `TransactionScope` `CommittableTransaction` destek içermez, bu nedenle birden çok kaynak yöneticisi arasında hareketleri kullanamaz veya koordine edemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="72602-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
