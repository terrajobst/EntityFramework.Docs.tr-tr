---
title: Tasarım zamanı DbContext oluşturma-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197567"
---
<a name="design-time-dbcontext-creation"></a>Tasarım Zamanında DbContext Oluşturma
==============================
EF Core araçları komutlarından bazıları (örneğin, [geçişler][1] komutları), uygulamanın varlık türleri ve bir veritabanı `DbContext` şemasına nasıl eşlendikleri hakkında bilgi toplamak için tasarım zamanında bir türetilmiş örnek oluşturulmasını gerektirir. Çoğu durumda, `DbContext` bu nedenle oluşturulması, [çalışma zamanında nasıl yapılandırılacağından][2]benzer bir şekilde yapılandırılmalıdır.

Araçların `DbContext`şunları oluşturmayı denemenin çeşitli yolları vardır:

<a name="from-application-services"></a>Uygulama hizmetlerinden
-------------------------
Başlangıç projeniz [ASP.NET Core Web konağını][3] veya [.NET Core genel konağını][4]kullanıyorsa, Araçlar DbContext nesnesini uygulamanın hizmet sağlayıcısından almaya çalışır.

Araçlar, sonra `Program.CreateHostBuilder()` `Services` özelliğe erişen, çağırarak `Build()`ve ardından hizmet sağlayıcısını almaya çalışır.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> Yeni bir ASP.NET Core uygulaması oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir.

`DbContext` Kendisinin ve oluşturucusunun içindeki bağımlılıkların, uygulamanın hizmet sağlayıcısında hizmet olarak kaydedilmesi gerekir. Bu [, bir bağımsız değişken `DbContext` `DbContextOptions<TContext>` ][5] [ olarakbirörneğialanvemetodunukullanarak,üzerindebirOluşturucubulundurarakkolaycaeldeedilebilir.`AddDbContext<TContext>` ][6]

<a name="using-a-constructor-with-no-parameters"></a>Parametresiz bir Oluşturucu kullanma
--------------------------------------
DbContext uygulama hizmeti sağlayıcısından alınamıyorsa, Araçlar proje içindeki türetilmiş `DbContext` türü arar. Daha sonra parametresiz bir Oluşturucu kullanarak bir örnek oluşturmaya çalışır. Yöntemi kullanılarak yapılandırıldıysa, `DbContext` varsayılan Oluşturucu bu olabilir. [`OnConfiguring`][7]

<a name="from-a-design-time-factory"></a>Tasarım zamanı fabrikasından
--------------------------
Ayrıca, araçları uygulayarak `IDesignTimeDbContextFactory<TContext>` DbContext 'nizi nasıl oluşturacağınız hakkında da söyleyebilirsiniz: Bu arabirimi uygulayan bir sınıf, türetilen `DbContext` ya da uygulamanın başlangıç projesindeki aynı projede bulunursa, Araçlar DbContext oluşturmanın diğer yollarını atlar ve bunun yerine tasarım zamanı fabrikasını kullanır.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> `args` Parametresi şu anda kullanılmıyor. Araçlardan tasarım zamanı bağımsız değişkenlerini belirtme yeteneği izlenirken [bir sorun][8] oluştu.

Bir tasarım zamanı fabrikası, çalışma zamanından daha önce DbContext 'i tasarım zamanı için farklı şekilde yapılandırmanız gerektiğinde kullanışlı olabilir. `DbContext` bu, Eğer Eğer Eğer Eğer Eğer Eğer bir `BuildWebHost` ASP.NETCore`Main` uygulamanızın sınıfında bir yöntemi olmaması tercih edersiniz.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
