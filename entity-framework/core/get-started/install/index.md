---
title: Varlık Çerçeve Çekirdeğinin Yüklenmesi - EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 6575b1ac028f8b67b49ca7f4e49d6f19500be98f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136178"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="f2ffe-102">Varlık Çerçeve Çekirdeğinin Yüklenmesi</span><span class="sxs-lookup"><span data-stu-id="f2ffe-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2ffe-103">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="f2ffe-103">Prerequisites</span></span>

* <span data-ttu-id="f2ffe-104">EF Core bir [.NET Standart 2.0](/dotnet/standard/net-standard) kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="f2ffe-105">Bu nedenle EF Core çalıştırmak için .NET Standart 2.0 destekleyen bir .NET uygulaması gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="f2ffe-106">EF Core diğer .NET Standart 2.0 kitaplıkları tarafından da başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="f2ffe-107">Örneğin, .NET Core'u hedefleyen uygulamalar geliştirmek için EF Core'u kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="f2ffe-108">.NET Core uygulamaları oluşturmak için [.NET Core SDK](https://dotnet.microsoft.com/download)gerekir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="f2ffe-109">İsteğe bağlı olarak, ayrıca [Visual Studio](https://visualstudio.microsoft.com/vs)gibi bir geliştirme ortamı kullanabilirsiniz , Mac için [Visual Studio](https://visualstudio.microsoft.com/vs/mac), veya Visual [Studio Kodu](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="f2ffe-109">Optionally, you can also use a development environment like [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac), or [Visual Studio Code](https://code.visualstudio.com).</span></span> <span data-ttu-id="f2ffe-110">Daha fazla bilgi için [.NET Core ile Başlarken'i](/dotnet/core/get-started)kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="f2ffe-111">Visual Studio'yu kullanarak Windows'da uygulama geliştirmek için EF Core'u kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-111">You can use EF Core to develop applications on Windows using Visual Studio.</span></span> <span data-ttu-id="f2ffe-112">[Visual Studio'nun](https://visualstudio.microsoft.com/vs) en son sürümü önerilir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-112">The latest version of [Visual Studio](https://visualstudio.microsoft.com/vs) is recommended.</span></span>

* <span data-ttu-id="f2ffe-113">EF Core, [Xamarin](https://dotnet.microsoft.com/apps/xamarin) ve .NET Native gibi diğer .NET uygulamalarında da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-113">EF Core can run on other .NET implementations like [Xamarin](https://dotnet.microsoft.com/apps/xamarin) and .NET Native.</span></span> <span data-ttu-id="f2ffe-114">Ancak uygulamada bu uygulamalar, EF Core'un uygulamanızda ne kadar iyi çalıştığını etkileyebilecek çalışma zamanı sınırlamalarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-114">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="f2ffe-115">Daha fazla bilgi için [bkz.](xref:core/platforms/index)</span><span class="sxs-lookup"><span data-stu-id="f2ffe-115">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="f2ffe-116">Son olarak, farklı veritabanı sağlayıcıları belirli veritabanı altyapısı sürümlerini, .NET uygulamalarını veya işletim sistemlerini gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-116">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="f2ffe-117">Uygulamanız için doğru ortamı destekleyen bir [EF Core veritabanı sağlayıcısının](xref:core/providers/index) kullanılabilir olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-117">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="f2ffe-118">Varlık Framework Core çalışma süresini alın</span><span class="sxs-lookup"><span data-stu-id="f2ffe-118">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="f2ffe-119">Bir uygulamaya EF Core eklemek için, kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-119">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="f2ffe-120">Bir ASP.NET Core uygulaması oluşturuyorsanız, bellek ve SQL Server sağlayıcılarını yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-120">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="f2ffe-121">Bu sağlayıcılar, ASP.NET Core'un geçerli sürümlerinde, EF Core çalışma süresinin yanı sıra dahildir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-121">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="f2ffe-122">NuGet paketlerini yüklemek veya güncellemek için .NET Core komut satırı arabirimini (CLI), Visual Studio Paket Yöneticisi İletişim Kutusunu veya Visual Studio Paket Yöneticisi Konsolunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-122">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="f2ffe-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f2ffe-123">.NET Core CLI</span></span>

* <span data-ttu-id="f2ffe-124">EF Core SQL Server sağlayıcısını yüklemek veya güncellemek için işletim sisteminin komut satırından aşağıdaki .NET Core CLI komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-124">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="f2ffe-125">`-v` Değiştiriciyi kullanarak `dotnet add package` komutta belirli bir sürümü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-125">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="f2ffe-126">Örneğin, EF Core 2.2.0 paketlerini `-v 2.2.0` yüklemek için komuta eklenir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-126">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="f2ffe-127">Daha fazla bilgi için [bkz.](/dotnet/core/tools/)</span><span class="sxs-lookup"><span data-stu-id="f2ffe-127">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="f2ffe-128">Visual Studio NuGet Paket Yöneticisi İletişim</span><span class="sxs-lookup"><span data-stu-id="f2ffe-128">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="f2ffe-129">Visual Studio menüsünden **NuGet Paketlerini Yönet> Proje'yi** seçin</span><span class="sxs-lookup"><span data-stu-id="f2ffe-129">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="f2ffe-130">**Gözat'a** veya **Güncellemeler** sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="f2ffe-130">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="f2ffe-131">SQL Server sağlayıcısını yüklemek veya `Microsoft.EntityFrameworkCore.SqlServer` güncelleştirmek için paketi seçin ve onaylayın.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-131">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="f2ffe-132">Daha fazla bilgi için [NuGet Paket Yöneticisi İletişim Kutusu'na](/nuget/tools/package-manager-ui)bakın.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-132">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="f2ffe-133">Visual Studio NuGet Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="f2ffe-133">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="f2ffe-134">Visual Studio **menüsünden, NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu > Araçlar'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="f2ffe-134">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="f2ffe-135">SQL Server sağlayıcısını yüklemek için Paket Yöneticisi Konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-135">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="f2ffe-136">Sağlayıcıyı güncelleştirmek için `Update-Package` komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-136">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="f2ffe-137">Belirli bir sürümü belirtmek `-Version` için değiştiriciyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-137">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="f2ffe-138">Örneğin, EF Core 2.2.0 paketlerini `-Version 2.2.0` yüklemek için, komutlara ek</span><span class="sxs-lookup"><span data-stu-id="f2ffe-138">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="f2ffe-139">Daha fazla bilgi için [Paket Yöneticisi Konsolu'na](/nuget/tools/package-manager-console)bakın.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-139">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="f2ffe-140">Varlık Çerçeve Core araçlarını alın</span><span class="sxs-lookup"><span data-stu-id="f2ffe-140">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="f2ffe-141">Projenizde, veritabanı geçişleri oluşturma ve uygulama veya varolan bir veritabanını temel alan bir EF Core modeli oluşturma gibi EF Core ile ilgili görevleri yürütmek için araçlar yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-141">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="f2ffe-142">İki araç kümesi mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-142">Two sets of tools are available:</span></span>

* <span data-ttu-id="f2ffe-143">[.NET Core komut satırı arabirimi (CLI) araçları](xref:core/miscellaneous/cli/dotnet) Windows, Linux veya macOS'ta kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-143">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="f2ffe-144">Bu komutlar `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-144">These commands begin with `dotnet ef`.</span></span>

* <span data-ttu-id="f2ffe-145">[Paket Yöneticisi Konsolu (PMC) araçları](xref:core/miscellaneous/cli/powershell) Windows'daki Visual Studio'da çalışır.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-145">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="f2ffe-146">Bu komutlar bir fiille `Add-Migration`başlar, örneğin . `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-146">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="f2ffe-147">Paket Yöneticisi Konsolu'ndaki `dotnet ef` komutları da kullanabilirsiniz, ancak Visual Studio'yu kullanırken Paket Yöneticisi Konsolu araçlarını kullanmanız önerilir:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-147">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="f2ffe-148">Görsel Studio'daki PMC'de seçilen geçerli projeyle, dizinleri el ile değiştirmeye gerek kalmadan otomatik olarak çalışırlar.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-148">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="f2ffe-149">Komut tamamlandıktan sonra Visual Studio'daki komutlar tarafından oluşturulan dosyaları otomatik olarak açarlar.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-149">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="f2ffe-150">.NET Core CLI araçlarını alın</span><span class="sxs-lookup"><span data-stu-id="f2ffe-150">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="f2ffe-151">.NET Core CLI araçları, daha önce [Önkoşullar'da](#prerequisites)belirtilen .NET Core SDK'yı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-151">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="f2ffe-152">Komutlar `dotnet ef` .NET Core SDK'nın geçerli sürümlerinde yer almakla birlikte, belirli bir projedeki `Microsoft.EntityFrameworkCore.Design` komutları etkinleştirmek için paketi yüklemeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-152">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> <span data-ttu-id="f2ffe-153">Her zaman çalışma zamanı paketlerinin ana sürümüyle eşleşen araçlar paketinin sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-153">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="f2ffe-154">Paket Yöneticisi Konsol uçağını alma</span><span class="sxs-lookup"><span data-stu-id="f2ffe-154">Get the Package Manager Console tools</span></span>

<span data-ttu-id="f2ffe-155">EF Core için Package Manager Console araçlarını `Microsoft.EntityFrameworkCore.Tools` almak için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-155">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="f2ffe-156">Örneğin, Visual Studio'dan:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-156">For example, from Visual Studio:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="f2ffe-157">core uygulamaları ASP.NET için bu paket otomatik olarak dahildir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-157">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="f2ffe-158">En son EF Çekirdeğine yükseltme</span><span class="sxs-lookup"><span data-stu-id="f2ffe-158">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="f2ffe-159">EF Core'un yeni bir sürümünü yayınladığımız her zaman, Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.InMemory gibi EF Core projesinin bir parçası olan sağlayıcıların yeni bir sürümünü de yayınlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-159">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="f2ffe-160">Tüm iyileştirmeleri almak için sağlayıcının yeni sürümüne yükseltebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-160">You can just upgrade to the new version of the provider to get all the improvements.</span></span>

* <span data-ttu-id="f2ffe-161">EF Core, SQL Server ve bellek içi sağlayıcılarla birlikte ASP.NET Core'un geçerli sürümlerinde yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-161">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="f2ffe-162">Varolan bir ASP.NET Core uygulamasını EF Core'un daha yeni bir sürümüne yükseltmek için, her zaman ASP.NET Core sürümünü yükseltin.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-162">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="f2ffe-163">Üçüncü taraf veritabanı sağlayıcısı nı kullanan bir uygulamayı güncelleştirmeniz gerekiyorsa, her zaman kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcının güncelleştirmesini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-163">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="f2ffe-164">Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core çalışma zamanının 2.0 sürümüyle uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-164">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="f2ffe-165">EF Core için üçüncü taraf sağlayıcılar genellikle EF Core çalışma zamanının yanında yama sürümleri yayınlamaz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-165">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="f2ffe-166">Bir üçüncü taraf sağlayıcıkullanan bir uygulamayı EF Core'un yama sürümüne yükseltmek için Microsoft.EntityFrameworkCore ve Microsoft.EntityFrameworkCore.Relational gibi tek tek EF Core çalışma zamanı bileşenlerine doğrudan başvuru eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-166">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="f2ffe-167">Varolan bir uygulamayı EF Core'un en son sürümüne yükseltiyorsanız, eski EF Core paketlerine yapılan bazı başvuruların el ile kaldırılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-167">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="f2ffe-168">Veritabanı sağlayıcısı tasarım zamanı `Microsoft.EntityFrameworkCore.SqlServer.Design` paketleri artık GEREKLI değildir veya EF Core 2.0 ve sonraki düzeylerinden desteklenmez, ancak diğer paketleri yükseltirken otomatik olarak kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="f2ffe-168">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="f2ffe-169">.NET CLI araçları sürüm 2.1'den beri .NET SDK'ya dahildir, böylece bu pakete yapılan başvuru proje dosyasından kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="f2ffe-169">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
