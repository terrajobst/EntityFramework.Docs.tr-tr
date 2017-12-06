---
title: "EF çekirdek yükleniyor"
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="installing-ef-core"></a><span data-ttu-id="bab76-102">EF çekirdek yükleniyor</span><span class="sxs-lookup"><span data-stu-id="bab76-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bab76-103">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="bab76-103">Prerequisites</span></span>

<span data-ttu-id="bab76-104">(.NET Core hedef ASP.NET Core 2.0 uygulamalar da dahil) .NET Core 2.0 uygulamaları geliştirmek için bir sürümünü karşıdan yükleyip gerekir [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) platformunuz için uygun olan.</span><span class="sxs-lookup"><span data-stu-id="bab76-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="bab76-105">**Visual Studio 2017 sürüm 15.3 yüklü olsa bile bu geçerlidir.**</span><span class="sxs-lookup"><span data-stu-id="bab76-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="bab76-106">EF çekirdek 2.0 veya herhangi bir .NET standart 2.0 kitaplığı .NET Core 2.0 yanı sıra .NET platformları ile kullanmak için (örneğin, .NET Framework 4.6.1 ile veya daha büyük) .NET standart 2.0 ve uyumlu çerçeveleri farkındadır NuGet sürümü gerekir.</span><span class="sxs-lookup"><span data-stu-id="bab76-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="bab76-107">Bu edinebilirsiniz birkaç şekilde şunlardır:</span><span class="sxs-lookup"><span data-stu-id="bab76-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="bab76-108">Visual Studio 2017 sürüm 15.3 yükleyin</span><span class="sxs-lookup"><span data-stu-id="bab76-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="bab76-109">Visual Studio 2015 kullanıyorsanız [indirin ve NuGet İstemcisi sürüm 3.6.0 yükseltin](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="bab76-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="bab76-110">Visual Studio ve .NET Framework'ü hedefleme önceki sürümleriyle oluşturulan projeleri .NET standart 2.0 kitaplıkları ile uyumlu olması için diğer tüm değişiklikleri gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="bab76-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="bab76-111">Proje dosyasını düzenleyin ve aşağıdaki girdiyi başlangıç özellik grubunda göründüğünden emin olun:</span><span class="sxs-lookup"><span data-stu-id="bab76-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="bab76-112">Test projeleri için ayrıca şu girdiyi bulunduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="bab76-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="bab76-113">BITS alma</span><span class="sxs-lookup"><span data-stu-id="bab76-113">Getting the bits</span></span>
<span data-ttu-id="bab76-114">EF çekirdek çalışma zamanı kitaplıkları bir uygulamaya eklemek için önerilen yol, bir EF çekirdek veritabanı sağlayıcısı Nuget'ten yüklemektir.</span><span class="sxs-lookup"><span data-stu-id="bab76-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="bab76-115">Çalışma zamanı kitaplıkları yanı sıra daha kolay oluşturma ve geçişler uygulama ve var olan bir veritabanını temel alan bir model oluşturma gibi tasarım zamanında projenizde birkaç EF çekirdek ile ilgili görevleri gerçekleştirmeniz için Araçlar yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bab76-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="bab76-116">Bir üçüncü taraf veritabanı sağlayıcısı kullanarak bir uygulamayı güncelleştirmek gerekiyorsa, her zaman bir güncelleştirme EF kullanmak istediğiniz çekirdek sürümü ile uyumlu olan sağlayıcının denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bab76-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="bab76-117">Örneğin</span><span class="sxs-lookup"><span data-stu-id="bab76-117">E.g.</span></span> <span data-ttu-id="bab76-118">Önceki sürümler için veritabanı sağlayıcısı EF çekirdeği çalışma zamanı 2.0 sürümü ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="bab76-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="bab76-119">ASP.NET Core 2.0 hedefleme uygulamaları EF çekirdek 2.0 üçüncü taraf veritabanı sağlayıcıları yanı sıra ek bağımlılıklar olmadan kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="bab76-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="bab76-120">EF çekirdek 2.0 kullanmak için ASP.NET Core 2.0 sürümüne yükseltmek ASP.NET Core önceki sürümlerini hedefleyen uygulama gerekir.</span><span class="sxs-lookup"><span data-stu-id="bab76-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="bab76-121">.NET Core komut satırı arabirimi (CLI) kullanarak platformlar arası geliştirme</span><span class="sxs-lookup"><span data-stu-id="bab76-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="bab76-122">Hedefleyen uygulamalar geliştirmek için [.NET Core](https://www.microsoft.com/net/download/core) kullanmayı tercih edebileceğiniz [ `dotnet` CLI komutları](https://docs.microsoft.com/dotnet/core/tools/) , sık kullandığınız metin düzenleyiciyi ya da bir tümleşik geliştirme ortamı (IDE) bu tür ile birlikte Visual Studio, Visual Studio için Mac veya Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bab76-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="bab76-123">Visual Studio belirli sürümlerini .NET Core hedefleyen uygulamalar gerektirir, .NET Core 2.0 geliştirme Visual Studio 2017 sürüm 15.3 gerektirse .NET Core 1.x geliştirme Visual Studio 2017, örneğin gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bab76-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="bab76-124">Yüklemek veya platformlar arası .NET Core uygulama SQL Server sağlayıcısında yükseltmek için uygulamanın dizinine geçin ve bir komut satırında aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bab76-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="bab76-125">Belirli bir sürümünü yüklemek için belirtebilir `dotnet add package` komutunu `-v` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="bab76-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="bab76-126">Örneğin</span><span class="sxs-lookup"><span data-stu-id="bab76-126">E.g.</span></span> <span data-ttu-id="bab76-127">EF çekirdek 2.0 paketleri yüklemek için append `-v 2.0.0` komutu.</span><span class="sxs-lookup"><span data-stu-id="bab76-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="bab76-128">EF çekirdek içeren bir dizi [için ek komutlar `dotnet` CLI](../../miscellaneous/cli/dotnet.md)sürümünden itibaren `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="bab76-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="bab76-129">Kullanmak için `dotnet ef` CLI komutları, uygulamanızın `.csproj` dosya şu girdiyi içermesi gerekiyor:</span><span class="sxs-lookup"><span data-stu-id="bab76-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="bab76-130">EF çekirdek için .NET Core CLI araçlarını da Microsoft.EntityFrameworkCore.Design adlı ayrı bir paket gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bab76-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="bab76-131">Yalnızca bu proje kullanmaya ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bab76-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="bab76-132">Her zaman ana sürümüne yönelik çalışma zamanı paketleri eşleşen araçları paketleri sürümlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="bab76-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="bab76-133">Visual Studio geliştirme</span><span class="sxs-lookup"><span data-stu-id="bab76-133">Visual Studio development</span></span>

