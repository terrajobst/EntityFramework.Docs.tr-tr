---
title: EF Core araçları başvurusu (Paket Yöneticisi Konsolu) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cb05e3fb66adf96f8a6778711a76520d0be24c71
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419776"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="20d2e-102">Entity Framework Core başvuru - Visual Studio'da Paket Yöneticisi konsolu araçları</span><span class="sxs-lookup"><span data-stu-id="20d2e-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="20d2e-103">Entity Framework Core için Paket Yöneticisi Konsolu (PMC) araçları tasarım zamanı geliştirme görevlerini gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="20d2e-104">Örneğin, oluşturdukları [geçişler](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), geçişler uygulamak ve var olan bir veritabanını temel alan bir modeli için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="20d2e-105">Visual Studio kullanarak içindeki komutları çalıştırmanız [Paket Yöneticisi Konsolu](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="20d2e-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="20d2e-106">Bu araçlar, .NET Framework hem de .NET Core projeleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="20d2e-107">Öneririz Visual Studio, kullanmadığınız, [EF Core komut satırı araçları](dotnet.md) yerine.</span><span class="sxs-lookup"><span data-stu-id="20d2e-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="20d2e-108">CLI araçları, platformlar arası ve bir komut istemi içinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="20d2e-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="20d2e-109">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="20d2e-109">Installing the tools</span></span>

<span data-ttu-id="20d2e-110">Yükleme ve güncelleştirme araçları yordamları, ASP.NET Core 2.1 + ve önceki sürümleri veya diğer proje türleri arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="20d2e-111">ASP.NET Core sürüm 2.1 ve üzeri</span><span class="sxs-lookup"><span data-stu-id="20d2e-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="20d2e-112">Çünkü araçları otomatik olarak bir ASP.NET Core 2.1 + projeye eklenir `Microsoft.EntityFrameworkCore.Tools` paket dahil [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="20d2e-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="20d2e-113">Bu nedenle, araçları yüklemek için herhangi bir şey yapmanız gerekmez, ancak göstermesi gerekmez:</span><span class="sxs-lookup"><span data-stu-id="20d2e-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="20d2e-114">Yeni bir projede Araçları'nı kullanmadan önce paketleri geri yükle.</span><span class="sxs-lookup"><span data-stu-id="20d2e-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="20d2e-115">Araçlar, yeni bir sürüme güncelleştirme paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="20d2e-116">Araçların en son sürümü aldığınızdan emin olmak için aşağıdaki adımı da yapmanız önerilir:</span><span class="sxs-lookup"><span data-stu-id="20d2e-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="20d2e-117">Düzenleme, *.csproj* dosya ve en son sürümünü belirten bir satır ekleyin [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) paket.</span><span class="sxs-lookup"><span data-stu-id="20d2e-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="20d2e-118">Örneğin, *.csproj* dosya içerebilir bir `ItemGroup` aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="20d2e-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="20d2e-119">Aşağıdaki örnekte olduğu gibi bir ileti aldığınızda Araçları'nı güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="20d2e-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="20d2e-120">EF Core Araçları '2.1.1-rtm-30846' çalışma zamanı '2.1.3-rtm-32065' eski sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="20d2e-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="20d2e-121">En son özellikler ve hata düzeltmeleri için araçları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="20d2e-122">Araçları güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="20d2e-122">To update the tools:</span></span>
* <span data-ttu-id="20d2e-123">En son .NET Core SDK'sını yükleyin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="20d2e-124">Visual Studio, en son sürüme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="20d2e-125">Düzen *.csproj* daha önce gösterildiği gibi bir paket başvurusu için en son araçları paketini içeren dosya.</span><span class="sxs-lookup"><span data-stu-id="20d2e-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="20d2e-126">Diğer sürümler ve proje türleri</span><span class="sxs-lookup"><span data-stu-id="20d2e-126">Other versions and project types</span></span>

<span data-ttu-id="20d2e-127">Aşağıdaki komutu çalıştırarak Paket Yöneticisi konsolu Araçları'nı yükleme **Paket Yöneticisi Konsolu**:</span><span class="sxs-lookup"><span data-stu-id="20d2e-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="20d2e-128">Aşağıdaki komutu çalıştırarak araçları güncelleştirme **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="20d2e-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="20d2e-129">Yüklemeyi doğrulama</span><span class="sxs-lookup"><span data-stu-id="20d2e-129">Verify the installation</span></span>

<span data-ttu-id="20d2e-130">Bu komutu çalıştırarak araçların yüklendiğini doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="20d2e-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="20d2e-131">(Bu, kullanmakta olduğunuz Araçlar'ın hangi sürümünün sunmayacaktır) çıktı şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="20d2e-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="20d2e-132">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="20d2e-132">Using the tools</span></span>

<span data-ttu-id="20d2e-133">Araçları'nı kullanmadan önce:</span><span class="sxs-lookup"><span data-stu-id="20d2e-133">Before using the tools:</span></span>
* <span data-ttu-id="20d2e-134">Hedef ve başlangıç projesi arasındaki farkı anlama.</span><span class="sxs-lookup"><span data-stu-id="20d2e-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="20d2e-135">Araçları ile .NET standart sınıf kitaplıkları kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="20d2e-136">ASP.NET Core projeleri için ortam ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="20d2e-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="20d2e-137">Hedef ve başlangıç projesi</span><span class="sxs-lookup"><span data-stu-id="20d2e-137">Target and startup project</span></span>

<span data-ttu-id="20d2e-138">Komutlar başvurduğu bir *proje* ve *başlangıç projesi*.</span><span class="sxs-lookup"><span data-stu-id="20d2e-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="20d2e-139">*Proje* olarak da bilinen *hedef proje* olduğundan burada komutlar dosyası ekleyebilir veya kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20d2e-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="20d2e-140">Varsayılan olarak, **varsayılan proje** seçili **Paket Yöneticisi Konsolu** hedef projedir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="20d2e-141">Hedef projesi olarak farklı bir proje kullanarak belirtebilirsiniz <nobr> `--project` </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="20d2e-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="20d2e-142">*Başlangıç projesi* araçları derlemek ve çalıştırmak sunucudur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="20d2e-143">Veritabanı bağlantı dizesi ve modelin yapılandırması gibi proje hakkında bilgi almak için tasarım zamanında uygulama kodu yürütmek araçlar vardır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="20d2e-144">Varsayılan olarak, **başlangıç projesi** içinde **Çözüm Gezgini** başlangıç projedir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="20d2e-145">Başlangıç projesi olarak farklı bir proje kullanarak belirtebilirsiniz <nobr> `--startup-project` </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="20d2e-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="20d2e-146">Hedef proje ve başlangıç projesi çoğunlukla aynı proje olur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="20d2e-147">Ayrı projeler oldukları tipik bir senaryo olduğunda:</span><span class="sxs-lookup"><span data-stu-id="20d2e-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="20d2e-148">EF Core bağlamını ve varlık içinde bir .NET Core sınıf kitaplığı sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="20d2e-149">Sınıf kitaplığı bir .NET Core konsol uygulaması veya web uygulamasına başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="20d2e-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="20d2e-150">Ayrıca filtrelenebilir [EF Core bağlamdan ayrı bir sınıf kitaplığı'nda geçiş kodu koyun](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="20d2e-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="20d2e-151">Diğer hedef çerçeveleri</span><span class="sxs-lookup"><span data-stu-id="20d2e-151">Other target frameworks</span></span>

<span data-ttu-id="20d2e-152">Paket Yöneticisi konsolu Araçlar, .NET Core veya .NET Framework projeleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="20d2e-153">.NET Core veya .NET Framework projesi bir .NET standart sınıf kitaplığında EF Core modeli olan uygulamalara olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="20d2e-154">Örneğin, bu Xamarin ve evrensel Windows platformu uygulamaları geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="20d2e-155">Böyle durumlarda, araçları için başlangıç projesi olarak davranmak üzere tek amacı olan bir .NET Core veya .NET Framework konsol uygulama projesi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20d2e-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="20d2e-156">Proje Gerçek kod olmadan işlevsiz bir proje olabilir &mdash; yalnızca bir hedef için bir araç sağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="20d2e-157">Neden gerekli işlevsiz bir proje mi?</span><span class="sxs-lookup"><span data-stu-id="20d2e-157">Why is a dummy project required?</span></span> <span data-ttu-id="20d2e-158">Daha önce bahsedildiği gibi tasarım zamanında uygulama kodu yürütmek araçlar vardır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="20d2e-159">Bunu yapmak için .NET Core veya .NET Framework çalışma zamanı kullanmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="20d2e-160">EF Core model .NET Core veya .NET Framework hedefleyen bir proje içinde olduğunda, EF Core Araçları çalışma zamanı'projesinden ödünç alın.</span><span class="sxs-lookup"><span data-stu-id="20d2e-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="20d2e-161">EF Core modeli bir .NET standart sınıf kitaplığında ise bunlar, yapamazsınız.</span><span class="sxs-lookup"><span data-stu-id="20d2e-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="20d2e-162">.NET Standard gerçek bir .NET uygulaması değil; .NET uygulamaları desteklemelidir API'leri kümesinin bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="20d2e-163">Bu nedenle .NET standart EF Core araçları için uygulama kodu yürütmek yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="20d2e-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="20d2e-164">Başlangıç projesi olarak kullanmak için oluşturduğunuz işlevsiz proje içine .NET Standard sınıf kitaplığı araçları yükleyebilir bir somut hedef platformu sağlar.</span><span class="sxs-lookup"><span data-stu-id="20d2e-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="20d2e-165">ASP.NET Core ortamı</span><span class="sxs-lookup"><span data-stu-id="20d2e-165">ASP.NET Core environment</span></span>

<span data-ttu-id="20d2e-166">ASP.NET Core projeleri için ortamını belirtmek için ayarlanmış **env:ASPNETCORE_ENVIRONMENT** komutları çalıştırmadan önce.</span><span class="sxs-lookup"><span data-stu-id="20d2e-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="20d2e-167">Ortak parametreleri</span><span class="sxs-lookup"><span data-stu-id="20d2e-167">Common parameters</span></span>

<span data-ttu-id="20d2e-168">Aşağıdaki tabloda, EF Core komutların tümü için ortak olan parametreler gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="20d2e-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="20d2e-169">Parametre</span><span class="sxs-lookup"><span data-stu-id="20d2e-169">Parameter</span></span>                 | <span data-ttu-id="20d2e-170">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20d2e-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="20d2e-171">-Bağlam \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-171">-Context \<String></span></span>        | <span data-ttu-id="20d2e-172">`DbContext` Kullanılacak sınıfı.</span><span class="sxs-lookup"><span data-stu-id="20d2e-172">The `DbContext` class to use.</span></span> <span data-ttu-id="20d2e-173">Yalnızca ya da ad alanları ile tam sınıf adı.</span><span class="sxs-lookup"><span data-stu-id="20d2e-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="20d2e-174">Bu parametre atlanırsa, bağlam sınıfını EF Core bulur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="20d2e-175">Birden çok bağlamı sınıfları varsa, bu parametre gereklidir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="20d2e-176">-Project \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-176">-Project \<String></span></span>        | <span data-ttu-id="20d2e-177">Hedef proje.</span><span class="sxs-lookup"><span data-stu-id="20d2e-177">The target project.</span></span> <span data-ttu-id="20d2e-178">Bu parametre atlanırsa, **varsayılan proje** için **Paket Yöneticisi Konsolu** hedef projesi olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="20d2e-179">-StartupProject \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-179">-StartupProject \<String></span></span> | <span data-ttu-id="20d2e-180">Başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="20d2e-180">The startup project.</span></span> <span data-ttu-id="20d2e-181">Bu parametre atlanırsa, **başlangıç projesi** içinde **çözüm özellikleri** hedef projesi olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="20d2e-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="20d2e-182">-Verbose</span></span>                  | <span data-ttu-id="20d2e-183">Ayrıntılı çıktıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="20d2e-184">Bir komut hakkında Yardım bilgilerini görüntülemek için PowerShell'in kullanın `Get-Help` komutu.</span><span class="sxs-lookup"><span data-stu-id="20d2e-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="20d2e-185">Bağlam, proje ve StartupProject parametreleri sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="20d2e-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="20d2e-186">Geçiş Ekle</span><span class="sxs-lookup"><span data-stu-id="20d2e-186">Add-Migration</span></span>

<span data-ttu-id="20d2e-187">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="20d2e-187">Adds a new migration.</span></span>

<span data-ttu-id="20d2e-188">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="20d2e-188">Parameters:</span></span>

| <span data-ttu-id="20d2e-189">Parametre</span><span class="sxs-lookup"><span data-stu-id="20d2e-189">Parameter</span></span>                         | <span data-ttu-id="20d2e-190">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20d2e-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="20d2e-191"><nobr>-Ad \<dizesi ><nobr></span><span class="sxs-lookup"><span data-stu-id="20d2e-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="20d2e-192">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="20d2e-192">The name of the migration.</span></span> <span data-ttu-id="20d2e-193">Bu, konumsal bir parametredir ve gereklidir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="20d2e-194"><nobr>-OutputDir \<dizesi ></nobr></span><span class="sxs-lookup"><span data-stu-id="20d2e-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="20d2e-195">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="20d2e-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="20d2e-196">Hedef proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="20d2e-197">Varsayılan olarak "Geçişler".</span><span class="sxs-lookup"><span data-stu-id="20d2e-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="20d2e-198">Veritabanını silme</span><span class="sxs-lookup"><span data-stu-id="20d2e-198">Drop-Database</span></span>

<span data-ttu-id="20d2e-199">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-199">Drops the database.</span></span>

<span data-ttu-id="20d2e-200">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="20d2e-200">Parameters:</span></span>

| <span data-ttu-id="20d2e-201">Parametre</span><span class="sxs-lookup"><span data-stu-id="20d2e-201">Parameter</span></span> | <span data-ttu-id="20d2e-202">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20d2e-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="20d2e-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="20d2e-203">-WhatIf</span></span>   | <span data-ttu-id="20d2e-204">Hangi veritabanı bırakılan ancak açılan yoksa gösterir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="20d2e-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="20d2e-205">Get-DbContext</span></span>

<span data-ttu-id="20d2e-206">Bilgi alır bir `DbContext` türü.</span><span class="sxs-lookup"><span data-stu-id="20d2e-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="20d2e-207">Remove-geçiş</span><span class="sxs-lookup"><span data-stu-id="20d2e-207">Remove-Migration</span></span>

<span data-ttu-id="20d2e-208">(Geçiş için yapıldığını kod değişiklikleri geri alır) son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="20d2e-209">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="20d2e-209">Parameters:</span></span>

| <span data-ttu-id="20d2e-210">Parametre</span><span class="sxs-lookup"><span data-stu-id="20d2e-210">Parameter</span></span> | <span data-ttu-id="20d2e-211">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20d2e-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="20d2e-212">-Force</span><span class="sxs-lookup"><span data-stu-id="20d2e-212">-Force</span></span>    | <span data-ttu-id="20d2e-213">Geçişi geri döndürme (veritabanına uygulanan değişiklikleri geri alma).</span><span class="sxs-lookup"><span data-stu-id="20d2e-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="20d2e-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="20d2e-214">Scaffold-DbContext</span></span>

<span data-ttu-id="20d2e-215">İçin kod oluşturur bir `DbContext` ve varlık türleri için bir veritabanı.</span><span class="sxs-lookup"><span data-stu-id="20d2e-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="20d2e-216">Sırayla `Scaffold-DbContext` bir varlık türü oluşturmak için veritabanı tablosunun birincil anahtarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="20d2e-217">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="20d2e-217">Parameters:</span></span>

| <span data-ttu-id="20d2e-218">Parametre</span><span class="sxs-lookup"><span data-stu-id="20d2e-218">Parameter</span></span>                          | <span data-ttu-id="20d2e-219">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20d2e-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="20d2e-220"><nobr>-Bağlantı \<dizesi ></nobr></span><span class="sxs-lookup"><span data-stu-id="20d2e-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="20d2e-221">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="20d2e-221">The connection string to the database.</span></span> <span data-ttu-id="20d2e-222">ASP.NET Core 2.x projeleri için bir değer olabilir *adı =\<bağlantı dizesi adı >*.</span><span class="sxs-lookup"><span data-stu-id="20d2e-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="20d2e-223">Bu durumda proje için ayarladığınız yapılandırma kaynaklarını adı gelir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="20d2e-224">Bu, konumsal bir parametredir ve gereklidir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="20d2e-225"><nobr>-Provider \<dizesi ></nobr></span><span class="sxs-lookup"><span data-stu-id="20d2e-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="20d2e-226">Kullanılacak sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="20d2e-226">The provider to use.</span></span> <span data-ttu-id="20d2e-227">Genellikle bu NuGet paketi, örneğin adıdır: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="20d2e-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="20d2e-228">Bu, konumsal bir parametredir ve gereklidir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="20d2e-229">-OutputDir \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-229">-OutputDir \<String></span></span>               | <span data-ttu-id="20d2e-230">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-230">The directory to put files in.</span></span> <span data-ttu-id="20d2e-231">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="20d2e-232">-ContextDir \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-232">-ContextDir \<String></span></span>              | <span data-ttu-id="20d2e-233">Yerleştirileceği dizin `DbContext` dosyası.</span><span class="sxs-lookup"><span data-stu-id="20d2e-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="20d2e-234">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="20d2e-235">-Bağlam \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-235">-Context \<String></span></span>                 | <span data-ttu-id="20d2e-236">Adını `DbContext` oluşturmak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="20d2e-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="20d2e-237">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="20d2e-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="20d2e-238">Şemaları için varlık türleri oluşturmak için tablo.</span><span class="sxs-lookup"><span data-stu-id="20d2e-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="20d2e-239">Bu parametre atlanırsa, tüm şemalar dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="20d2e-240">-Tablolar \<String [] ></span><span class="sxs-lookup"><span data-stu-id="20d2e-240">-Tables \<String[]></span></span>                | <span data-ttu-id="20d2e-241">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="20d2e-241">The tables to generate entity types for.</span></span> <span data-ttu-id="20d2e-242">Bu parametre atlanırsa, tüm tabloları dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="20d2e-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="20d2e-243">-DataAnnotations</span></span>                   | <span data-ttu-id="20d2e-244">Öznitelikler, model (mümkünse) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="20d2e-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="20d2e-245">Bu parametre atlanırsa, yalnızca fluent API'si kullanılır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="20d2e-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="20d2e-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="20d2e-247">Veritabanında tam olarak göründükleri gibi tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="20d2e-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="20d2e-248">Bu parametre atlanırsa, veritabanı adları daha yakından C# ad stil kurallarına uygun şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="20d2e-249">-Force</span><span class="sxs-lookup"><span data-stu-id="20d2e-249">-Force</span></span>                             | <span data-ttu-id="20d2e-250">Varolan dosyaların üzerine yaz.</span><span class="sxs-lookup"><span data-stu-id="20d2e-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="20d2e-251">Örnek:</span><span class="sxs-lookup"><span data-stu-id="20d2e-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="20d2e-252">Yalnızca seçilen tabloları iskele oluşturulduğunu ve ayrı bir klasörde belirtilen ada sahip bir bağlam oluşturur. örnek:</span><span class="sxs-lookup"><span data-stu-id="20d2e-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="20d2e-253">Geçiş betiği</span><span class="sxs-lookup"><span data-stu-id="20d2e-253">Script-Migration</span></span>

<span data-ttu-id="20d2e-254">Tüm değişiklikleri başka bir seçili geçiş için seçilen bir geçiş geçerli bir SQL betiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="20d2e-255">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="20d2e-255">Parameters:</span></span>

| <span data-ttu-id="20d2e-256">Parametre</span><span class="sxs-lookup"><span data-stu-id="20d2e-256">Parameter</span></span>                | <span data-ttu-id="20d2e-257">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20d2e-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="20d2e-258">*-From* \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-258">*-From* \<String></span></span>        | <span data-ttu-id="20d2e-259">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="20d2e-259">The starting migration.</span></span> <span data-ttu-id="20d2e-260">Geçişler, ada veya kimliğe göre tanımlanan</span><span class="sxs-lookup"><span data-stu-id="20d2e-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="20d2e-261">Sayısı 0 anlamına gelen bir özel durumdur *ilk geçişten önce*.</span><span class="sxs-lookup"><span data-stu-id="20d2e-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="20d2e-262">Varsayılan olarak 0.</span><span class="sxs-lookup"><span data-stu-id="20d2e-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="20d2e-263">*-Çok* \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-263">*-To* \<String></span></span>          | <span data-ttu-id="20d2e-264">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="20d2e-264">The ending migration.</span></span> <span data-ttu-id="20d2e-265">Son varsayılan olarak geçiş.</span><span class="sxs-lookup"><span data-stu-id="20d2e-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="20d2e-266"><nobr>-Bir kez etkili</nobr></span><span class="sxs-lookup"><span data-stu-id="20d2e-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="20d2e-267">Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="20d2e-268">-Çıkış \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="20d2e-268">-Output \<String></span></span>        | <span data-ttu-id="20d2e-269">Yazma sonucu dosyası.</span><span class="sxs-lookup"><span data-stu-id="20d2e-269">The file to write the result to.</span></span> <span data-ttu-id="20d2e-270">Bu parametre ATLANIRSA, uygulamanın çalışma zamanı dosyalarını, örneğin oluşturuldukça dosyası ile aynı klasörde oluşturulan bir ad oluşturulur: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="20d2e-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="20d2e-271">Kime, ve çıkış parametreleri sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="20d2e-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="20d2e-272">Aşağıdaki örnek, taşıma adını kullanarak InitialCreate geçiş için bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="20d2e-273">Aşağıdaki örnek, geçiş kimliğini kullanarak InitialCreate geçişten sonra tüm geçişler için bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="20d2e-274">Veritabanını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="20d2e-274">Update-Database</span></span>

<span data-ttu-id="20d2e-275">Son geçiş veya belirtilen bir geçiş için veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="20d2e-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="20d2e-276">Parametre</span><span class="sxs-lookup"><span data-stu-id="20d2e-276">Parameter</span></span>                           | <span data-ttu-id="20d2e-277">Açıklama</span><span class="sxs-lookup"><span data-stu-id="20d2e-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="20d2e-278"><nobr>*-Geçiş* \<dizesi ></nobr></span><span class="sxs-lookup"><span data-stu-id="20d2e-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="20d2e-279">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="20d2e-279">The target migration.</span></span> <span data-ttu-id="20d2e-280">Geçişler, ada veya kimliğe göre tanımlanan</span><span class="sxs-lookup"><span data-stu-id="20d2e-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="20d2e-281">Sayısı 0 anlamına gelen bir özel durumdur *ilk geçişten önce* ve tüm geçişler döndürülmesi neden olur.</span><span class="sxs-lookup"><span data-stu-id="20d2e-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="20d2e-282">Geçiş belirtilmişse komutu son geçiş varsayılan olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="20d2e-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="20d2e-283">Geçiş parametresi sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="20d2e-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="20d2e-284">Aşağıdaki örnek, tüm geçişler döner.</span><span class="sxs-lookup"><span data-stu-id="20d2e-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="20d2e-285">Aşağıdaki örnekler, belirtilen bir geçiş için veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="20d2e-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="20d2e-286">İlk geçiş adı ve ikinci geçiş kimliği kullanır:</span><span class="sxs-lookup"><span data-stu-id="20d2e-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="20d2e-287">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="20d2e-287">Additional resources</span></span>

* [<span data-ttu-id="20d2e-288">Geçişler</span><span class="sxs-lookup"><span data-stu-id="20d2e-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="20d2e-289">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="20d2e-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
