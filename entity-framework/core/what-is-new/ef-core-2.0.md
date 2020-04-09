---
title: EF Core 2.0'daki yenilikler - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 83f6b819409d502dba17a678d44a0746a4a77f4b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417498"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="5bfa7-102">EF Core 2.0'daki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="5bfa7-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="5bfa7-103">.NET Standart 2.0</span><span class="sxs-lookup"><span data-stu-id="5bfa7-103">.NET Standard 2.0</span></span>

<span data-ttu-id="5bfa7-104">EF Core artık .NET Standard 2.0'ı hedefliyor, bu da .NET Core 2.0, .NET Framework 4.6.1 ve .NET Standard 2.0'ı uygulayan diğer kitaplıklarla çalışabileceği anlamına geliyor.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="5bfa7-105">[Desteklenenler](../platforms/index.md) hakkında daha fazla bilgi için Desteklenen .NET Uygulamaları'na bakın.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="5bfa7-106">Modelleme</span><span class="sxs-lookup"><span data-stu-id="5bfa7-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="5bfa7-107">Tablo bölme</span><span class="sxs-lookup"><span data-stu-id="5bfa7-107">Table splitting</span></span>

<span data-ttu-id="5bfa7-108">Artık iki veya daha fazla varlık türünü birincil anahtar sütunun(lar) paylaşılacak ve her satırın iki veya daha fazla varlığa karşılık geldiği aynı tabloyla eşlemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="5bfa7-109">Tabloyu tanımlayan bir ilişkiyi bölmek için (yabancı anahtar özelliklerinin birincil anahtarı oluşturduğu) tabloyu paylaşan tüm varlık türleri arasında yapılandırılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

<span data-ttu-id="5bfa7-110">Bu özellik hakkında daha fazla bilgi için [tablo bölme bölümünü](xref:core/modeling/table-splitting) okuyun.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="5bfa7-111">Sahip olunan türler</span><span class="sxs-lookup"><span data-stu-id="5bfa7-111">Owned types</span></span>

<span data-ttu-id="5bfa7-112">Sahip olunan bir varlık türü aynı .NET türünü başka bir sahip olunan varlık türüyle paylaşabilir, ancak sadece .NET türüyle tanımlanamadığı için başka bir varlık türünden bir gezinti olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-112">An owned entity type can share the same .NET type with another owned entity type, but since it cannot be identified just by the .NET type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="5bfa7-113">Tanımlayıcı gezintiyi içeren varlık sahibidir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="5bfa7-114">Sahibini sorgularken, sahip olunan türler varsayılan olarak eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="5bfa7-115">Kural olarak, sahip olunan tür için bir gölge birincil anahtar oluşturulur ve tablo bölme kullanılarak sahibiyle aynı tabloya eşlenir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="5bfa7-116">Bu, ef6'da karmaşık türlerin nasıl kullanıldığına benzer şekilde sahip olunan türleri kullanmanıza olanak tanır:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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

<span data-ttu-id="5bfa7-117">Bu özellik hakkında daha fazla bilgi için [sahip olunan varlık türleri hakkındaki bölümü](xref:core/modeling/owned-entities) okuyun.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="5bfa7-118">Model düzeyinde sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="5bfa7-118">Model-level query filters</span></span>

