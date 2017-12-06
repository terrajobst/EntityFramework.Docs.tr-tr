---
title: "DbContext - EF çekirdek yapılandırma"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a>Bir yapılandırma

Bu makalede yapılandırma desenleri gösterilmektedir bir `DbContext` ile `DbContextOptions`. Seçenekler, öncelikle seçin ve veri deposunu yapılandırmak için kullanılır.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions yapılandırma

`DbContext`bir örneği olmalıdır `DbContextOptions` yürütmek için. Bu geçersiz kılma tarafından yapılandırılabilir `OnConfiguring`, ya da dışarıdan oluşturucu bağımsız değişkeni sağlanan.

Her ikisi de kullanılıyorsa, `OnConfiguring` ADDITIVE ve can olduğu anlamına gelir sağlanan seçeneklerin, yürütülen üzerine yazma seçeneklerini oluşturucu bağımsız değişkeni için sağlanan.

### <a name="constructor-argument"></a>Oluşturucu bağımsız değişkeni

Bağlam koduyla Oluşturucusu

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
> Temel DbContext oluşturucusunun genel olmayan sürümünü de kabul eder `DbContextOptions`. Genel olmayan sürümünü kullanarak birden fazla bağlam türü olan uygulamalar için önerilmez.

Uygulama kodu oluşturucu bağımsız değişkenden başlatmak için

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`Son oluşur ve üzerine yazma seçeneklerini dı veya oluşturucusu elde. Bu yaklaşım kendisini (tam veritabanı hedef sürece) sınama için ödünç değil.

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

Uygulama kodu ile başlatmak için `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>DbContext ile bağımlılık ekleme kullanılarak

EF destekleyen kullanarak `DbContext` bir bağımlılık ekleme kapsayıcısını ile. DbContext türü kullanarak hizmet kapsayıcısı eklenebilir `AddDbContext<TContext>`.

`AddDbContext`hem sizin DbContext türü, yapacak `TContext`, ve `DbContextOptions<TContext>` hizmet kapsayıcısından yerleştirme için kullanılabilir.

Bkz: [daha fazla okuma](#more-reading) aşağıda bağımlılık ekleme hakkında bilgi için.

Bağımlılık ekleme dbcontext ekleme

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Bu ekleme gerektiren bir [oluşturucu bağımsız değişkeni](#constructor-argument) kabul eder, DbContext türü `DbContextOptions`.

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
public MyController(BloggingContext context)
```

Uygulama kodu (ServiceProvider, daha az yaygın kullanarak doğrudan):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>Kullanma`IDesignTimeDbContextFactory<TContext>`

Yukarıdaki seçeneklerin alternatif olarak, bir uygulaması, ayrıca sağlayabilen `IDesignTimeDbContextFactory<TContext>`. EF araçları bu Fabrika, DbContext örneği oluşturmak için kullanabilirsiniz. Geçişler gibi belirli tasarım zamanı deneyimleri etkinleştirmek için gerekli olabilir.

Genel varsayılan bir oluşturucu yok bağlam türleri için tasarım zamanı hizmetlerini etkinleştirmek için bu arabirimi uygular. Tasarım zamanı Hizmetleri türetilmiş bağlamı olarak aynı bütünleştirilmiş kodda olan bu arabirim uygulamaları otomatik olarak bulur.

Örnek:

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a>Daha fazla okuma

* Okuma [Başlarken ASP.NET Core üzerinde](../get-started/aspnetcore/index.md) EF ASP.NET Core ile kullanma hakkında daha fazla bilgi için.
* Okuma [bağımlılık ekleme](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) dı kullanma hakkında daha fazla bilgi edinmek için.
* Okuma [test](testing/index.md) daha fazla bilgi için.
