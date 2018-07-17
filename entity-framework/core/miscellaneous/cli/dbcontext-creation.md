---
title: Tasarım zamanında DbContext oluşturma - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 7c16017d3b97d115841050fe6ac0fdbeb5e71d94
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067537"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="462e8-102">Tasarım zamanında DbContext oluşturma</span><span class="sxs-lookup"><span data-stu-id="462e8-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="462e8-103">EF Core araçları komutlardan bazıları (örneğin, [geçişler] [ 1] komutları) türetilmiş gerektiren `DbContext` uygulamanın ayrıntılarını toplamak için tasarım zamanında oluşturulacak örnek varlık türleri ve bunların veritabanı şemasına nasıl eşleneceğine.</span><span class="sxs-lookup"><span data-stu-id="462e8-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="462e8-104">Çoğu durumda, tercih edilir, `DbContext` böylece oluşturulan nasıl olacaktır benzer bir biçimde yapılandırılmış [çalışma zamanında yapılandırılmış][2].</span><span class="sxs-lookup"><span data-stu-id="462e8-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="462e8-105">Araçları deneyebilirsiniz oluşturmak için çeşitli yollar vardır `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="462e8-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="462e8-106">Uygulama Hizmetleri'nden</span><span class="sxs-lookup"><span data-stu-id="462e8-106">From application services</span></span>
-------------------------
<span data-ttu-id="462e8-107">Başlangıç projeniz bir ASP.NET Core uygulaması ise, uygulamanın hizmet sağlayıcısından DbContext nesnesini almak Araçlar'ı deneyin.</span><span class="sxs-lookup"><span data-stu-id="462e8-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="462e8-108">Aracı ilk deneyin çağırarak hizmet sağlayıcısı edinmek `Program.BuildWebHost()` erişerek `IWebHost.Services` özelliği.</span><span class="sxs-lookup"><span data-stu-id="462e8-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="462e8-109">Yeni bir ASP.NET Core 2.0 uygulama oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="462e8-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="462e8-110">Önceki sürümlerinde EF Core ve ASP.NET Core, çağrılacak araçları deneyebilirsiniz `Startup.ConfigureServices` doğrudan uygulamanın hizmet sağlayıcısı, ancak bu düzen artık almak için düzgün şekilde ASP.NET Core 2.0 uygulamaları çalışır.</span><span class="sxs-lookup"><span data-stu-id="462e8-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="462e8-111">ASP.NET Core 1.x uygulamaya 2.0 yükseltiyorsanız, şunları yapabilirsiniz [değiştirmek, `Program` yeni desende sınıfı][3].</span><span class="sxs-lookup"><span data-stu-id="462e8-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="462e8-112">`DbContext` Kendisi ve herhangi bir bağımlılığın oluşturucusuna Hizmetleri'nde uygulamanın hizmet sağlayıcısı olarak kaydedilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="462e8-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="462e8-113">Bu kolayca sağlanarak elde edilebilir [bir oluşturucu `DbContext` örneğini alır `DbContextOptions<TContext>` bir bağımsız değişken olarak] [ 4] ve kullanarak [ `AddDbContext<TContext>` yöntemi] [5].</span><span class="sxs-lookup"><span data-stu-id="462e8-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="462e8-114">Parametresiz bir oluşturucu kullanılarak</span><span class="sxs-lookup"><span data-stu-id="462e8-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="462e8-115">Uygulama hizmet sağlayıcısından DbContext alınamıyorsa araçları için türetilmiş Ara `DbContext` türü proje içinde.</span><span class="sxs-lookup"><span data-stu-id="462e8-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="462e8-116">Ardından parametresiz bir Oluşturucusu kullanarak örneği oluşturmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="462e8-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="462e8-117">Bu varsayılan oluşturucu olabilir `DbContext` kullanılarak yapılandırılan [ `OnConfiguring` ] [ 6] yöntemi.</span><span class="sxs-lookup"><span data-stu-id="462e8-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="462e8-118">Tasarım zamanı fabrikadan</span><span class="sxs-lookup"><span data-stu-id="462e8-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="462e8-119">Uygulayarak, DbContext oluşturma araçları da söyleyebilirsiniz `IDesignTimeDbContextFactory<TContext>` arabirimi: Bu arabirimi uygulayan bir sınıfa veya türetilmiş projenin bulunursa `DbContext` veya uygulamanın başlangıç projesi araçları atlama DbContext ve tasarım zamanı Fabrika kullanmak yerine oluşturmanın diğer yolu.</span><span class="sxs-lookup"><span data-stu-id="462e8-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="462e8-120">`args` Parametresi, şu anda kullanılmayan.</span><span class="sxs-lookup"><span data-stu-id="462e8-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="462e8-121">Var. [soruna] [ 7] izleme araçları tasarım zamanı bağımsız değişkenleri belirtme olanağı.</span><span class="sxs-lookup"><span data-stu-id="462e8-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="462e8-122">Tasarım zamanı Fabrika DbContext çalışma zamanında tasarım zamanından farklı şekilde yapılandırmak, gerekirse özellikle kullanışlı olabilir `DbContext` ek parametreler kaydedilmedi DI içinde DI hiç kullanmıyorsanız Oluşturucusu alır ya da Eğer bazı için tercih ettiğiniz yüklüyse neden bir `BuildWebHost` ASP.NET Core uygulamanızın yönteminde `Main` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="462e8-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
