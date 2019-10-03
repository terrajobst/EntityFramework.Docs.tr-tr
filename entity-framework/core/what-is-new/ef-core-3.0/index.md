---
title: Entity Framework Core 3,0 ' deki yeni özellikler
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ce53d0fa21acfd52dc5e8b37911202cab02f00c8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813472"
---
# <a name="new-features-in-entity-framework-core-30"></a><span data-ttu-id="4643e-102">Entity Framework Core 3,0 ' deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="4643e-102">New features in Entity Framework Core 3.0</span></span>

<span data-ttu-id="4643e-103">Aşağıdaki listede EF Core 3,0 ' deki başlıca yeni özellikler yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="4643e-103">The following list includes the major new features in EF Core 3.0.</span></span>

<span data-ttu-id="4643e-104">Büyük bir sürüm olarak EF Core 3,0, mevcut uygulamalar üzerinde olumsuz etkileri olabilecek API geliştirmeleri olan çok sayıda [Son değişiklik](xref:core/what-is-new/ef-core-3.0/breaking-changes)de içerir.</span><span class="sxs-lookup"><span data-stu-id="4643e-104">As a major release, EF Core 3.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-overhaul"></a><span data-ttu-id="4643e-105">LINQ fazla mesafe</span><span class="sxs-lookup"><span data-stu-id="4643e-105">LINQ overhaul</span></span> 

<span data-ttu-id="4643e-106">LINQ, IntelliSense ve derleme zamanı tür denetimi sunmak için zengin tür bilgilerinin avantajlarından yararlanarak tercih ettiğiniz .NET dilini kullanarak veritabanı sorguları yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4643e-106">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="4643e-107">Ancak LINQ, rastgele ifadeler (Yöntem çağrıları veya işlemler) içeren sınırsız sayıda karmaşık sorgu yazmanızı de sağlar.</span><span class="sxs-lookup"><span data-stu-id="4643e-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="4643e-108">Bu kombinasyonların nasıl işleneceği, LINQ sağlayıcıları için Ana zorluk dır.</span><span class="sxs-lookup"><span data-stu-id="4643e-108">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="4643e-109">EF Core 3,0 ' de, LINQ sağlayıcımız, daha fazla sorgu desenini SQL 'e çevirmeyi, daha fazla durumda verimli sorgular oluşturmayı ve verimsiz sorguların algılanışlanmasını engellemeyi sağlayan bir şekilde yeniden tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4643e-109">In EF Core 3.0, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="4643e-110">Yeni LINQ sağlayıcısı, mevcut uygulamaları ve veri sağlayıcılarını bozmadan gelecek sürümlerde yeni sorgu özellikleri ve performans iyileştirmeleri sunabilediğimiz temel bir temelidir.</span><span class="sxs-lookup"><span data-stu-id="4643e-110">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="4643e-111">Kısıtlanmış istemci değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="4643e-111">Restricted client evaluation</span></span>
<span data-ttu-id="4643e-112">En önemli tasarım değişikliği, parametrelere dönüştürülemeyen veya SQL 'e çevrilemeyen LINQ ifadelerini nasıl işleytiğimiz ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4643e-112">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="4643e-113">Önceki sürümlerde, bir sorgunun hangi bölümlerinin SQL 'e çevrilebileceğini ve bu sorgunun geri kalanını istemcide yürütüldüğünü EF Core.</span><span class="sxs-lookup"><span data-stu-id="4643e-113">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="4643e-114">Bu tür istemci tarafı yürütme, bazı durumlarda istenebilir, ancak çoğu durumda verimsiz sorgulara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="4643e-114">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="4643e-115">Örneğin, EF Core 2,2 bir `Where()` çağrı içindeki bir koşulu çeviremediğinden, filtre olmadan bir SQL ifadesini yürütür, veritabanından tüm satırları aktardı ve sonra bunları bellek içinde filtreledi:</span><span class="sxs-lookup"><span data-stu-id="4643e-115">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = 
  context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="4643e-116">Bu, veritabanı az sayıda satır içeriyorsa ancak önemli performans sorunlarına yol açabilir, hatta veritabanı büyük bir sayı veya satır içeriyorsa uygulama başarısızlığından kaynaklanabilir.</span><span class="sxs-lookup"><span data-stu-id="4643e-116">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>

