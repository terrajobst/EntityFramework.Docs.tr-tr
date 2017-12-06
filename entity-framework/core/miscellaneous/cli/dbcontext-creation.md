---
title: "Tasarım zamanı DbContext oluşturma - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="4a576-102">Tasarım zamanı DbContext oluşturma</span><span class="sxs-lookup"><span data-stu-id="4a576-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="4a576-103">EF tasarım sırasında oluşturulacak bir DbContext örnek komutları gerektiren araçların bazıları (örneğin, geçişler komutları çalışırken) süresi.</span><span class="sxs-lookup"><span data-stu-id="4a576-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="4a576-104">Oluşturmak için Araçlar deneyin çeşitli yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="4a576-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="4a576-105">Uygulama hizmetlerinden</span><span class="sxs-lookup"><span data-stu-id="4a576-105">From application services</span></span>
-------------------------
<span data-ttu-id="4a576-106">Başlangıç projesi ASP.NET Core uygulama ise, uygulamanın hizmet sağlayıcısı'ndan DbContext nesnesi edinmek için Araçlar deneyin.</span><span class="sxs-lookup"><span data-stu-id="4a576-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="4a576-107">Bunlar çağırarak elde `Program.BuildWebHost()` ve erişme `IWebHost.Services` özelliği.</span><span class="sxs-lookup"><span data-stu-id="4a576-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="4a576-108">Tüm DbContext kullanarak kayıtlı `IServiceCollection.AddDbContext<TContext>()` bulundu ve bu şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4a576-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="4a576-109">Bu desen edildi [ASP.NET Core 2. 0 ' sunulan][1]</span><span class="sxs-lookup"><span data-stu-id="4a576-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="4a576-110">Varsayılan Oluşturucu kullanma</span><span class="sxs-lookup"><span data-stu-id="4a576-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="4a576-111">DbContext uygulama hizmet sağlayıcısından alınamıyorsa araçları projesi içinde DbContext türünü arayın.</span><span class="sxs-lookup"><span data-stu-id="4a576-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="4a576-112">Bunlar, kendi varsayılan oluşturucusunu kullanarak oluşturmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="4a576-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="4a576-113">Tasarım zamanı fabrikası'ndan</span><span class="sxs-lookup"><span data-stu-id="4a576-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="4a576-114">Araçlar uygulayarak, DbContext oluşturmak nasıl anlayabilirsiniz `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="4a576-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="4a576-115">Bu arabirimi uygulayan bir sınıf projenizi içinde bulunursa, Araçlar DbContext oluşturma diğer yolları atlama.</span><span class="sxs-lookup"><span data-stu-id="4a576-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="4a576-116">Her zaman Fabrika tasarım zamanında kullanırlar.</span><span class="sxs-lookup"><span data-stu-id="4a576-116">They always use the factory at design time.</span></span> <span data-ttu-id="4a576-117">Bir Fabrika DbContext farklı çalışma zamanında tasarım zamanından yapılandırmak gerekirse özellikle yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="4a576-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

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

> [!NOTE]
> <span data-ttu-id="4a576-118">`args` Parametresi şu anda kullanılmayan bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4a576-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="4a576-119">Yoktur [bir sorun] [ 2] araçları tasarım zamanı bağımsız değişkenlerini belirtme olanağı izleme.</span><span class="sxs-lookup"><span data-stu-id="4a576-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
