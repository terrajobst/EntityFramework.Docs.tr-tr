---
title: EF Core - DbContext yapılandırma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: f5a9ae17471391442170d8c40264e4db6922cb08
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980008"
---
# <a name="configuring-a-dbcontext"></a>DbContext yapılandırma

Yapılandırma temel düzenlerden Bu makale bir `DbContext` aracılığıyla bir `DbContextOptions` belirli bir EF Core sağlayıcısını ve isteğe bağlı davranışları kullanarak bir veritabanına bağlanmak için.

## <a name="design-time-dbcontext-configuration"></a>Tasarım zamanında DbContext yapılandırma

EF Core tasarım zamanı araçlarını gibi [geçişler](xref:core/managing-schemas/migrations/index) bulmak ve bir çalışma örneğini oluşturmak gereken bir `DbContext` uygulamanın varlık türleri ve bunların veritabanı şemasına nasıl eşleneceğine hakkındaki ayrıntıları toplamak için türü. Aracı bir kolayca oluşturabilir sürece bu işlem otomatik olabilir `DbContext` şekilde, benzer şekilde nasıl, çalışma zamanında yapılandırılır için yapılandırılır.

While için gerekli yapılandırma bilgileri sağlayan herhangi bir desen `DbContext` çalışma zamanında, kullanılmasını araçları çalışabilir bir `DbContext` tasarım zamanında yalnızca sınırlı sayıda desenleri ile çalışabilir. Bunlar daha ayrıntılı olarak ele alınmaktadır [tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümü.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions yapılandırma

`DbContext` bir örneğine sahip olması `DbContextOptions` herhangi bir çalışmayı gerçekleştirmek için. `DbContextOptions` Örneği yapılandırma bilgileri gibi yapar:

- Veritabanı sağlayıcısı kullanmak için genellikle seçili bir yöntemi çağırarak `UseSqlServer` veya `UseSqlite`. Bu uzantı yöntemleri gibi ilgili sağlayıcı paketi gereken `Microsoft.EntityFrameworkCore.SqlServer` veya `Microsoft.EntityFrameworkCore.Sqlite`. Yöntemleri tanımlanan `Microsoft.EntityFrameworkCore` ad alanı.
- Herhangi bir gerekli bağlantı dizesini veya veritabanı örneğinin tanımlayıcı genellikle geçirilen bağımsız değişken olarak yukarıda belirtilen sağlayıcısı seçme yöntemi
- Genellikle sağlayıcısı seçme yöntemi çağrısı içinde da zincirleme tüm sağlayıcısı düzeyi isteğe bağlı davranışını seçiciler
- Sağlayıcı Seçici yöntemi önce veya sonra genellikle zincirleme tüm genel EF Core davranışı seçiciler

Aşağıdaki örnek yapılandırır `DbContextOptions` ve SQL Server sağlayıcıyı kullanmak için bir bağlantı bulunan `connectionString` değişkeni, bir sağlayıcı düzeyi komut zaman aşımı ve içinde yürütülen tüm sorguları yapan bir EF Core davranışı Seçici `DbContext` [Hayır izleme](xref:core/querying/tracking#no-tracking-queries) varsayılan olarak:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Sağlayıcı Seçici ve yukarıda belirtilen diğer davranışı Seçici yöntemleri üzerinde genişletme yöntemleri olan `DbContextOptions` veya sağlayıcıya özgü seçenek sınıfı. Bir ad alanı sağlamak için ihtiyacınız olan bu genişletme yöntemlerini erişimi için (genellikle `Microsoft.EntityFrameworkCore`), kapsam ve projede ek paket bağımlılıklarını içerir.

`DbContextOptions` İçin sağlanan `DbContext` kılarak `OnConfiguring` yöntem veya oluşturucu bağımsız değişkeni aracılığıyla harici olarak.

Her ikisi de kullanılıyorsa `OnConfiguring` son uygulanan ve oluşturucu bağımsız değişkeni için sağlanan seçenekleri geçersiz kılabilirsiniz.

### <a name="constructor-argument"></a>Oluşturucu bağımsız değişkeni

Bağlam Kod Oluşturucu ile:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> DbContext, temel oluşturucu, genel olmayan sürümü de kabul eder `DbContextOptions`, ancak genel olmayan sürümüyle önerilmez birden çok içerik türü olan uygulamalar için.

Oluşturucu bağımsız değişkeninden başlatmak için uygulama kodu:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Bağlam koduyla `OnConfiguring`:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Uygulama kodu başlatmak için bir `DbContext` kullanan `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Testlerin tam bir veritabanı hedef sürece bu yaklaşımın kendisi test için uygun olmayan.

### <a name="using-dbcontext-with-dependency-injection"></a>DbContext bağımlılık ekleme ile kullanma

EF Core destekler kullanarak `DbContext` bağımlılık ekleme kapsayıcısına sahip. DbContext türünüz kullanarak hizmet kapsayıcıya eklenebilir `AddDbContext<TContext>` yöntemi.

`AddDbContext<TContext>` Her iki DbContext türünüzü hale getirecek `TContext`ve karşılık gelen `DbContextOptions<TContext>` hizmet kapsayıcısından yerleştirme için kullanılabilir.

Bkz: [daha fazla okuma](#more-reading) aşağıdaki bağımlılık ekleme hakkında daha fazla bilgi için.

Ekleme `Dbcontext` bağımlılık ekleme:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Bu ekleme gerektirir bir [oluşturucu bağımsız değişkeni](#constructor-argument) kabul eden DbContext türünüz için `DbContextOptions<TContext>`.

Bağlam kodu:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Uygulama kodunda (ASP.NET Core):

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Uygulama kodu (doğrudan kullanarak daha az ortak ServiceProvider):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Daha fazla okuma

* Okuma [Başlarken üzerinde ASP.NET Core](../get-started/aspnetcore/index.md) EF ASP.NET Core ile kullanma hakkında daha fazla bilgi için.
* Okuma [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI kullanma hakkında daha fazla bilgi edinmek için.
* Okuma [test](testing/index.md) daha fazla bilgi için.