<span data-ttu-id="5bfa7-119">EF Core 2.0, Model düzeyinde sorgu filtreleri dediğimiz yeni bir özellik içerir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="5bfa7-120">Bu özellik, LINQ sorgu yüklemlerinin (genellikle LINQ Where query işlecine geçirilen boolean ifadesinin) meta veri modelindeki Varlık Türleri üzerinde (genellikle OnModelCreating'da) doğrudan tanımlanmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="5bfa7-121">Bu tür filtreler, Dolaylı olarak başvurulan Varlık Türleri de dahil olmak üzere, bu Varlık Türlerini içeren tüm LINQ sorgularına (Örneğin veya doğrudan gezinme özelliği başvuruları nın kullanımı yoluyla) otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="5bfa7-122">Bu özelliğin bazı yaygın uygulamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="5bfa7-123">Yumuşak silme - Bir Varlık Türleri Silinmiş özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="5bfa7-124">Çoklu kira - Varlık Türü Kiracı Kimliği özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="5bfa7-125">Aşağıda, yukarıda listelenen iki senaryo için özelliği gösteren basit bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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
            && p.TenantId == this.TenantId);
    }
}
```

<span data-ttu-id="5bfa7-126">`Post` Varlık Türü örnekleri için çoklu kira ve yumuşak silme uygulayan bir model düzeyinde filtre tanımlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="5bfa7-127">Örnek düzeyinde bir `DbContext` özelliğin kullanımına `TenantId`dikkat edin: .</span><span class="sxs-lookup"><span data-stu-id="5bfa7-127">Note the use of a `DbContext` instance-level property: `TenantId`.</span></span> <span data-ttu-id="5bfa7-128">Model düzeyindefiltreler, doğru bağlam örneğindeki değeri (diğer bir şekilde, sorguyu yürüten bağlam örneği) kullanır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="5bfa7-129">Filtreler, IgnoreQueryFilters() işleci kullanarak tek tek LINQ sorguları için devre dışı bırakılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="5bfa7-130">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="5bfa7-130">Limitations</span></span>

- <span data-ttu-id="5bfa7-131">Gezinti başvurularına izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-131">Navigation references are not allowed.</span></span> <span data-ttu-id="5bfa7-132">Bu özellik geri bildirime dayalı olarak eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="5bfa7-133">Filtreler yalnızca bir hiyerarşinin kök Varlık Türü'nde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="5bfa7-134">Veritabanı skaler fonksiyon eşleme</span><span class="sxs-lookup"><span data-stu-id="5bfa7-134">Database scalar function mapping</span></span>

<span data-ttu-id="5bfa7-135">EF Core 2.0, [Paul Middleton'ın,](https://github.com/pmiddleton) veritabanı skaler işlevlerini linq sorgularında kullanılabilmesi ve SQL'e çevrilebilmesi için yöntem saplamalarına olanak tanıyan önemli bir katkısını içerir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="5bfa7-136">Özelliğin nasıl kullanılabileceğinin kısa bir açıklaması aşağıda veda edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="5bfa7-137">Statik bir yöntemi `DbContext` bildirin ve açıklamanız: `DbFunctionAttribute`</span><span class="sxs-lookup"><span data-stu-id="5bfa7-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new NotImplementedException();
    }
}
```

<span data-ttu-id="5bfa7-138">Bu gibi yöntemler otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="5bfa7-139">Bir kez kaydedildikten sonra, LINQ sorgusundaki yönteme yapılan çağrılar SQL'deki işlev çağrılarına çevrilebilir:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="5bfa7-140">Dikkat edilecek birkaç şey:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-140">A few things to note:</span></span>

- <span data-ttu-id="5bfa7-141">Kural olarak yöntemin adı, SQL'i oluştururken bir işlevin adı (bu durumda kullanıcı tarafından tanımlanan bir işlev) olarak kullanılır, ancak yöntem kaydı sırasında adı ve şema geçersiz kılınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-141">By convention the name of the method is used as the name of a function (in this case a user-defined function) when generating the SQL, but you can override the name and schema during method registration.</span></span>
- <span data-ttu-id="5bfa7-142">Şu anda yalnızca skaler işlevler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-142">Currently only scalar functions are supported.</span></span>
- <span data-ttu-id="5bfa7-143">Veritabanında eşlenen işlevi oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="5bfa7-144">EF Core geçişleri onu oluşturmaya dikkat etmez.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-144">EF Core migrations will not take care of creating it.</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="5bfa7-145">Önce kod için bağımsız tip yapılandırması</span><span class="sxs-lookup"><span data-stu-id="5bfa7-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="5bfa7-146">EF6'da *EntityTypeConfiguration'dan*türeerek belirli bir varlık türünün kod ilk yapılandırmasını kapsüllemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="5bfa7-147">EF Core 2.0'da bu deseni geri getiriyoruz:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="5bfa7-148">Yüksek Performans</span><span class="sxs-lookup"><span data-stu-id="5bfa7-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="5bfa7-149">DbContext havuzlama</span><span class="sxs-lookup"><span data-stu-id="5bfa7-149">DbContext pooling</span></span>

