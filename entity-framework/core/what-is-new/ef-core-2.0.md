---
title: EF Core 2,0 ' deki yenilikler-EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 72393e96c195af1df5a169025ca2ce7a7acb16bb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656218"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="17755-102">EF Core 2,0 ' deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="17755-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="17755-103">.NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="17755-103">.NET Standard 2.0</span></span>

<span data-ttu-id="17755-104">EF Core artık .NET Standard 2,0 ' i hedeflediğinden, .NET Core 2,0, .NET Framework 4.6.1 ve .NET Standard 2,0 uygulayan diğer kitaplıklarla birlikte çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="17755-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="17755-105">Desteklenen özellikler hakkında daha fazla bilgi için bkz. [desteklenen .NET uygulamaları](../platforms/index.md) .</span><span class="sxs-lookup"><span data-stu-id="17755-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="17755-106">Modelleme</span><span class="sxs-lookup"><span data-stu-id="17755-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="17755-107">Tablo bölme</span><span class="sxs-lookup"><span data-stu-id="17755-107">Table splitting</span></span>

<span data-ttu-id="17755-108">Artık iki veya daha fazla varlık türünü birincil anahtar sütunların paylaşılacağını ve her satırın iki veya daha fazla varlığa karşılık geldiğini aynı tabloyla eşleştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="17755-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="17755-109">Tabloyu kullanarak bir tanımlayıcı ilişkiyi (birincil anahtar olan yabancı anahtar özellikler formu) kullanmak için tabloyu paylaşan tüm varlık türleri arasında yapılandırılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="17755-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

<span data-ttu-id="17755-110">Bu özellik hakkında daha fazla bilgi için [tablo bölme bölümündeki bölümü](xref:core/modeling/table-splitting) okuyun.</span><span class="sxs-lookup"><span data-stu-id="17755-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="17755-111">Sahip olunan türler</span><span class="sxs-lookup"><span data-stu-id="17755-111">Owned types</span></span>

<span data-ttu-id="17755-112">Sahip olunan bir varlık türü, aynı .NET türünü sahip olan başka bir varlık türüyle paylaşabilir, ancak yalnızca .NET türü tarafından tanımlanamıyorsa, başka bir varlık türünden bir gezinti olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="17755-112">An owned entity type can share the same .NET type with another owned entity type, but since it cannot be identified just by the .NET type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="17755-113">Tanımlama gezintisini içeren varlık, sahibidir.</span><span class="sxs-lookup"><span data-stu-id="17755-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="17755-114">Sahibi sorgulanırken sahip olan türler varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="17755-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="17755-115">Kurala göre, sahip olunan tür için bir gölge birincil anahtar oluşturulur ve tablo bölme kullanılarak aynı tabloyla aynı tabloyla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="17755-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="17755-116">Bu, EF6 ' de karmaşık türlerin kullanılmasına benzer şekilde sahip olan türleri kullanılmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="17755-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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

<span data-ttu-id="17755-117">Bu özellik hakkında daha fazla bilgi için, [sahip olduğunuz varlık türlerindeki bölümü](xref:core/modeling/owned-entities) okuyun.</span><span class="sxs-lookup"><span data-stu-id="17755-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="17755-118">Model düzeyi sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="17755-118">Model-level query filters</span></span>

<span data-ttu-id="17755-119">EF Core 2,0, model düzeyi sorgu filtrelerini çağırdığımız yeni bir özellik içeriyor.</span><span class="sxs-lookup"><span data-stu-id="17755-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="17755-120">Bu özellik LINQ sorgu koşullarına (genellikle LINQ WHERE sorgu işlecine geçirilen bir Boole ifadesi) meta veri modelindeki varlık türlerinde (genellikle Onmodeloluþturma 'da) doğrudan tanımlanacak şekilde izin verir.</span><span class="sxs-lookup"><span data-stu-id="17755-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="17755-121">Bu tür filtreler, dolaylı olarak başvurulan varlık türleri dahil olmak üzere bu varlık türlerini içeren herhangi bir LINQ sorgusuna otomatik olarak uygulanır; Örneğin, ekleme veya doğrudan gezinme özelliği başvuruları kullanımı.</span><span class="sxs-lookup"><span data-stu-id="17755-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="17755-122">Bu özelliğin bazı yaygın uygulamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="17755-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="17755-123">Geçici silme-bir varlık türü, IsDeleted özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="17755-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="17755-124">Çok kiracılı-varlık türü bir Tenantıd özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="17755-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="17755-125">Aşağıda listelenen iki senaryo için özelliği gösteren basit bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="17755-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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

