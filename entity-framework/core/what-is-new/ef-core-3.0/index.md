---
title: Entity Framework Core 3.0'daki yeni özellikler - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebc676930ffc396aa70bb8afb91cf5a0cd43e04d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417944"
---
# <a name="new-features-in-entity-framework-core-30"></a><span data-ttu-id="1f8e2-102">Entity Framework Core 3.0'daki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="1f8e2-102">New features in Entity Framework Core 3.0</span></span>

<span data-ttu-id="1f8e2-103">Aşağıdaki liste, EF Core 3.0'daki başlıca yeni özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-103">The following list includes the major new features in EF Core 3.0.</span></span>

<span data-ttu-id="1f8e2-104">Önemli bir sürüm olarak, EF Core 3.0 da varolan uygulamalar üzerinde olumsuz etkisi olabilir API iyileştirmeleri olan birkaç [kesme değişiklikleri](xref:core/what-is-new/ef-core-3.0/breaking-changes)içerir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-104">As a major release, EF Core 3.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-overhaul"></a><span data-ttu-id="1f8e2-105">LINQ revizyonu</span><span class="sxs-lookup"><span data-stu-id="1f8e2-105">LINQ overhaul</span></span>

<span data-ttu-id="1f8e2-106">LINQ, intelliSense sunmak ve derleme zamanı türü denetimi sunmak için zengin tür bilgilerinden yararlanarak seçtiğiniz .NET dilini kullanarak veritabanı sorguları yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-106">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="1f8e2-107">Ancak LINQ, rasgele ifadeler (yöntem çağrıları veya işlemler) içeren sınırsız sayıda karmaşık sorgu yazmanızı da sağlar.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="1f8e2-108">Tüm bu kombinasyonların nasıl işleyeceğinilinq sağlayıcıları için en büyük zorluk.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-108">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="1f8e2-109">EF Core 3.0'da, DAHA fazla sorgu deseninin SQL'e çevrilmesini, daha fazla durumda verimli sorgular oluşturmasını ve verimsiz sorguların algılanmamasını önlemek için LINQ sağlayıcımızı yeniden yeniden biçimlendirdik.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-109">In EF Core 3.0, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="1f8e2-110">Yeni LINQ sağlayıcısı, mevcut uygulamaları ve veri sağlayıcılarını bozmadan gelecek sürümlerde yeni sorgu yetenekleri ve performans iyileştirmeleri sunabileceğimiz temeldir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-110">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="1f8e2-111">Kısıtlı istemci değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="1f8e2-111">Restricted client evaluation</span></span>

<span data-ttu-id="1f8e2-112">En önemli tasarım değişikliği, parametrelere dönüştürülemeyen veya SQL'e çevrilemeyen LINQ ifadelerini nasıl işlediğimizle ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-112">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="1f8e2-113">Önceki sürümlerde, EF Core sorgunun hangi bölümlerinin SQL'e çevrilebileceğini belirlemiş ve sorgunun geri kalanını istemciüzerinde yürütmüştür.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-113">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="1f8e2-114">İstemci tarafı yürütme bu tür bazı durumlarda arzu edilir, ancak diğer birçok durumda verimsiz sorguları neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-114">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="1f8e2-115">Örneğin, EF Core 2.2 bir `Where()` çağrıda bir yüklemi çeviremiyorsa, filtresiz bir SQL deyimi yürüttü, tüm satırları veritabanından aktardı ve sonra bunları bellekte filtreledi:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-115">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="1f8e2-116">Veritabanı az sayıda satır içeriyorsa, ancak veritabanı çok sayıda satır içeriyorsa önemli performans sorunlarına ve hatta uygulama hatasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-116">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number of rows.</span></span>

<span data-ttu-id="1f8e2-117">EF Core 3.0'da, istemci değerlendirmesini yalnızca üst düzey projeksiyonda (aslında son `Select()`çağrı) gerçekleşmesi için kısıtladık.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-117">In EF Core 3.0, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="1f8e2-118">EF Core 3.0 sorguda başka hiçbir yerde çevrilemez ifadeler algılar, bir çalışma zamanı özel durum atar.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-118">When EF Core 3.0 detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="1f8e2-119">Önceki örnekte olduğu gibi istemci üzerinde bir yüklem koşulunu değerlendirmek için, geliştiricilerin artık sorgunun değerlendirmesini linq'e nesnelere açıkça değiştirmeleri gerekir:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-119">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="1f8e2-120">Bunun varolan uygulamaları nasıl etkileyebileceği hakkında daha fazla ayrıntı için [sondaki değişiklikler belgelerine](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) bakın.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-120">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="1f8e2-121">LINQ sorgusu başına tek SQL deyimi</span><span class="sxs-lookup"><span data-stu-id="1f8e2-121">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="1f8e2-122">3.0'da önemli ölçüde değişen tasarımın bir diğer yönü de, artık LINQ sorgusu başına her zaman tek bir SQL deyimi oluşturmamızdır.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-122">Another aspect of the design that changed significantly in 3.0 is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="1f8e2-123">Önceki sürümlerde, belirli durumlarda birden çok SQL deyimi, koleksiyon gezinti özellikleri yle ilgili çevrilmiş `Include()` çağrılar ve alt sorgularla belirli desenleri izleyen çevrilmiş sorgular oluşturmak için kullanılırdık.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-123">In previous versions, we used to generate multiple SQL statements in certain cases, translated `Include()` calls on collection navigation properties and translated queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="1f8e2-124">Bu bazı durumlarda uygun olmasına `Include()` rağmen ve hatta tel üzerinden gereksiz veri göndermemek yardımcı oldu, uygulama karmaşık tı ve bazı son derece verimsiz davranışlar (N +1 sorguları) sonuçlandı.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-124">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, and it resulted in some extremely inefficient behaviors (N+1 queries).</span></span> <span data-ttu-id="1f8e2-125">Birden çok sorguda döndürülen verilerin tutarsız olabileceği durumlar vardı.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-125">There were situations in which the data returned across multiple queries was potentially inconsistent.</span></span>

