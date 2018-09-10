---
title: Entiy Framework Core yükleme
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250328"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="55534-102">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="55534-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55534-103">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="55534-103">Prerequisites</span></span>

* <span data-ttu-id="55534-104">.NET Core 2.1'i hedefleyen uygulamalar geliştirmek için yükleme [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="55534-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="55534-105">SDK, Visual Studio 2017'in en son sürümü olsa bile yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="55534-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="55534-106">Visual Studio .NET Core 2.1 hedefleyen uygulamalar geliştirilmesi için kullanılacak Visual Studio 2017 sürüm 15.7 veya sonraki bir sürümü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="55534-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="55534-107">Entity Framework 2.1 ASP.NET Core uygulamaları kullanmak için ASP.NET Core 2.1 kullanın.</span><span class="sxs-lookup"><span data-stu-id="55534-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="55534-108">ASP.NET Core önceki sürümlerini kullanan uygulamaları 2.1 için güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="55534-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="55534-109">Visual Studio 2015, .NET Framework 4.6.1'i hedefleyen uygulamalar için kullanabilirsiniz veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="55534-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="55534-110">Ancak, uyumlu çerçeveleri ve .NET Standard 2.0 ile uyumlu olan NuGet sürümü gerekir.</span><span class="sxs-lookup"><span data-stu-id="55534-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="55534-111">Visual Studio 2015'te bu alınacağı [3.6.0 sürümüne NuGet istemci yükseltme](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="55534-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="55534-112">Entity Framework Core çalışma zamanı Al</span><span class="sxs-lookup"><span data-stu-id="55534-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="55534-113">EF Core çalışma zamanı kitaplıkları bir uygulamaya eklemek için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="55534-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="55534-114">Desteklenen sağlayıcılar ve NuGet paket adlarının bir listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="55534-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="55534-115">NuGet paketlerini güncelleştirmek veya yüklemek için .NET Core CLI, Visual Studio Paket Yöneticisi iletişim kutusu ya da Visual Studio Paket Yöneticisi konsolu kullanın.</span><span class="sxs-lookup"><span data-stu-id="55534-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="55534-116">Ayrı olarak yüklemeniz gerekmez. Bu nedenle ASP.NET Core 2.1 uygulamaları için SQL Server sağlayıcıları ve bellek içi otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="55534-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="55534-117">Bir üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmek gerekiyorsa, kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcısını güncelleştirmesi için her zaman denetleyin.</span><span class="sxs-lookup"><span data-stu-id="55534-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="55534-118">Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core runtime'nın 2.1 sürümü ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="55534-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="55534-119">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="55534-119">.NET Core CLI</span></span>

<span data-ttu-id="55534-120">Aşağıdaki .NET Core CLI komutunu yükler veya SQL Server sağlayıcısını güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="55534-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="55534-121">Belirli bir sürümde belirtebilir `dotnet add package` komutu `-v` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="55534-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="55534-122">Örneğin, EF Core 2.1.0 paketleri yüklemek için URL'ye `-v 2.1.0` komutu.</span><span class="sxs-lookup"><span data-stu-id="55534-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="55534-123">Visual Studio NuGet Paket Yöneticisi iletişim kutusu</span><span class="sxs-lookup"><span data-stu-id="55534-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="55534-124">Menüden **Proje > NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="55534-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="55534-125">Tıklayarak **Gözat** veya **güncelleştirmeleri** sekmesi</span><span class="sxs-lookup"><span data-stu-id="55534-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="55534-126">SQL Server sağlayıcısı güncelleştirmek veya yüklemek için seçin `Microsoft.EntityFrameworkCore.SqlServer` paketini ve onaylayın.</span><span class="sxs-lookup"><span data-stu-id="55534-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="55534-127">Daha fazla bilgi için [NuGet Paket Yöneticisi iletişim](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="55534-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="55534-128">Visual Studio, NuGet Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="55534-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="55534-129">Menüden **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="55534-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="55534-130">SQL Server sağlayıcıyı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="55534-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="55534-131">Sağlayıcı güncelleştirmek için `Update-Package` komutu.</span><span class="sxs-lookup"><span data-stu-id="55534-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="55534-132">Belirli bir sürümünü belirtmek için kullanın `-Version` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="55534-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="55534-133">Örneğin, EF Core 2.1.0 paketleri yüklemek için URL'ye `-Version 2.1.0` komutlar</span><span class="sxs-lookup"><span data-stu-id="55534-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="55534-134">Daha fazla bilgi için [Paket Yöneticisi Konsolu](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="55534-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="55534-135">Entity Framework Core Araçları'nı edinin</span><span class="sxs-lookup"><span data-stu-id="55534-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="55534-136">Çalışma zamanı kitaplıklarının yanı sıra, EF Core ile ilgili bazı görevleri tasarım zamanında projenizde gerçekleştirebilirsiniz Araçlar yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55534-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="55534-137">Örneğin, geçişler oluşturmak, geçişler uygulamak ve var olan bir veritabanını temel alan bir model oluşturma.</span><span class="sxs-lookup"><span data-stu-id="55534-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="55534-138">İki araç vardır:</span><span class="sxs-lookup"><span data-stu-id="55534-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="55534-139">.NET Core [komut satırı arabirimi (CLI) araçlarını](../../miscellaneous/cli/dotnet.md) Windows, Linux veya Macos'ta kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="55534-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="55534-140">Bu komutlar şununla `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="55534-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="55534-141">[Paket Yöneticisi konsolu Araçları](../../miscellaneous/cli/powershell.md) Visual Studio 2017'de Windows üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="55534-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="55534-142">Bu komutlar, örneğin bir fiili ile başlatın `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="55534-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="55534-143">Hizmetini kullanıyor olsanız da `dotnet ef` Paket Yöneticisi konsolundan çalıştığınızda, Visual Studio Paket Yöneticisi konsolu araçları kullanmak daha kullanışlı olduğu komutları:</span><span class="sxs-lookup"><span data-stu-id="55534-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="55534-144">Bunlar Paket Yöneticisi konsolunda el ile dizin değiştirmeyi gerek kalmadan seçilen geçerli proje ile otomatik olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="55534-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="55534-145">Bunlar, komut tamamlandıktan sonra Visual Studio'da komutlar tarafından üretilen dosyalar otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="55534-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="55534-146">CLI araçları edinin</span><span class="sxs-lookup"><span data-stu-id="55534-146">Get the CLI tools</span></span>

<span data-ttu-id="55534-147">`dotnet ef` Komutları, .NET Core SDK'sı dahil edilir, ancak yüklemeniz gereken komutları etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paket:</span><span class="sxs-lookup"><span data-stu-id="55534-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="55534-148">ASP.NET Core 2.1 uygulamaları için bu paket otomatik olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="55534-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="55534-149">Daha önce açıklandığı gibi [önkoşulları](#prerequisites), ayrıca .NET Core 2.1 SDK'yı yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="55534-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="55534-150">Her zaman ana sürümü çalışma zamanı paketlerini eşleşen araçları paket sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="55534-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="55534-151">Paket Yöneticisi konsolu araçları edinin</span><span class="sxs-lookup"><span data-stu-id="55534-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="55534-152">Paket Yöneticisi konsolu EF Core için araçları almak için yükleyin `Microsoft.EntityFrameworkCore.Tools` paket:</span><span class="sxs-lookup"><span data-stu-id="55534-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="55534-153">ASP.NET Core 2.1 uygulamaları için bu paket otomatik olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="55534-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="55534-154">EF Core 2.1 yükseltme</span><span class="sxs-lookup"><span data-stu-id="55534-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="55534-155">EF Core 2.1 varolan bir uygulamaya yükseltme yapıyorsanız, eski EF Core paketleri bazı başvuruları el ile kaldırılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="55534-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="55534-156">Sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli ve EF Core 2.1 içinde desteklenmez, ancak diğer paketleri yükseltme sırasında otomatik olarak kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="55534-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="55534-157">Bu paket başvurusu gelen kaldırılabilmesi için .NET CLI araçları artık .NET SDK'da dahil *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="55534-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="55534-158">Visual Studio'nun önceki sürümleri tarafından oluşturulmuş ve .NET Framework'ü hedefleyen uygulamalar için .NET Standard 2.0 kitaplıkları ile uyumlu olduklarından emin olun:</span><span class="sxs-lookup"><span data-stu-id="55534-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="55534-159">Proje dosyasını düzenleyin ve şu girişi ilk özellik grubunda göründüğünden emin olun:</span><span class="sxs-lookup"><span data-stu-id="55534-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="55534-160">Test projeleri için şu girdiyi mevcut olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="55534-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
