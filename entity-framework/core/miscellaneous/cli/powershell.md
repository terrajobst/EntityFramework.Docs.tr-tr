---
title: EF Core araçları başvurusu (Paket Yöneticisi konsolu)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: a9ce6d5b5f36a72e3715a9de787f1f00e989a58c
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811906"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="66286-102">Visual Studio 'da Entity Framework Core araçları başvurusu-Paket Yöneticisi konsolu</span><span class="sxs-lookup"><span data-stu-id="66286-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="66286-103">Entity Framework Core tasarım zamanı geliştirme görevlerini gerçekleştirmek için Paket Yöneticisi Konsolu (PMC) araçları.</span><span class="sxs-lookup"><span data-stu-id="66286-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="66286-104">Örneğin, [geçişler](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0)oluşturur, geçişleri uygular ve var olan bir veritabanını temel alan bir model için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66286-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="66286-105">Komutlar, Visual Studio içinde [Paket Yöneticisi konsolu](/nuget/tools/package-manager-console)kullanılarak çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="66286-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="66286-106">Bu araçlar hem .NET Framework hem de .NET Core projeleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="66286-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="66286-107">Visual Studio kullanmıyorsanız bunun yerine [EF Core komut satırı araçlarının](dotnet.md) kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="66286-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="66286-108">CLı araçları platformlar arası ve komut istemi içinde çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="66286-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="66286-109">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="66286-109">Installing the tools</span></span>

<span data-ttu-id="66286-110">Araçları yükleme ve güncelleştirme yordamları ASP.NET Core 2.1 + ile önceki sürümler veya diğer proje türleri arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="66286-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="66286-111">ASP.NET Core sürüm 2,1 ve üzeri</span><span class="sxs-lookup"><span data-stu-id="66286-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="66286-112">`Microsoft.EntityFrameworkCore.Tools` paketi [Microsoft. AspNetCore. app metapackage](/aspnet/core/fundamentals/metapackage-app)içinde yer aldığı için Araçlar ASP.NET Core 2.1 + projesine otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="66286-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="66286-113">Bu nedenle, araçları yüklemek için herhangi bir şey yapmanız gerekmez, ancak şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="66286-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="66286-114">Yeni bir projedeki araçları kullanmadan önce paketleri geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="66286-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="66286-115">Araçları daha yeni bir sürüme güncelleştirmek için bir paket yükler.</span><span class="sxs-lookup"><span data-stu-id="66286-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="66286-116">Araçların en son sürümünü aldığınızdan emin olmak için aşağıdaki adımı da yapmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="66286-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="66286-117">*. Csproj* dosyanızı düzenleyin ve [Microsoft. entityframeworkcore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) paketinin en son sürümünü belirten bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="66286-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="66286-118">Örneğin, *. csproj* dosyası şuna benzer bir `ItemGroup` içerebilir:</span><span class="sxs-lookup"><span data-stu-id="66286-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="66286-119">Aşağıdaki örnekte olduğu gibi bir ileti aldığınızda araçları güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="66286-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="66286-120">EF Core araçları sürümü ' 2.1.1-RTM-30846 ', ' 2.1.3-RTM-32065 ' çalışma zamanından daha eski.</span><span class="sxs-lookup"><span data-stu-id="66286-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="66286-121">En son özellikler ve hata düzeltmeleri için araçları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="66286-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="66286-122">Araçları güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="66286-122">To update the tools:</span></span>

* <span data-ttu-id="66286-123">En son .NET Core SDK yükler.</span><span class="sxs-lookup"><span data-stu-id="66286-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="66286-124">Visual Studio 'Yu en son sürüme güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="66286-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="66286-125">*. Csproj* dosyasını, daha önce gösterildiği gibi en son araçlar paketine bir paket başvurusu içerecek şekilde düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="66286-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="66286-126">Diğer sürümler ve proje türleri</span><span class="sxs-lookup"><span data-stu-id="66286-126">Other versions and project types</span></span>

<span data-ttu-id="66286-127">Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırarak Paket Yöneticisi konsol araçlarını **yükler:**</span><span class="sxs-lookup"><span data-stu-id="66286-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="66286-128">**Paket Yöneticisi konsolunda**aşağıdaki komutu çalıştırarak araçları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="66286-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="66286-129">Yüklemeyi doğrulama</span><span class="sxs-lookup"><span data-stu-id="66286-129">Verify the installation</span></span>