<span data-ttu-id="17755-126">`Post` varlık türünün örnekleri için çok kiracılı ve geçici silme uygulayan model düzeyinde bir filtre tanımlandık.</span><span class="sxs-lookup"><span data-stu-id="17755-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="17755-127">DbContext örnek düzeyi özelliğinin kullanımını Note: `TenantId`.</span><span class="sxs-lookup"><span data-stu-id="17755-127">Note the use of a DbContext instance level property: `TenantId`.</span></span> <span data-ttu-id="17755-128">Model düzeyi filtreleri doğru bağlam örneğindeki değeri kullanır (diğer bir deyişle, sorguyu yürüten bağlam örneği).</span><span class="sxs-lookup"><span data-stu-id="17755-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="17755-129">Filtreler, IgnoreQueryFilters () işleci kullanılarak tekil LINQ sorguları için devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="17755-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="17755-130">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="17755-130">Limitations</span></span>

- <span data-ttu-id="17755-131">Gezinti başvurularına izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="17755-131">Navigation references are not allowed.</span></span> <span data-ttu-id="17755-132">Bu özellik geri bildirime göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="17755-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="17755-133">Filtreler yalnızca bir hiyerarşinin kök varlık türünde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="17755-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="17755-134">Veritabanı skaler işlev eşlemesi</span><span class="sxs-lookup"><span data-stu-id="17755-134">Database scalar function mapping</span></span>

