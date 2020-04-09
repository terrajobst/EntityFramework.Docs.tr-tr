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
# <a name="new-features-in-ef-core-20"></a>EF Core 2.0'daki yeni özellikler

## <a name="net-standard-20"></a>.NET Standart 2.0

EF Core artık .NET Standard 2.0'ı hedefliyor, bu da .NET Core 2.0, .NET Framework 4.6.1 ve .NET Standard 2.0'ı uygulayan diğer kitaplıklarla çalışabileceği anlamına geliyor.
[Desteklenenler](../platforms/index.md) hakkında daha fazla bilgi için Desteklenen .NET Uygulamaları'na bakın.

## <a name="modeling"></a>Modelleme

### <a name="table-splitting"></a>Tablo bölme

Artık iki veya daha fazla varlık türünü birincil anahtar sütunun(lar) paylaşılacak ve her satırın iki veya daha fazla varlığa karşılık geldiği aynı tabloyla eşlemek mümkündür.

Tabloyu tanımlayan bir ilişkiyi bölmek için (yabancı anahtar özelliklerinin birincil anahtarı oluşturduğu) tabloyu paylaşan tüm varlık türleri arasında yapılandırılması gerekir:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

Bu özellik hakkında daha fazla bilgi için [tablo bölme bölümünü](xref:core/modeling/table-splitting) okuyun.

### <a name="owned-types"></a>Sahip olunan türler

Sahip olunan bir varlık türü aynı .NET türünü başka bir sahip olunan varlık türüyle paylaşabilir, ancak sadece .NET türüyle tanımlanamadığı için başka bir varlık türünden bir gezinti olması gerekir. Tanımlayıcı gezintiyi içeren varlık sahibidir. Sahibini sorgularken, sahip olunan türler varsayılan olarak eklenecektir.

Kural olarak, sahip olunan tür için bir gölge birincil anahtar oluşturulur ve tablo bölme kullanılarak sahibiyle aynı tabloya eşlenir. Bu, ef6'da karmaşık türlerin nasıl kullanıldığına benzer şekilde sahip olunan türleri kullanmanıza olanak tanır:

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

Bu özellik hakkında daha fazla bilgi için [sahip olunan varlık türleri hakkındaki bölümü](xref:core/modeling/owned-entities) okuyun.

### <a name="model-level-query-filters"></a>Model düzeyinde sorgu filtreleri