<span data-ttu-id="66286-130">Şu komutu çalıştırarak araçların yüklendiğini doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="66286-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="66286-131">Çıktı şuna benzer (hangi araçların hangi sürümünü kullandığınızı söylemez):</span><span class="sxs-lookup"><span data-stu-id="66286-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="66286-132">Araçları kullanma</span><span class="sxs-lookup"><span data-stu-id="66286-132">Using the tools</span></span>

<span data-ttu-id="66286-133">Araçları kullanmadan önce:</span><span class="sxs-lookup"><span data-stu-id="66286-133">Before using the tools:</span></span>

* <span data-ttu-id="66286-134">Hedef ve başlangıç projesi arasındaki farkı anlayın.</span><span class="sxs-lookup"><span data-stu-id="66286-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="66286-135">.NET Standard sınıf kitaplıklarıyla araçları nasıl kullanacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="66286-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="66286-136">ASP.NET Core projeler için ortamı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="66286-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="66286-137">Hedef ve başlangıç projesi</span><span class="sxs-lookup"><span data-stu-id="66286-137">Target and startup project</span></span>

<span data-ttu-id="66286-138">Komutlar bir *projeye* ve bir *başlangıç projesine*başvurur.</span><span class="sxs-lookup"><span data-stu-id="66286-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="66286-139">Ayrıca, komutların dosya eklemesi veya kaldırması nedeniyle *Proje* *hedef proje* olarak da bilinir.</span><span class="sxs-lookup"><span data-stu-id="66286-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="66286-140">Varsayılan olarak, **Paket Yöneticisi konsolunda** seçilen **varsayılan proje** hedef projem tir.</span><span class="sxs-lookup"><span data-stu-id="66286-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="66286-141"><nobr>`--project`</nobr> seçeneğini kullanarak, hedef proje olarak farklı bir proje belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66286-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="66286-142">*Başlangıç projesi* , araçların oluşturup çalıştırdığı bir.</span><span class="sxs-lookup"><span data-stu-id="66286-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="66286-143">Araçlar, veritabanı bağlantı dizesi ve modelin yapılandırması gibi proje hakkında bilgi almak için tasarım zamanında uygulama kodu yürütmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="66286-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="66286-144">Varsayılan olarak, Çözüm Gezgini **Başlangıç projesi** başlangıç projem ' dir.</span><span class="sxs-lookup"><span data-stu-id="66286-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="66286-145"><nobr>`--startup-project`</nobr> seçeneğini kullanarak, başlangıç projesi olarak farklı bir proje belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66286-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="66286-146">Başlangıç projesi ve hedef proje genellikle aynı projem.</span><span class="sxs-lookup"><span data-stu-id="66286-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="66286-147">Farklı projeler oldukları tipik bir senaryo şunlardır:</span><span class="sxs-lookup"><span data-stu-id="66286-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="66286-148">EF Core bağlamı ve varlık sınıfları bir .NET Core sınıf kitaplığında bulunur.</span><span class="sxs-lookup"><span data-stu-id="66286-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="66286-149">.NET Core konsol uygulaması veya Web uygulaması, sınıf kitaplığına başvurur.</span><span class="sxs-lookup"><span data-stu-id="66286-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="66286-150">[Geçiş kodunu, EF Core bağlamından ayrı bir sınıf kitaplığına yerleştirmek](xref:core/managing-schemas/migrations/projects)de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="66286-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="66286-151">Diğer hedef çerçeveler</span><span class="sxs-lookup"><span data-stu-id="66286-151">Other target frameworks</span></span>

