---
title: "DbContext - EF çekirdek yapılandırma"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: de26e3b28851d4dc4e50f0490093dd05ad489b31
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="configuring-a-dbcontext"></a>Bir yapılandırma

Bu makalede yapılandırma temel düzenlerden gösterilmektedir bir `DbContext` aracılığıyla bir `DbContextOptions` belirli EF çekirdek sağlayıcısını ve isteğe bağlı davranışları kullanarak bir veritabanına bağlanmak için.

## <a name="design-time-dbcontext-configuration"></a>Tasarım zamanı DbContext yapılandırma

EF çekirdek tasarım zamanı araçları gibi [geçişler](xref:core/managing-schemas/migrations/index) bulmak ve bir çalışma örneği oluşturmak gereken bir `DbContext` uygulamanın varlık türlerini ve bunların bir veritabanı şeması nasıl eşleneceğine ayrıntılarını toplama amacıyla türü. Aracı kolayca oluşturabilir sürece bu işlemi otomatik olabilir `DbContext` , benzer şekilde nasıl çalışma zamanında yapılandırılması için yapılandırılır şekilde.

While gerekli yapılandırma bilgileri sağlar düzeni `DbContext` çalışma zamanında, kullanmak için gerekli araçları çalışabilir bir `DbContext` tasarım zamanında yalnızca sınırlı sayıda desenler ile çalışabilirsiniz. Bunlar daha ayrıntılı olarak ele alınmıştır [tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümü.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions yapılandırma

`DbContext`bir örneği olmalıdır `DbContextOptions` herhangi bir iş gerçekleştirmek için. `DbContextOptions` Örneği yapılandırma bilgilerini aşağıdaki gibi yapar:

- Genellikle seçili kullanmak için veritabanı sağlayıcısı gibi bir yöntemini çağırarak `UseSqlServer` veya`UseSqlite`
- Tüm gerekli bağlantı dizesi veya veritabanı örneğinin tanıtıcısı genellikle geçirilen bağımsız değişken olarak yukarıda belirtilen sağlayıcı seçimi yöntemi
- Sağlayıcı seçimi yöntem çağrısı içinde genellikle de zincirleme tüm sağlayıcısı düzeyi isteğe bağlı davranışını seçiciler
- Sağlayıcı Seçici yöntemi önce veya sonra genellikle zincirleme tüm genel EF çekirdek davranışı seçiciler

Aşağıdaki örnek yapılandırır `DbContextOptions` SQL Server sağlayıcısı kullanmak için bir bağlantı içinde yer alan `connectionString` değişkeni, sağlayıcı düzeyi komut zaman aşımı ve içinde yürütülen tüm sorguları yapar EF çekirdek davranışı Seçici `DbContext` [Hayır izleme](xref:core/querying/tracking#no-tracking-queries) varsayılan olarak:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Sağlayıcı Seçici ve yukarıda belirtilen diğer davranış Seçici yöntemleri olan genişletme yöntemleri `DbContextOptions` veya sağlayıcıya özgü seçenek sınıfları. Erişilebilmesi için bir ad alanı içerecek şekilde gerekebilir bu uzantı yöntemleri (genellikle `Microsoft.EntityFrameworkCore`) içinde kapsam ve ek paket bağımlılıklarını projeye ekleyin.

`DbContextOptions` İçin sağlanan `DbContext` kılarak `OnConfiguring` yöntemi veya oluşturucu bağımsız değişkeni aracılığıyla harici olarak.

Her ikisi de kullanılıyorsa, `OnConfiguring` son olarak uygulanır ve oluşturucu bağımsız değişkeni için sağlanan seçenekleri geçersiz kılabilirsiniz.

### <a name="constructor-argument"></a>Oluşturucu bağımsız değişkeni

Bağlam Kod Oluşturucusu ile:

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
> Temel DbContext oluşturucusunun genel olmayan sürümünü de kabul eder `DbContextOptions`, ancak genel olmayan sürümüyle önerilmez birden fazla bağlam türü olan uygulamalar için.

Oluşturucu bağımsız değişkenden başlatmak için uygulama kodu:

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
> Tam veritabanı testleri hedef sürece bu yaklaşım kendisini test için ödünç değil.

### <a name="using-dbcontext-with-dependency-injection"></a>DbContext ile bağımlılık ekleme kullanılarak

EF çekirdek destekleyen kullanarak `DbContext` bir bağımlılık ekleme kapsayıcısını ile. DbContext türü kullanarak hizmet kapsayıcısı eklenebilir `AddDbContext<TContext>` yöntemi.

`AddDbContext<TContext>`hem sizin DbContext türü, yapacak `TContext`ve karşılık gelen `DbContextOptions<TContext>` hizmet kapsayıcısından yerleştirme için kullanılabilir.

Bkz: [daha fazla okuma](#more-reading) aşağıda bağımlılık ekleme hakkında daha fazla bilgi için.

Ekleme `Dbcontext` bağımlılık ekleme için:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Bu ekleme gerektiren bir [oluşturucu bağımsız değişkeni](#constructor-argument) kabul eder, DbContext türü `DbContextOptions<TContext>`.

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

Uygulama kodu (ServiceProvider, daha az yaygın kullanarak doğrudan):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Daha fazla okuma

* Okuma [Başlarken ASP.NET Core üzerinde](../get-started/aspnetcore/index.md) EF ASP.NET Core ile kullanma hakkında daha fazla bilgi için.
* Okuma [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) dı kullanma hakkında daha fazla bilgi edinmek için.
* Okuma [test](testing/index.md) daha fazla bilgi için.
