---
title: Entity Framework Core yükleme
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 58c79d477d590eea355a922b3e1233bbecb305cc
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181987"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="06138-102">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="06138-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="06138-103">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="06138-103">Prerequisites</span></span>

* <span data-ttu-id="06138-104">EF Core bir [.NET Standard 2.0](/dotnet/standard/net-standard) kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="06138-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="06138-105">Bu nedenle EF Core çalıştırmak için .NET Standard 2.0 destekleyen bir .NET uygulaması gerektirir.</span><span class="sxs-lookup"><span data-stu-id="06138-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="06138-106">EF Core, ayrıca diğer .NET Standard 2.0 kitaplıkları tarafından başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="06138-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span> 

* <span data-ttu-id="06138-107">Örneğin, EF Core, .NET Core'u hedefleyen uygulamalar geliştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06138-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="06138-108">.NET Core uygulamaları oluşturma gerektirir [.NET Core SDK'sı](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="06138-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="06138-109">İsteğe bağlı olarak, Visual Studio, Visual Studio gibi bir geliştirme ortamı Mac ya da Visual Studio Code için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06138-109">Optionally, you can also use a development environment like Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span> <span data-ttu-id="06138-110">Daha fazla bilgi için kontrol [.NET Core ile çalışmaya başlama](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="06138-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="06138-111">EF Core, Visual Studio kullanarak Windows, .NET Framework 4.6.1 veya sonraki bir sürümü hedefleyen uygulamalar geliştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06138-111">You can use EF Core to develop applications that target .NET Framework 4.6.1 or later on Windows, using Visual Studio.</span></span> <span data-ttu-id="06138-112">Visual Studio'nun en son sürümü önerilir.</span><span class="sxs-lookup"><span data-stu-id="06138-112">The latest version of Visual Studio is recommended.</span></span> <span data-ttu-id="06138-113">Visual Studio 2015 gibi eski bir sürümünü kullanmak istiyorsanız, emin [3.6.0 sürümüne NuGet istemci yükseltme](https://www.nuget.org/downloads) .NET Standard 2.0 kitaplıkları ile çalışmak için.</span><span class="sxs-lookup"><span data-stu-id="06138-113">If you want to use an older version, like Visual Studio 2015, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="06138-114">EF Core, Xamarin ve .NET Native gibi diğer .NET uygulamaları üzerinde çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06138-114">EF Core can run on other .NET implementations like Xamarin and .NET Native.</span></span> <span data-ttu-id="06138-115">Ancak, uygulamada söz konusu uygulamaları EF Core uygulamanızı nasıl çalıştığı etkileyebilecek çalışma zamanı sınırlamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="06138-115">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="06138-116">Daha fazla bilgi için [EF Core tarafından desteklenen .NET uygulamalarıyla](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="06138-116">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="06138-117">Son olarak, farklı veritabanı sağlayıcıları belirli veritabanı altyapısı sürümleri, .NET uygulamaları veya işletim sistemi gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="06138-117">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="06138-118">Emin bir [EF Core veritabanı sağlayıcısı](xref:core/providers/index) olan kullanılabilir, doğru ortamda uygulamanız için destekler.</span><span class="sxs-lookup"><span data-stu-id="06138-118">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="06138-119">Entity Framework Core çalışma zamanı Al</span><span class="sxs-lookup"><span data-stu-id="06138-119">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="06138-120">EF Core bir uygulamaya eklemek için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="06138-120">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="06138-121">Bir ASP.NET Core uygulaması derliyorsanız, bellek ve SQL Server sağlayıcıları yüklemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="06138-121">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="06138-122">Sağlayıcılar, ASP.NET Core, geçerli sürümlerinde EF Core çalışma zamanı ile birlikte eklenir.</span><span class="sxs-lookup"><span data-stu-id="06138-122">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="06138-123">NuGet paketlerini güncelleştirmek veya yüklemek için [.NET Core komut satırı arabirimi (CLI), Visual Studio Paket Yöneticisi iletişim kutusu veya Visual Studio Paket Yöneticisi konsolu. kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06138-123">To install or update NuGet packages, you can use the [.NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="06138-124">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="06138-124">.NET Core CLI</span></span>

* <span data-ttu-id="06138-125">Yükleme veya güncelleştirme EF Core SQL Server sağlayıcısı için işletim sistemi komut satırından aşağıdaki .NET Core CLI komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="06138-125">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="06138-126">Belirli bir sürümde belirtebilir `dotnet add package` komutu `-v` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="06138-126">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="06138-127">Örneğin, EF Core 2.2.0 paketleri yüklemek için URL'ye `-v 2.2.0` komutu.</span><span class="sxs-lookup"><span data-stu-id="06138-127">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="06138-128">Daha fazla bilgi için [.NET komut satırı arabirimi (CLI) araçlarını](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="06138-128">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="06138-129">Visual Studio NuGet Paket Yöneticisi iletişim kutusu</span><span class="sxs-lookup"><span data-stu-id="06138-129">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="06138-130">Visual Studio menüden **Proje > NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="06138-130">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="06138-131">Tıklayarak **Gözat** veya **güncelleştirmeleri** sekmesi</span><span class="sxs-lookup"><span data-stu-id="06138-131">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="06138-132">SQL Server sağlayıcısı güncelleştirmek veya yüklemek için seçin `Microsoft.EntityFrameworkCore.SqlServer` paketini ve onaylayın.</span><span class="sxs-lookup"><span data-stu-id="06138-132">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="06138-133">Daha fazla bilgi için [NuGet Paket Yöneticisi iletişim](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="06138-133">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="06138-134">Visual Studio, NuGet Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="06138-134">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="06138-135">Visual Studio menüden **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="06138-135">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="06138-136">SQL Server sağlayıcıyı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="06138-136">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="06138-137">Sağlayıcı güncelleştirmek için `Update-Package` komutu.</span><span class="sxs-lookup"><span data-stu-id="06138-137">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="06138-138">Belirli bir sürümünü belirtmek için kullanın `-Version` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="06138-138">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="06138-139">Örneğin, EF Core 2.2.0 paketleri yüklemek için URL'ye `-Version 2.2.0` komutlar</span><span class="sxs-lookup"><span data-stu-id="06138-139">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="06138-140">Daha fazla bilgi için [Paket Yöneticisi Konsolu](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="06138-140">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="06138-141">Entity Framework Core araçları edinin</span><span class="sxs-lookup"><span data-stu-id="06138-141">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="06138-142">EF Core ile ilgili görevleri oluşturmak ve veritabanı geçişlerini uygulama veya varolan bir veritabanını temel alan bir EF Core modeli oluşturma gibi projenizdeki yürütmek için Araçlar yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06138-142">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="06138-143">İki araç vardır:</span><span class="sxs-lookup"><span data-stu-id="06138-143">Two sets of tools are available:</span></span>

* <span data-ttu-id="06138-144">[.NET Core komut satırı arabirimi (CLI) araçlarını](xref:core/miscellaneous/cli/dotnet) Windows, Linux veya Macos'ta kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="06138-144">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="06138-145">Bu komutlar şununla `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="06138-145">These commands begin with `dotnet ef`.</span></span> 

* <span data-ttu-id="06138-146">[Paket Yöneticisi Konsolu (PMC) araçları](xref:core/miscellaneous/cli/powershell) Windows üzerinde Visual Studio'da çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="06138-146">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="06138-147">Bu komutlar, örneğin bir fiili ile başlatın `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="06138-147">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="06138-148">De kullanabilirsiniz ancak `dotnet ef` komutları Paket Yöneticisi konsolunda, Visual Studio kullanıyorsanız, Paket Yöneticisi konsolu araçlarını kullanmak için önerilir:</span><span class="sxs-lookup"><span data-stu-id="06138-148">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="06138-149">Bunlar otomatik olarak geçerli proje dizinleri el ile geçiş gerek kalmadan Visual Studio'da PMC'yi seçili çalışın.</span><span class="sxs-lookup"><span data-stu-id="06138-149">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="06138-150">Bunlar, komut tamamlandıktan sonra Visual Studio'da komutlar tarafından üretilen dosyalar otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="06138-150">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="06138-151">.NET Core CLI araçları edinin</span><span class="sxs-lookup"><span data-stu-id="06138-151">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="06138-152">.NET core CLI araçları, daha önce bahsedilen .NET Core SDK gerektirir [önkoşulları](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="06138-152">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="06138-153">`dotnet ef` Komutları, .NET Core SDK'ın geçerli sürümlerinde dahil edilir, ancak yüklemeniz gereken komutları belirli bir projede etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paket:</span><span class="sxs-lookup"><span data-stu-id="06138-153">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="06138-154">ASP.NET Core uygulamaları için bu paket otomatik olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="06138-154">For ASP.NET Core apps, this package is included automatically.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="06138-155">Her zaman ana sürümü çalışma zamanı paketlerini eşleşen araçları paket sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="06138-155">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="06138-156">Paket Yöneticisi konsolu araçları edinin</span><span class="sxs-lookup"><span data-stu-id="06138-156">Get the Package Manager Console tools</span></span>

<span data-ttu-id="06138-157">Paket Yöneticisi konsolu EF Core için araçları almak için yükleyin `Microsoft.EntityFrameworkCore.Tools` paket.</span><span class="sxs-lookup"><span data-stu-id="06138-157">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="06138-158">Örneğin, Visual Studio'dan:</span><span class="sxs-lookup"><span data-stu-id="06138-158">For example, from Visual Studio:</span></span>

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="06138-159">ASP.NET Core uygulamaları için bu paket otomatik olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="06138-159">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="06138-160">En son EF Core için yükseltme</span><span class="sxs-lookup"><span data-stu-id="06138-160">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="06138-161">Herhangi bir zamanda EF Core yeni bir sürümünü yayınlayabilir, ayrıca sağlayıcıları gibi Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, EF Core projenin bir parçası olan yeni bir sürümünü yayınlayabilir ve Microsoft.EntityFrameworkCore.InMemory.</span><span class="sxs-lookup"><span data-stu-id="06138-161">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="06138-162">Yalnızca tüm geliştirmeleri almak için sağlayıcı yeni sürüme yükseltebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06138-162">You can just upgrade to the new version of the provider to get all the improvements.</span></span> 

* <span data-ttu-id="06138-163">SQL Server ve bellek içi sağlayıcıları ile birlikte EF çekirdekli ASP.NET Core'nın geçerli sürümlerinde dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="06138-163">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="06138-164">Mevcut bir ASP.NET Core uygulaması EF Core daha yeni bir sürüme yükseltmek için her zaman ASP.NET Core sürümünü yükseltin.</span><span class="sxs-lookup"><span data-stu-id="06138-164">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="06138-165">Bir üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmek gerekiyorsa, kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcısını güncelleştirmesi için her zaman denetleyin.</span><span class="sxs-lookup"><span data-stu-id="06138-165">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="06138-166">Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core çalışma zamanı 2.0 sürümü ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="06138-166">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="06138-167">Genellikle üçüncü taraf sağlayıcılar EF Core için düzeltme eki sürümleri yanında EF Core çalışma zamanı sürüm yok.</span><span class="sxs-lookup"><span data-stu-id="06138-167">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="06138-168">Bir üçüncü taraf sağlayıcı için bir düzeltme eki sürümü EF Core kullanan bir uygulamayı yükseltmeye Microsoft.EntityFrameworkCore ve Microsoft.EntityFrameworkCore.Relational gibi bireysel EF Core çalışma zamanı bileşenleri için doğrudan bir başvuru eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="06138-168">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="06138-169">Var olan uygulamanın EF Core en son sürümüne yükseltme yapıyorsanız, eski EF Core paketleri bazı başvuruları el ile kaldırılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="06138-169">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="06138-170">Sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli ve üzeri veya EF Core 2.0 desteklenir, ancak diğer paketleri yükseltme sırasında otomatik olarak kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="06138-170">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="06138-171">Bu paket başvurusu proje dosyasından kaldırılabilmesi için .NET CLI araçları 2.1 sürümünden itibaren .NET SDK'yı dahil edilmiştir:</span><span class="sxs-lookup"><span data-stu-id="06138-171">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* <span data-ttu-id="06138-172">.NET Framework'ü hedefleyen uygulamalar .NET Standard 2.0 kitaplıkları ile çalışmak için değişiklikleri gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="06138-172">Applications that target .NET Framework may need changes to work with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="06138-173">Proje dosyasını düzenleyin ve şu girişi ilk özellik grubunda göründüğünden emin olun:</span><span class="sxs-lookup"><span data-stu-id="06138-173">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="06138-174">Test projeleri için şu girdiyi mevcut olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="06138-174">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