<span data-ttu-id="66286-152">Paket Yöneticisi konsol araçları, .NET Core veya .NET Framework projeleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="66286-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="66286-153">.NET Standard Sınıf kitaplığındaki EF Core modeli olan uygulamalarda .NET Core veya .NET Framework projesi bulunmayabilir.</span><span class="sxs-lookup"><span data-stu-id="66286-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="66286-154">Örneğin, bu, Xamarin ve Evrensel Windows Platformu uygulamaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="66286-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="66286-155">Bu gibi durumlarda, yalnızca amacı araçlar için başlangıç projesi olarak davranacak olan bir .NET Core veya .NET Framework konsol uygulaması projesi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66286-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="66286-156">Proje, gerçek kod içermeyen bir kukla proje olabilir &mdash; yalnızca araç için bir hedef sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="66286-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="66286-157">İşlevsiz bir proje neden gereklidir?</span><span class="sxs-lookup"><span data-stu-id="66286-157">Why is a dummy project required?</span></span> <span data-ttu-id="66286-158">Daha önce belirtildiği gibi, araçların uygulama kodunu tasarım zamanında yürütmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="66286-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="66286-159">Bunu yapmak için, .NET Core veya .NET Framework çalışma zamanını kullanmaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="66286-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="66286-160">EF Core modeli .NET Core veya .NET Framework hedefleyen bir projede olduğunda, EF Core araçları projeden çalışma zamanını ödünç.</span><span class="sxs-lookup"><span data-stu-id="66286-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="66286-161">EF Core modeli .NET Standard bir sınıf kitaplığınlarsa bunu yapamazlar.</span><span class="sxs-lookup"><span data-stu-id="66286-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="66286-162">.NET Standard gerçek bir .NET uygulamasını değil; .NET uygulamalarının desteklemesi gereken bir API kümesine yönelik bir belirtimdir.</span><span class="sxs-lookup"><span data-stu-id="66286-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="66286-163">Bu nedenle .NET Standard uygulama kodunu yürütmek için EF Core araçları yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="66286-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="66286-164">Başlangıç projesi olarak kullanmak için oluşturduğunuz kukla proje, araçların .NET Standard sınıf kitaplığını yükleyebileceği somut bir hedef platform sağlar.</span><span class="sxs-lookup"><span data-stu-id="66286-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="66286-165">ASP.NET Core ortamı</span><span class="sxs-lookup"><span data-stu-id="66286-165">ASP.NET Core environment</span></span>

<span data-ttu-id="66286-166">ASP.NET Core projelerine yönelik ortamı belirtmek için, komutları çalıştırmadan önce **env: ASPNETCORE_ENVIRONMENT** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="66286-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="66286-167">Ortak Parametreler</span><span class="sxs-lookup"><span data-stu-id="66286-167">Common parameters</span></span>

<span data-ttu-id="66286-168">Aşağıdaki tabloda tüm EF Core komutlarında ortak olan parametreler gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="66286-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="66286-169">Parametre</span><span class="sxs-lookup"><span data-stu-id="66286-169">Parameter</span></span>                 | <span data-ttu-id="66286-170">Açıklama</span><span class="sxs-lookup"><span data-stu-id="66286-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="66286-171">-Context \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-171">-Context \<String></span></span>        | <span data-ttu-id="66286-172">Kullanılacak `DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="66286-172">The `DbContext` class to use.</span></span> <span data-ttu-id="66286-173">Yalnızca sınıf adı veya ad alanları ile tam nitelikli.</span><span class="sxs-lookup"><span data-stu-id="66286-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="66286-174">Bu parametre atlanırsa, EF Core bağlam sınıfını bulur.</span><span class="sxs-lookup"><span data-stu-id="66286-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="66286-175">Birden çok bağlam sınıfı varsa, bu parametre gereklidir.</span><span class="sxs-lookup"><span data-stu-id="66286-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="66286-176">-Proje \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-176">-Project \<String></span></span>        | <span data-ttu-id="66286-177">Hedef proje.</span><span class="sxs-lookup"><span data-stu-id="66286-177">The target project.</span></span> <span data-ttu-id="66286-178">Bu parametre atlanırsa, hedef proje olarak **Package Manager konsolunun** **varsayılan projesi** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66286-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="66286-179">-StartupProject \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-179">-StartupProject \<String></span></span> | <span data-ttu-id="66286-180">Başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="66286-180">The startup project.</span></span> <span data-ttu-id="66286-181">Bu parametre atlanırsa, **çözüm özelliklerindeki** **Başlangıç projesi** hedef proje olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66286-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="66286-182">-Ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="66286-182">-Verbose</span></span>                  | <span data-ttu-id="66286-183">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="66286-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="66286-184">Bir komutla ilgili yardım bilgilerini göstermek için PowerShell 'in `Get-Help` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="66286-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="66286-185">Bağlam, proje ve StartupProject parametreleri sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="66286-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="66286-186">Geçiş Ekle</span><span class="sxs-lookup"><span data-stu-id="66286-186">Add-Migration</span></span>