<span data-ttu-id="4643e-117">EF Core 3,0 ' de, istemci değerlendirmesinin yalnızca en üst düzey projeksiyde (temelde, son çağrı `Select()`) gerçekleşmesini kısıtlarız.</span><span class="sxs-lookup"><span data-stu-id="4643e-117">In EF Core 3.0, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="4643e-118">EF Core 3,0, sorguda başka herhangi bir yere çevrilemeyen ifadeler algıladığında, çalışma zamanı özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4643e-118">When EF Core 3.0 detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="4643e-119">Önceki örnekte olduğu gibi, istemci üzerindeki bir koşul koşulunu değerlendirmek için, geliştiricilerin artık sorgunun değerlendirmesini LINQ to Objects için açıkça geçiş yapması gerekir:</span><span class="sxs-lookup"><span data-stu-id="4643e-119">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span> 

``` csharp
var specialCustomers =
  context.Customers
    .Where(c => c.Name.StartsWith(n)) 
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="4643e-120">Bunun var olan uygulamaları nasıl etkileyebileceği hakkında daha fazla ayrıntı için bkz. [son değişiklikler belgeleri](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) .</span><span class="sxs-lookup"><span data-stu-id="4643e-120">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="4643e-121">LINQ sorgusu başına tek SQL ekstresi</span><span class="sxs-lookup"><span data-stu-id="4643e-121">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="4643e-122">Tasarımın 3,0 ' de önemli ölçüde değiştiği başka bir yönü de her LINQ sorgusu için her zaman tek bir SQL ekstresi oluşturmamız.</span><span class="sxs-lookup"><span data-stu-id="4643e-122">Another aspect of the design that changed significantly in 3.0 is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="4643e-123">Önceki sürümlerde, bazı durumlarda birden çok SQL deyimi oluşturmak için kullanıldığımız gibi, koleksiyon gezinti özelliklerine `Include()` yapılan çağrıları çevirmek ve belirli desenleri izleyen sorguları alt sorgular halinde çevirmek gibi.</span><span class="sxs-lookup"><span data-stu-id="4643e-123">In previous versions, we used to generate multiple SQL statements in certain cases, like to translate `Include()` calls on collection navigation properties and to translate queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="4643e-124">Bu, bazı durumlarda kullanışlı olsa da, `Include()` hatta yedekli verileri kablo üzerinden göndermekten kaçınmaya yardımcı olsa da, uygulama karmaşıktır, son derece verimsiz davranışlar (N + 1 sorgu) ile sonuçlanır ve birden çok sorgu arasında döndürülen veriler tutarsız olabilir.</span><span class="sxs-lookup"><span data-stu-id="4643e-124">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, it resulted in some extremely inefficient behaviors (N+1 queries), and there was situations in which the data returned across multiple queries could be inconsistent.</span></span>

<span data-ttu-id="4643e-125">İstemci değerlendirmesine benzer şekilde, EF Core 3,0 bir LINQ sorgusunu tek bir SQL ifadesine çeviremiyorsa, çalışma zamanı özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4643e-125">Similarly to client evaluation, if EF Core 3.0 can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="4643e-126">Ancak birleşimli tek bir sorgu için birden çok sorgu oluşturmak için kullanılan yaygın desenlerin çoğunu çevirdiğimiz EF Core yaptık.</span><span class="sxs-lookup"><span data-stu-id="4643e-126">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="4643e-127">Cosmos DB desteği</span><span class="sxs-lookup"><span data-stu-id="4643e-127">Cosmos DB support</span></span> 

<span data-ttu-id="4643e-128">EF Core için Cosmos DB sağlayıcısı, EF programlama modeliyle tanıdık geliştiricilerin, uygulama veritabanı olarak Azure Cosmos DB kolayca hedeflemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4643e-128">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="4643e-129">Amaç, küresel dağıtım, "her zaman açık" kullanılabilirlik, elastik ölçeklenebilirlik ve düşük gecikme süresi gibi Cosmos DB avantajlarından bazılarını, hatta .NET geliştiricilerine daha erişilebilir hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4643e-129">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="4643e-130">Sağlayıcı, Cosmos DB içindeki SQL API 'sine karşı otomatik değişiklik izleme, LINQ ve değer dönüştürmeleri gibi EF Core özelliklerinin çoğunu mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="4643e-130">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="4643e-131">Daha fazla bilgi için [Cosmos DB sağlayıcı belgelerine](xref:core/providers/cosmos/index) bakın.</span><span class="sxs-lookup"><span data-stu-id="4643e-131">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="4643e-132">C#8,0 desteği</span><span class="sxs-lookup"><span data-stu-id="4643e-132">C# 8.0 support</span></span>

<span data-ttu-id="4643e-133">EF Core 3,0, [8,0 ' deki C# yeni özelliklerden](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)faydalanır:</span><span class="sxs-lookup"><span data-stu-id="4643e-133">EF Core 3.0 takes advantage of a couple of the [new features in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="4643e-134">Zaman uyumsuz akışlar</span><span class="sxs-lookup"><span data-stu-id="4643e-134">Asynchronous streams</span></span>

<span data-ttu-id="4643e-135">Zaman uyumsuz sorgu sonuçları artık yeni standart `IAsyncEnumerable<T>` arabirim kullanılarak kullanıma sunulmuştur ve kullanılarak `await foreach`tüketilebilir.</span><span class="sxs-lookup"><span data-stu-id="4643e-135">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

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

<span data-ttu-id="4643e-136">Daha fazla bilgi için [ C# belgelerindeki zaman uyumsuz akışlara](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) bakın.</span><span class="sxs-lookup"><span data-stu-id="4643e-136">See the [asynchronous streams in the C# documentation](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="4643e-137">Boş değer atanabilir başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="4643e-137">Nullable reference types</span></span> 

<span data-ttu-id="4643e-138">Kodunuzda bu yeni özellik etkinleştirildiğinde EF Core, başvuru türü özelliklerinin null değer alabilme durumunu inceler ve veritabanındaki karşılık gelen sütunlara ve ilişkilere uygular: null yapılamayan başvurular türlerinin özellikleri, `[Required]` veri ek açıklaması özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4643e-138">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="4643e-139">Örneğin, aşağıdaki sınıfta, türü `string?` olarak işaretlenen özellikler isteğe bağlı olarak yapılandırılır, ancak `string` gereken şekilde yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="4643e-139">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
  public int Id { get; set; }
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public string? MiddleName { get; set; }
}
```

