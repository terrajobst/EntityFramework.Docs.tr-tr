---
title: EF Core'u yükleme
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996934"
---
# <a name="installing-ef-core"></a><span data-ttu-id="ab8ed-102">EF Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="ab8ed-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab8ed-103">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ab8ed-103">Prerequisites</span></span>

<span data-ttu-id="ab8ed-104">.NET Core 2.1 uygulamaları (.NET Core hedefleyen ASP.NET Core 2.1 uygulamalar dahil) geliştirmek için bir sürümünü karşıdan yükleyip gerekir [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) platformunuz için uygun olan.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="ab8ed-105">**Visual Studio 2017 sürüm 15.7 yüklü olsa bile bu geçerlidir.**</span><span class="sxs-lookup"><span data-stu-id="ab8ed-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="ab8ed-106">EF Core 2.1 veya herhangi bir .NET Standard 2.0 kitaplığı bir .NET Core 2.1 yanı sıra .NET platformu ile kullanmak için (örneğin, .NET Framework 4.6.1 ile veya üzeri) tanımaz ve bunun uyumlu çerçeveleri .NET Standard 2.0 NuGet sürümü gerekir.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="ab8ed-107">Bunu elde edebilirsiniz birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="ab8ed-108">Visual Studio 2017 sürüm 15.7 yükleyin</span><span class="sxs-lookup"><span data-stu-id="ab8ed-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="ab8ed-109">Visual Studio 2015 kullanıyorsanız [indirin ve NuGet istemci 3.6.0 sürümüne yükseltme](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="ab8ed-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="ab8ed-110">Visual Studio ve .NET Framework'ü hedefleyen önceki sürümleriyle oluşturulan projeleri .NET Standard 2.0 kitaplıklarla uyumlu olması için ek değişiklikleri gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="ab8ed-111">Proje dosyasını düzenleyin ve şu girişi ilk özellik grubunda göründüğünden emin olun:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="ab8ed-112">Test projeleri için şu girdiyi mevcut olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="ab8ed-113">BITS alma</span><span class="sxs-lookup"><span data-stu-id="ab8ed-113">Getting the bits</span></span>
<span data-ttu-id="ab8ed-114">EF Core çalışma zamanı kitaplıkları bir uygulamaya eklemek için önerilen yol, bir EF Core veritabanı sağlayıcısı Nuget'ten yüklemektir.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="ab8ed-115">Çalışma zamanı kitaplıklarının yanı sıra, oluşturma ve geçişler uygulama ve var olan bir veritabanını temel alan bir model oluşturma gibi tasarım zamanında projenizde EF Core ile ilgili çeşitli görevleri gerçekleştirmek kolaylaştıran Araçlar yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="ab8ed-116">Bir üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmek gerekiyorsa, kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcısını güncelleştirmesi için her zaman denetleyin.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="ab8ed-117">Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core runtime'nın 2.1 sürümü ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="ab8ed-118">ASP.NET Core 2.1 hedefleyen uygulamalar, üçüncü taraf veritabanı sağlayıcıları yanı sıra ek bağımlılıkları olmadan EF Core 2.1 kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="ab8ed-119">ASP.NET Core'nın önceki sürümlerini hedefleyen uygulamalar EF Core 2.1 kullanabilmek için ASP.NET Core 2.1'ı yükseltmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="ab8ed-120">.NET Core komut satırı arabirimi (CLI) kullanarak platformlar arası geliştirme</span><span class="sxs-lookup"><span data-stu-id="ab8ed-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="ab8ed-121">Hedefleyen uygulamalar geliştirmek için [.NET Core](https://www.microsoft.com/net/download/core) kullanmayı da tercih edebilirsiniz [ `dotnet` CLI komutları](https://docs.microsoft.com/dotnet/core/tools/) sevdiğiniz bir metin düzenleyici ya da bir tümleşik geliştirme ortamı (IDE) tür ile birlikte Visual Studio, Visual Studio Code'u veya Mac için Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ab8ed-122">.NET Core hedefleyen uygulamalar, Visual Studio'nun belirli sürümlerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="ab8ed-123">Örneğin, .NET Core 2.1 geliştirme Visual Studio 2017 sürüm 15.7 gerekirken Visual Studio 2017, .NET Core 1.x geliştirme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="ab8ed-124">Platformlar arası .NET Core uygulamasında SQL Server sağlayıcıyı yükseltin veya yüklemek için uygulamanın dizinine geçin ve komut satırında aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="ab8ed-125">Belirli bir sürümünü yükleme, belirtebilir `dotnet add package` komutu `-v` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="ab8ed-126">Örneğin, EF Core 2.1 paketleri yüklemek için URL'ye `-v 2.1.0` komutu.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="ab8ed-127">EF Core içeren bir dizi [ek komutlar için `dotnet` CLI](../../miscellaneous/cli/dotnet.md)ile başlayan `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="ab8ed-128">Adlı bir paketi EF Core için .NET Core CLI araçları gerektiren `Microsoft.EntityFrameworkCore.Design`.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="ab8ed-129">Bunu kullanarak projenize ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="ab8ed-130">Her zaman ana sürümü çalışma zamanı paketlerini eşleşen araçları paket sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="ab8ed-131">Visual Studio geliştirme</span><span class="sxs-lookup"><span data-stu-id="ab8ed-131">Visual Studio development</span></span>

<span data-ttu-id="ab8ed-132">Hedefleyen .NET Core, .NET Framework ve Visual Studio kullanarak EF Core tarafından desteklenen diğer platformlarda farklı türlerde uygulamalar geliştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="ab8ed-133">Uygulamanızı Visual Studio'dan bir EF Core veritabanı sağlayıcısı yüklemeden iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="ab8ed-134">NuGet kullanarak [Paket Yöneticisi kullanıcı arabirimi](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="ab8ed-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="ab8ed-135">Seçim menüsünde **Proje > NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="ab8ed-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="ab8ed-136">Tıklayarak **Gözat** veya **güncelleştirmeleri** sekmesi</span><span class="sxs-lookup"><span data-stu-id="ab8ed-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="ab8ed-137">Seçin `Microsoft.EntityFrameworkCore.SqlServer` paket ve istenen sürüm ve onaylayın</span><span class="sxs-lookup"><span data-stu-id="ab8ed-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="ab8ed-138">NuGet kullanarak [Paket Yöneticisi Konsolu (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="ab8ed-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="ab8ed-139">Seçim menüsünde **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="ab8ed-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ab8ed-140">Yazın ve PMC'de aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="ab8ed-141">Kullanabileceğiniz `Update-Package` daha yeni bir sürümü zaten yüklü olan bir paketi güncelleştirmek için bunun yerine komutu</span><span class="sxs-lookup"><span data-stu-id="ab8ed-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="ab8ed-142">Belirli bir sürümünü belirlemek için kullanabileceğiniz `-Version` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="ab8ed-143">Örneğin, EF Core 2.1 paketleri yüklemek için URL'ye `-Version 2.1.0` komutlar</span><span class="sxs-lookup"><span data-stu-id="ab8ed-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="ab8ed-144">Araçlar</span><span class="sxs-lookup"><span data-stu-id="ab8ed-144">Tools</span></span>

<span data-ttu-id="ab8ed-145">Bir PowerShell sürümü de mevcuttur [EF Core komutlar hangi çalışma PMC'yi içinde](../../miscellaneous/cli/powershell.md) için benzer özelliklere sahip, Visual Studio'da `dotnet ef` komutları.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="ab8ed-146">Kullanılması mümkün olsa da `dotnet ef` komutları Visual Studio'da PMC'yi gelen PowerShell sürümü kullanmak çok daha kullanışlı olduğu:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="ab8ed-147">Bunlar otomatik olarak geçerli proje dizinleri el ile geçiş gerek kalmadan PMC'de seçili çalışın.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="ab8ed-148">Bunlar, komut tamamlandıktan sonra Visual Studio'da komutlar tarafından üretilen dosyalar otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ab8ed-149">**EF Core 2.1 paketlerinde kullanım dışı:** EF Core 2.1 varolan bir uygulamaya yükseltme yapıyorsanız, eski EF Core paketleri bazı başvuruları el ile kaldırılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="ab8ed-150">Sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli ve EF Core 2.1 içinde desteklenmez ancak otomatik olarak diğer paketleri yükseltirken kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="ab8ed-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="ab8ed-151">Bu paket başvurusu gelen kaldırılabilmesi için .NET CLI araçları artık .NET SDK'da dahil *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ab8ed-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
