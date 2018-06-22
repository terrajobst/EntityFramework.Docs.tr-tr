---
title: Tasarım zamanı DbContext oluşturma - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202490"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="c0d1b-102">Tasarım zamanı DbContext oluşturma</span><span class="sxs-lookup"><span data-stu-id="c0d1b-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="c0d1b-103">Bazı EF çekirdek araçları komutlar (örneğin, [geçişler] [ 1] komutları) türetilmiş gerektiren `DbContext` uygulamanın ayrıntılarını toplama amacıyla tasarım zamanında oluşturulacak örneği varlık türlerini ve bunların bir veritabanı şeması nasıl eşleneceğine.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c0d1b-104">Çoğu durumda, tercih edilir, `DbContext` böylece oluşturulan nasıl olacaktır için benzer bir şekilde yapılandırılmış [çalışma zamanında yapılandırılmış][2].</span><span class="sxs-lookup"><span data-stu-id="c0d1b-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="c0d1b-105">Araçlar deneyin oluşturmak için çeşitli yolları vardır `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="c0d1b-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="c0d1b-106">Uygulama hizmetlerinden</span><span class="sxs-lookup"><span data-stu-id="c0d1b-106">From application services</span></span>
-------------------------
<span data-ttu-id="c0d1b-107">Başlangıç projesi ASP.NET Core uygulama ise, uygulamanın hizmet sağlayıcısı'ndan DbContext nesnesi edinmek için Araçlar deneyin.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="c0d1b-108">Aracı ilk deneyin çağırarak hizmet sağlayıcısı edinmek `Program.BuildWebHost()` ve erişme `IWebHost.Services` özelliği.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="c0d1b-109">Yeni bir ASP.NET Core 2.0 uygulaması oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="c0d1b-110">Önceki sürümlerinde EF Core ve ASP.NET Core, çağrılacak araçları deneyin `Startup.ConfigureServices` doğrudan uygulamanın hizmet sağlayıcısı, ancak bu deseni artık almak için düzgün şekilde ASP.NET Core 2.0 uygulamalarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="c0d1b-111">ASP.NET Core 1.x uygulamaya 2.0 yükseltiyorsanız, şunları yapabilirsiniz [değiştirmek, `Program` yeni deseni izlemesini sınıfı][3].</span><span class="sxs-lookup"><span data-stu-id="c0d1b-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="c0d1b-112">`DbContext` Kendisi ve bağımlılıkları kendi oluşturucusuna uygulamanın hizmet sağlayıcısı'nda Hizmetleri olarak kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="c0d1b-113">Bu kolayca sağlanarak elde edilebilir [bir oluşturucu `DbContext` örneği alır `DbContextOptions<TContext>` bağımsız değişkeni olarak] [ 4] ve kullanarak [ `AddDbContext<TContext>` yöntemi] [5].</span><span class="sxs-lookup"><span data-stu-id="c0d1b-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="c0d1b-114">Parametresiz bir oluşturucu kullanma</span><span class="sxs-lookup"><span data-stu-id="c0d1b-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="c0d1b-115">DbContext uygulama hizmet sağlayıcısından alınamıyorsa araçları türetilmiş için Ara `DbContext` projesi içinde türü.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="c0d1b-116">Ardından parametresiz bir Oluşturucu kullanarak bir örneğini oluşturmak deneyin.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="c0d1b-117">Bu varsayılan oluşturucu olabilir `DbContext` kullanılarak yapılandırılmış [ `OnConfiguring` ] [ 6] yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="c0d1b-118">Tasarım zamanı fabrikası'ndan</span><span class="sxs-lookup"><span data-stu-id="c0d1b-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="c0d1b-119">Araçlar uygulayarak, DbContext oluşturmak nasıl anlayabilirsiniz `IDesignTimeDbContextFactory<TContext>` arabirimi: Bu arabirimi uygulayan bir sınıf ya da aynı projesi türetilmiş olarak bulunursa `DbContext` veya uygulamanın başlangıç projesi araçları atla Bunun yerine DbContext ve kullanım tasarım zamanı Fabrika oluşturma diğer yolları.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="c0d1b-120">`args` Parametresi şu anda kullanılmayan bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="c0d1b-121">Yoktur [bir sorun] [ 7] araçları tasarım zamanı bağımsız değişkenlerini belirtme olanağı izleme.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="c0d1b-122">Tasarım zamanı Fabrika DbContext farklı çalışma zamanında tasarım zamanından yapılandırmak, gerekirse çok kullanışlı olabilir `DbContext` ek parametreler kaydedilmedi dı içinde dı hiç kullanmıyorsanız Oluşturucusu alır ya da Eğer bazı için tercih ettiğiniz yüklüyse neden bir `BuildWebHost` ASP.NET Core uygulamanızın yöntemi</span><span class="sxs-lookup"><span data-stu-id="c0d1b-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="c0d1b-123">`Main` Sınıf.</span><span class="sxs-lookup"><span data-stu-id="c0d1b-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
