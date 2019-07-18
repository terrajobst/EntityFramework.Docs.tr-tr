---
title: Entity Framework Core yükleniyor
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: db1b83a9222e00a5e226a134085b18247b31f29a
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306479"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="ff541-102">Entity Framework Core yükleniyor</span><span class="sxs-lookup"><span data-stu-id="ff541-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff541-103">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ff541-103">Prerequisites</span></span>

* <span data-ttu-id="ff541-104">EF Core bir [.NET Standard 2,0](/dotnet/standard/net-standard) kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="ff541-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="ff541-105">EF Core .NET Standard 2,0 ' i destekleyen bir .NET uygulamasının çalışmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ff541-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="ff541-106">EF Core diğer .NET Standard 2,0 kitaplıkları tarafından da başvurulabilirler.</span><span class="sxs-lookup"><span data-stu-id="ff541-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span> 

* <span data-ttu-id="ff541-107">Örneğin, .NET Core 'u hedefleyen uygulamalar geliştirmek için EF Core kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="ff541-108">.NET Core uygulamaları oluşturmak için [.NET Core SDK](https://dotnet.microsoft.com/download)gerekir.</span><span class="sxs-lookup"><span data-stu-id="ff541-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="ff541-109">İsteğe bağlı olarak, Visual Studio, Mac için Visual Studio veya Visual Studio Code gibi bir geliştirme ortamı da kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-109">Optionally, you can also use a development environment like Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span> <span data-ttu-id="ff541-110">Daha fazla bilgi için [.NET Core Ile çalışmaya](/dotnet/core/get-started)başlama konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="ff541-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="ff541-111">Visual Studio 'Yu kullanarak Windows 'da .NET Framework 4.6.1 veya üstünü hedefleyen uygulamalar geliştirmek için EF Core kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-111">You can use EF Core to develop applications that target .NET Framework 4.6.1 or later on Windows, using Visual Studio.</span></span> <span data-ttu-id="ff541-112">Visual Studio 'nun en son sürümü önerilir.</span><span class="sxs-lookup"><span data-stu-id="ff541-112">The latest version of Visual Studio is recommended.</span></span> <span data-ttu-id="ff541-113">Visual Studio 2015 gibi eski bir sürümü kullanmak istiyorsanız, NuGet istemcisini, .NET Standard 2,0 kitaplıklarıyla çalışmak üzere [3.6.0 sürümüne yükseltdiğinizden](https://www.nuget.org/downloads) emin olun.</span><span class="sxs-lookup"><span data-stu-id="ff541-113">If you want to use an older version, like Visual Studio 2015, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="ff541-114">EF Core, Xamarin ve .NET Native gibi diğer .NET uygulamalarında çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ff541-114">EF Core can run on other .NET implementations like Xamarin and .NET Native.</span></span> <span data-ttu-id="ff541-115">Ancak uygulamada bu uygulamalar, EF Core uygulamanızın ne kadar iyi çalıştığını etkileyebilecek çalışma zamanı kısıtlamalarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ff541-115">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="ff541-116">Daha fazla bilgi için bkz. [EF Core tarafından desteklenen .NET uygulamaları](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="ff541-116">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="ff541-117">Son olarak, farklı veritabanı sağlayıcıları belirli veritabanı altyapısı sürümleri, .NET uygulamaları veya işletim sistemleri gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="ff541-117">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="ff541-118">Uygulamanız için doğru ortamı destekleyen bir [EF Core veritabanı sağlayıcısının](xref:core/providers/index) bulunduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="ff541-118">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="ff541-119">Entity Framework Core çalışma zamanını al</span><span class="sxs-lookup"><span data-stu-id="ff541-119">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="ff541-120">Bir uygulamaya EF Core eklemek için, kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-120">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="ff541-121">ASP.NET Core uygulaması oluşturuyorsanız, bellek içi ve SQL Server sağlayıcılarını yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ff541-121">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="ff541-122">Bu sağlayıcılar, EF Core çalışma zamanının yanı sıra geçerli ASP.NET Core sürümlerine dahil edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ff541-122">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="ff541-123">NuGet paketlerini yüklemek veya güncelleştirmek için, .NET Core komut satırı arabirimini (CLı), Visual Studio Paket Yöneticisi Iletişim kutusunu veya Visual Studio Paket Yöneticisi konsolunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-123">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="ff541-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ff541-124">.NET Core CLI</span></span>

* <span data-ttu-id="ff541-125">EF Core SQL Server sağlayıcısını yüklemek veya güncelleştirmek için işletim sisteminin komut satırından aşağıdaki .NET Core CLI komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ff541-125">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="ff541-126">Değiştiriciyi`-v` kullanarak `dotnet add package` komutta belirli bir sürümü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-126">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="ff541-127">Örneğin, EF Core 2.2.0 paketlerini yüklemek için komuta ekleyin `-v 2.2.0` .</span><span class="sxs-lookup"><span data-stu-id="ff541-127">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="ff541-128">Daha fazla bilgi için bkz. [.NET komut satırı arabirimi (CLI) araçları](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="ff541-128">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="ff541-129">Visual Studio NuGet Paket Yöneticisi Iletişim kutusu</span><span class="sxs-lookup"><span data-stu-id="ff541-129">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="ff541-130">Visual Studio menüsünden **proje > NuGet Paketlerini Yönet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ff541-130">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="ff541-131">**Gözatmaya** veya **güncelleştirmeler** sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="ff541-131">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="ff541-132">SQL Server sağlayıcıyı yüklemek veya güncelleştirmek için, `Microsoft.EntityFrameworkCore.SqlServer` paketi seçin ve onaylayın.</span><span class="sxs-lookup"><span data-stu-id="ff541-132">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="ff541-133">Daha fazla bilgi için bkz. [NuGet Paket Yöneticisi Iletişim kutusu](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="ff541-133">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="ff541-134">Visual Studio NuGet Paket Yöneticisi konsolu</span><span class="sxs-lookup"><span data-stu-id="ff541-134">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="ff541-135">Visual Studio menüsünden **araçlar > NuGet paket yöneticisi > Paket Yöneticisi konsolu** ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="ff541-135">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ff541-136">SQL Server sağlayıcıyı yüklemek için, Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ff541-136">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="ff541-137">Sağlayıcıyı güncelleştirmek için `Update-Package` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff541-137">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="ff541-138">Belirli bir sürümü belirtmek için `-Version` değiştiricisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff541-138">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="ff541-139">Örneğin, EF Core 2.2.0 paketlerini yüklemek için, komutlara ekleyin `-Version 2.2.0`</span><span class="sxs-lookup"><span data-stu-id="ff541-139">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="ff541-140">Daha fazla bilgi için bkz. [Paket Yöneticisi konsolu](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="ff541-140">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="ff541-141">Entity Framework Core araçlarını al</span><span class="sxs-lookup"><span data-stu-id="ff541-141">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="ff541-142">Projenizde, veritabanı geçişleri oluşturma ve uygulama ya da var olan bir veritabanını temel alan bir EF Core modeli oluşturma gibi EF Core ilgili görevleri yürütmek için araçlar yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-142">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="ff541-143">İki araç kümesi mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="ff541-143">Two sets of tools are available:</span></span>

* <span data-ttu-id="ff541-144">[.NET Core komut satırı arabirimi (CLI) araçları](xref:core/miscellaneous/cli/dotnet) Windows, Linux veya MacOS 'ta kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ff541-144">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="ff541-145">Bu komutlar ile `dotnet ef`başlar.</span><span class="sxs-lookup"><span data-stu-id="ff541-145">These commands begin with `dotnet ef`.</span></span> 

* <span data-ttu-id="ff541-146">[Paket Yöneticisi Konsolu (PMC) araçları](xref:core/miscellaneous/cli/powershell) , Windows üzerinde Visual Studio 'da çalışır.</span><span class="sxs-lookup"><span data-stu-id="ff541-146">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="ff541-147">Bu komutlar, örneğin `Add-Migration`, `Update-Database`bir fiil ile başlar.</span><span class="sxs-lookup"><span data-stu-id="ff541-147">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="ff541-148">Ayrıca, paket yöneticisi konsolundan `dotnet ef` komutları da kullanabilirsiniz, ancak Visual Studio 'yu kullanırken Paket Yöneticisi konsol araçlarının kullanılması önerilir:</span><span class="sxs-lookup"><span data-stu-id="ff541-148">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="ff541-149">Bunlar, el ile geçiş yapmak zorunda kalmadan, Visual Studio 'da PMC 'de seçilen geçerli projeyle otomatik olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="ff541-149">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="ff541-150">Komut tamamlandıktan sonra Visual Studio 'da komutlar tarafından oluşturulan dosyaları otomatik olarak açar.</span><span class="sxs-lookup"><span data-stu-id="ff541-150">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="ff541-151">.NET Core CLI araçlarını al</span><span class="sxs-lookup"><span data-stu-id="ff541-151">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="ff541-152">.NET Core CLI araçlar, [önkoşullardan](#prerequisites)daha önce bahsedilen .NET Core SDK gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ff541-152">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="ff541-153">Komutlar .NET Core SDK geçerli sürümlerine dahil edilmiştir, ancak belirli bir projedeki komutları etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paketini yüklemelisiniz: `dotnet ef`</span><span class="sxs-lookup"><span data-stu-id="ff541-153">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="ff541-154">ASP.NET Core uygulamalar için bu paket otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="ff541-154">For ASP.NET Core apps, this package is included automatically.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="ff541-155">Her zaman çalışma zamanı paketlerinin ana sürümü ile eşleşen Araçlar paketinin sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff541-155">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="ff541-156">Paket Yöneticisi konsol araçlarını al</span><span class="sxs-lookup"><span data-stu-id="ff541-156">Get the Package Manager Console tools</span></span>

<span data-ttu-id="ff541-157">EF Core için Paket Yöneticisi konsol araçları 'nı almak için `Microsoft.EntityFrameworkCore.Tools` paketini yükledikten sonra.</span><span class="sxs-lookup"><span data-stu-id="ff541-157">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="ff541-158">Örneğin, Visual Studio 'dan:</span><span class="sxs-lookup"><span data-stu-id="ff541-158">For example, from Visual Studio:</span></span>

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="ff541-159">ASP.NET Core uygulamalar için bu paket otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="ff541-159">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="ff541-160">En son EF Core yükseltme</span><span class="sxs-lookup"><span data-stu-id="ff541-160">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="ff541-161">EF Core yeni bir sürümünü yayınlıyoruz, Microsoft. EntityFrameworkCore. SqlServer, Microsoft. EntityFrameworkCore. SQLite gibi EF Core projenin parçası olan sağlayıcıların yeni bir sürümünü de yayınlarız. Microsoft. EntityFrameworkCore. InMemory.</span><span class="sxs-lookup"><span data-stu-id="ff541-161">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="ff541-162">Tüm geliştirmeleri almak için yalnızca sağlayıcının yeni sürümüne yükseltebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff541-162">You can just upgrade to the new version of the provider to get all the improvements.</span></span> 

* <span data-ttu-id="ff541-163">EF Core, SQL Server ve bellek içi sağlayıcılar ASP.NET Core güncel sürümlerine dahildir.</span><span class="sxs-lookup"><span data-stu-id="ff541-163">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="ff541-164">Mevcut bir ASP.NET Core uygulamasını EF Core daha yeni bir sürüme yükseltmek için ASP.NET Core sürümünü her zaman yükseltin.</span><span class="sxs-lookup"><span data-stu-id="ff541-164">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="ff541-165">Üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmeniz gerekiyorsa, kullanmak istediğiniz EF Core sürümü ile uyumlu bir sağlayıcının güncelleştirilmesini her zaman denetleyin.</span><span class="sxs-lookup"><span data-stu-id="ff541-165">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="ff541-166">Örneğin, önceki sürümlere ait veritabanı sağlayıcıları EF Core çalışma zamanının 2,0 sürümü ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="ff541-166">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="ff541-167">EF Core için üçüncü taraf sağlayıcılar genellikle düzeltme eki sürümlerini EF Core çalışma zamanına göre serbest bırakmaz.</span><span class="sxs-lookup"><span data-stu-id="ff541-167">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="ff541-168">Üçüncü taraf sağlayıcıyı kullanan bir uygulamayı EF Core bir düzeltme eki sürümüne yükseltmek için, Microsoft. EntityFrameworkCore ve Microsoft. EntityFrameworkCore. Ilikisel gibi bireysel EF Core çalışma zamanı bileşenlerine doğrudan başvuru eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ff541-168">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="ff541-169">Mevcut bir uygulamayı EF Core en son sürümüne yükseltiyorsanız, eski EF Core paketlerine yapılan bazı başvuruların el ile kaldırılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="ff541-169">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="ff541-170">Veritabanı sağlayıcısı tasarım zamanı paketleri `Microsoft.EntityFrameworkCore.SqlServer.Design` , gibi EF Core 2,0 ve sonraki sürümlerde artık gerekli değildir veya desteklenmemektedir, ancak diğer paketler yükseltilirken otomatik olarak kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="ff541-170">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="ff541-171">.NET CLı araçları, sürüm 2,1 ' den beri .NET SDK 'da bulunur, bu nedenle bu pakete yönelik başvuru proje dosyasından kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ff541-171">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* <span data-ttu-id="ff541-172">.NET Framework hedef uygulamaların .NET Standard 2,0 kitaplıklarıyla çalışması için değişiklikler gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="ff541-172">Applications that target .NET Framework may need changes to work with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="ff541-173">Proje dosyasını düzenleyin ve aşağıdaki girdinin ilk özellik grubunda göründüğünden emin olun:</span><span class="sxs-lookup"><span data-stu-id="ff541-173">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="ff541-174">Test projeleri için aşağıdaki girişin mevcut olduğundan da emin olun:</span><span class="sxs-lookup"><span data-stu-id="ff541-174">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