<span data-ttu-id="bab76-134">Birçok farklı türde uygulamayı hedefleyen .NET Core, .NET Framework ve Visual Studio kullanarak EF çekirdek tarafından desteklenen diğer platformlar geliştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bab76-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="bab76-135">Visual Studio'da, uygulamanızdaki bir EF çekirdek veritabanı sağlayıcısı yükleyebilirsiniz iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="bab76-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="bab76-136">NuGet kullanarak [Paket Yöneticisi kullanıcı arabirimi](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="bab76-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="bab76-137">Select menüsünde **Proje > NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="bab76-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="bab76-138">Tıklayın **Gözat** veya **güncelleştirmeleri** sekmesi</span><span class="sxs-lookup"><span data-stu-id="bab76-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="bab76-139">Seçin `Microsoft.EntityFrameworkCore.SqlServer` paket ve istediğiniz sürümü ve onaylayın</span><span class="sxs-lookup"><span data-stu-id="bab76-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="bab76-140">NuGet kullanarak [Paket Yöneticisi Konsolu (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="bab76-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="bab76-141">Select menüsünde **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="bab76-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="bab76-142">Yazın ve PMC aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bab76-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="bab76-143">Kullanabileceğiniz `Update-Package` daha yeni bir sürümü zaten yüklü olan bir paketi güncelleştirmek için bunun yerine komutu</span><span class="sxs-lookup"><span data-stu-id="bab76-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="bab76-144">Belirli bir sürüm belirtmek için kullanabilirsiniz `-Version` değiştiricisi, örneğin EF çekirdek 2.0 paketleri yüklemek için append `-Version 2.0.0` komutları</span><span class="sxs-lookup"><span data-stu-id="bab76-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="bab76-145">Araçlar</span><span class="sxs-lookup"><span data-stu-id="bab76-145">Tools</span></span>

<span data-ttu-id="bab76-146">Ayrıca bir PowerShell sürümü olan [EF çekirdek komutlar hangi çalışma PMC içinde](../../miscellaneous/cli/powershell.md) benzer özelliklere sahip Visual Studio'da `dotnet ef` komutları.</span><span class="sxs-lookup"><span data-stu-id="bab76-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="bab76-147">Bunlar kullanmak üzere yükleyin `Microsoft.EntityFrameworkCore.Tools` Paket Yöneticisi kullanıcı Arabirimi veya PMC kullanarak paket.</span><span class="sxs-lookup"><span data-stu-id="bab76-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="bab76-148">Her zaman ana sürümüne yönelik çalışma zamanı paketleri eşleşen araçları paketleri sürümlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="bab76-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="bab76-149">Kullanmak mümkün olmakla `dotnet ef` komutları Visual Studio'da PMC gelen PowerShell sürümü kullanmak daha kullanışlı olduğu:</span><span class="sxs-lookup"><span data-stu-id="bab76-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="bab76-150">Bunlar otomatik olarak dizinler el ile geçiş gerek kalmadan PMC seçili geçerli proje ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="bab76-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="bab76-151">Bunlar, komut tamamlandıktan sonra Visual Studio komutları tarafından üretilen dosyalar otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="bab76-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="bab76-152">**Kullanım dışı EF çekirdek 2.0 paketlerinde:** EF çekirdek 2.0 varolan bir uygulamaya yükseltme yapıyorsanız, eski EF temel paketler bazı başvuruları el ile kaldırılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="bab76-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="bab76-153">Özellikle, sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli veya EF çekirdek 2. 0'desteklenir, ancak otomatik olarak diğer paketleri yükseltirken kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="bab76-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
