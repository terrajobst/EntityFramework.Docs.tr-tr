---
title: EF çekirdek 2.0 - EF çekirdek yenilikler nelerdir?
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 02d0b6fe2956e819e08e08c9a0658008abd36c34
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29680164"
---
# <a name="new-features-in-ef-core-20"></a>EF çekirdek 2.0 yeni özellikler

## <a name="net-standard-20"></a>.NET Standard 2.0
EF çekirdek şimdi .NET Core 2.0, .NET Framework 4.6.1 ve .NET standart 2.0 uygulayan diğer kitaplıkları ile çalışabilirsiniz anlamı .NET standart 2.0, hedefler.
Bkz: [desteklenen .NET uygulamalarında](../platforms/index.md) nelerin desteklendiği hakkında daha fazla bilgi.

## <a name="modeling"></a>Modelleme

### <a name="table-splitting"></a>Tablo bölme

Artık, burada birincil anahtar sütunları paylaşılır ve her satır için iki veya daha fazla varlık karşılık gelen aynı tabloda iki veya daha fazla varlık türlerine eşlemek mümkündür.

Tablosunu kullanmak için (burada yabancı anahtar özelliklerini birincil anahtar oluşturmak) bir tanımlayıcı ilişkisi bölme tüm tablo paylaşımı varlık türleri yapılandırılması gerekir:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a>Ait türleri

Başka bir ait varlık türü ile aynı CLR türüne ait varlık türü paylaşabilir, ancak yalnızca CLR türü tarafından tanımlanamıyor beri olmalıdır bir gezinti için başka bir varlık türü. Tanımlayan gezinmeyi içeren varlık sahibi değil. Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir.

Kurala göre ait türü için gölge birincil bir anahtar oluşturulur ve onu sahibi olarak aynı tabloya tablo bölme kullanarak eşleşecektir. Bunun yapılması sağlar nasıl karmaşık türler EF6 kullanılan ait kullanım benzer şekilde türleri:

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
Okuma [ait varlık türleri bölümüne](xref:core/modeling/owned-entities) bu özellik hakkında daha fazla bilgi için.

### <a name="model-level-query-filters"></a>Model düzeyindeki sorgu filtreleri

EF çekirdek 2.0 Model düzeyindeki sorgu filtreleri diyoruz yeni bir özellik içerir. LINQ Sorgu koşulları (genellikle burada LINQ sorgu işleci için geçirilen Boole ifadesi) bu özelliği sağlar (genellikle içinde OnModelCreating) meta veri modelindeki varlık türlerinde doğrudan tanımlanmamış. Bu tür filtreleri, varlık türleri INCLUDE kullanımıyla dolaylı olarak gibi başvurulan veya doğrudan gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ Sorgu otomatik olarak uygulanır. Bu özellik, bazı ortak uygulamalar şunlardır:

- Yumuşak delete - IsDeleted özelliği bir varlık türlerini tanımlar.
- Çoklu kiracı - bir varlık türü bir Tenantıd özelliği tanımlar.

Aşağıda, yukarıda listelenen iki senaryo özelliğini gösteren basit bir örnek verilmiştir:

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
Çoklu kiracı ve soft-delete uygulayan bir model düzeyindeki filtre tanımlarız örneklerini ```Post``` varlık türü. DbContext örnek düzeyi özelliği kullanımına dikkat edin: ```TenantId```. Model düzeyindeki filtreleri doğru bağlamı örneğinden değeri kullanır. Yani sorgu yürütülürken bir.

Filtreler IgnoreQueryFilters() işlecini kullanarak tek tek LINQ sorguları için devre dışı olabilir.

#### <a name="limitations"></a>Sınırlamalar

- Gezinti başvurulara izin verilmez. Bu özellik, geri bildirim göre eklenebilir.
- Filtreler yalnızca varlık türü bir hiyerarşinin kökünde tanımlanabilir.

### <a name="database-scalar-function-mapping"></a>Veritabanı skaler işlev eşleme

