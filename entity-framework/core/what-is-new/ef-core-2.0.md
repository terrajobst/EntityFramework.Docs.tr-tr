---
title: EF Core 2,0 ' deki yenilikler-EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 83f6b819409d502dba17a678d44a0746a4a77f4b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417498"
---
# <a name="new-features-in-ef-core-20"></a>EF Core 2,0 ' deki yeni özellikler

## <a name="net-standard-20"></a>.NET Standard 2,0

EF Core artık .NET Standard 2,0 ' i hedeflediğinden, .NET Core 2,0, .NET Framework 4.6.1 ve .NET Standard 2,0 uygulayan diğer kitaplıklarla birlikte çalışabilir.
Desteklenen özellikler hakkında daha fazla bilgi için bkz. [desteklenen .NET uygulamaları](../platforms/index.md) .

## <a name="modeling"></a>Modelleme

### <a name="table-splitting"></a>Tablo bölme

Artık iki veya daha fazla varlık türünü birincil anahtar sütunların paylaşılacağını ve her satırın iki veya daha fazla varlığa karşılık geldiğini aynı tabloyla eşleştirmek mümkündür.

Tabloyu kullanarak bir tanımlayıcı ilişkiyi (birincil anahtar olan yabancı anahtar özellikler formu) kullanmak için tabloyu paylaşan tüm varlık türleri arasında yapılandırılması gerekir:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

Bu özellik hakkında daha fazla bilgi için [tablo bölme bölümündeki bölümü](xref:core/modeling/table-splitting) okuyun.

### <a name="owned-types"></a>Sahip olunan türler

Sahip olunan bir varlık türü, aynı .NET türünü sahip olan başka bir varlık türüyle paylaşabilir, ancak yalnızca .NET türü tarafından tanımlanamıyorsa, başka bir varlık türünden bir gezinti olması gerekir. Tanımlama gezintisini içeren varlık, sahibidir. Sahibi sorgulanırken sahip olan türler varsayılan olarak dahil edilir.

Kurala göre, sahip olunan tür için bir gölge birincil anahtar oluşturulur ve tablo bölme kullanılarak aynı tabloyla aynı tabloyla eşleştirilir. Bu, EF6 ' de karmaşık türlerin kullanılmasına benzer şekilde sahip olan türleri kullanılmasına izin verir:

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

Bu özellik hakkında daha fazla bilgi için, [sahip olduğunuz varlık türlerindeki bölümü](xref:core/modeling/owned-entities) okuyun.

### <a name="model-level-query-filters"></a>Model düzeyi sorgu filtreleri