<span data-ttu-id="66286-187">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="66286-187">Adds a new migration.</span></span>

<span data-ttu-id="66286-188">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="66286-188">Parameters:</span></span>

| <span data-ttu-id="66286-189">Parametre</span><span class="sxs-lookup"><span data-stu-id="66286-189">Parameter</span></span>                         | <span data-ttu-id="66286-190">Açıklama</span><span class="sxs-lookup"><span data-stu-id="66286-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="66286-191"><nobr>ad \<dize ><nobr></span><span class="sxs-lookup"><span data-stu-id="66286-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="66286-192">Geçişin adı.</span><span class="sxs-lookup"><span data-stu-id="66286-192">The name of the migration.</span></span> <span data-ttu-id="66286-193">Bu bir Konumsal parametredir ve gereklidir.</span><span class="sxs-lookup"><span data-stu-id="66286-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="66286-194"><nobr>-OutputDir \<dize ></nobr></span><span class="sxs-lookup"><span data-stu-id="66286-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="66286-195">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="66286-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="66286-196">Yollar, hedef proje dizini ile ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="66286-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="66286-197">Varsayılan olarak "geçişler" olur.</span><span class="sxs-lookup"><span data-stu-id="66286-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="66286-198">Veritabanını bırak</span><span class="sxs-lookup"><span data-stu-id="66286-198">Drop-Database</span></span>

<span data-ttu-id="66286-199">Veritabanını bırakır.</span><span class="sxs-lookup"><span data-stu-id="66286-199">Drops the database.</span></span>

<span data-ttu-id="66286-200">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="66286-200">Parameters:</span></span>

| <span data-ttu-id="66286-201">Parametre</span><span class="sxs-lookup"><span data-stu-id="66286-201">Parameter</span></span> | <span data-ttu-id="66286-202">Açıklama</span><span class="sxs-lookup"><span data-stu-id="66286-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="66286-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="66286-203">-WhatIf</span></span>   | <span data-ttu-id="66286-204">Hangi veritabanının bırakılacağını gösterir, ancak onları düşürüyor.</span><span class="sxs-lookup"><span data-stu-id="66286-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="66286-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="66286-205">Get-DbContext</span></span>

<span data-ttu-id="66286-206">`DbContext` türü hakkında bilgi alır.</span><span class="sxs-lookup"><span data-stu-id="66286-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="66286-207">Geçişi Kaldır</span><span class="sxs-lookup"><span data-stu-id="66286-207">Remove-Migration</span></span>

<span data-ttu-id="66286-208">Son geçişi kaldırır (geçiş için yapılan kod değişikliklerini geri kaydeder).</span><span class="sxs-lookup"><span data-stu-id="66286-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="66286-209">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="66286-209">Parameters:</span></span>

| <span data-ttu-id="66286-210">Parametre</span><span class="sxs-lookup"><span data-stu-id="66286-210">Parameter</span></span> | <span data-ttu-id="66286-211">Açıklama</span><span class="sxs-lookup"><span data-stu-id="66286-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="66286-212">-Zorla</span><span class="sxs-lookup"><span data-stu-id="66286-212">-Force</span></span>    | <span data-ttu-id="66286-213">Geçişi geri alma (veritabanına uygulanan değişiklikleri geri alın).</span><span class="sxs-lookup"><span data-stu-id="66286-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="66286-214">Yapı iskelesi-DbContext</span><span class="sxs-lookup"><span data-stu-id="66286-214">Scaffold-DbContext</span></span>

<span data-ttu-id="66286-215">Bir veritabanı için `DbContext` ve varlık türleri için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="66286-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="66286-216">`Scaffold-DbContext` bir varlık türü oluşturmak için veritabanı tablosunun birincil anahtarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="66286-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="66286-217">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="66286-217">Parameters:</span></span>

