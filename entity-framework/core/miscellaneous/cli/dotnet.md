---
title: .NET core CLI - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754502"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="1bdf2-102">EF çekirdek .NET komut satırı araçları</span><span class="sxs-lookup"><span data-stu-id="1bdf2-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="1bdf2-103">Platformlar arası uzantısı Entity Framework Çekirdek .NET komut satırı araçları olan **dotnet** parçasıdır komutu, [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="1bdf2-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="1bdf2-104">Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="1bdf2-105">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="1bdf2-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="1bdf2-106">.NET Core SDK'sı sürüm 2.1.300 ve daha yeni içerir **dotnet ef** EF çekirdek 2.0 ve sonraki sürümleri ile uyumlu olan komutları.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="1bdf2-107">Bu nedenle .NET Core SDK'sı ve EF çekirdeği çalışma zamanı en güncel sürümlerini kullanıyorsanız, herhangi bir yükleme gereklidir ve bu bölümde rest yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="1bdf2-108">Diğer taraftan, **dotnet ef** aracı .NET Core SDK sürüm 2.1.300 içerdiği ve daha yeni sürümü 1.0 ve 1.1 EF çekirdek sürümü ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="1bdf2-109">.NET Core SDK 2.1.300 sahip bir bilgisayarda bu sürümlerde EF çekirdek kullanan bir proje ile çalışabilirsiniz ya da daha yeni yüklü önce sürüm 2.1.200 de yüklemeniz gerekir ya da SDK'ın eski ve uygulamayı değiştirerek, eski sürümünün kullanacak şekilde yapılandırın,  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) dosya.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="1bdf2-110">Bu dosya, normalde çözüm dizini (bir proje üzerinde) bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="1bdf2-111">Ardından installlation talimatıyla devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="1bdf2-112">.NET Core SDK önceki sürümleri için aşağıdaki adımları kullanarak EF çekirdek .NET komut satırı araçları yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="1bdf2-113">Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle</span><span class="sxs-lookup"><span data-stu-id="1bdf2-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="1bdf2-114">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="1bdf2-115">Elde edilen proje aşağıdakine benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="1bdf2-116">Bir paket başvurusuyla `PrivateAssets="All"` değil sunulan genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu proje başvurusu projelerine anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="1bdf2-117">Her şeyi doğru kaldırdıysanız, başarılı bir şekilde bir komut isteminde aşağıdaki komutu çalıştırın olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="1bdf2-118">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="1bdf2-118">Using the tools</span></span>
---------------
<span data-ttu-id="1bdf2-119">Bir komut çağırma her iki proje ilgili vardır:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="1bdf2-120">Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="1bdf2-121">Hedef projeyi projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **--proje** </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="1bdf2-122">Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="1bdf2-123">Ayrıca projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir **--başlangıç projesi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="1bdf2-124">Örneğin, web uygulamanızın EF çekirdek farklı projede yüklü olan veritabanını güncelleştirme şuna benzer: `dotnet ef database update --project {project-path}` (dizininizden web uygulaması)</span><span class="sxs-lookup"><span data-stu-id="1bdf2-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="1bdf2-125">Ortak seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="1bdf2-126">--json</span><span class="sxs-lookup"><span data-stu-id="1bdf2-126">--json</span></span>                           | <span data-ttu-id="1bdf2-127">JSON çıktısını gösterir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-127">Show JSON output.</span></span>           |
| <span data-ttu-id="1bdf2-128">-c</span><span class="sxs-lookup"><span data-stu-id="1bdf2-128">-c</span></span> | <span data-ttu-id="1bdf2-129">--bağlam \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="1bdf2-130">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="1bdf2-131">-p</span><span class="sxs-lookup"><span data-stu-id="1bdf2-131">-p</span></span> | <span data-ttu-id="1bdf2-132">--Proje \<Proje ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-132">--project \<PROJECT></span></span>             | <span data-ttu-id="1bdf2-133">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-133">The project to use.</span></span>         |
| <span data-ttu-id="1bdf2-134">-s</span><span class="sxs-lookup"><span data-stu-id="1bdf2-134">-s</span></span> | <span data-ttu-id="1bdf2-135">--başlangıç projesi \<Proje ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="1bdf2-136">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="1bdf2-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="1bdf2-138">Hedef çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-138">The target framework.</span></span>       |
|    | <span data-ttu-id="1bdf2-139">--configuration \<yapılandırma ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="1bdf2-140">Kullanılacak yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="1bdf2-141">--çalışma zamanı \<TANIMLAYICISI ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="1bdf2-142">Çalışma zamanı.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-142">The runtime to use.</span></span>         |
| <span data-ttu-id="1bdf2-143">-h</span><span class="sxs-lookup"><span data-stu-id="1bdf2-143">-h</span></span> | <span data-ttu-id="1bdf2-144">--Yardım</span><span class="sxs-lookup"><span data-stu-id="1bdf2-144">--help</span></span>                           | <span data-ttu-id="1bdf2-145">Yardım bilgilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-145">Show help information.</span></span>      |
| <span data-ttu-id="1bdf2-146">-v</span><span class="sxs-lookup"><span data-stu-id="1bdf2-146">-v</span></span> | <span data-ttu-id="1bdf2-147">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="1bdf2-147">--verbose</span></span>                        | <span data-ttu-id="1bdf2-148">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="1bdf2-149">--renk yok</span><span class="sxs-lookup"><span data-stu-id="1bdf2-149">--no-color</span></span>                       | <span data-ttu-id="1bdf2-150">Çıktı renklendirme yok.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="1bdf2-151">--önek çıktı</span><span class="sxs-lookup"><span data-stu-id="1bdf2-151">--prefix-output</span></span>                  | <span data-ttu-id="1bdf2-152">Düzeyiyle çıktı öneki.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="1bdf2-153">ASP.NET Core ortam belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="1bdf2-154">Komutlar</span><span class="sxs-lookup"><span data-stu-id="1bdf2-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="1bdf2-155">DotNet ef veritabanı bırakma</span><span class="sxs-lookup"><span data-stu-id="1bdf2-155">dotnet ef database drop</span></span>

<span data-ttu-id="1bdf2-156">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-156">Drops the database.</span></span>

<span data-ttu-id="1bdf2-157">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="1bdf2-158">-f</span><span class="sxs-lookup"><span data-stu-id="1bdf2-158">-f</span></span> | <span data-ttu-id="1bdf2-159">--zorla</span><span class="sxs-lookup"><span data-stu-id="1bdf2-159">--force</span></span>   | <span data-ttu-id="1bdf2-160">Onaylayın yok.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="1bdf2-161">--çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1bdf2-161">--dry-run</span></span> | <span data-ttu-id="1bdf2-162">Hangi veritabanı olduğundan bırakılamaz, ancak bırakma yok gösterir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="1bdf2-163">DotNet ef veritabanı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="1bdf2-163">dotnet ef database update</span></span>

<span data-ttu-id="1bdf2-164">Belirtilen geçiş için veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="1bdf2-165">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="1bdf2-166">\<GEÇİŞ &GT;</span><span class="sxs-lookup"><span data-stu-id="1bdf2-166">\<MIGRATION></span></span> | <span data-ttu-id="1bdf2-167">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-167">The target migration.</span></span> <span data-ttu-id="1bdf2-168">0 ise, tüm geçişler geri alınacak.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="1bdf2-169">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="1bdf2-170">DotNet ef dbcontext bilgisi</span><span class="sxs-lookup"><span data-stu-id="1bdf2-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="1bdf2-171">DbContext türü hakkındaki bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="1bdf2-172">DotNet ef dbcontext listesi</span><span class="sxs-lookup"><span data-stu-id="1bdf2-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="1bdf2-173">Kullanılabilir DbContext türleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="1bdf2-174">DotNet ef dbcontext iskele</span><span class="sxs-lookup"><span data-stu-id="1bdf2-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="1bdf2-175">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="1bdf2-176">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="1bdf2-177">\<BAĞLANTI &GT;</span><span class="sxs-lookup"><span data-stu-id="1bdf2-177">\<CONNECTION></span></span> | <span data-ttu-id="1bdf2-178">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="1bdf2-179">\<SAĞLAYICI &GT;</span><span class="sxs-lookup"><span data-stu-id="1bdf2-179">\<PROVIDER></span></span>   | <span data-ttu-id="1bdf2-180">Kullanılacak sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-180">The provider to use.</span></span> <span data-ttu-id="1bdf2-181">(örneğin, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="1bdf2-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="1bdf2-182">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1bdf2-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="1bdf2-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="1bdf2-184">--Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="1bdf2-184">--data-annotations</span></span>                      | <span data-ttu-id="1bdf2-185">Öznitelikler, model (mümkün olduğunda) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="1bdf2-186">Atlanırsa, yalnızca fluent API kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="1bdf2-187">-c</span><span class="sxs-lookup"><span data-stu-id="1bdf2-187">-c</span></span>              | <span data-ttu-id="1bdf2-188">--bağlam \<adı ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-188">--context \<NAME></span></span>                       | <span data-ttu-id="1bdf2-189">DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="1bdf2-190">--bağlam dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="1bdf2-191">DbContext dosyasını yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="1bdf2-192">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="1bdf2-193">-f</span><span class="sxs-lookup"><span data-stu-id="1bdf2-193">-f</span></span>              | <span data-ttu-id="1bdf2-194">--zorla</span><span class="sxs-lookup"><span data-stu-id="1bdf2-194">--force</span></span>                                 | <span data-ttu-id="1bdf2-195">Var olan dosyaların üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="1bdf2-196">-o</span><span class="sxs-lookup"><span data-stu-id="1bdf2-196">-o</span></span>              | <span data-ttu-id="1bdf2-197">--Çıkış dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="1bdf2-198">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-198">The directory to put files in.</span></span> <span data-ttu-id="1bdf2-199">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="1bdf2-200"><nobr>--şema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="1bdf2-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="1bdf2-201">Varlık türleri oluşturmak için tabloları şemalar.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="1bdf2-202">-t</span><span class="sxs-lookup"><span data-stu-id="1bdf2-202">-t</span></span>              | <span data-ttu-id="1bdf2-203">--Tablo \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="1bdf2-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="1bdf2-204">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="1bdf2-205">--adları veritabanı kullan</span><span class="sxs-lookup"><span data-stu-id="1bdf2-205">--use-database-names</span></span>                    | <span data-ttu-id="1bdf2-206">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="1bdf2-207">DotNet ef geçişler ekleme</span><span class="sxs-lookup"><span data-stu-id="1bdf2-207">dotnet ef migrations add</span></span>

<span data-ttu-id="1bdf2-208">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-208">Adds a new migration.</span></span>

<span data-ttu-id="1bdf2-209">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="1bdf2-210">\<ADI &GT;</span><span class="sxs-lookup"><span data-stu-id="1bdf2-210">\<NAME></span></span> | <span data-ttu-id="1bdf2-211">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-211">The name of the migration.</span></span> |

<span data-ttu-id="1bdf2-212">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1bdf2-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="1bdf2-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="1bdf2-214"><nobr>--Çıkış dir \<yolu ></nobr></span><span class="sxs-lookup"><span data-stu-id="1bdf2-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="1bdf2-215">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="1bdf2-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="1bdf2-216">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="1bdf2-217">Varsayılan olarak "Geçiş".</span><span class="sxs-lookup"><span data-stu-id="1bdf2-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="1bdf2-218">DotNet ef geçişler listesi</span><span class="sxs-lookup"><span data-stu-id="1bdf2-218">dotnet ef migrations list</span></span>

<span data-ttu-id="1bdf2-219">Kullanılabilir geçişler listeler.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="1bdf2-220">DotNet ef geçişler Kaldır</span><span class="sxs-lookup"><span data-stu-id="1bdf2-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="1bdf2-221">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-221">Removes the last migration.</span></span>

<span data-ttu-id="1bdf2-222">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="1bdf2-223">-f</span><span class="sxs-lookup"><span data-stu-id="1bdf2-223">-f</span></span> | <span data-ttu-id="1bdf2-224">--zorla</span><span class="sxs-lookup"><span data-stu-id="1bdf2-224">--force</span></span> | <span data-ttu-id="1bdf2-225">Veritabanına uyguladıysanız geçişi geri alın.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="1bdf2-226">DotNet ef geçişler komut dosyası</span><span class="sxs-lookup"><span data-stu-id="1bdf2-226">dotnet ef migrations script</span></span>

<span data-ttu-id="1bdf2-227">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="1bdf2-228">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="1bdf2-229">\<GELEN &GT;</span><span class="sxs-lookup"><span data-stu-id="1bdf2-229">\<FROM></span></span> | <span data-ttu-id="1bdf2-230">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-230">The starting migration.</span></span> <span data-ttu-id="1bdf2-231">Varsayılan ayar: 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="1bdf2-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="1bdf2-232">\<İÇİN &GT;</span><span class="sxs-lookup"><span data-stu-id="1bdf2-232">\<TO></span></span>   | <span data-ttu-id="1bdf2-233">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-233">The ending migration.</span></span> <span data-ttu-id="1bdf2-234">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="1bdf2-235">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="1bdf2-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="1bdf2-236">-o</span><span class="sxs-lookup"><span data-stu-id="1bdf2-236">-o</span></span> | <span data-ttu-id="1bdf2-237">--Çıkış \<dosyası ></span><span class="sxs-lookup"><span data-stu-id="1bdf2-237">--output \<FILE></span></span> | <span data-ttu-id="1bdf2-238">Sonucu yazmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="1bdf2-239">-i</span><span class="sxs-lookup"><span data-stu-id="1bdf2-239">-i</span></span> | <span data-ttu-id="1bdf2-240">--ıdempotent</span><span class="sxs-lookup"><span data-stu-id="1bdf2-240">--idempotent</span></span>     | <span data-ttu-id="1bdf2-241">Bir veritabanında hiçbir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1bdf2-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
