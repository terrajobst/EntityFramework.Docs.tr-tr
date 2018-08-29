---
title: EF Core 2.0 - EF Core yenilikleri
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: b52b1fe6b2d5a585f4d55b0299891f61cbc968a3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997576"
---
# <a name="new-features-in-ef-core-20"></a>EF Core 2.0 yenilikleri

## <a name="net-standard-20"></a>.NET standard 2.0
EF Core, .NET Core 2.0, .NET Framework 4.6.1 ve .NET Standard 2.0 uygulayan diğer kitaplıkları ile çalışabilir yani .NET Standard 2.0, artık hedefler.
Bkz: [desteklenen .NET uygulamalarıyla](../platforms/index.md) desteklenen özellikler hakkında daha fazla bilgi.

## <a name="modeling"></a>Modelleme

### <a name="table-splitting"></a>Tablo bölme

Şimdi, burada birincil anahtar sütunları paylaşılır ve her satır için iki veya daha fazla varlık karşılık gelen aynı tabloya iki veya daha fazla varlık türleri eşleştirmek mümkündür.

Tablosunu kullanmak için (burada yabancı anahtar özellikleri birincil anahtar oluşturmak) tanımlayan bir ilişki bölme tüm tablo paylaşımı varlık türleri arasında yapılandırılması gerekir:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a>Sahip olunan türleri

Sahip olunan varlık türü aynı CLR türü başka bir sahip olunan varlık türü ile paylaşabilir, ancak yalnızca CLR türü tanımlanamıyor beri olmalıdır bir gezinti için başka bir varlık türü. Tanımlama Gezinti içeren varlık sahibi değil. Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir.

Kural gereği sahip olunan türü için gölge birincil bir anahtar oluşturulur ve onu aynı tablonun sahibi olarak tablo bölmeyi kullanarak eşleneceğini. Bu olanak için nasıl karmaşık türler içinde EF6 kullanılan ait kullanım benzer şekilde türleri:

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
Okuma [bölümüne sahip olunan varlık türleri](xref:core/modeling/owned-entities) bu özellik hakkında daha fazla bilgi için.

### <a name="model-level-query-filters"></a>Model düzeyi sorgu filtreleri

EF Core 2.0 Model düzeyi sorgu filtreleri diyoruz yeni bir özellik içerir. LINQ Sorgu koşullarına (genellikle nereye LINQ sorgu işleci için geçirilen bir Boole ifadesi) bu özelliği sağlar (genellikle, OnModelCreating) meta veri modeli varlık türleri üzerinde doğrudan tanımlanmalıdır. Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır. Bu özelliğin bazı ortak uygulamalar şunlardır:

- Geçici silme - IsDeleted özelliği bir varlık türlerini tanımlar.
- -Çok kiracılı bir varlık türü bir Tenantıd özelliği tanımlar.

Yukarıda listelenen iki senaryo için özellik gösteren basit bir örnek aşağıda verilmiştir:

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
Çok kiracılılık ve geçici silme için uygulayan bir model düzeyi filtresi tanımlarız örneklerini ```Post``` varlık türü. DbContext örnek düzeyi özelliği kullanımına dikkat edin: ```TenantId```. Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten bağlam örnek) doğru bağlam örneğinin değerini kullanır.

Filtreler IgnoreQueryFilters() işlecini kullanarak tek tek LINQ sorguları için devre dışı bırakılabilir.

#### <a name="limitations"></a>Sınırlamalar

- Gezinti başvurulara izin verilmez. Bu özellik, geri bildirim göre eklenebilir.
- Filtreler yalnızca kök varlık türü bir hiyerarşinin tanımlanabilir.

### <a name="database-scalar-function-mapping"></a>Veritabanı skaler bir işlev eşlemesi

