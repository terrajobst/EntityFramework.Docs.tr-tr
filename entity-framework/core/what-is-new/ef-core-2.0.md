---
title: EF Core 2.0 - EF Core yenilikleri
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: b5ac31722f49589f1494a3d8d1c8a7011a4cf9ce
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463275"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="05cbd-102">EF Core 2.0 yenilikleri</span><span class="sxs-lookup"><span data-stu-id="05cbd-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="05cbd-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="05cbd-103">.NET Standard 2.0</span></span>
<span data-ttu-id="05cbd-104">EF Core, .NET Core 2.0, .NET Framework 4.6.1 ve .NET Standard 2.0 uygulayan diğer kitaplıkları ile çalışabilir yani .NET Standard 2.0, artık hedefler.</span><span class="sxs-lookup"><span data-stu-id="05cbd-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="05cbd-105">Bkz: [desteklenen .NET uygulamalarıyla](../platforms/index.md) desteklenen özellikler hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="05cbd-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="05cbd-106">Modelleme</span><span class="sxs-lookup"><span data-stu-id="05cbd-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="05cbd-107">Tablo bölme</span><span class="sxs-lookup"><span data-stu-id="05cbd-107">Table splitting</span></span>

<span data-ttu-id="05cbd-108">Şimdi, burada birincil anahtar sütunları paylaşılır ve her satır için iki veya daha fazla varlık karşılık gelen aynı tabloya iki veya daha fazla varlık türleri eşleştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="05cbd-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="05cbd-109">Tablosunu kullanmak için (burada yabancı anahtar özellikleri birincil anahtar oluşturmak) tanımlayan bir ilişki bölme tüm tablo paylaşımı varlık türleri arasında yapılandırılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="05cbd-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="05cbd-110">Sahip olunan türleri</span><span class="sxs-lookup"><span data-stu-id="05cbd-110">Owned types</span></span>

<span data-ttu-id="05cbd-111">Sahip olunan varlık türü aynı CLR türü başka bir sahip olunan varlık türü ile paylaşabilir, ancak yalnızca CLR türü tanımlanamıyor beri olmalıdır bir gezinti için başka bir varlık türü.</span><span class="sxs-lookup"><span data-stu-id="05cbd-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="05cbd-112">Tanımlama Gezinti içeren varlık sahibi değil.</span><span class="sxs-lookup"><span data-stu-id="05cbd-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="05cbd-113">Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="05cbd-114">Kural gereği sahip olunan türü için gölge birincil bir anahtar oluşturulur ve onu aynı tablonun sahibi olarak tablo bölmeyi kullanarak eşleneceğini.</span><span class="sxs-lookup"><span data-stu-id="05cbd-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="05cbd-115">Bu olanak için nasıl karmaşık türler içinde EF6 kullanılan ait kullanım benzer şekilde türleri:</span><span class="sxs-lookup"><span data-stu-id="05cbd-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```
<span data-ttu-id="05cbd-116">Okuma [bölümüne sahip olunan varlık türleri](xref:core/modeling/owned-entities) bu özellik hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="05cbd-116">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="05cbd-117">Model düzeyi sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="05cbd-117">Model-level query filters</span></span>

<span data-ttu-id="05cbd-118">EF Core 2.0 Model düzeyi sorgu filtreleri diyoruz yeni bir özellik içerir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-118">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="05cbd-119">LINQ Sorgu koşullarına (genellikle nereye LINQ sorgu işleci için geçirilen bir Boole ifadesi) bu özelliği sağlar (genellikle, OnModelCreating) meta veri modeli varlık türleri üzerinde doğrudan tanımlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="05cbd-119">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="05cbd-120">Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="05cbd-120">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="05cbd-121">Bu özelliğin bazı ortak uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="05cbd-121">Some common applications of this feature are:</span></span>

- <span data-ttu-id="05cbd-122">Geçici silme - IsDeleted özelliği bir varlık türlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="05cbd-122">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="05cbd-123">-Çok kiracılı bir varlık türü bir Tenantıd özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="05cbd-123">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="05cbd-124">Yukarıda listelenen iki senaryo için özellik gösteren basit bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="05cbd-124">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId );
    }
}
```
<span data-ttu-id="05cbd-125">Çok kiracılılık ve geçici silme için uygulayan bir model düzeyi filtresi tanımlarız örneklerini ```Post``` varlık türü.</span><span class="sxs-lookup"><span data-stu-id="05cbd-125">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="05cbd-126">DbContext örnek düzeyi özelliği kullanımına dikkat edin: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="05cbd-126">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="05cbd-127">Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten bağlam örnek) doğru bağlam örneğinin değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="05cbd-127">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="05cbd-128">Filtreler IgnoreQueryFilters() işlecini kullanarak tek tek LINQ sorguları için devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-128">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="05cbd-129">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="05cbd-129">Limitations</span></span>