| <span data-ttu-id="66286-218">Parametre</span><span class="sxs-lookup"><span data-stu-id="66286-218">Parameter</span></span>                          | <span data-ttu-id="66286-219">Açıklama</span><span class="sxs-lookup"><span data-stu-id="66286-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="66286-220"><nobr>-Bağlantı \<dize ></nobr></span><span class="sxs-lookup"><span data-stu-id="66286-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="66286-221">Veritabanına bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="66286-221">The connection string to the database.</span></span> <span data-ttu-id="66286-222">ASP.NET Core 2. x projeleri için değer *=\<bağlantı dizesi >* adı olabilir.</span><span class="sxs-lookup"><span data-stu-id="66286-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="66286-223">Bu durumda, ad proje için ayarlanan yapılandırma kaynaklarından gelir.</span><span class="sxs-lookup"><span data-stu-id="66286-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="66286-224">Bu bir Konumsal parametredir ve gereklidir.</span><span class="sxs-lookup"><span data-stu-id="66286-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="66286-225"><nobr>-Sağlayıcı \<dize ></nobr></span><span class="sxs-lookup"><span data-stu-id="66286-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="66286-226">Kullanılacak sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="66286-226">The provider to use.</span></span> <span data-ttu-id="66286-227">Genellikle bu, NuGet paketinin adıdır, örneğin: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="66286-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="66286-228">Bu bir Konumsal parametredir ve gereklidir.</span><span class="sxs-lookup"><span data-stu-id="66286-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="66286-229">-OutputDir \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-229">-OutputDir \<String></span></span>               | <span data-ttu-id="66286-230">Dosyaları içine koyabileceğiniz dizin.</span><span class="sxs-lookup"><span data-stu-id="66286-230">The directory to put files in.</span></span> <span data-ttu-id="66286-231">Yollar proje dizinine göredir.</span><span class="sxs-lookup"><span data-stu-id="66286-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="66286-232">-ContextDir \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-232">-ContextDir \<String></span></span>              | <span data-ttu-id="66286-233">`DbContext` dosyasının içine yerleştirilecek dizin.</span><span class="sxs-lookup"><span data-stu-id="66286-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="66286-234">Yollar proje dizinine göredir.</span><span class="sxs-lookup"><span data-stu-id="66286-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="66286-235">-Context \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-235">-Context \<String></span></span>                 | <span data-ttu-id="66286-236">Oluşturulacak `DbContext` sınıfın adı.</span><span class="sxs-lookup"><span data-stu-id="66286-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="66286-237">-Şema \<dize [] ></span><span class="sxs-lookup"><span data-stu-id="66286-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="66286-238">İçin varlık türleri oluşturulacak tablo şemaları.</span><span class="sxs-lookup"><span data-stu-id="66286-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="66286-239">Bu parametre atlanırsa, tüm şemalar dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="66286-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="66286-240">-Tables \<dize [] ></span><span class="sxs-lookup"><span data-stu-id="66286-240">-Tables \<String[]></span></span>                | <span data-ttu-id="66286-241">İçin varlık türleri oluşturulacak tablolar.</span><span class="sxs-lookup"><span data-stu-id="66286-241">The tables to generate entity types for.</span></span> <span data-ttu-id="66286-242">Bu parametre atlanırsa, tüm tablolar dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="66286-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="66286-243">-Datanot açıklamaları</span><span class="sxs-lookup"><span data-stu-id="66286-243">-DataAnnotations</span></span>                   | <span data-ttu-id="66286-244">Modeli yapılandırmak için öznitelikleri kullanın (mümkün olduğunda).</span><span class="sxs-lookup"><span data-stu-id="66286-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="66286-245">Bu parametre atlanırsa yalnızca Fluent API kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66286-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="66286-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="66286-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="66286-247">Tablo ve sütun adlarını tam olarak veritabanında göründükleri gibi kullanın.</span><span class="sxs-lookup"><span data-stu-id="66286-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="66286-248">Bu parametre atlanırsa, veritabanı adları C# ad stili kurallarıyla daha yakından uyumlu olacak şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="66286-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="66286-249">-Zorla</span><span class="sxs-lookup"><span data-stu-id="66286-249">-Force</span></span>                             | <span data-ttu-id="66286-250">Varolan dosyaların üzerine yaz.</span><span class="sxs-lookup"><span data-stu-id="66286-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="66286-251">Örnek:</span><span class="sxs-lookup"><span data-stu-id="66286-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="66286-252">Yalnızca seçili tabloları oluşturan ve bağlamı belirtilen bir adla ayrı bir klasörde oluşturan örnek:</span><span class="sxs-lookup"><span data-stu-id="66286-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="66286-253">Betik geçişi</span><span class="sxs-lookup"><span data-stu-id="66286-253">Script-Migration</span></span>