<span data-ttu-id="5bfa7-150">EF Core'u bir ASP.NET Core uygulamasında kullanmak için temel desen genellikle bağımlılık enjeksiyon sistemine özel bir DbContext türünü kaydetmeyi ve daha sonra denetleyicilerde yapıcı parametreler aracılığıyla bu tür örnekleri elde etmektir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="5bfa7-151">Bu, her istek için DbContext'ın yeni bir örneğinin oluşturulduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-151">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="5bfa7-152">Sürüm 2.0'da, yeniden kullanılabilir DbContext örnekleri havuzunü saydam bir şekilde tanıtan bağımlılık enjeksiyonuna özel DbContext türlerini kaydetmek için yeni bir yol sıyoruz.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="5bfa7-153">DbContext havuzunu kullanmak için, `AddDbContext` hizmet kaydı sırasında `AddDbContextPool` yerine kullanın:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="5bfa7-154">Bu yöntem kullanılırsa, bir DbContext örneği bir denetleyici tarafından istendiğinde, havuzda kullanılabilir bir örnek olup olmadığını önce denetleriz.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="5bfa7-155">İstek işleme sonuçlandıktan sonra, örnekteki herhangi bir durum sıfırlanır ve örnek havuza döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="5bfa7-156">Bu, kavramsal olarak bağlantı havuzunun ADO.NET sağlayıcılarda nasıl çalıştığına benzer ve DbContext örneğinin başlatma maliyetinin bir kısmını kaydetme avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="5bfa7-157">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="5bfa7-157">Limitations</span></span>

<span data-ttu-id="5bfa7-158">Yeni yöntem, DbContext `OnConfiguring()` yönteminde neler yapılabileceğini birkaç sınırlama lar getirsin.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="5bfa7-159">Türemiş DbContext sınıfınızda istekler arasında paylaşılmaması gereken kendi durumunuzu (örneğin, özel alanlar) koruyorsanız, DbContext Pooling'i kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="5bfa7-160">EF Core yalnızca havuza bir DbContext örneği eklemeden önce farkında olan durumu sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-160">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="5bfa7-161">Açıkça derlenmiş sorgular</span><span class="sxs-lookup"><span data-stu-id="5bfa7-161">Explicitly compiled queries</span></span>

