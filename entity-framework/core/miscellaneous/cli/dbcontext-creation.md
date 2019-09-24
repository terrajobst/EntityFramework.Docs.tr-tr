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
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="4db8c-102">Tasarım Zamanında DbContext Oluşturma</span><span class="sxs-lookup"><span data-stu-id="4db8c-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="4db8c-103">EF Core araçları komutlarından bazıları (örneğin, [geçişler][1] komutları), uygulamanın varlık türleri ve bir veritabanı `DbContext` şemasına nasıl eşlendikleri hakkında bilgi toplamak için tasarım zamanında bir türetilmiş örnek oluşturulmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4db8c-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="4db8c-104">Çoğu durumda, `DbContext` bu nedenle oluşturulması, [çalışma zamanında nasıl yapılandırılacağından][2]benzer bir şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db8c-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="4db8c-105">Araçların `DbContext`şunları oluşturmayı denemenin çeşitli yolları vardır:</span><span class="sxs-lookup"><span data-stu-id="4db8c-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="4db8c-106">Uygulama hizmetlerinden</span><span class="sxs-lookup"><span data-stu-id="4db8c-106">From application services</span></span>
-------------------------
<span data-ttu-id="4db8c-107">Başlangıç projeniz [ASP.NET Core Web konağını][3] veya [.NET Core genel konağını][4]kullanıyorsa, Araçlar DbContext nesnesini uygulamanın hizmet sağlayıcısından almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db8c-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="4db8c-108">Araçlar, sonra `Program.CreateHostBuilder()` `Services` özelliğe erişen, çağırarak `Build()`ve ardından hizmet sağlayıcısını almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db8c-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

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
> <span data-ttu-id="4db8c-109">Yeni bir ASP.NET Core uygulaması oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="4db8c-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="4db8c-110">`DbContext` Kendisinin ve oluşturucusunun içindeki bağımlılıkların, uygulamanın hizmet sağlayıcısında hizmet olarak kaydedilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4db8c-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="4db8c-111">Bu [, bir bağımsız değişken `DbContext` `DbContextOptions<TContext>` ][5] [ olarakbirörneğialanvemetodunukullanarak,üzerindebirOluşturucubulundurarakkolaycaeldeedilebilir.`AddDbContext<TContext>` ][6]</span><span class="sxs-lookup"><span data-stu-id="4db8c-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="4db8c-112">Parametresiz bir Oluşturucu kullanma</span><span class="sxs-lookup"><span data-stu-id="4db8c-112">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="4db8c-113">DbContext uygulama hizmeti sağlayıcısından alınamıyorsa, Araçlar proje içindeki türetilmiş `DbContext` türü arar.</span><span class="sxs-lookup"><span data-stu-id="4db8c-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="4db8c-114">Daha sonra parametresiz bir Oluşturucu kullanarak bir örnek oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db8c-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="4db8c-115">Yöntemi kullanılarak yapılandırıldıysa, `DbContext` varsayılan Oluşturucu bu olabilir. [`OnConfiguring`][7]</span><span class="sxs-lookup"><span data-stu-id="4db8c-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="4db8c-116">Tasarım zamanı fabrikasından</span><span class="sxs-lookup"><span data-stu-id="4db8c-116">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="4db8c-117">Ayrıca, araçları uygulayarak `IDesignTimeDbContextFactory<TContext>` DbContext 'nizi nasıl oluşturacağınız hakkında da söyleyebilirsiniz: Bu arabirimi uygulayan bir sınıf, türetilen `DbContext` ya da uygulamanın başlangıç projesindeki aynı projede bulunursa, Araçlar DbContext oluşturmanın diğer yollarını atlar ve bunun yerine tasarım zamanı fabrikasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db8c-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="4db8c-118">`args` Parametresi şu anda kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="4db8c-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="4db8c-119">Araçlardan tasarım zamanı bağımsız değişkenlerini belirtme yeteneği izlenirken [bir sorun][8] oluştu.</span><span class="sxs-lookup"><span data-stu-id="4db8c-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="4db8c-120">Bir tasarım zamanı fabrikası, çalışma zamanından daha önce DbContext 'i tasarım zamanı için farklı şekilde yapılandırmanız gerektiğinde kullanışlı olabilir. `DbContext` bu, Eğer Eğer Eğer Eğer Eğer Eğer bir `BuildWebHost` ASP.NETCore`Main` uygulamanızın sınıfında bir yöntemi olmaması tercih edersiniz.</span><span class="sxs-lookup"><span data-stu-id="4db8c-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