<span data-ttu-id="4643e-140">Daha fazla bilgi için EF Core belgelerinde [null yapılabilir başvuru türleriyle çalışma](xref:core/miscellaneous/nullable-reference-types) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="4643e-140">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="4643e-141">Veritabanı işlemlerinin yakaçıkarılması</span><span class="sxs-lookup"><span data-stu-id="4643e-141">Interception of database operations</span></span>

<span data-ttu-id="4643e-142">EF Core 3,0 ' deki yeni yakama API 'SI, EF Core normal işleminin bir parçası olarak düşük düzey veritabanı işlemleri gerçekleşdiğinde otomatik olarak çağrılması için özel mantık sağlamaya olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4643e-142">The new interception API in EF Core 3.0 allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="4643e-143">Örneğin, bağlantılar açılırken, işlemler uygulanırken veya komutları yürütürken.</span><span class="sxs-lookup"><span data-stu-id="4643e-143">For example, when opening connections, committing transactions, or executing commands.</span></span> 

<span data-ttu-id="4643e-144">EF 6 ' da var olan ele geçirme özelliklerine benzer şekilde, kırıcılar, işlemleri gerçekleşmeden önce veya sonra ele aktarmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4643e-144">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="4643e-145">Bunları gerçekleşmeden önce zaman geçitirsiniz, yürütmeye göre yürütme ve yedek mantığdan alternatif sonuçlar sağlama izni verilir.</span><span class="sxs-lookup"><span data-stu-id="4643e-145">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span> 

<span data-ttu-id="4643e-146">Örneğin, komut metnini işlemek için şunu oluşturabilirsiniz `IDbCommandInterceptor`:</span><span class="sxs-lookup"><span data-stu-id="4643e-146">For example, to manipulate command text, you can create an `IDbCommandInterceptor`:</span></span>

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