<span data-ttu-id="1f8e2-126">İstemci değerlendirmesine benzer şekilde, EF Core 3.0 bir LINQ sorgusunu tek bir SQL deyimine çeviremezse, çalışma zamanı özel bir durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-126">Similarly to client evaluation, if EF Core 3.0 can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="1f8e2-127">Ancak EF Core'u, birden çok sorguyu JO'larla tek bir sorguya oluşturmak için kullanılan ortak desenlerin çoğunu çevirebilme özelliğine sahip kıldık.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-127">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="1f8e2-128">Cosmos DB desteği</span><span class="sxs-lookup"><span data-stu-id="1f8e2-128">Cosmos DB support</span></span>

<span data-ttu-id="1f8e2-129">EF Core için Cosmos DB sağlayıcısı, EF programlama modeline aşina olan geliştiricilerin Azure Cosmos DB'yi uygulama veritabanı olarak kolayca hedeflemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-129">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="1f8e2-130">Amaç, küresel dağıtım gibi Cosmos DB'nin bazı avantajlarını ,.her zaman açık, elastik ölçeklenebilirlik ve düşük gecikme süresini ,.NET geliştiricileri için daha da erişilebilir hale getirmektir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-130">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="1f8e2-131">Sağlayıcı, Cosmos DB'deki SQL API'ye karşı otomatik değişiklik izleme, LINQ ve değer dönüşümleri gibi çoğu EF Core özelliğine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-131">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="1f8e2-132">Daha fazla ayrıntı için [Cosmos DB sağlayıcı belgelerine](xref:core/providers/cosmos/index) bakın.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-132">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="1f8e2-133">C# 8.0 desteği</span><span class="sxs-lookup"><span data-stu-id="1f8e2-133">C# 8.0 support</span></span>

<span data-ttu-id="1f8e2-134">EF Core 3.0 C # [8.0 yeni özellikler](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)bir çift yararlanır:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-134">EF Core 3.0 takes advantage of a couple of the [new features in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="1f8e2-135">Asenkron akarsular</span><span class="sxs-lookup"><span data-stu-id="1f8e2-135">Asynchronous streams</span></span>

<span data-ttu-id="1f8e2-136">Asynchronous sorgu sonuçları artık yeni `IAsyncEnumerable<T>` standart arabirim kullanılarak `await foreach`açıklanır ve .</span><span class="sxs-lookup"><span data-stu-id="1f8e2-136">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