<span data-ttu-id="17755-135">EF Core 2,0, [Paul Middleton](https://github.com/pmiddleton) 'TAN, LINQ sorgularında KULLANıLABILMESI ve SQL 'e çevrilebilmesi için, veritabanı skaler işlevlerinin Yöntem saplamalarla eşleştirilmesini sağlayan önemli bir katkı içerir.</span><span class="sxs-lookup"><span data-stu-id="17755-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="17755-136">Özelliğin nasıl kullanılabileceği hakkında kısa bir açıklama aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="17755-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="17755-137">`DbContext` statik bir yöntem bildirin ve `DbFunctionAttribute`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="17755-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

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

<span data-ttu-id="17755-138">Bunun gibi yöntemler otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="17755-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="17755-139">Kaydedildikten sonra, LINQ sorgusundaki yöntemine yapılan çağrılar SQL 'de işlev çağrılarına çevrilebilir:</span><span class="sxs-lookup"><span data-stu-id="17755-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="17755-140">Dikkat etmeniz gereken birkaç nokta:</span><span class="sxs-lookup"><span data-stu-id="17755-140">A few things to note:</span></span>

- <span data-ttu-id="17755-141">Kurala göre yöntemin adı, SQL oluştururken bir işlevin adı (Bu örnekte Kullanıcı tanımlı bir işlev) olarak kullanılır, ancak yöntem kaydı sırasında adı ve şemayı geçersiz kılabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="17755-141">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="17755-142">Şu anda yalnızca skaler işlevler destekleniyor</span><span class="sxs-lookup"><span data-stu-id="17755-142">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="17755-143">Eşlenmiş işlevi veritabanında oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="17755-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="17755-144">EF Core geçişleri oluşturma işlemini gerçekleşmeyecek</span><span class="sxs-lookup"><span data-stu-id="17755-144">EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="17755-145">Önce kod için kendi kendine içerilen tür yapılandırması</span><span class="sxs-lookup"><span data-stu-id="17755-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="17755-146">EF6 içinde, *EntityTypeConfiguration*'dan türeterek belirli bir varlık türünün kod ilk yapılandırmasını kapsüllemek mümkün.</span><span class="sxs-lookup"><span data-stu-id="17755-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="17755-147">EF Core 2,0 ' de bu kalıbı geri sunuyoruz:</span><span class="sxs-lookup"><span data-stu-id="17755-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="17755-148">Yüksek performans</span><span class="sxs-lookup"><span data-stu-id="17755-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="17755-149">DbContext havuzu</span><span class="sxs-lookup"><span data-stu-id="17755-149">DbContext pooling</span></span>

<span data-ttu-id="17755-150">Bir ASP.NET Core uygulamasındaki EF Core kullanmanın temel deseninin genellikle özel bir DbContext türünün bağımlılık ekleme sistemine kaydedilmesi ve daha sonra bu türün örneklerini denetleyicilerde Oluşturucu parametreleri aracılığıyla elde edilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="17755-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="17755-151">Bu, her istek için yeni bir DbContext örneği oluşturulduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="17755-151">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="17755-152">Sürüm 2,0 ' de, özel DbContext türlerini, bir yeniden kullanılabilir DbContext örnekleri havuzunu saydam bir şekilde sunan bağımlılık ekleme 'ye kaydetmek için yeni bir yol sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="17755-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="17755-153">DbContext havuzunu kullanmak için hizmet kaydı sırasında `AddDbContext` yerine `AddDbContextPool` kullanın:</span><span class="sxs-lookup"><span data-stu-id="17755-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="17755-154">Bu yöntem kullanılırsa, bir DbContext örneği bir denetleyici tarafından istendiğinde, önce havuzda kullanılabilir bir örnek olup olmadığını kontrol edeceğiz.</span><span class="sxs-lookup"><span data-stu-id="17755-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="17755-155">İstek işleme sonlandırıldıktan sonra, örnekteki herhangi bir durum sıfırlanır ve örnek havuza döndürülür.</span><span class="sxs-lookup"><span data-stu-id="17755-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="17755-156">Bu, ADO.NET sağlayıcıları 'nda bağlantı havuzunun nasıl çalıştığı ve DbContext örneği başlatma maliyetinin bir kısmını kaydetme avantajına benzer.</span><span class="sxs-lookup"><span data-stu-id="17755-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="17755-157">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="17755-157">Limitations</span></span>

<span data-ttu-id="17755-158">New Yöntemi, DbContext 'in `OnConfiguring()` yönteminde neler yapabileceğinize ilişkin birkaç sınırlama getirir.</span><span class="sxs-lookup"><span data-stu-id="17755-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="17755-159">İstek genelinde paylaşılmaması gereken türetilmiş DbContext sınıfınıza kendi eyaletinizi (örneğin, özel alanlar) korumanız durumunda DbContext havuzlamayı kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="17755-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="17755-160">EF Core, havuza bir DbContext örneği eklemeden önce yalnızca farkında olan durumu sıfırlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="17755-160">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="17755-161">Açıkça derlenmiş sorgular</span><span class="sxs-lookup"><span data-stu-id="17755-161">Explicitly compiled queries</span></span>

<span data-ttu-id="17755-162">Bu, yüksek ölçekli senaryolarda avantajlar sunmak için tasarlanan ikinci bir katılım performansı özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="17755-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="17755-163">El ile veya açıkça derlenmiş sorgu API 'Leri, daha önceki EF sürümlerinde ve ayrıca, uygulamaların yalnızca bir kez hesaplanabilmesi ve birçok kez yürütülebilmeleri için sorguların çevirisini önbelleğe almak üzere LINQ to SQL ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="17755-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="17755-164">Genel EF Core, sorgu ifadelerinin karma hale getirilmiş bir gösterimine göre sorguları otomatik olarak derleyip önbelleğe alabilir, bu mekanizma, karma ve önbellek araması hesaplamasını atlayarak küçük bir performans kazancı elde etmek için kullanılabilir ve buna izin verebilir bir temsilci çağrısı aracılığıyla zaten derlenmiş bir sorguyu kullanacak şekilde uygulama.</span><span class="sxs-lookup"><span data-stu-id="17755-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="17755-165">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="17755-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="17755-166">Attach, yeni ve var olan varlıkların bir grafiğini izleyebilir</span><span class="sxs-lookup"><span data-stu-id="17755-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="17755-167">EF Core, çeşitli mekanizmalarda anahtar değerlerini otomatik olarak oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="17755-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="17755-168">Bu özellik kullanılırken, anahtar özelliği CLR varsayılan ise (genellikle sıfır veya null) bir değer oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="17755-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="17755-169">Bu, bir varlık grafiğinin `DbContext.Attach` veya `DbSet.Attach` geçirilecek olabileceği anlamına gelir; EF Core anahtar kümesi olmayan bu varlıklar `Added`olarak işaretlenirken, bir anahtara sahip olan varlıkların zaten `Unchanged` olarak ayarlanmış olduğunu işaretleyecek.</span><span class="sxs-lookup"><span data-stu-id="17755-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="17755-170">Bu, oluşturulan anahtarlar kullanılırken karışık yeni ve var olan varlıkların bir grafiğini eklemeyi kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="17755-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="17755-171">`DbContext.Update` ve `DbSet.Update` aynı şekilde çalışır, ancak anahtar kümesi olan varlıklar `Unchanged`yerine `Modified` olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="17755-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="17755-172">Sorgu</span><span class="sxs-lookup"><span data-stu-id="17755-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="17755-173">Geliştirilmiş LINQ çevirisi</span><span class="sxs-lookup"><span data-stu-id="17755-173">Improved LINQ Translation</span></span>

<span data-ttu-id="17755-174">Veritabanında daha fazla mantığın (bellek içi değil) ve veritabanından gereksiz bir şekilde veri alınmasından daha fazla mantığı olan başarılı şekilde yürütmek için daha fazla sorgu sağlar.</span><span class="sxs-lookup"><span data-stu-id="17755-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="17755-175">Groupjoın geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="17755-175">GroupJoin improvements</span></span>

<span data-ttu-id="17755-176">Bu çalışma, Grup birleşimleri için oluşturulan SQL 'i geliştirir.</span><span class="sxs-lookup"><span data-stu-id="17755-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="17755-177">Grup birleştirmeleri genellikle isteğe bağlı gezinme özelliklerindeki alt sorguların bir sonucudur.</span><span class="sxs-lookup"><span data-stu-id="17755-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="17755-178">FromSql ve ExecuteSqlCommand 'da dize ilişkilendirme</span><span class="sxs-lookup"><span data-stu-id="17755-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="17755-179">C#6, C# ifadelerin dize değişmez değerlerinde doğrudan gömülmesini sağlayan, çalışma zamanında dizeler oluşturmanın iyi bir yolunu sağlayan bir özellik olan dize ilişkilendirmeyi sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="17755-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="17755-180">EF Core 2,0 ' de, ham SQL dizelerini kabul eden iki birincil API 'imize enterpolasyonlu dizeler için özel destek ekledik: `FromSql` ve `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="17755-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="17755-181">Bu yeni destek, C# dize ilişkilendirme ' güvenli ' bir biçimde kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="17755-181">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="17755-182">Diğer bir deyişle, çalışma zamanında dinamik olarak SQL oluştururken ortaya çıkabilecek yaygın SQL ekleme hatalarına karşı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="17755-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="17755-183">Aşağıda bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="17755-183">Here is an example:</span></span>

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

<span data-ttu-id="17755-184">Bu örnekte, SQL biçim dizesinde gömülü iki değişken vardır.</span><span class="sxs-lookup"><span data-stu-id="17755-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="17755-185">EF Core aşağıdaki SQL 'i oluşturacak:</span><span class="sxs-lookup"><span data-stu-id="17755-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="17755-186">Aşv. Functions. like ()</span><span class="sxs-lookup"><span data-stu-id="17755-186">EF.Functions.Like()</span></span>

<span data-ttu-id="17755-187">EF 'i ekledik. İşlev özelliği, LINQ sorgularında çağrılabilecek şekilde veritabanı işlevleri veya işleçleriyle eşlenen yöntemleri tanımlamak için EF Core veya sağlayıcılar tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17755-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="17755-188">Bu tür bir yöntemin ilk örneği () gibidir:</span><span class="sxs-lookup"><span data-stu-id="17755-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="17755-189">Örneğin (), bellek içi bir veritabanına göre çalışırken veya koşulun değerlendirmesi istemci tarafında gerçekleşeceği zaman yararlı olabilecek, bellek içi bir uygulamayla birlikte geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="17755-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="17755-190">Veritabanı yönetimi</span><span class="sxs-lookup"><span data-stu-id="17755-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="17755-191">DbContext Scafkatlaması için pluralization kancası</span><span class="sxs-lookup"><span data-stu-id="17755-191">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="17755-192">EF Core 2,0, varlık türü adlarını ve pluri DbSet adlarını bir kez eklemek için kullanılan yeni bir *ıpluralizer* hizmeti sunar.</span><span class="sxs-lookup"><span data-stu-id="17755-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="17755-193">Varsayılan uygulama bir op değildir, bu nedenle bu yalnızca katlara kendi pluralizer 'ı kolayca ekleyebileceğiniz bir kanca olur.</span><span class="sxs-lookup"><span data-stu-id="17755-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="17755-194">Geliştirici kendi pluralizer ' de kanca için aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="17755-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="17755-195">Diğerleri</span><span class="sxs-lookup"><span data-stu-id="17755-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="17755-196">ADO.NET SQLite sağlayıcısını SQLitePCL. RAW öğesine taşı</span><span class="sxs-lookup"><span data-stu-id="17755-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>

<span data-ttu-id="17755-197">Bu, yerel SQLite ikililerini farklı platformlarda dağıtmak için Microsoft. Data. sqlite ' da daha sağlam bir çözüm sunar.</span><span class="sxs-lookup"><span data-stu-id="17755-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="17755-198">Her model için yalnızca bir sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="17755-198">Only one provider per model</span></span>

<span data-ttu-id="17755-199">Sağlayıcıların modelle nasıl etkileşime gireceğini önemli ölçüde genişlettiğini ve kuralların, ek açıklamaların ve akıcı API 'Lerin farklı sağlayıcılarla nasıl çalıştığını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="17755-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="17755-200">EF Core 2,0, artık kullanılan her farklı sağlayıcı için farklı bir [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="17755-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="17755-201">Bu genellikle uygulama için saydamdır.</span><span class="sxs-lookup"><span data-stu-id="17755-201">This is usually transparent to the application.</span></span> <span data-ttu-id="17755-202">Bu, alt düzey meta veri API 'Lerinin basitleştirdiğini, *yaygın ilişkisel meta veri kavramlarını* her zaman `.SqlServer`, `.Sqlite`vb. yerine `.Relational` çağrısıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="17755-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="17755-203">Birleştirilmiş günlüğe kaydetme ve tanılama</span><span class="sxs-lookup"><span data-stu-id="17755-203">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="17755-204">Günlüğe kaydetme (ILogger tabanlı) ve tanılama (DiagnosticSource tabanlı) mekanizmalarına artık daha fazla kod paylaşmalıdır.</span><span class="sxs-lookup"><span data-stu-id="17755-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="17755-205">Bir ILogger 'a gönderilen iletiler için olay kimlikleri 2,0 içinde değişmiştir.</span><span class="sxs-lookup"><span data-stu-id="17755-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="17755-206">Olay kimlikleri artık EF Core kod genelinde benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="17755-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="17755-207">Bu iletiler artık, örneğin MVC gibi kullanılan yapılandırılmış günlük için standart kalıbı de izler.</span><span class="sxs-lookup"><span data-stu-id="17755-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="17755-208">Günlükçü kategorileri de değişmiştir.</span><span class="sxs-lookup"><span data-stu-id="17755-208">Logger categories have also changed.</span></span> <span data-ttu-id="17755-209">Artık [Dbloggercategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs)aracılığıyla erişilen iyi bilinen bir kategori kümesi vardır.</span><span class="sxs-lookup"><span data-stu-id="17755-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="17755-210">DiagnosticSource olayları artık karşılık gelen `ILogger` iletileriyle aynı olay KIMLIĞI adlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="17755-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