<span data-ttu-id="4643e-147">Ve şu şekilde kaydolun `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="4643e-147">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
  .UseSqlServer(connectionString)
  .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="4643e-148">Veritabanı görünümlerinin tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="4643e-148">Reverse engineering of database views</span></span>

<span data-ttu-id="4643e-149">Veritabanından okunabilecek ancak güncelleştirilmemiş verileri temsil eden sorgu türleri, [anahtarsız varlık türleri](xref:core/modeling/keyless-entity-types)olarak yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="4643e-149">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span> <span data-ttu-id="4643e-150">Çoğu senaryoda veritabanı görünümlerini eşlemek için mükemmel bir uyum olduğundan, EF Core artık tersine mühendislik veritabanı görünümlerinde otomatik olarak anahtarsız varlık türleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4643e-150">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="4643e-151">Örneğin, [DotNet EF komut satırı aracını](xref:core/miscellaneous/cli/dotnet) kullanarak şunu yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4643e-151">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

``` console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="4643e-152">Ve araç artık, anahtarlar olmadan görünümler ve tablolar için fkatlama türlerini otomatik olarak dolandırıcılardır:</span><span class="sxs-lookup"><span data-stu-id="4643e-152">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

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

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="4643e-153">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="4643e-153">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="4643e-154">EF Core 3,0 ' den başlayarak, `OrderDetails` aynı tabloya `Order` ait veya açıkça eşlenmiş ise, birincil anahtar ile eşleştirilecektir, ancak tüm özellikler olmadan `Order` `OrderDetails` bir eklenebilir. `OrderDetails` null yapılabilir sütunlar.</span><span class="sxs-lookup"><span data-stu-id="4643e-154">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="4643e-155">Sorgulama yaparken, gerekli özelliklerinden herhangi `OrderDetails` birinin `null` bir değeri yoksa veya birincil `null`anahtar ve tüm Özellikler ' in yanı sıra gerekli özellikleri yoksa EF Core olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4643e-155">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="4643e-156">.NET Core üzerinde EF 6,3</span><span class="sxs-lookup"><span data-stu-id="4643e-156">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="4643e-157">Bu aslında EF Core 3,0 özelliği değildir, ancak geçerli müşterilerimizin birçoğu için önemli olduğunu düşündük.</span><span class="sxs-lookup"><span data-stu-id="4643e-157">This isn't really an EF Core 3.0 feature, but we think it is important to many of our current customers.</span></span> 

<span data-ttu-id="4643e-158">Mevcut birçok uygulamanın daha önceki EF sürümlerini kullandığını ve yalnızca .NET Core 'un avantajlarından yararlanmak için EF Core aktarmak için önemli bir çaba gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="4643e-158">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span> <span data-ttu-id="4643e-159">Bu nedenle, .NET Core 3,0 ' de çalıştırmak için en yeni EF 6 sürümü bağlantı noktasına karar verdik.</span><span class="sxs-lookup"><span data-stu-id="4643e-159">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.0.</span></span> 

<span data-ttu-id="4643e-160">Daha ayrıntılı bilgi için bkz. [EF 6 ' daki](xref:ef6/what-is-new/index)yenilikler.</span><span class="sxs-lookup"><span data-stu-id="4643e-160">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="4643e-161">Ertelenmiş Özellikler</span><span class="sxs-lookup"><span data-stu-id="4643e-161">Postponed features</span></span>

<span data-ttu-id="4643e-162">EF Core 3,0 için başlangıçta planlanmış bazı özellikler gelecek sürümlere ertelendi:</span><span class="sxs-lookup"><span data-stu-id="4643e-162">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span>

- <span data-ttu-id="4643e-163">[#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)olarak izlenen, geçişler içindeki bir modelin parçalarını yok saybilme özelliği.</span><span class="sxs-lookup"><span data-stu-id="4643e-163">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="4643e-164">İki ayrı sorun olarak izlenen özellik paketi varlıkları: paylaşılan türdeki varlıklar hakkında [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) ve dizinli özellik eşleme desteği hakkında [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) .</span><span class="sxs-lookup"><span data-stu-id="4643e-164">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