EF çekirdek 2.0 içeren gelen önemli bir katkı [Paul Middleton](https://github.com/pmiddleton) yöntemine eşleme veritabanı skaler işlevler sağlayan böylece bunlar LINQ sorgularında kullanılır ve SQL çevrilen yerleştirir.

Aşağıda, özellik nasıl kullanılabileceğini kısa bir açıklaması verilmiştir:

Üzerinde bir statik yöntem bildirin, `DbContext` ve onunla açıklama `DbFunctionAttribute`:

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

Bu gibi yöntemleri otomatik olarak kaydedilir. Kaydedildikten sonra bir LINQ Sorgu yöntemine yönelik çağrılar SQL işlevi çağrılarının çevrilebilecek:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Dikkat edilecek bazı noktalar:

- Yöntemin adı bir işlev (Bu durumda kullanıcı tanımlı bir işlev) adı olarak kullanıldığında kurala göre SQL, ancak oluşturma adı ve şema yöntemi kaydı sırasında geçersiz kılabilirsiniz
- Şu anda yalnızca skaler işlevler desteklenir
- Veritabanını, örneğin geçişler oluşturma ilgilenebilmek değil EF çekirdek eşlenen işlevi oluşturmanız gerekir

### <a name="self-contained-type-configuration-for-code-first"></a>Kod için kendi içinde bulunan tür yapılandırma ilk

İçinde EF6 türetme tarafından belirli bir varlık türünün kodu ilk yapılandırması kapsülleyen mümkün *EntityTypeConfiguration*. EF çekirdek 2. 0'biz bu deseni geri getirme:

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

Özel bir DbContext türü bağımlılık ekleme sistemine kaydetme ve daha sonra denetleyicileriyle Oluşturucu parametreleri üzerinden bu türdeki örneklerin alma EF çekirdeği ASP.NET Core uygulamada genellikle kullanmak için temel düzeni içerir. Başka bir deyişle, DbContext yeni bir örneğini her istek için oluşturulur.

Sürüm 2.0, saydam olarak yeniden kullanılabilir DbContext örneklerinin bir havuzu tanıtır bağımlılık ekleme özel DbContext türleri kaydetmek için yeni bir yol sunuyoruz. DbContext havuzu kullanmak için ```AddDbContextPool``` yerine ```AddDbContext``` hizmet kaydı sırasında:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Bu yöntem kullandıysanız, aynı anda biz öncelikle olup olmadığını örneği kullanılabilir havuzda kontrol eder denetleyicisi tarafından bir DbContext örneği istendi. İstek işleme sonlandırır sonra örneği üzerinde herhangi bir durumu sıfırlanır ve kendisini havuza geri döner örneğidir.

Nasıl bağlantı havuzu ADO.NET sağlayıcıları çalışır ve bazı DbContext örneğinin başlatma maliyetini kaydetme avantajı vardır, bu kavramsal olarak benzer.

### <a name="limitations"></a>Sınırlamalar

Bazı sınırlamalar ne yapılabilir iki yöntem sunar ```OnConfiguring()``` DbContext yöntemi.

> [!WARNING]  
> İstekler genelinde Paylaşılmaması gereken, türetilmiş bir DbContext sınıfı içinde kendi durumunu (örneğin, özel alanları) korumak DbContext havuzu kullanarak kaçının. EF çekirdek yalnızca DbContext örnek havuzuna eklemeden önce farkında durumuna sıfırlar.

### <a name="explicitly-compiled-queries"></a>Açıkça derlenmiş sorguları

Büyük ölçekli senaryolarda yararlar için tasarlanmış ikinci katılımı performans özellikleri budur.

API EF önceki sürümlerini hem de LINQ-SQL sorguları çevrilmesi yalnızca bir kez hesaplanabilir böylece önbelleğe almak uygulamaların izin vermek için kullanılabilir ve birçok kez yürütülmüş el ile veya açıkça derlenmiş sorgu.

Karma ve önbellek araması hesaplama atlayarak küçük performans kazanç elde etmek için genel EF çekirdek otomatik olarak derleyin ve önbellek karma bir sorgu ifadeleri gösterimini üzerinde temel alan sorgular rağmen bu mekanizma kullanılabilir izin verme önceden derlenmiş bir sorgu çağırma temsilcisi ile kullanmak için uygulama.

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

## <a name="change-tracking"></a>Değişiklik izleme

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Attach yeni ve mevcut varlıkların bir grafik izleyebilirsiniz

EF çekirdek çeşitli mekanizmalar anahtarı değerlerini otomatik olarak oluşturulmasını destekler. Anahtar özelliği CLR varsayılan--genellikle sıfır ya da null ise, bu özelliği kullanıldığında, bir değer oluşturulur. Bu varlıkların bir grafik için geçirilebilir anlamına gelir `DbContext.Attach` veya `DbSet.Attach` ve EF çekirdek zaten olarak ayarlanmış bir anahtara sahip bu varlıkların işaretleyecek `Unchanged` bir anahtar kümesi yok Bu varlıkların olarak işaretlenmiş ancak `Added`. Bu karma, yeni bir grafik eklemek kolaylaştırır ve anahtarlarını kullanırken, var olan varlıkları oluşturulur. `DbContext.Update` ve `DbSet.Update` bir anahtar kümesi ile varlıklar olarak işaretlenmiş dışında aynı şekilde, iş `Modified` yerine `Unchanged`.

## <a name="query"></a>Sorgu

### <a name="improved-linq-translation"></a>Geliştirilmiş LINQ çevirisi

Başarılı bir şekilde, veritabanı (yerine bellek içi) ve daha az gereksiz yere veritabanından alınan veri değerlendirilen daha fazla mantığı ile yürütmek daha fazla sorguları sağlar.

### <a name="groupjoin-improvements"></a>GroupJoin geliştirmeleri

Bu çalışma grubu birleştirmeler için oluşturulan SQL artırır. Grup birleştirmeleri çoğunlukla isteğe bağlı Gezinti özellikleri üzerinde alt sorgularda sonucudur.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>FromSql ve ExecuteSqlCommand dize ilişkilendirme

C# 6 dize yapı dizeleri çalışma zamanında iyi bir yol sağlayarak ilişkilendirme, dize değişmez değerleri doğrudan katıştırılacak C# ifadeleri sağlayan bir özelliği kullanıma sunuldu. EF çekirdek 2.0 ara değerli dizeler için özel destek ham SQL dizeleri kabul bizim iki birincil API'leri için eklediğimiz: ```FromSql``` ve ```ExecuteSqlCommand```. Bu yeni Destek 'güvenli' bir biçimde kullanılacak C# ilişkilendirme dize verir. Yani bir şekilde, SQL çalışma zamanında dinamik olarak oluşturma sırasında oluşabilecek yaygın SQL Yerleştirme hataları karşı korur.

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

Bu örnekte, iki değişken SQL biçim dizesine katıştırılmış vardır. EF çekirdek aşağıdaki SQL üretir:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

EF ekledik. EF çekirdek veya sağlayıcıları tarafından veritabanı işlevleri veya işleçleri eşleme ve böylece bu LINQ sorgularında çağrılabilir yöntemlerini tanımlamak için kullanılabilir işlevler özelliği. Bu tür bir yöntem ilk örneği Like() verilmiştir:

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%");
    select c;
```

Bellek içi veritabanına karşı çalışırken veya değerlendirme, koşulun istemci tarafında ortaya gerektiğinde faydalı olabilen Like() bir bellek içi uygulama ile geldiğini unutmayın.

## <a name="database-management"></a>Veritabanı Yönetimi

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>DbContext İskele çoğullaştırma kanca

EF çekirdek 2.0 tanıtır yeni *IPluralizer* varlık tekil hale getirin için kullanılan hizmet adlarını yazın ve DbSet adlarını çoğul. Burada insanların kendi pluralizer kolayca ekleyebilirsiniz bir kanca bu nedenle varsayılan bir no-op, uygulamasıdır.

İşte bu şekilde bir geliştirici, kendi pluralizer kanca için görünür:

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

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>ADO.NET SQLite sağlayıcısına SQLitePCL.raw taşıma
Bu bize daha güçlü bir çözüm içinde Microsoft.Data.Sqlite farklı platformlarda yerel SQLite ikili dosyaları dağıtılmasında sağlar.

### <a name="only-one-provider-per-model"></a>Model başına yalnızca bir sağlayıcı
Önemli ölçüde sağlayıcıları modeli ile nasıl etkileşim kurabilirsiniz güçlendirir ve kuralları, ek açıklamalar ve fluent API'ları farklı sağlayıcıları ile nasıl basitleştirir.

EF çekirdek 2.0 artık farklı bir oluşturacaksınız [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) kullanılan her farklı bir sağlayıcı için. Bu, genellikle uygulamanız için saydamdır. Erişim herhangi bir şekilde bu alt düzey meta verilerin API'leri basitleştirme sayesinde kolaylaşır *ortak ilişkisel meta verileri kavramlar* her zaman bir çağrı aracılığıyla yapılan `.Relational` yerine `.SqlServer`, `.Sqlite`vb.

### <a name="consolidated-logging-and-diagnostics"></a>Birleştirilmiş günlüğe kaydetme ve tanılama

(ILogger üzerinde göre) günlüğe kaydetme ve tanılama (üzerinde DiagnosticSource bağlı olarak) mekanizmaları artık daha fazla kod paylaşır.

2. 0'bir ILogger gönderilen iletiler için olay kimlikleri değişti. Olay kimlikleri, şimdi EF çekirdek kodu arasında benzersizdir. Bu iletiler de artık yapılandırılmış günlük kullandığı, örneğin, MVC için standart yol izler.

Günlükçü kategoriler de değiştirilmiştir. Kategoriler iyi bilinen bir dizi üzerinden erişilen artık yoktur [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

DiagnosticSource olaylarını şimdi ilgili olarak aynı olay kimliği adları kullanma `ILogger` iletileri.