<span data-ttu-id="1f8e2-137">Daha fazla ayrıntı için [C# belgelerindeki eşzamanlı akışlara](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) bakın.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-137">See the [asynchronous streams in the C# documentation](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="1f8e2-138">Boş değer atanabilir başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="1f8e2-138">Nullable reference types</span></span>

<span data-ttu-id="1f8e2-139">Bu yeni özellik kodunuzda etkinleştirildiğinde, EF Core başvuru türü özelliklerinin nullable'ını inceler ve veritabanındaki ilgili sütunlara ve ilişkilere uygular: `[Required]` nullable olmayan başvuru türlerinin özellikleri, veri ek açıklama özelliğine sahipmiş gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-139">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="1f8e2-140">Örneğin, aşağıdaki sınıfta, tür `string?` olarak işaretlenmiş özellikler isteğe bağlı olarak `string` yapılandırılırken, gerektiği gibi yapılandırılacaktır:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-140">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

<span data-ttu-id="1f8e2-141">Daha fazla ayrıntı için EF Core belgelerinde [geçersiz başvuru türleri ile çalışma](xref:core/miscellaneous/nullable-reference-types) hakkında bilgi verilir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-141">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="1f8e2-142">Veritabanı işlemlerinin engellenmesi</span><span class="sxs-lookup"><span data-stu-id="1f8e2-142">Interception of database operations</span></span>

<span data-ttu-id="1f8e2-143">EF Core 3.0'daki yeni durdurma API'si, EF Core'un normal çalışmasının bir parçası olarak düşük düzeyli veritabanı işlemleri gerçekleştiğinde özel mantığın otomatik olarak çağrılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-143">The new interception API in EF Core 3.0 allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="1f8e2-144">Örneğin, bağlantıları açarken, hareketler işlerken veya komutları yürüterken.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-144">For example, when opening connections, committing transactions, or executing commands.</span></span>

<span data-ttu-id="1f8e2-145">EF 6'da bulunan durdurma özelliklerine benzer şekilde, durdurucular da operasyonlar gerçekleşmeden önce veya sonra durdurmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-145">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="1f8e2-146">Bunlar olmadan önce onları yakaladığınızda, yürütmeyi by-pass ve durdurma mantığından alternatif sonuçlar sağlamanıza izin verilir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-146">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span>

<span data-ttu-id="1f8e2-147">Örneğin, komut metnini işlemek için aşağıdakileri `IDbCommandInterceptor`oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-147">For example, to manipulate command text, you can create an `IDbCommandInterceptor`:</span></span>

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

<span data-ttu-id="1f8e2-148">Ve ile kaydedin: `DbContext`</span><span class="sxs-lookup"><span data-stu-id="1f8e2-148">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="1f8e2-149">Veritabanı görünümlerinin ters mühendislik</span><span class="sxs-lookup"><span data-stu-id="1f8e2-149">Reverse engineering of database views</span></span>

<span data-ttu-id="1f8e2-150">Veritabanından okunabilen ancak güncelleştirilmeyen verileri temsil eden sorgu türleri [anahtarsız varlık türlerine](xref:core/modeling/keyless-entity-types)yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-150">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span>
<span data-ttu-id="1f8e2-151">Çoğu senaryoda veritabanı görünümlerini eşleme için mükemmel bir uyum sağladıklarından, EF Core artık veritabanı görünümlerini tersine çevirdiğinde anahtarsız varlık türleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-151">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="1f8e2-152">Örneğin, [dotnet ef komut satırı aracını](xref:core/miscellaneous/cli/dotnet) kullanarak yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-152">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="1f8e2-153">Ve araç artık anahtarsız görünümler ve tablolar için otomatik olarak iskele tipleri olacaktır:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-153">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1f8e2-154">Tabloyu anaparayla paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="1f8e2-154">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="1f8e2-155">EF Core 3.0 ile `OrderDetails` başlayarak, `Order` sahip olunan veya açıkça aynı tabloya eşlenmişse, birincil anahtar hariç, bir `Order` özellik olmadan ve `OrderDetails` tüm `OrderDetails` özellikleri niçin geçersiz sütunlara eşlenecektir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-155">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="1f8e2-156">Sorgulanırken, EF Core `OrderDetails` `null` gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar dışında gerekli özellikleri yoksa `null`ve tüm özellikleri.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-156">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a><span data-ttu-id="1f8e2-157">EF 6.3 üzerinde .NET Core</span><span class="sxs-lookup"><span data-stu-id="1f8e2-157">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="1f8e2-158">Bu gerçekten bir EF Core 3.0 özelliği değil, ama biz mevcut müşterilerinin çoğu için önemli olduğunu düşünüyorum.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-158">This isn't really an EF Core 3.0 feature, but we think it is important to many of our current customers.</span></span>

<span data-ttu-id="1f8e2-159">Varolan birçok uygulamanın EF'nin önceki sürümlerini kullandığını ve bunları yalnızca .NET Core'dan yararlanmak için EF Core'a taşımanın önemli bir çaba gerektirebileceğini biliyoruz.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-159">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span>
<span data-ttu-id="1f8e2-160">Bu nedenle, .NET Core 3.0 üzerinde çalıştırmak için EF 6 en yeni sürümünü bağlantı noktasına karar verdi.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-160">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.0.</span></span>

<span data-ttu-id="1f8e2-161">Daha fazla bilgi [için, EF 6'daki yeniliklere](xref:ef6/what-is-new/index)bakın.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-161">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="1f8e2-162">Ertelenen özellikler</span><span class="sxs-lookup"><span data-stu-id="1f8e2-162">Postponed features</span></span>

<span data-ttu-id="1f8e2-163">Başlangıçta EF Core 3.0 için planlanan bazı özellikler gelecek sürümlere ertelendi:</span><span class="sxs-lookup"><span data-stu-id="1f8e2-163">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span>

- <span data-ttu-id="1f8e2-164">[Geçişlerde](https://github.com/aspnet/EntityFrameworkCore/issues/2725)bir modelin bölümlerini yok sayma yeteneği, #2725 olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="1f8e2-164">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="1f8e2-165">Mülkiyet çanta varlıklar, iki ayrı konu olarak izlenen: paylaşılan tür varlıklar hakkında [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) ve endeksli özellik haritalama desteği hakkında [#13610.](https://github.com/aspnet/EntityFrameworkCore/issues/13610)</span><span class="sxs-lookup"><span data-stu-id="1f8e2-165">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