<span data-ttu-id="5bfa7-162">Bu, yüksek ölçekli senaryolarda avantajlar sunmak üzere tasarlanmış ikinci tercih performansı özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="5bfa7-163">El ile veya açıkça derlenmiş sorgu API'leri, uygulamaların yalnızca bir kez hesaplanabilmesi ve birçok kez yürütülebilmesi için sorguların çevirisini önbelleğe almalarına izin vermek için EF'nin önceki sürümlerinde ve ayrıca LINQ'dan SQL'e sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="5bfa7-164">Genel olarak EF Core sorguları sorgu ifadelerinin karma gösterimine dayalı olarak otomatik olarak derleyebilir ve önbelleğe alabilir, ancak bu mekanizma karma ve önbellek aramasının hesaplamasını atlayarak küçük bir performans kazancı elde etmek için kullanılabilir ve uygulamanın bir temsilcinin çağırması yoluyla zaten derlenmiş bir sorgu kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="5bfa7-165">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="5bfa7-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="5bfa7-166">Ekle, yeni ve varolan varlıkların grafiğini izleyebilir</span><span class="sxs-lookup"><span data-stu-id="5bfa7-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="5bfa7-167">EF Core, çeşitli mekanizmalar aracılığıyla anahtar değerlerin otomatik olarak üretimi destekler.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="5bfa7-168">Bu özelliği kullanırken, anahtar özelliği CLR varsayılanı ise genellikle sıfır veya null olan bir değer oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="5bfa7-169">Bu, varlıkların bir `DbContext.Attach` grafiğine geçirilebileceği `DbSet.Attach` veya EF Core'un anahtar kümesi olmayan `Unchanged` varlıklar `Added`olarak işaretlenirken zaten ayarlanmış bir anahtara sahip varlıkları işaretleyeceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="5bfa7-170">Bu, oluşturulan anahtarları kullanırken karışık yeni ve varolan varlıkların bir grafiği eklemeyi kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="5bfa7-171">`DbContext.Update`ve `DbSet.Update` anahtar kümesine sahip varlıkların yerine `Modified` `Unchanged`'' olarak işaretli olması dışında, aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="5bfa7-172">Sorgu</span><span class="sxs-lookup"><span data-stu-id="5bfa7-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="5bfa7-173">Geliştirilmiş LINQ çevirisi</span><span class="sxs-lookup"><span data-stu-id="5bfa7-173">Improved LINQ translation</span></span>

<span data-ttu-id="5bfa7-174">Veritabanında daha fazla mantık (bellek yerine) değerlendirilen ve gereksiz yere veritabanından daha az veri alınarak daha fazla sorgunun başarıyla yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="5bfa7-175">GroupJoin geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="5bfa7-175">GroupJoin improvements</span></span>