- <span data-ttu-id="05cbd-130">Gezinti başvurulara izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="05cbd-130">Navigation references are not allowed.</span></span> <span data-ttu-id="05cbd-131">Bu özellik, geri bildirim göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-131">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="05cbd-132">Filtreler yalnızca kök varlık türü bir hiyerarşinin tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-132">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="05cbd-133">Veritabanı skaler bir işlev eşlemesi</span><span class="sxs-lookup"><span data-stu-id="05cbd-133">Database scalar function mapping</span></span>

<span data-ttu-id="05cbd-134">EF Core 2.0 içeren gelen önemli bir katkı [Paul Middleton](https://github.com/pmiddleton) böylece bunlar, LINQ sorgularında kullanılan ve SQL çevrilmiş Saplamaları yöntemi için eşleme veritabanı skaler işlevler sağlar.</span><span class="sxs-lookup"><span data-stu-id="05cbd-134">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="05cbd-135">Aşağıda, özellikle nasıl kullanılabileceği hakkında kısa bir açıklaması verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="05cbd-135">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="05cbd-136">Üzerinde statik bir yöntemi bildirmek, `DbContext` ve onunla açıklama `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="05cbd-136">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

<span data-ttu-id="05cbd-137">Bu gibi yöntemleri otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-137">Methods like this are automatically registered.</span></span> <span data-ttu-id="05cbd-138">Kaydedildikten sonra bir LINQ Sorgu yöntemine yönelik çağrılar işlev çağrıları SQL çevrilebilir:</span><span class="sxs-lookup"><span data-stu-id="05cbd-138">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="05cbd-139">Dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="05cbd-139">A few things to note:</span></span>

- <span data-ttu-id="05cbd-140">Yöntemin adı (Bu durumda bir işlevde kullanıcı tanımlı) bir işlev adı olarak kullanıldığında Kural gereği SQL, ancak oluşturma adını ve şemayı yöntemi kayıt sırasında geçersiz kılabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="05cbd-140">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="05cbd-141">Şu anda yalnızca skaler işlevler desteklenir</span><span class="sxs-lookup"><span data-stu-id="05cbd-141">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="05cbd-142">Veritabanında eşleşen işlev oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-142">You must create the mapped function in the database.</span></span> <span data-ttu-id="05cbd-143">EF Core geçişleri oluşturma ilgileniriz değil</span><span class="sxs-lookup"><span data-stu-id="05cbd-143">EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="05cbd-144">Kod için kendi içinde bulunan tür yapılandırma ilk</span><span class="sxs-lookup"><span data-stu-id="05cbd-144">Self-contained type configuration for code first</span></span>

<span data-ttu-id="05cbd-145">EF6 içinde kod ilk yapılandırma bir özel varlık türünün türeterek yalıtılacak olası *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="05cbd-145">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="05cbd-146">EF Core 2.0 sürümünde Biz bu düzen geri getirme:</span><span class="sxs-lookup"><span data-stu-id="05cbd-146">In EF Core 2.0 we are bringing this pattern back:</span></span>

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
  public void Configure(EntityTypeBuilder<Customer> builder)
  {
     builder.HasKey(c => c.AlternateKey);
     builder.Property(c => c.Name).HasMaxLength(200);
   }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a><span data-ttu-id="05cbd-147">Yüksek Performans</span><span class="sxs-lookup"><span data-stu-id="05cbd-147">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="05cbd-148">DbContext havuzu</span><span class="sxs-lookup"><span data-stu-id="05cbd-148">DbContext pooling</span></span>

<span data-ttu-id="05cbd-149">EF Core ASP.NET Core uygulaması genellikle kullanmak için temel düzeni, özel bir DbContext tür bağımlılık ekleme sistemine kaydetme ve daha sonra denetleyicileri Oluşturucusu parametrelerinde aracılığıyla bu türün örneklerini alma içerir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-149">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="05cbd-150">Başka bir deyişle, her istek için yeni bir örneğini DbContext oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="05cbd-150">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="05cbd-151">Sürüm 2.0 şeffaf bir şekilde yeniden kullanılabilir DbContext örneklerinden oluşan bir havuz tanıtan bağımlılık ekleme özel DbContext türleri kaydetmek için yeni bir yolunu sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="05cbd-151">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="05cbd-152">DbContext havuzu kullanmak için ```AddDbContextPool``` yerine ```AddDbContext``` hizmeti kaydı sırasında:</span><span class="sxs-lookup"><span data-stu-id="05cbd-152">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="05cbd-153">Bu yöntem kullanılıyorsa, zaman biz olup olmadığını örneği kullanılabilir havuzda ilk denetleyecek denetleyicisi tarafından bir DbContext örneği istenir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-153">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="05cbd-154">İstek işleme sonlandırır örneğinde herhangi bir durumu sıfırlanır ve kendisini havuza geri döner örnektir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-154">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="05cbd-155">Bu, kavramsal olarak benzer nasıl bağlantı havuzu ADO.NET sağlayıcıları çalışır ve bazı DbContext örneğinin başlatma maliyetini kaydetme avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-155">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="05cbd-156">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="05cbd-156">Limitations</span></span>

<span data-ttu-id="05cbd-157">Yöntem ne yapılabilir bazı sınırlamalar sunar ```OnConfiguring()``` DbContext yöntemi.</span><span class="sxs-lookup"><span data-stu-id="05cbd-157">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="05cbd-158">DbContext havuzu istekler genelinde Paylaşılmaması gereken, türetilmiş bir DbContext sınıfı içinde kendi durumu (örneğin, özel alanları) korur kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="05cbd-158">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="05cbd-159">EF Core, yalnızca bir DbContext örneği havuza eklemeden önce farkında durumuna sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="05cbd-159">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="05cbd-160">Açıkça derlenmiş sorgular</span><span class="sxs-lookup"><span data-stu-id="05cbd-160">Explicitly compiled queries</span></span>

<span data-ttu-id="05cbd-161">Bu, büyük ölçekli senaryolar avantajları sunmak için tasarlanan ikinci katılımı performans özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-161">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="05cbd-162">API'leri EF'ın önceki sürümlerini hem de LINQ to SQL sorguları çevirisi önbelleğe alınması yalnızca bir kez hesaplanabilir uygulamalarının izin vermek için kullanılabilir ve birçok kez yürütülen el ile veya açıkça derlenmiş sorgusu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-162">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="05cbd-163">Önbellek araması ve karma hesaplama atlayarak bir küçük bir performans kazancı elde etmek için genel EF Core otomatik olarak derleyin ve önbelleğe alma, karma bir sorgu ifadeleri gösterimini üzerinde temel alan sorguları olsa da, bu mekanizma kullanılabilir izin verme önceden derlenmiş bir sorgu bir temsilcinin Çağırma ile kullanmak için uygulama.</span><span class="sxs-lookup"><span data-stu-id="05cbd-163">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a><span data-ttu-id="05cbd-164">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="05cbd-164">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="05cbd-165">Ekleme yeni ve var olan varlıkları grafiğini izleyebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="05cbd-165">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="05cbd-166">EF Core çeşitli mekanizmalar aracılığıyla anahtar değerlerini otomatik olarak oluşturulmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="05cbd-166">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="05cbd-167">Anahtar özelliği CLR varsayılan--genellikle sıfır ya da null ise bu özelliği kullanırken, bir değer oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="05cbd-167">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="05cbd-168">Bu varlıkların bir grafik için geçirilebilir anlamına gelir `DbContext.Attach` veya `DbSet.Attach` ve EF Core zaten olarak ayarlanmış bir anahtara sahip kişilikleri işaretleyecek `Unchanged` kişilikleri anahtar kümesi olmayan olarak işaretlenir ancak `Added`.</span><span class="sxs-lookup"><span data-stu-id="05cbd-168">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="05cbd-169">Bu karma, yeni bir grafik eklemek kolaylaştırır ve kullanırken var olan varlıkları anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="05cbd-169">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="05cbd-170">`DbContext.Update` ve `DbSet.Update` anahtar kümesi ile varlıkları olarak işaretlenmiş dışında aynı şekilde, iş `Modified` yerine `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="05cbd-170">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="05cbd-171">Sorgu</span><span class="sxs-lookup"><span data-stu-id="05cbd-171">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="05cbd-172">Gelişmiş LINQ çeviri</span><span class="sxs-lookup"><span data-stu-id="05cbd-172">Improved LINQ Translation</span></span>

<span data-ttu-id="05cbd-173">Daha fazla sorgu başarılı bir şekilde, daha fazla mantıksal veritabanı (yerine bellek içi) ve daha az gereksiz yere veritabanından alınan veri değerlendirilen çalışmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="05cbd-173">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="05cbd-174">GroupJoin geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="05cbd-174">GroupJoin improvements</span></span>

<span data-ttu-id="05cbd-175">Bu iş grup birleştirmeleri için oluşturulan SQL artırır.</span><span class="sxs-lookup"><span data-stu-id="05cbd-175">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="05cbd-176">Grup birleştirmeleri çoğunlukla bir isteğe bağlı Gezinti özellikleri üzerinde alt sorgularda sonucudur.</span><span class="sxs-lookup"><span data-stu-id="05cbd-176">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="05cbd-177">SQL ve ExecuteSqlCommand dize ilişkilendirme</span><span class="sxs-lookup"><span data-stu-id="05cbd-177">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="05cbd-178">C# 6 yapı dizeleri çalışma zamanında iyi bir yol sağlayarak, dize ilişkilendirme dize değişmez değerleri, doğrudan gömülü olması C# ifadelerini sağlayan bir özelliği kullanıma sunuldu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-178">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="05cbd-179">EF Core 2.0 sürümünde ham SQL dizeleri kabul iki birincil Apı'lerimizi için ilişkilendirilmiş dizeler için özel destek eklendi: ```FromSql``` ve ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="05cbd-179">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="05cbd-180">Bu yeni Destek 'güvenli' bir şekilde kullanılacak C# dize ilişkilendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="05cbd-180">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="05cbd-181">Diğer bir deyişle, dinamik olarak oluşabilir ortak SQL ekleme hatalarına karşı koruyan bir şekilde çalışma zamanında SQL oluşturma.</span><span class="sxs-lookup"><span data-stu-id="05cbd-181">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="05cbd-182">Aşağıda bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="05cbd-182">Here is an example:</span></span>

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

<span data-ttu-id="05cbd-183">Bu örnekte, iki değişken SQL Biçim dizesinde katıştırılmış vardır.</span><span class="sxs-lookup"><span data-stu-id="05cbd-183">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="05cbd-184">EF Core aşağıdaki SQL oluşturacak:</span><span class="sxs-lookup"><span data-stu-id="05cbd-184">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="05cbd-185">EF. Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="05cbd-185">EF.Functions.Like()</span></span>

<span data-ttu-id="05cbd-186">EF ekledik. EF Core veya sağlayıcıları tarafından veritabanı işlevleri veya işleçler eşlemek ve böylece bunlar, LINQ sorgularında çağrılabilir yöntemleri tanımlamak için kullanılabilecek işlevleri özelliği.</span><span class="sxs-lookup"><span data-stu-id="05cbd-186">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="05cbd-187">İlk tür bir yöntem Like() örneğidir:</span><span class="sxs-lookup"><span data-stu-id="05cbd-187">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="05cbd-188">Bir bellek içi veritabanına karşı çalışırken veya değerlendirme, koşulun istemci tarafında ortaya gerektiğinde yararlı olabilen Like() bir bellek içi uygulamasıyla geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="05cbd-188">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="05cbd-189">Veritabanı Yönetimi</span><span class="sxs-lookup"><span data-stu-id="05cbd-189">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="05cbd-190">Çoğullaştırmayı kanca DbContext yapı İskelesi için</span><span class="sxs-lookup"><span data-stu-id="05cbd-190">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="05cbd-191">EF Core 2.0 tanıtır, yeni bir *IPluralizer* varlık singularize için kullanılan hizmet adlarını yazabilir ve pluralize olan DB adları.</span><span class="sxs-lookup"><span data-stu-id="05cbd-191">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="05cbd-192">Varsayılan uygulaması bir İşlemsiz olduğundan burada istedim kendi pluralizer kolayca takılabilir bir kanca budur.</span><span class="sxs-lookup"><span data-stu-id="05cbd-192">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="05cbd-193">İşte bu şekilde kendi pluralizer yeteneklerinizi bir geliştiricinin görünür:</span><span class="sxs-lookup"><span data-stu-id="05cbd-193">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a><span data-ttu-id="05cbd-194">Diğerleri</span><span class="sxs-lookup"><span data-stu-id="05cbd-194">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="05cbd-195">ADO.NET SQLite sağlayıcısına SQLitePCL.raw Taşı</span><span class="sxs-lookup"><span data-stu-id="05cbd-195">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="05cbd-196">Bu daha sağlam bir çözüm içinde Microsoft.Data.Sqlite farklı platformlarda yerel bir SQLite ikili dağıtılmasında sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="05cbd-196">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="05cbd-197">Model başına yalnızca bir sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="05cbd-197">Only one provider per model</span></span>
<span data-ttu-id="05cbd-198">Önemli ölçüde artırmaktadır sağlayıcıları modeli ile nasıl etkileşim kurabileceğine ve kuralları, ek açıklamalar ve fluent API'ler farklı sağlayıcıları ile nasıl çalıştığını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-198">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="05cbd-199">EF Core 2.0 artık farklı bir derler [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) için kullanılan her farklı bir sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="05cbd-199">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="05cbd-200">Bu genellikle uygulamaya saydamdır.</span><span class="sxs-lookup"><span data-stu-id="05cbd-200">This is usually transparent to the application.</span></span> <span data-ttu-id="05cbd-201">Tüm erişimi olacak şekilde bu düşük düzeyli meta verileri API'leri bir basitleştirme kolaylaştırılan *ortak ilişkisel meta veri kavramlarını* her zaman bir çağrı yoluyla yapılan `.Relational` yerine `.SqlServer`, `.Sqlite`, vb.</span><span class="sxs-lookup"><span data-stu-id="05cbd-201">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="05cbd-202">Birleştirilmiş günlüğe kaydetme ve tanılama</span><span class="sxs-lookup"><span data-stu-id="05cbd-202">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="05cbd-203">(Üzerinde ILogger göre) günlüğe kaydetme ve tanılama (üzerinde DiagnosticSource göre) mekanizmaları artık daha fazla kod paylaşın.</span><span class="sxs-lookup"><span data-stu-id="05cbd-203">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="05cbd-204">2. 0'için bir ILogger gönderilen iletiler için olay kimlikleri değişti.</span><span class="sxs-lookup"><span data-stu-id="05cbd-204">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="05cbd-205">Olay kimlikleri, artık EF Core kod arasında benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-205">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="05cbd-206">Bu iletiler şimdi de, örneğin, MVC kullanılan yapılandırılmış günlük kaydı için standart deseni izleyin.</span><span class="sxs-lookup"><span data-stu-id="05cbd-206">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="05cbd-207">Günlükçü kategoriler de değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="05cbd-207">Logger categories have also changed.</span></span> <span data-ttu-id="05cbd-208">İyi bilinen bir kategori kümesini aracılığıyla erişilen artık yoktur [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="05cbd-208">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="05cbd-209">DiagnosticSource olayları artık ilgili olarak aynı olay kimliği adları kullanma `ILogger` iletileri.</span><span class="sxs-lookup"><span data-stu-id="05cbd-209">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
