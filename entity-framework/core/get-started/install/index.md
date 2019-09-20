---
title: Entity Framework Core yükleniyor
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: eb808dd9d9b1b214947524cd83999f67be9cc0ff
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149078"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="3f690-102">Entity Framework Core yükleniyor</span><span class="sxs-lookup"><span data-stu-id="3f690-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f690-103">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="3f690-103">Prerequisites</span></span>

* <span data-ttu-id="3f690-104">EF Core bir [.NET Standard 2,1](/dotnet/standard/net-standard) kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="3f690-104">EF Core is a [.NET Standard 2.1](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="3f690-105">EF Core .NET Standard 2,1 ' i destekleyen bir .NET uygulamasının çalışmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3f690-105">So EF Core requires a .NET implementation that supports .NET Standard 2.1 to run.</span></span> <span data-ttu-id="3f690-106">EF Core diğer .NET Standard 2,1 kitaplıkları tarafından da başvurulabilirler.</span><span class="sxs-lookup"><span data-stu-id="3f690-106">EF Core can also be referenced by other .NET Standard 2.1 libraries.</span></span> 

* <span data-ttu-id="3f690-107">Örneğin, .NET Core 'u hedefleyen uygulamalar geliştirmek için EF Core kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="3f690-108">.NET Core uygulamaları oluşturmak için [.NET Core SDK](https://dotnet.microsoft.com/download)gerekir.</span><span class="sxs-lookup"><span data-stu-id="3f690-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="3f690-109">İsteğe bağlı olarak, Visual Studio, Mac için Visual Studio veya Visual Studio Code gibi bir geliştirme ortamı da kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-109">Optionally, you can also use a development environment like Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span> <span data-ttu-id="3f690-110">Daha fazla bilgi için [.NET Core Ile çalışmaya](/dotnet/core/get-started)başlama konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="3f690-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="3f690-111">Visual Studio 'Yu kullanarak Windows 'da uygulama geliştirmek için EF Core kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-111">You can use EF Core to develop applications on Windows using Visual Studio.</span></span> <span data-ttu-id="3f690-112">[Visual Studio](https://visualstudio.microsoft.com/vs) 'nun en son sürümü önerilir.</span><span class="sxs-lookup"><span data-stu-id="3f690-112">The latest version of [Visual Studio](https://visualstudio.microsoft.com/vs) is recommended.</span></span>

* <span data-ttu-id="3f690-113">EF Core, [Xamarin](https://dotnet.microsoft.com/apps/xamarin) ve .NET Native gibi diğer .NET uygulamalarında çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3f690-113">EF Core can run on other .NET implementations like [Xamarin](https://dotnet.microsoft.com/apps/xamarin) and .NET Native.</span></span> <span data-ttu-id="3f690-114">Ancak uygulamada bu uygulamalar, EF Core uygulamanızın ne kadar iyi çalıştığını etkileyebilecek çalışma zamanı kısıtlamalarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3f690-114">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="3f690-115">Daha fazla bilgi için bkz. [EF Core tarafından desteklenen .NET uygulamaları](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="3f690-115">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="3f690-116">Son olarak, farklı veritabanı sağlayıcıları belirli veritabanı altyapısı sürümleri, .NET uygulamaları veya işletim sistemleri gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="3f690-116">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="3f690-117">Uygulamanız için doğru ortamı destekleyen bir [EF Core veritabanı sağlayıcısının](xref:core/providers/index) bulunduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="3f690-117">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="3f690-118">Entity Framework Core çalışma zamanını al</span><span class="sxs-lookup"><span data-stu-id="3f690-118">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="3f690-119">Bir uygulamaya EF Core eklemek için, kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-119">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="3f690-120">ASP.NET Core uygulaması oluşturuyorsanız, bellek içi ve SQL Server sağlayıcılarını yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3f690-120">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="3f690-121">Bu sağlayıcılar, EF Core çalışma zamanının yanı sıra geçerli ASP.NET Core sürümlerine dahil edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3f690-121">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="3f690-122">NuGet paketlerini yüklemek veya güncelleştirmek için, .NET Core komut satırı arabirimini (CLı), Visual Studio Paket Yöneticisi Iletişim kutusunu veya Visual Studio Paket Yöneticisi konsolunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-122">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="3f690-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3f690-123">.NET Core CLI</span></span>

* <span data-ttu-id="3f690-124">EF Core SQL Server sağlayıcısını yüklemek veya güncelleştirmek için işletim sisteminin komut satırından aşağıdaki .NET Core CLI komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="3f690-124">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="3f690-125">Değiştiriciyi`-v` kullanarak `dotnet add package` komutta belirli bir sürümü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-125">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="3f690-126">Örneğin, EF Core 2.2.0 paketlerini yüklemek için komuta ekleyin `-v 2.2.0` .</span><span class="sxs-lookup"><span data-stu-id="3f690-126">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="3f690-127">Daha fazla bilgi için bkz. [.NET komut satırı arabirimi (CLI) araçları](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="3f690-127">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="3f690-128">Visual Studio NuGet Paket Yöneticisi Iletişim kutusu</span><span class="sxs-lookup"><span data-stu-id="3f690-128">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="3f690-129">Visual Studio menüsünden **proje > NuGet Paketlerini Yönet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="3f690-129">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="3f690-130">**Gözatmaya** veya **güncelleştirmeler** sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="3f690-130">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="3f690-131">SQL Server sağlayıcıyı yüklemek veya güncelleştirmek için, `Microsoft.EntityFrameworkCore.SqlServer` paketi seçin ve onaylayın.</span><span class="sxs-lookup"><span data-stu-id="3f690-131">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="3f690-132">Daha fazla bilgi için bkz. [NuGet Paket Yöneticisi Iletişim kutusu](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="3f690-132">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="3f690-133">Visual Studio NuGet Paket Yöneticisi konsolu</span><span class="sxs-lookup"><span data-stu-id="3f690-133">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="3f690-134">Visual Studio menüsünden **araçlar > NuGet paket yöneticisi > Paket Yöneticisi konsolu** ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="3f690-134">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="3f690-135">SQL Server sağlayıcıyı yüklemek için, Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3f690-135">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="3f690-136">Sağlayıcıyı güncelleştirmek için `Update-Package` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="3f690-136">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="3f690-137">Belirli bir sürümü belirtmek için `-Version` değiştiricisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="3f690-137">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="3f690-138">Örneğin, EF Core 2.2.0 paketlerini yüklemek için, komutlara ekleyin `-Version 2.2.0`</span><span class="sxs-lookup"><span data-stu-id="3f690-138">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="3f690-139">Daha fazla bilgi için bkz. [Paket Yöneticisi konsolu](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="3f690-139">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="3f690-140">Entity Framework Core araçlarını al</span><span class="sxs-lookup"><span data-stu-id="3f690-140">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="3f690-141">Projenizde, veritabanı geçişleri oluşturma ve uygulama ya da var olan bir veritabanını temel alan bir EF Core modeli oluşturma gibi EF Core ilgili görevleri yürütmek için araçlar yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-141">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="3f690-142">İki araç kümesi mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="3f690-142">Two sets of tools are available:</span></span>

* <span data-ttu-id="3f690-143">[.NET Core komut satırı arabirimi (CLI) araçları](xref:core/miscellaneous/cli/dotnet) Windows, Linux veya MacOS 'ta kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3f690-143">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="3f690-144">Bu komutlar ile `dotnet ef`başlar.</span><span class="sxs-lookup"><span data-stu-id="3f690-144">These commands begin with `dotnet ef`.</span></span> 

* <span data-ttu-id="3f690-145">[Paket Yöneticisi Konsolu (PMC) araçları](xref:core/miscellaneous/cli/powershell) , Windows üzerinde Visual Studio 'da çalışır.</span><span class="sxs-lookup"><span data-stu-id="3f690-145">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="3f690-146">Bu komutlar, örneğin `Add-Migration`, `Update-Database`bir fiil ile başlar.</span><span class="sxs-lookup"><span data-stu-id="3f690-146">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="3f690-147">Ayrıca, paket yöneticisi konsolundan `dotnet ef` komutları da kullanabilirsiniz, ancak Visual Studio 'yu kullanırken Paket Yöneticisi konsol araçlarının kullanılması önerilir:</span><span class="sxs-lookup"><span data-stu-id="3f690-147">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="3f690-148">Bunlar, el ile geçiş yapmak zorunda kalmadan, Visual Studio 'da PMC 'de seçilen geçerli projeyle otomatik olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="3f690-148">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="3f690-149">Komut tamamlandıktan sonra Visual Studio 'da komutlar tarafından oluşturulan dosyaları otomatik olarak açar.</span><span class="sxs-lookup"><span data-stu-id="3f690-149">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="3f690-150">.NET Core CLI araçlarını al</span><span class="sxs-lookup"><span data-stu-id="3f690-150">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="3f690-151">.NET Core CLI araçlar, [önkoşullardan](#prerequisites)daha önce bahsedilen .NET Core SDK gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3f690-151">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="3f690-152">Komutlar .NET Core SDK geçerli sürümlerine dahil edilmiştir, ancak belirli bir projedeki komutları etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paketini yüklemelisiniz: `dotnet ef`</span><span class="sxs-lookup"><span data-stu-id="3f690-152">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="3f690-153">ASP.NET Core uygulamalar için bu paket otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="3f690-153">For ASP.NET Core apps, this package is included automatically.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="3f690-154">Her zaman çalışma zamanı paketlerinin ana sürümü ile eşleşen Araçlar paketinin sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="3f690-154">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="3f690-155">Paket Yöneticisi konsol araçlarını al</span><span class="sxs-lookup"><span data-stu-id="3f690-155">Get the Package Manager Console tools</span></span>

<span data-ttu-id="3f690-156">EF Core için Paket Yöneticisi konsol araçları 'nı almak için `Microsoft.EntityFrameworkCore.Tools` paketini yükledikten sonra.</span><span class="sxs-lookup"><span data-stu-id="3f690-156">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="3f690-157">Örneğin, Visual Studio 'dan:</span><span class="sxs-lookup"><span data-stu-id="3f690-157">For example, from Visual Studio:</span></span>

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="3f690-158">ASP.NET Core uygulamalar için bu paket otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="3f690-158">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="3f690-159">En son EF Core yükseltme</span><span class="sxs-lookup"><span data-stu-id="3f690-159">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="3f690-160">EF Core yeni bir sürümünü yayınlıyoruz, Microsoft. EntityFrameworkCore. SqlServer, Microsoft. EntityFrameworkCore. SQLite gibi EF Core projenin parçası olan sağlayıcıların yeni bir sürümünü de yayınlarız. Microsoft. EntityFrameworkCore. InMemory.</span><span class="sxs-lookup"><span data-stu-id="3f690-160">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="3f690-161">Tüm geliştirmeleri almak için yalnızca sağlayıcının yeni sürümüne yükseltebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f690-161">You can just upgrade to the new version of the provider to get all the improvements.</span></span> 

* <span data-ttu-id="3f690-162">EF Core, SQL Server ve bellek içi sağlayıcılar ASP.NET Core güncel sürümlerine dahildir.</span><span class="sxs-lookup"><span data-stu-id="3f690-162">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="3f690-163">Mevcut bir ASP.NET Core uygulamasını EF Core daha yeni bir sürüme yükseltmek için ASP.NET Core sürümünü her zaman yükseltin.</span><span class="sxs-lookup"><span data-stu-id="3f690-163">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="3f690-164">Üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmeniz gerekiyorsa, kullanmak istediğiniz EF Core sürümü ile uyumlu bir sağlayıcının güncelleştirilmesini her zaman denetleyin.</span><span class="sxs-lookup"><span data-stu-id="3f690-164">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="3f690-165">Örneğin, önceki sürümlere ait veritabanı sağlayıcıları EF Core çalışma zamanının 2,0 sürümü ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="3f690-165">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="3f690-166">EF Core için üçüncü taraf sağlayıcılar genellikle düzeltme eki sürümlerini EF Core çalışma zamanına göre serbest bırakmaz.</span><span class="sxs-lookup"><span data-stu-id="3f690-166">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="3f690-167">Üçüncü taraf sağlayıcıyı kullanan bir uygulamayı EF Core bir düzeltme eki sürümüne yükseltmek için, Microsoft. EntityFrameworkCore ve Microsoft. EntityFrameworkCore. Ilikisel gibi bireysel EF Core çalışma zamanı bileşenlerine doğrudan başvuru eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3f690-167">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="3f690-168">Mevcut bir uygulamayı EF Core en son sürümüne yükseltiyorsanız, eski EF Core paketlerine yapılan bazı başvuruların el ile kaldırılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="3f690-168">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="3f690-169">Veritabanı sağlayıcısı tasarım zamanı paketleri `Microsoft.EntityFrameworkCore.SqlServer.Design` , gibi EF Core 2,0 ve sonraki sürümlerde artık gerekli değildir veya desteklenmemektedir, ancak diğer paketler yükseltilirken otomatik olarak kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="3f690-169">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="3f690-170">.NET CLı araçları, sürüm 2,1 ' den beri .NET SDK 'da bulunur, bu nedenle bu pakete yönelik başvuru proje dosyasından kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="3f690-170">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