EF Core 2,0, model düzeyi sorgu filtrelerini çağırdığımız yeni bir özellik içeriyor. Bu özellik LINQ sorgu koşullarına (genellikle LINQ WHERE sorgu işlecine geçirilen bir Boole ifadesi) meta veri modelindeki varlık türlerinde (genellikle Onmodeloluþturma 'da) doğrudan tanımlanacak şekilde izin verir. Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır. Bu özelliğin bazı ortak uygulamalar şunlardır:

- Geçici silme-bir varlık türü, IsDeleted özelliğini tanımlar.
- Çok kiracılı-varlık türü bir Tenantıd özelliğini tanımlar.

Aşağıda listelenen iki senaryo için özelliği gösteren basit bir örnek aşağıda verilmiştir:

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

`Post` varlık türünün örnekleri için çok kiracılı ve geçici silme uygulayan model düzeyinde bir filtre tanımlandık. `DbContext` örnek düzeyi özelliğinin kullanımını aklınızda kullanın: `TenantId`. Model düzeyi filtreleri doğru bağlam örneğindeki değeri kullanır (diğer bir deyişle, sorguyu yürüten bağlam örneği).

Filtreler, IgnoreQueryFilters () işleci kullanılarak tekil LINQ sorguları için devre dışı bırakılabilir.

#### <a name="limitations"></a>Sınırlamalar

- Gezinti başvurularına izin verilmiyor. Bu özellik geri bildirime göre eklenebilir.
- Filtreler yalnızca bir hiyerarşinin kök varlık türünde tanımlanabilir.

### <a name="database-scalar-function-mapping"></a>Veritabanı skaler işlev eşlemesi

EF Core 2,0, [Paul Middleton](https://github.com/pmiddleton) 'TAN, LINQ sorgularında KULLANıLABILMESI ve SQL 'e çevrilebilmesi için, veritabanı skaler işlevlerinin Yöntem saplamalarla eşleştirilmesini sağlayan önemli bir katkı içerir.

Özelliğin nasıl kullanılabileceği hakkında kısa bir açıklama aşağıda verilmiştir:

`DbContext` statik bir yöntem bildirin ve `DbFunctionAttribute`ekleyin:

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

Bunun gibi yöntemler otomatik olarak kaydedilir. Kaydedildikten sonra, LINQ sorgusundaki yöntemine yapılan çağrılar SQL 'de işlev çağrılarına çevrilebilir:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Dikkat edilecek birkaç şey:

- Kurala göre yöntemin adı, SQL oluştururken bir işlevin adı (Bu örnekte Kullanıcı tanımlı bir işlev) olarak kullanılır, ancak yöntem kaydı sırasında adı ve şemayı geçersiz kılabilirsiniz.
- Şu anda yalnızca skaler işlevler desteklenir.
- Eşlenmiş işlevi veritabanında oluşturmanız gerekir. EF Core geçişler bunu oluşturmak için gerekli olmayacaktır.

### <a name="self-contained-type-configuration-for-code-first"></a>Önce kod için kendi kendine içerilen tür yapılandırması

EF6 içinde, *EntityTypeConfiguration*'dan türeterek belirli bir varlık türünün kod ilk yapılandırmasını kapsüllemek mümkün. EF Core 2,0 ' de bu kalıbı geri sunuyoruz:

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

### <a name="dbcontext-pooling"></a>DbContext havuzu

Bir ASP.NET Core uygulamasındaki EF Core kullanmanın temel deseninin genellikle özel bir DbContext türünün bağımlılık ekleme sistemine kaydedilmesi ve daha sonra bu türün örneklerini denetleyicilerde Oluşturucu parametreleri aracılığıyla elde edilmesi gerekir. Bu, her istek için yeni bir DbContext örneği oluşturulduğu anlamına gelir.

Sürüm 2,0 ' de, özel DbContext türlerini, bir yeniden kullanılabilir DbContext örnekleri havuzunu saydam bir şekilde sunan bağımlılık ekleme 'ye kaydetmek için yeni bir yol sunuyoruz. DbContext havuzunu kullanmak için hizmet kaydı sırasında `AddDbContext` yerine `AddDbContextPool` kullanın:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Bu yöntem kullanılırsa, bir DbContext örneği bir denetleyici tarafından istendiğinde, önce havuzda kullanılabilir bir örnek olup olmadığını kontrol edeceğiz. İstek işleme sonlandırıldıktan sonra, örnekteki herhangi bir durum sıfırlanır ve örnek havuza döndürülür.

Bu, ADO.NET sağlayıcıları 'nda bağlantı havuzunun nasıl çalıştığı ve DbContext örneği başlatma maliyetinin bir kısmını kaydetme avantajına benzer.

### <a name="limitations"></a>Sınırlamalar

New Yöntemi, DbContext 'in `OnConfiguring()` yönteminde neler yapabileceğinize ilişkin birkaç sınırlama getirir.

> [!WARNING]  
> İstek genelinde paylaşılmaması gereken türetilmiş DbContext sınıfınıza kendi eyaletinizi (örneğin, özel alanlar) korumanız durumunda DbContext havuzlamayı kullanmaktan kaçının. EF Core, havuza bir DbContext örneği eklemeden önce yalnızca farkında olan durumu sıfırlayacaktır.

### <a name="explicitly-compiled-queries"></a>Açıkça derlenmiş sorgular

Bu, yüksek ölçekli senaryolarda avantajlar sunmak için tasarlanan ikinci bir katılım performansı özelliğidir.

El ile veya açıkça derlenmiş sorgu API 'Leri, daha önceki EF sürümlerinde ve ayrıca, uygulamaların yalnızca bir kez hesaplanabilmesi ve birçok kez yürütülebilmeleri için sorguların çevirisini önbelleğe almak üzere LINQ to SQL ' de kullanıma sunulmuştur.

Genel EF Core, sorgu ifadelerinin karma hale getirilmiş bir gösterimine göre sorguları otomatik olarak derleyip önbelleğe alabilir, bu mekanizma, karma ve önbellek araması hesaplamasını atlayarak küçük bir performans kazancı elde etmek için kullanılabilir ve buna izin verebilir bir temsilci çağrısı aracılığıyla zaten derlenmiş bir sorguyu kullanacak şekilde uygulama.

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

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Attach, yeni ve var olan varlıkların bir grafiğini izleyebilir

EF Core, çeşitli mekanizmalarda anahtar değerlerini otomatik olarak oluşturmayı destekler. Bu özellik kullanılırken, anahtar özelliği CLR varsayılan ise (genellikle sıfır veya null) bir değer oluşturulur. Bu, bir varlık grafiğinin `DbContext.Attach` veya `DbSet.Attach` geçirilecek olabileceği anlamına gelir; EF Core anahtar kümesi olmayan bu varlıklar `Added`olarak işaretlenirken, bir anahtara sahip olan varlıkların zaten `Unchanged` olarak ayarlanmış olduğunu işaretleyecek. Bu, oluşturulan anahtarlar kullanılırken karışık yeni ve var olan varlıkların bir grafiğini eklemeyi kolaylaştırır. `DbContext.Update` ve `DbSet.Update` aynı şekilde çalışır, ancak anahtar kümesi olan varlıklar `Unchanged`yerine `Modified` olarak işaretlenir.

## <a name="query"></a>Sorgu

### <a name="improved-linq-translation"></a>Geliştirilmiş LINQ çevirisi

Veritabanında daha fazla mantığın (bellek içi değil) ve veritabanından gereksiz bir şekilde veri alınmasından daha fazla mantığı olan başarılı şekilde yürütmek için daha fazla sorgu sağlar.

### <a name="groupjoin-improvements"></a>Groupjoın geliştirmeleri

Bu çalışma, Grup birleşimleri için oluşturulan SQL 'i geliştirir. Grup birleştirmeleri genellikle isteğe bağlı gezinme özelliklerindeki alt sorguların bir sonucudur.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>FromSql ve ExecuteSqlCommand 'da dize ilişkilendirme

C#6, C# ifadelerin dize değişmez değerlerinde doğrudan gömülmesini sağlayan, çalışma zamanında dizeler oluşturmanın iyi bir yolunu sağlayan bir özellik olan dize ilişkilendirmeyi sunmuştur. EF Core 2,0 ' de, ham SQL dizelerini kabul eden iki birincil API 'imize enterpolasyonlu dizeler için özel destek ekledik: `FromSql` ve `ExecuteSqlCommand`. Bu yeni destek, C# dize ilişkilendirbir "güvenli" biçimde kullanılmasına izin verir. Diğer bir deyişle, çalışma zamanında dinamik olarak SQL oluştururken ortaya çıkabilecek yaygın SQL ekleme hatalarına karşı koruma sağlar.

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

Bu örnekte, SQL biçim dizesinde gömülü iki değişken vardır. EF Core aşağıdaki SQL 'i oluşturacak:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>Aşv. Functions. like ()

EF 'i ekledik. İşlev özelliği, LINQ sorgularında çağrılabilecek şekilde veritabanı işlevleri veya işleçleriyle eşlenen yöntemleri tanımlamak için EF Core veya sağlayıcılar tarafından kullanılabilir. Bu tür bir yöntemin ilk örneği () gibidir:

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Örneğin (), bellek içi bir veritabanına göre çalışırken veya koşulun değerlendirmesi istemci tarafında gerçekleşeceği zaman yararlı olabilecek, bellek içi bir uygulamayla birlikte geldiğini unutmayın.

## <a name="database-management"></a>Veritabanı yönetimi

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>DbContext scafkatlaması için pluralization kancası

EF Core 2,0, varlık türü adlarını ve pluri DbSet adlarını bir kez eklemek için kullanılan yeni bir *ıpluralizer* hizmeti sunar. Varsayılan uygulama bir op değildir, bu nedenle bu yalnızca katlara kendi pluralizer 'ı kolayca ekleyebileceğiniz bir kanca olur.

Geliştirici kendi pluralizer ' de kanca için aşağıdaki gibidir:

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

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>ADO.NET SQLite sağlayıcısını SQLitePCL. RAW öğesine taşı

Bu, yerel SQLite ikililerini farklı platformlarda dağıtmak için Microsoft. Data. sqlite ' da daha sağlam bir çözüm sunar.

### <a name="only-one-provider-per-model"></a>Her model için yalnızca bir sağlayıcı

Sağlayıcıların modelle nasıl etkileşime gireceğini önemli ölçüde genişlettiğini ve kuralların, ek açıklamaların ve akıcı API 'Lerin farklı sağlayıcılarla nasıl çalıştığını basitleştirir.

EF Core 2,0, artık kullanılan her farklı sağlayıcı için farklı bir [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) oluşturacak. Bu genellikle uygulama için saydamdır. Bu, alt düzey meta veri API 'Lerinin basitleştirdiğini, *yaygın ilişkisel meta veri kavramlarını* her zaman `.SqlServer`, `.Sqlite`vb. yerine `.Relational` çağrısıyla yapılır.

### <a name="consolidated-logging-and-diagnostics"></a>Birleştirilmiş günlüğe kaydetme ve tanılama

Günlüğe kaydetme (ILogger tabanlı) ve tanılama (DiagnosticSource tabanlı) mekanizmalarına artık daha fazla kod paylaşmalıdır.

Bir ILogger 'a gönderilen iletiler için olay kimlikleri 2,0 içinde değişmiştir. Olay kimlikleri artık EF Core kod genelinde benzersizdir. Bu iletiler artık, örneğin MVC gibi kullanılan yapılandırılmış günlük için standart kalıbı de izler.

Günlükçü kategorileri de değişmiştir. Artık [Dbloggercategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs)aracılığıyla erişilen iyi bilinen bir kategori kümesi vardır.

DiagnosticSource olayları artık karşılık gelen `ILogger` iletileriyle aynı olay KIMLIĞI adlarını kullanır.
