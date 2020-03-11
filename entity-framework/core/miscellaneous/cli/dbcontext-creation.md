---
title: Tasarım zamanı DbContext oluşturma-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416744"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="8decc-102">Tasarım Zamanında DbContext Oluşturma</span><span class="sxs-lookup"><span data-stu-id="8decc-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="8decc-103">EF Core araçları komutlarından bazıları (örneğin, [geçişler][1] komutları), uygulamanın varlık türleriyle ilgili ayrıntıların toplanması ve bir veritabanı şemasına nasıl eşlendikleri hakkında, tasarım zamanında oluşturulacak türetilmiş bir `DbContext` örneği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8decc-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="8decc-104">Çoğu durumda, bu nedenle oluşturulan `DbContext`, [çalışma zamanında nasıl yapılandırılacağından][2]benzer bir şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8decc-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="8decc-105">Araçların `DbContext`oluşturmaya ne kadar farklı şekilde deneme vardır:</span><span class="sxs-lookup"><span data-stu-id="8decc-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="8decc-106">Uygulama hizmetlerinden</span><span class="sxs-lookup"><span data-stu-id="8decc-106">From application services</span></span>

<span data-ttu-id="8decc-107">Başlangıç projeniz [ASP.NET Core Web konağını][3] veya [.NET Core genel konağını][4]kullanıyorsa, Araçlar DbContext nesnesini uygulamanın hizmet sağlayıcısından almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="8decc-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="8decc-108">Araçlar önce `Program.CreateHostBuilder()`çağırarak, `Build()`çağırarak `Services` özelliğine erişerek hizmet sağlayıcısını edinmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="8decc-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="8decc-109">Yeni bir ASP.NET Core uygulaması oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="8decc-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="8decc-110">`DbContext` kendisi ve oluşturucudaki tüm bağımlılıklar uygulamanın hizmet sağlayıcısında hizmet olarak kaydedilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8decc-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="8decc-111">Bu, bir [`DbContextOptions<TContext>` örneğini bir bağımsız değişken olarak alan][5] ve [`AddDbContext<TContext>` metodunu][6]kullanan `DbContext` bir Oluşturucu bulundurarak kolayca elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="8decc-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="8decc-112">Parametresiz bir Oluşturucu kullanma</span><span class="sxs-lookup"><span data-stu-id="8decc-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="8decc-113">DbContext uygulama hizmeti sağlayıcısından alınamıyorsa, Araçlar proje içinde türetilmiş `DbContext` türünü arar.</span><span class="sxs-lookup"><span data-stu-id="8decc-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="8decc-114">Daha sonra parametresiz bir Oluşturucu kullanarak bir örnek oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="8decc-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="8decc-115">Bu, `DbContext` [`OnConfiguring`][7] yöntemi kullanılarak yapılandırıldıysa varsayılan Oluşturucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="8decc-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="8decc-116">Tasarım zamanı fabrikasından</span><span class="sxs-lookup"><span data-stu-id="8decc-116">From a design-time factory</span></span>

<span data-ttu-id="8decc-117">Ayrıca, `IDesignTimeDbContextFactory<TContext>` arabirimini uygulayarak DbContext 'in nasıl oluşturulacağını de söyleyebilirsiniz: Bu arabirimi uygulayan bir sınıf türetilmiş `DbContext` aynı projede veya uygulamanın başlangıç projesinde bulunursa, Araçlar DbContext oluşturmanın diğer yollarını atlar ve bunun yerine tasarım zamanı fabrikasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="8decc-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="8decc-118">`args` parametresi şu anda kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="8decc-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="8decc-119">Araçlardan tasarım zamanı bağımsız değişkenlerini belirtme yeteneği izlenirken [bir sorun][8] oluştu.</span><span class="sxs-lookup"><span data-stu-id="8decc-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="8decc-120">Tasarım zamanı fabrikası, çalışma zamanından farklı olarak DbContext 'i tasarım zamanı için farklı şekilde yapılandırmanız gerektiğinde kullanışlı olabilir. `DbContext` Oluşturucu başka parametreler alırsa, ara ' ya hiçbir nedenden dolayı yoksa, hiçbir nedenden dolayı ASP.NET Core uygulamanızın `Main` sınıfında bir `BuildWebHost` yöntemi bulundurmayı tercih etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8decc-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