EF Core 2.0 içeren gelen önemli bir katkı [Paul Middleton](https://github.com/pmiddleton) böylece bunlar, LINQ sorgularında kullanılan ve SQL çevrilmiş Saplamaları yöntemi için eşleme veritabanı skaler işlevler sağlar.

Aşağıda, özellikle nasıl kullanılabileceği hakkında kısa bir açıklaması verilmiştir:

Üzerinde statik bir yöntemi bildirmek, `DbContext` ve onunla açıklama `DbFunctionAttribute`:

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

Bu gibi yöntemleri otomatik olarak kaydedilir. Kaydedildikten sonra bir LINQ Sorgu yöntemine yönelik çağrılar işlev çağrıları SQL çevrilebilir:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Dikkat edilecek bazı noktalar:

- Yöntemin adı (Bu durumda bir işlevde kullanıcı tanımlı) bir işlev adı olarak kullanıldığında Kural gereği SQL, ancak oluşturma adını ve şemayı yöntemi kayıt sırasında geçersiz kılabilirsiniz
- Şu anda yalnızca skaler işlevler desteklenir
- Veritabanında eşleşen işlev oluşturmanız gerekir. EF Core geçişleri oluşturma ilgileniriz değil

### <a name="self-contained-type-configuration-for-code-first"></a>Kod için kendi içinde bulunan tür yapılandırma ilk

EF6 içinde kod ilk yapılandırma bir özel varlık türünün türeterek yalıtılacak olası *EntityTypeConfiguration*. EF Core 2.0 sürümünde Biz bu düzen geri getirme:

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

## <a name="high-performance"></a>Yüksek performans

### <a name="dbcontext-pooling"></a>DbContext havuzu

EF Core ASP.NET Core uygulaması genellikle kullanmak için temel düzeni, özel bir DbContext tür bağımlılık ekleme sistemine kaydetme ve daha sonra denetleyicileri Oluşturucusu parametrelerinde aracılığıyla bu türün örneklerini alma içerir. Başka bir deyişle, her istek için yeni bir örneğini DbContext oluşturulur.

Sürüm 2.0 şeffaf bir şekilde yeniden kullanılabilir DbContext örneklerinden oluşan bir havuz tanıtan bağımlılık ekleme özel DbContext türleri kaydetmek için yeni bir yolunu sunuyoruz. DbContext havuzu kullanmak için ```AddDbContextPool``` yerine ```AddDbContext``` hizmeti kaydı sırasında:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Bu yöntem kullanılıyorsa, zaman biz olup olmadığını örneği kullanılabilir havuzda ilk denetleyecek denetleyicisi tarafından bir DbContext örneği istenir. İstek işleme sonlandırır örneğinde herhangi bir durumu sıfırlanır ve kendisini havuza geri döner örnektir.

Bu, kavramsal olarak benzer nasıl bağlantı havuzu ADO.NET sağlayıcıları çalışır ve bazı DbContext örneğinin başlatma maliyetini kaydetme avantajına sahiptir.

### <a name="limitations"></a>Sınırlamalar

Yöntem ne yapılabilir bazı sınırlamalar sunar ```OnConfiguring()``` DbContext yöntemi.

> [!WARNING]  
> DbContext havuzu istekler genelinde Paylaşılmaması gereken, türetilmiş bir DbContext sınıfı içinde kendi durumu (örneğin, özel alanları) korur kullanmaktan kaçının. EF Core, yalnızca bir DbContext örneği havuza eklemeden önce farkında durumuna sıfırlar.

### <a name="explicitly-compiled-queries"></a>Açıkça derlenmiş sorgular

Büyük ölçekli senaryolar avantajları sunmak için tasarlanan ikinci katılımı performans özelliklerini budur.

API'leri EF'ın önceki sürümlerini hem de LINQ to SQL sorguları çevirisi önbelleğe alınması yalnızca bir kez hesaplanabilir uygulamalarının izin vermek için kullanılabilir ve birçok kez yürütülen el ile veya açıkça derlenmiş sorgusu.

Önbellek araması ve karma hesaplama atlayarak bir küçük bir performans kazancı elde etmek için genel EF Core otomatik olarak derleyin ve önbelleğe alma, karma bir sorgu ifadeleri gösterimini üzerinde temel alan sorguları olsa da, bu mekanizma kullanılabilir izin verme önceden derlenmiş bir sorgu bir temsilcinin Çağırma ile kullanmak için uygulama.

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

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Ekleme yeni ve var olan varlıkları grafiğini izleyebilirsiniz

EF Core çeşitli mekanizmalar aracılığıyla anahtar değerlerini otomatik olarak oluşturulmasını destekler. Anahtar özelliği CLR varsayılan--genellikle sıfır ya da null ise bu özelliği kullanırken, bir değer oluşturulur. Bu varlıkların bir grafik için geçirilebilir anlamına gelir `DbContext.Attach` veya `DbSet.Attach` ve EF Core zaten olarak ayarlanmış bir anahtara sahip kişilikleri işaretleyecek `Unchanged` kişilikleri anahtar kümesi olmayan olarak işaretlenir ancak `Added`. Bu karma, yeni bir grafik eklemek kolaylaştırır ve kullanırken var olan varlıkları anahtarları oluşturulur. `DbContext.Update` ve `DbSet.Update` anahtar kümesi ile varlıkları olarak işaretlenmiş dışında aynı şekilde, iş `Modified` yerine `Unchanged`.

## <a name="query"></a>Sorgu

### <a name="improved-linq-translation"></a>Gelişmiş LINQ çeviri

Daha fazla sorgu başarılı bir şekilde, daha fazla mantıksal veritabanı (yerine bellek içi) ve daha az gereksiz yere veritabanından alınan veri değerlendirilen çalışmasına olanak tanır.

### <a name="groupjoin-improvements"></a>GroupJoin geliştirmeleri

Bu iş grup birleştirmeleri için oluşturulan SQL artırır. Grup birleştirmeleri çoğunlukla bir isteğe bağlı Gezinti özellikleri üzerinde alt sorgularda sonucudur.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>SQL ve ExecuteSqlCommand dize ilişkilendirme

C# 6 yapı dizeleri çalışma zamanında iyi bir yol sağlayarak, dize ilişkilendirme dize değişmez değerleri, doğrudan gömülü olması C# ifadelerini sağlayan bir özelliği kullanıma sunuldu. EF Core 2.0 sürümünde ham SQL dizeleri kabul iki birincil Apı'lerimizi için ilişkilendirilmiş dizeler için özel destek eklendi: ```FromSql``` ve ```ExecuteSqlCommand```. Bu yeni Destek 'güvenli' bir şekilde kullanılacak C# dize ilişkilendirme sağlar. Diğer bir deyişle, dinamik olarak oluşabilir ortak SQL ekleme hatalarına karşı koruyan bir şekilde çalışma zamanında SQL oluşturma.

Aşağıda bir örnek verilmiştir:

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

Bu örnekte, iki değişken SQL Biçim dizesinde katıştırılmış vardır. EF Core aşağıdaki SQL oluşturacak:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF. Functions.Like()

EF ekledik. EF Core veya sağlayıcıları tarafından veritabanı işlevleri veya işleçler eşlemek ve böylece bunlar, LINQ sorgularında çağrılabilir yöntemleri tanımlamak için kullanılabilecek işlevleri özelliği. İlk tür bir yöntem Like() örneğidir:

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Bir bellek içi veritabanına karşı çalışırken veya değerlendirme, koşulun istemci tarafında ortaya gerektiğinde yararlı olabilen Like() bir bellek içi uygulamasıyla geldiğini unutmayın.

## <a name="database-management"></a>Veritabanı Yönetimi

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Çoğullaştırmayı kanca DbContext yapı İskelesi için

EF Core 2.0 tanıtır, yeni bir *IPluralizer* varlık singularize için kullanılan hizmet adlarını yazabilir ve pluralize olan DB adları. Varsayılan uygulaması bir İşlemsiz olduğundan burada istedim kendi pluralizer kolayca takılabilir bir kanca budur.

İşte bu şekilde kendi pluralizer yeteneklerinizi bir geliştiricinin görünür:

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

## <a name="others"></a>Diğerleri

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>ADO.NET SQLite sağlayıcısına SQLitePCL.raw Taşı
Bu daha sağlam bir çözüm içinde Microsoft.Data.Sqlite farklı platformlarda yerel bir SQLite ikili dağıtılmasında sağlıyor.

### <a name="only-one-provider-per-model"></a>Model başına yalnızca bir sağlayıcı
Önemli ölçüde artırmaktadır sağlayıcıları modeli ile nasıl etkileşim kurabileceğine ve kuralları, ek açıklamalar ve fluent API'ler farklı sağlayıcıları ile nasıl çalıştığını basitleştirir.

EF Core 2.0 artık farklı bir derler [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) için kullanılan her farklı bir sağlayıcı. Bu genellikle uygulamaya saydamdır. Tüm erişimi olacak şekilde bu düşük düzeyli meta verileri API'leri bir basitleştirme kolaylaştırılan *ortak ilişkisel meta veri kavramlarını* her zaman bir çağrı yoluyla yapılan `.Relational` yerine `.SqlServer`, `.Sqlite`, vb.

### <a name="consolidated-logging-and-diagnostics"></a>Birleştirilmiş günlüğe kaydetme ve tanılama

(Üzerinde ILogger göre) günlüğe kaydetme ve tanılama (üzerinde DiagnosticSource göre) mekanizmaları artık daha fazla kod paylaşın.

2. 0'için bir ILogger gönderilen iletiler için olay kimlikleri değişti. Olay kimlikleri, artık EF Core kod arasında benzersizdir. Bu iletiler şimdi de, örneğin, MVC kullanılan yapılandırılmış günlük kaydı için standart deseni izleyin.

Günlükçü kategoriler de değiştirilmiştir. İyi bilinen bir kategori kümesini aracılığıyla erişilen artık yoktur [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

DiagnosticSource olayları artık ilgili olarak aynı olay kimliği adları kullanma `ILogger` iletileri.