<span data-ttu-id="66286-254">Seçilen bir geçişin tüm değişikliklerini seçili başka bir geçişe uygulayan bir SQL betiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66286-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="66286-255">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="66286-255">Parameters:</span></span>

| <span data-ttu-id="66286-256">Parametre</span><span class="sxs-lookup"><span data-stu-id="66286-256">Parameter</span></span>                | <span data-ttu-id="66286-257">Açıklama</span><span class="sxs-lookup"><span data-stu-id="66286-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="66286-258">*-* \<dizeden ></span><span class="sxs-lookup"><span data-stu-id="66286-258">*-From* \<String></span></span>        | <span data-ttu-id="66286-259">Geçiş başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="66286-259">The starting migration.</span></span> <span data-ttu-id="66286-260">Geçişler, ada veya KIMLIĞE göre tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="66286-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="66286-261">0 sayısı, *ilk geçişten önceki*anlamına gelen özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="66286-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="66286-262">Varsayılan değer 0 ' dır.</span><span class="sxs-lookup"><span data-stu-id="66286-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="66286-263">*-* \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-263">*-To* \<String></span></span>          | <span data-ttu-id="66286-264">Son geçiş.</span><span class="sxs-lookup"><span data-stu-id="66286-264">The ending migration.</span></span> <span data-ttu-id="66286-265">Son geçişin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="66286-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="66286-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="66286-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="66286-267">Herhangi bir geçişte veritabanında kullanılabilecek bir betik oluşturun.</span><span class="sxs-lookup"><span data-stu-id="66286-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="66286-268">-Output \<dize ></span><span class="sxs-lookup"><span data-stu-id="66286-268">-Output \<String></span></span>        | <span data-ttu-id="66286-269">Sonucun yazılacağı dosya.</span><span class="sxs-lookup"><span data-stu-id="66286-269">The file to write the result to.</span></span> <span data-ttu-id="66286-270">Bu parametre atlanırsa dosya, uygulamanın çalışma zamanı dosyaları oluşturulduğu klasörde oluşturulmuş bir adla oluşturulur, örneğin: */obj/Debug/netcoreapp2,/ghbkztfz.exe*.</span><span class="sxs-lookup"><span data-stu-id="66286-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="66286-271">To, from ve OUTPUT parametreleri sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="66286-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="66286-272">Aşağıdaki örnek, geçiş adı kullanılarak, ınitialcreate geçişi için bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66286-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="66286-273">Aşağıdaki örnek, geçiş KIMLIĞI kullanılarak, ınitialcreate geçişinden sonra tüm geçişler için bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66286-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="66286-274">Güncelleştirme-veritabanı</span><span class="sxs-lookup"><span data-stu-id="66286-274">Update-Database</span></span>

<span data-ttu-id="66286-275">Veritabanını son geçişe veya belirtilen bir geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="66286-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="66286-276">Parametre</span><span class="sxs-lookup"><span data-stu-id="66286-276">Parameter</span></span>                           | <span data-ttu-id="66286-277">Açıklama</span><span class="sxs-lookup"><span data-stu-id="66286-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="66286-278"><nobr> *-Geçiş* \<dize ></nobr></span><span class="sxs-lookup"><span data-stu-id="66286-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="66286-279">Hedef geçişi.</span><span class="sxs-lookup"><span data-stu-id="66286-279">The target migration.</span></span> <span data-ttu-id="66286-280">Geçişler, ada veya KIMLIĞE göre tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="66286-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="66286-281">0 sayısı, *ilk geçişten önceki* ve tüm geçişlerin geri alınmasına neden olan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="66286-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="66286-282">Hiçbir geçiş belirtilmemişse, komut en son geçişe varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="66286-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="66286-283">Geçiş parametresi sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="66286-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="66286-284">Aşağıdaki örnek tüm geçişleri geri döndürür.</span><span class="sxs-lookup"><span data-stu-id="66286-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="66286-285">Aşağıdaki örnekler veritabanını belirtilen bir geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="66286-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="66286-286">İlki geçiş adını, ikincisi ise geçiş KIMLIĞINI kullanır:</span><span class="sxs-lookup"><span data-stu-id="66286-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="66286-287">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="66286-287">Additional resources</span></span>

* [<span data-ttu-id="66286-288">Geçişler</span><span class="sxs-lookup"><span data-stu-id="66286-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="66286-289">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="66286-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