<span data-ttu-id="5bfa7-176">Bu çalışma, grup birleştirmeleri için oluşturulan SQL geliştirir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="5bfa7-177">Grup birleştirmeleri genellikle isteğe bağlı gezinme özelliklerindeki alt sorguların bir sonucudur.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="5bfa7-178">FromSql ve ExecuteSqlCommand'da string enterpolasyonu</span><span class="sxs-lookup"><span data-stu-id="5bfa7-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="5bfa7-179">C# 6, C# ifadelerinin dize literallerine doğrudan gömülmesine olanak tanıyan ve çalışma zamanında dizeleri oluşturmanın güzel bir yolunu sağlayan String Enterpolasyon'u tanıttı.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="5bfa7-180">EF Core 2.0'da, ham SQL dizelerini kabul eden iki birincil API'mize enterpolasyonlu dizeleri için özel destek ekledik: `FromSql` ve `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="5bfa7-181">Bu yeni destek, C# dize enterpolasyonunun "güvenli" bir şekilde kullanılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-181">This new support allows C# string interpolation to be used in a "safe" manner.</span></span> <span data-ttu-id="5bfa7-182">Diğer bir deyişle, çalışma zamanında dinamik olarak SQL oluştururken oluşabilecek yaygın SQL enjeksiyon hatalarına karşı koruyan bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="5bfa7-183">Örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-183">Here is an example:</span></span>

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

<span data-ttu-id="5bfa7-184">Bu örnekte, SQL biçim dizesinde gömülü iki değişken vardır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="5bfa7-185">EF Core aşağıdaki SQL'i üretecektir:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="5bfa7-186">Ef. Fonksiyonlar.Like()</span><span class="sxs-lookup"><span data-stu-id="5bfa7-186">EF.Functions.Like()</span></span>

<span data-ttu-id="5bfa7-187">EF'yi ekledik. EFCore veya sağlayıcılar tarafından, LINQ sorgularında çağrılabilmeleri için veritabanı işlevlerine veya işleçlerle eşleyen yöntemleri tanımlamak için kullanılabilecek işlevler özelliği.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="5bfa7-188">Böyle bir yöntemin ilk örneği Like():</span><span class="sxs-lookup"><span data-stu-id="5bfa7-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="5bfa7-189">Like() bir bellek veritabanına karşı çalışırken veya yüklemin değerlendirilmesi istemci tarafında gerçekleşmesi gerektiğinde kullanışlı olabilir bir bellek içi uygulama ile birlikte geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="5bfa7-190">Veritabanı yönetimi</span><span class="sxs-lookup"><span data-stu-id="5bfa7-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="5bfa7-191">DbContext iskelesi için çoğulculuk kancası</span><span class="sxs-lookup"><span data-stu-id="5bfa7-191">Pluralization hook for DbContext scaffolding</span></span>

<span data-ttu-id="5bfa7-192">EF Core 2.0, varlık türü adlarını tekilleştirmek ve DbSet adlarını çoğullaştırmak için kullanılan yeni bir *IPluralizer* hizmetini sunar.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="5bfa7-193">Varsayılan uygulama bir no-op, bu yüzden bu sadece millet kolayca kendi çoğulcu takabilirsiniz bir kanca olduğunu.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="5bfa7-194">Burada kendi çoğulcu kanca bir geliştirici için nasıl göründüğünü:</span><span class="sxs-lookup"><span data-stu-id="5bfa7-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="5bfa7-195">Diğer</span><span class="sxs-lookup"><span data-stu-id="5bfa7-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="5bfa7-196">SQLite sağlayıcısıADO.NET SQLitePCL.raw'a taşıyın</span><span class="sxs-lookup"><span data-stu-id="5bfa7-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>

<span data-ttu-id="5bfa7-197">Bu bize farklı platformlarda yerel SQLite ikilileri dağıtmak için Microsoft.Data.Sqlite daha sağlam bir çözüm sağlar.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="5bfa7-198">Model başına yalnızca bir sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="5bfa7-198">Only one provider per model</span></span>

<span data-ttu-id="5bfa7-199">Sağlayıcıların modelle nasıl etkileşimde bulunabilirlerini önemli ölçüde artar ve sözleşmelerin, ek açıklamaların ve akıcı API'lerin farklı sağlayıcılarla nasıl çalıştığını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="5bfa7-200">EF Core 2.0 artık kullanılan her farklı sağlayıcı için farklı bir [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="5bfa7-201">Bu genellikle uygulama için saydamdır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-201">This is usually transparent to the application.</span></span> <span data-ttu-id="5bfa7-202">Bu, alt düzey meta veri API'lerinin basitleştirilmesini kolaylaştırmıştır, böylece *ortak ilişkisel meta veri kavramlarına* herhangi bir erişim her zaman `.Relational` `.SqlServer`, `.Sqlite`vb. yerine bir çağrı yoluyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="5bfa7-203">Birleştirilmiş günlük ve tanılama</span><span class="sxs-lookup"><span data-stu-id="5bfa7-203">Consolidated logging and diagnostics</span></span>

<span data-ttu-id="5bfa7-204">Günlük (ILogger dayalı) ve Tanılama (DiagnosticSource dayalı) mekanizmaları artık daha fazla kod paylaşmak.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="5bfa7-205">ILogger'a gönderilen iletilerin olay iD'leri 2.0'da değişti.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="5bfa7-206">Olay lı işlmeler, EF Core kodlarında şimşkırılık gününde benzersizdir</span><span class="sxs-lookup"><span data-stu-id="5bfa7-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="5bfa7-207">Bu iletiler artık MVC tarafından kullanılan yapılandırılmış günlüğe kaydetme için standart deseni de izler.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="5bfa7-208">Logger kategorileri de değişti.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-208">Logger categories have also changed.</span></span> <span data-ttu-id="5bfa7-209">Artık [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs)üzerinden erişilen tanınmış bir kategoriler kümesi var.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="5bfa7-210">DiagnosticSource olayları artık ilgili `ILogger` iletilerle aynı olay kimliği adlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="5bfa7-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