EF Core 2.0, Model düzeyinde sorgu filtreleri dediğimiz yeni bir özellik içerir. Bu özellik, LINQ sorgu yüklemlerinin (genellikle LINQ Where query işlecine geçirilen boolean ifadesinin) meta veri modelindeki Varlık Türleri üzerinde (genellikle OnModelCreating'da) doğrudan tanımlanmasına olanak tanır. Bu tür filtreler, Dolaylı olarak başvurulan Varlık Türleri de dahil olmak üzere, bu Varlık Türlerini içeren tüm LINQ sorgularına (Örneğin veya doğrudan gezinme özelliği başvuruları nın kullanımı yoluyla) otomatik olarak uygulanır. Bu özelliğin bazı yaygın uygulamaları şunlardır:

- Yumuşak silme - Bir Varlık Türleri Silinmiş özelliği tanımlar.
- Çoklu kira - Varlık Türü Kiracı Kimliği özelliğini tanımlar.

Aşağıda, yukarıda listelenen iki senaryo için özelliği gösteren basit bir örnek verilmiştir:

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

`Post` Varlık Türü örnekleri için çoklu kira ve yumuşak silme uygulayan bir model düzeyinde filtre tanımlıyoruz. Örnek düzeyinde bir `DbContext` özelliğin kullanımına `TenantId`dikkat edin: . Model düzeyindefiltreler, doğru bağlam örneğindeki değeri (diğer bir şekilde, sorguyu yürüten bağlam örneği) kullanır.

Filtreler, IgnoreQueryFilters() işleci kullanarak tek tek LINQ sorguları için devre dışı bırakılmış olabilir.

#### <a name="limitations"></a>Sınırlamalar

- Gezinti başvurularına izin verilmez. Bu özellik geri bildirime dayalı olarak eklenebilir.
- Filtreler yalnızca bir hiyerarşinin kök Varlık Türü'nde tanımlanabilir.

### <a name="database-scalar-function-mapping"></a>Veritabanı skaler fonksiyon eşleme

EF Core 2.0, [Paul Middleton'ın,](https://github.com/pmiddleton) veritabanı skaler işlevlerini linq sorgularında kullanılabilmesi ve SQL'e çevrilebilmesi için yöntem saplamalarına olanak tanıyan önemli bir katkısını içerir.

Özelliğin nasıl kullanılabileceğinin kısa bir açıklaması aşağıda veda edebilirsiniz:

Statik bir yöntemi `DbContext` bildirin ve açıklamanız: `DbFunctionAttribute`

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

Bu gibi yöntemler otomatik olarak kaydedilir. Bir kez kaydedildikten sonra, LINQ sorgusundaki yönteme yapılan çağrılar SQL'deki işlev çağrılarına çevrilebilir:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Dikkat edilecek birkaç şey:

- Kural olarak yöntemin adı, SQL'i oluştururken bir işlevin adı (bu durumda kullanıcı tarafından tanımlanan bir işlev) olarak kullanılır, ancak yöntem kaydı sırasında adı ve şema geçersiz kılınabilirsiniz.
- Şu anda yalnızca skaler işlevler desteklenir.
- Veritabanında eşlenen işlevi oluşturmanız gerekir. EF Core geçişleri onu oluşturmaya dikkat etmez.

### <a name="self-contained-type-configuration-for-code-first"></a>Önce kod için bağımsız tip yapılandırması

EF6'da *EntityTypeConfiguration'dan*türeerek belirli bir varlık türünün kod ilk yapılandırmasını kapsüllemek mümkündür. EF Core 2.0'da bu deseni geri getiriyoruz:

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

## <a name="high-performance"></a>Yüksek Performans

### <a name="dbcontext-pooling"></a>DbContext havuzlama

EF Core'u bir ASP.NET Core uygulamasında kullanmak için temel desen genellikle bağımlılık enjeksiyon sistemine özel bir DbContext türünü kaydetmeyi ve daha sonra denetleyicilerde yapıcı parametreler aracılığıyla bu tür örnekleri elde etmektir. Bu, her istek için DbContext'ın yeni bir örneğinin oluşturulduğu anlamına gelir.

Sürüm 2.0'da, yeniden kullanılabilir DbContext örnekleri havuzunü saydam bir şekilde tanıtan bağımlılık enjeksiyonuna özel DbContext türlerini kaydetmek için yeni bir yol sıyoruz. DbContext havuzunu kullanmak için, `AddDbContext` hizmet kaydı sırasında `AddDbContextPool` yerine kullanın:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Bu yöntem kullanılırsa, bir DbContext örneği bir denetleyici tarafından istendiğinde, havuzda kullanılabilir bir örnek olup olmadığını önce denetleriz. İstek işleme sonuçlandıktan sonra, örnekteki herhangi bir durum sıfırlanır ve örnek havuza döndürülür.

Bu, kavramsal olarak bağlantı havuzunun ADO.NET sağlayıcılarda nasıl çalıştığına benzer ve DbContext örneğinin başlatma maliyetinin bir kısmını kaydetme avantajına sahiptir.

### <a name="limitations"></a>Sınırlamalar

Yeni yöntem, DbContext `OnConfiguring()` yönteminde neler yapılabileceğini birkaç sınırlama lar getirsin.

> [!WARNING]  
> Türemiş DbContext sınıfınızda istekler arasında paylaşılmaması gereken kendi durumunuzu (örneğin, özel alanlar) koruyorsanız, DbContext Pooling'i kullanmaktan kaçının. EF Core yalnızca havuza bir DbContext örneği eklemeden önce farkında olan durumu sıfırlar.

### <a name="explicitly-compiled-queries"></a>Açıkça derlenmiş sorgular

Bu, yüksek ölçekli senaryolarda avantajlar sunmak üzere tasarlanmış ikinci tercih performansı özelliğidir.

El ile veya açıkça derlenmiş sorgu API'leri, uygulamaların yalnızca bir kez hesaplanabilmesi ve birçok kez yürütülebilmesi için sorguların çevirisini önbelleğe almalarına izin vermek için EF'nin önceki sürümlerinde ve ayrıca LINQ'dan SQL'e sunulmuştur.

Genel olarak EF Core sorguları sorgu ifadelerinin karma gösterimine dayalı olarak otomatik olarak derleyebilir ve önbelleğe alabilir, ancak bu mekanizma karma ve önbellek aramasının hesaplamasını atlayarak küçük bir performans kazancı elde etmek için kullanılabilir ve uygulamanın bir temsilcinin çağırması yoluyla zaten derlenmiş bir sorgu kullanmasına izin verir.

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

## <a name="change-tracking"></a>Değişiklik İzleme

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Ekle, yeni ve varolan varlıkların grafiğini izleyebilir

EF Core, çeşitli mekanizmalar aracılığıyla anahtar değerlerin otomatik olarak üretimi destekler. Bu özelliği kullanırken, anahtar özelliği CLR varsayılanı ise genellikle sıfır veya null olan bir değer oluşturulur. Bu, varlıkların bir `DbContext.Attach` grafiğine geçirilebileceği `DbSet.Attach` veya EF Core'un anahtar kümesi olmayan `Unchanged` varlıklar `Added`olarak işaretlenirken zaten ayarlanmış bir anahtara sahip varlıkları işaretleyeceği anlamına gelir. Bu, oluşturulan anahtarları kullanırken karışık yeni ve varolan varlıkların bir grafiği eklemeyi kolaylaştırır. `DbContext.Update`ve `DbSet.Update` anahtar kümesine sahip varlıkların yerine `Modified` `Unchanged`'' olarak işaretli olması dışında, aynı şekilde çalışır.

## <a name="query"></a>Sorgu

### <a name="improved-linq-translation"></a>Geliştirilmiş LINQ çevirisi

Veritabanında daha fazla mantık (bellek yerine) değerlendirilen ve gereksiz yere veritabanından daha az veri alınarak daha fazla sorgunun başarıyla yürütülmesini sağlar.

### <a name="groupjoin-improvements"></a>GroupJoin geliştirmeleri

Bu çalışma, grup birleştirmeleri için oluşturulan SQL geliştirir. Grup birleştirmeleri genellikle isteğe bağlı gezinme özelliklerindeki alt sorguların bir sonucudur.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>FromSql ve ExecuteSqlCommand'da string enterpolasyonu

C# 6, C# ifadelerinin dize literallerine doğrudan gömülmesine olanak tanıyan ve çalışma zamanında dizeleri oluşturmanın güzel bir yolunu sağlayan String Enterpolasyon'u tanıttı. EF Core 2.0'da, ham SQL dizelerini kabul eden iki birincil API'mize enterpolasyonlu dizeleri için özel destek ekledik: `FromSql` ve `ExecuteSqlCommand`. Bu yeni destek, C# dize enterpolasyonunun "güvenli" bir şekilde kullanılmasını sağlar. Diğer bir deyişle, çalışma zamanında dinamik olarak SQL oluştururken oluşabilecek yaygın SQL enjeksiyon hatalarına karşı koruyan bir şekilde.

Örnek aşağıda verilmiştir:

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

Bu örnekte, SQL biçim dizesinde gömülü iki değişken vardır. EF Core aşağıdaki SQL'i üretecektir:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>Ef. Fonksiyonlar.Like()

EF'yi ekledik. EFCore veya sağlayıcılar tarafından, LINQ sorgularında çağrılabilmeleri için veritabanı işlevlerine veya işleçlerle eşleyen yöntemleri tanımlamak için kullanılabilecek işlevler özelliği. Böyle bir yöntemin ilk örneği Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Like() bir bellek veritabanına karşı çalışırken veya yüklemin değerlendirilmesi istemci tarafında gerçekleşmesi gerektiğinde kullanışlı olabilir bir bellek içi uygulama ile birlikte geldiğini unutmayın.

## <a name="database-management"></a>Veritabanı yönetimi

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>DbContext iskelesi için çoğulculuk kancası

EF Core 2.0, varlık türü adlarını tekilleştirmek ve DbSet adlarını çoğullaştırmak için kullanılan yeni bir *IPluralizer* hizmetini sunar. Varsayılan uygulama bir no-op, bu yüzden bu sadece millet kolayca kendi çoğulcu takabilirsiniz bir kanca olduğunu.

Burada kendi çoğulcu kanca bir geliştirici için nasıl göründüğünü:

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

## <a name="others"></a>Diğer

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>SQLite sağlayıcısıADO.NET SQLitePCL.raw'a taşıyın

Bu bize farklı platformlarda yerel SQLite ikilileri dağıtmak için Microsoft.Data.Sqlite daha sağlam bir çözüm sağlar.

### <a name="only-one-provider-per-model"></a>Model başına yalnızca bir sağlayıcı

Sağlayıcıların modelle nasıl etkileşimde bulunabilirlerini önemli ölçüde artar ve sözleşmelerin, ek açıklamaların ve akıcı API'lerin farklı sağlayıcılarla nasıl çalıştığını basitleştirir.

EF Core 2.0 artık kullanılan her farklı sağlayıcı için farklı bir [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) oluşturacak. Bu genellikle uygulama için saydamdır. Bu, alt düzey meta veri API'lerinin basitleştirilmesini kolaylaştırmıştır, böylece *ortak ilişkisel meta veri kavramlarına* herhangi bir erişim her zaman `.Relational` `.SqlServer`, `.Sqlite`vb. yerine bir çağrı yoluyla yapılır.

### <a name="consolidated-logging-and-diagnostics"></a>Birleştirilmiş günlük ve tanılama

Günlük (ILogger dayalı) ve Tanılama (DiagnosticSource dayalı) mekanizmaları artık daha fazla kod paylaşmak.

ILogger'a gönderilen iletilerin olay iD'leri 2.0'da değişti. Olay lı işlmeler, EF Core kodlarında şimşkırılık gününde benzersizdir Bu iletiler artık MVC tarafından kullanılan yapılandırılmış günlüğe kaydetme için standart deseni de izler.

Logger kategorileri de değişti. Artık [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs)üzerinden erişilen tanınmış bir kategoriler kümesi var.

DiagnosticSource olayları artık ilgili `ILogger` iletilerle aynı olay kimliği adlarını kullanır.
