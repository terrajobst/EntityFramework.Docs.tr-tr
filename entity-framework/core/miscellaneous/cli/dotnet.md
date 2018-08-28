---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995303"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="d9b79-102">EF Core .NET komut satırı araçları</span><span class="sxs-lookup"><span data-stu-id="d9b79-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="d9b79-103">Entity Framework Core ve .NET komut satırı araçları, platformlar arası bir uzantısı olan **dotnet** parçası olan komut, [.NET Core SDK'sı][2].</span><span class="sxs-lookup"><span data-stu-id="d9b79-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="d9b79-104">Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.</span><span class="sxs-lookup"><span data-stu-id="d9b79-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="d9b79-105">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="d9b79-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="d9b79-106">.NET Core SDK'sı sürüm 2.1.300 ve yeni içerir **dotnet ef** EF Core 2.0 ve sonraki sürümler ile uyumlu olan komutları.</span><span class="sxs-lookup"><span data-stu-id="d9b79-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="d9b79-107">Bu nedenle .NET Core SDK'sını ve EF Core çalışma zamanı son sürümlerini kullanıyorsanız, herhangi bir yükleme gereklidir ve bu bölümün geri kalanında yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9b79-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="d9b79-108">Öte yandan, **dotnet ef** aracı .NET Core SDK'sı sürüm 2.1.300 içerdiği ve yeni EF Core 1.0 ve 1.1 sürümüyle uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="d9b79-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="d9b79-109">.NET Core SDK'sı 2.1.300 olan bir bilgisayarda bu sürümlerde EF Core kullanan bir proje ile çalışabilir veya üzerinin yüklü önce sürüm 2.1.200 da yüklemeniz gerekir ya da SDK'ın eski ve değiştirerek, eski sürümünün kullanmak için uygulamayı yapılandırma,  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) dosya.</span><span class="sxs-lookup"><span data-stu-id="d9b79-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="d9b79-110">Normalde bu dosya çözüm dizini (bir proje üzerinde) dahildir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="d9b79-111">Ardından installlation talimatıyla geçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9b79-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="d9b79-112">.NET Core SDK'sının önceki sürümleri için aşağıdaki adımları kullanarak EF Core .NET komut satırı araçları yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d9b79-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="d9b79-113">Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle</span><span class="sxs-lookup"><span data-stu-id="d9b79-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="d9b79-114">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d9b79-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="d9b79-115">Elde edilen proje şunun gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="d9b79-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="d9b79-116">Bir paket başvurusu ile `PrivateAssets="All"` değilse ifşa genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu projeye başvuran projeler için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="d9b79-117">Her şeyi doğru kaldırdıysanız, başarıyla bir komut isteminde aşağıdaki komutu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9b79-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="d9b79-118">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="d9b79-118">Using the tools</span></span>
---------------
<span data-ttu-id="d9b79-119">Bir komut çağırma her iki proje kullanılan vardır:</span><span class="sxs-lookup"><span data-stu-id="d9b79-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="d9b79-120">Hedef dosyalar eklenir (veya bazı durumlarda kaldırıldı) projesidir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="d9b79-121">Hedef proje geçerli dizinde proje için varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **--proje** </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="d9b79-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="d9b79-122">Başlangıç projesi, proje kodunuzun yürütülürken araçları tarafından Öykünülen bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="d9b79-123">Ayrıca geçerli dizinde proje için varsayılan olarak, ancak kullanılarak değiştirilebilir **--başlangıç projesi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="d9b79-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="d9b79-124">Örneğin, web uygulamanızın yüklü farklı bir projede EF Core sahip veritabanını güncelleme şuna benzer: `dotnet ef database update --project {project-path}` (dizininizden web uygulaması)</span><span class="sxs-lookup"><span data-stu-id="d9b79-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="d9b79-125">Genel Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="d9b79-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="d9b79-126">--json</span><span class="sxs-lookup"><span data-stu-id="d9b79-126">--json</span></span>                           | <span data-ttu-id="d9b79-127">JSON çıktıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-127">Show JSON output.</span></span>           |
| <span data-ttu-id="d9b79-128">-c</span><span class="sxs-lookup"><span data-stu-id="d9b79-128">-c</span></span> | <span data-ttu-id="d9b79-129">--bağlam \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="d9b79-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="d9b79-130">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="d9b79-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="d9b79-131">-p</span><span class="sxs-lookup"><span data-stu-id="d9b79-131">-p</span></span> | <span data-ttu-id="d9b79-132">--Proje \<Proje ></span><span class="sxs-lookup"><span data-stu-id="d9b79-132">--project \<PROJECT></span></span>             | <span data-ttu-id="d9b79-133">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="d9b79-133">The project to use.</span></span>         |
| <span data-ttu-id="d9b79-134">-s</span><span class="sxs-lookup"><span data-stu-id="d9b79-134">-s</span></span> | <span data-ttu-id="d9b79-135">--başlangıç projesi \<Proje ></span><span class="sxs-lookup"><span data-stu-id="d9b79-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="d9b79-136">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="d9b79-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="d9b79-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="d9b79-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="d9b79-138">Hedef Framework'ü.</span><span class="sxs-lookup"><span data-stu-id="d9b79-138">The target framework.</span></span>       |
|    | <span data-ttu-id="d9b79-139">--configuration \<yapılandırma ></span><span class="sxs-lookup"><span data-stu-id="d9b79-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="d9b79-140">Kullanılacak yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="d9b79-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="d9b79-141">--çalışma zamanı \<TANIMLAYICI ></span><span class="sxs-lookup"><span data-stu-id="d9b79-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="d9b79-142">Kullanmak için çalışma zamanı.</span><span class="sxs-lookup"><span data-stu-id="d9b79-142">The runtime to use.</span></span>         |
| <span data-ttu-id="d9b79-143">-h</span><span class="sxs-lookup"><span data-stu-id="d9b79-143">-h</span></span> | <span data-ttu-id="d9b79-144">--Yardım</span><span class="sxs-lookup"><span data-stu-id="d9b79-144">--help</span></span>                           | <span data-ttu-id="d9b79-145">Yardım bilgilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-145">Show help information.</span></span>      |
| <span data-ttu-id="d9b79-146">-v</span><span class="sxs-lookup"><span data-stu-id="d9b79-146">-v</span></span> | <span data-ttu-id="d9b79-147">--verbose</span><span class="sxs-lookup"><span data-stu-id="d9b79-147">--verbose</span></span>                        | <span data-ttu-id="d9b79-148">Ayrıntılı çıktıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="d9b79-149">--no-rengi</span><span class="sxs-lookup"><span data-stu-id="d9b79-149">--no-color</span></span>                       | <span data-ttu-id="d9b79-150">Çıkış renklendirmeye yok.</span><span class="sxs-lookup"><span data-stu-id="d9b79-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="d9b79-151">--Çıktı ön eki</span><span class="sxs-lookup"><span data-stu-id="d9b79-151">--prefix-output</span></span>                  | <span data-ttu-id="d9b79-152">Düzeyiyle çıkış öneki.</span><span class="sxs-lookup"><span data-stu-id="d9b79-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="d9b79-153">ASP.NET Core ortamını belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d9b79-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="d9b79-154">Komutlar</span><span class="sxs-lookup"><span data-stu-id="d9b79-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="d9b79-155">DotNet ef veritabanını bırak</span><span class="sxs-lookup"><span data-stu-id="d9b79-155">dotnet ef database drop</span></span>

<span data-ttu-id="d9b79-156">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="d9b79-156">Drops the database.</span></span>

<span data-ttu-id="d9b79-157">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="d9b79-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="d9b79-158">-f</span><span class="sxs-lookup"><span data-stu-id="d9b79-158">-f</span></span> | <span data-ttu-id="d9b79-159">--force</span><span class="sxs-lookup"><span data-stu-id="d9b79-159">--force</span></span>   | <span data-ttu-id="d9b79-160">Onay isteme.</span><span class="sxs-lookup"><span data-stu-id="d9b79-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="d9b79-161">--Prova amaçlı</span><span class="sxs-lookup"><span data-stu-id="d9b79-161">--dry-run</span></span> | <span data-ttu-id="d9b79-162">Hangi veritabanı bırakılan ancak açılan yoksa gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="d9b79-163">DotNet ef veritabanı güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="d9b79-163">dotnet ef database update</span></span>

<span data-ttu-id="d9b79-164">Belirtilen geçiş veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="d9b79-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="d9b79-165">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="d9b79-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="d9b79-166">\<GEÇİŞ &GT;</span><span class="sxs-lookup"><span data-stu-id="d9b79-166">\<MIGRATION></span></span> | <span data-ttu-id="d9b79-167">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="d9b79-167">The target migration.</span></span> <span data-ttu-id="d9b79-168">0 ise, tüm geçişler döndürülecek.</span><span class="sxs-lookup"><span data-stu-id="d9b79-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="d9b79-169">Son varsayılan olarak geçiş.</span><span class="sxs-lookup"><span data-stu-id="d9b79-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="d9b79-170">DotNet ef dbcontext bilgileri</span><span class="sxs-lookup"><span data-stu-id="d9b79-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="d9b79-171">DbContext tür bilgilerini alır.</span><span class="sxs-lookup"><span data-stu-id="d9b79-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="d9b79-172">DotNet ef dbcontext listesi</span><span class="sxs-lookup"><span data-stu-id="d9b79-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="d9b79-173">Kullanılabilir DbContext türlerini listeler.</span><span class="sxs-lookup"><span data-stu-id="d9b79-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="d9b79-174">DotNet ef dbcontext iskele</span><span class="sxs-lookup"><span data-stu-id="d9b79-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="d9b79-175">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="d9b79-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="d9b79-176">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="d9b79-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="d9b79-177">\<BAĞLANTI &GT;</span><span class="sxs-lookup"><span data-stu-id="d9b79-177">\<CONNECTION></span></span> | <span data-ttu-id="d9b79-178">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="d9b79-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="d9b79-179">\<SAĞLAYICI &GT;</span><span class="sxs-lookup"><span data-stu-id="d9b79-179">\<PROVIDER></span></span>   | <span data-ttu-id="d9b79-180">Kullanılacak sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="d9b79-180">The provider to use.</span></span> <span data-ttu-id="d9b79-181">(örneğin, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="d9b79-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="d9b79-182">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="d9b79-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d9b79-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="d9b79-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="d9b79-184">--Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d9b79-184">--data-annotations</span></span>                      | <span data-ttu-id="d9b79-185">Öznitelikler, model (mümkünse) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="d9b79-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="d9b79-186">Atlanırsa, yalnızca fluent API'si kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9b79-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="d9b79-187">-c</span><span class="sxs-lookup"><span data-stu-id="d9b79-187">-c</span></span>              | <span data-ttu-id="d9b79-188">--bağlam \<adı ></span><span class="sxs-lookup"><span data-stu-id="d9b79-188">--context \<NAME></span></span>                       | <span data-ttu-id="d9b79-189">DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="d9b79-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="d9b79-190">--bağlam dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="d9b79-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="d9b79-191">DbContext dosya yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="d9b79-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="d9b79-192">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="d9b79-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="d9b79-193">-f</span><span class="sxs-lookup"><span data-stu-id="d9b79-193">-f</span></span>              | <span data-ttu-id="d9b79-194">--force</span><span class="sxs-lookup"><span data-stu-id="d9b79-194">--force</span></span>                                 | <span data-ttu-id="d9b79-195">Varolan dosyaların üzerine yaz.</span><span class="sxs-lookup"><span data-stu-id="d9b79-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="d9b79-196">-o</span><span class="sxs-lookup"><span data-stu-id="d9b79-196">-o</span></span>              | <span data-ttu-id="d9b79-197">--çıktı dizini \<yolu ></span><span class="sxs-lookup"><span data-stu-id="d9b79-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="d9b79-198">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="d9b79-198">The directory to put files in.</span></span> <span data-ttu-id="d9b79-199">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="d9b79-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="d9b79-200"><nobr>--şema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="d9b79-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="d9b79-201">Şemaları için varlık türleri oluşturmak için tablo.</span><span class="sxs-lookup"><span data-stu-id="d9b79-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="d9b79-202">-t</span><span class="sxs-lookup"><span data-stu-id="d9b79-202">-t</span></span>              | <span data-ttu-id="d9b79-203">--Tablo \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="d9b79-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="d9b79-204">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="d9b79-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="d9b79-205">--veritabanı adları kullan</span><span class="sxs-lookup"><span data-stu-id="d9b79-205">--use-database-names</span></span>                    | <span data-ttu-id="d9b79-206">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="d9b79-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="d9b79-207">DotNet ef geçişleri Ekle</span><span class="sxs-lookup"><span data-stu-id="d9b79-207">dotnet ef migrations add</span></span>

<span data-ttu-id="d9b79-208">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="d9b79-208">Adds a new migration.</span></span>

<span data-ttu-id="d9b79-209">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="d9b79-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="d9b79-210">\<ADI &GT;</span><span class="sxs-lookup"><span data-stu-id="d9b79-210">\<NAME></span></span> | <span data-ttu-id="d9b79-211">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="d9b79-211">The name of the migration.</span></span> |

<span data-ttu-id="d9b79-212">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="d9b79-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d9b79-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="d9b79-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="d9b79-214"><nobr>--çıktı dizini \<yolu ></nobr></span><span class="sxs-lookup"><span data-stu-id="d9b79-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="d9b79-215">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="d9b79-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="d9b79-216">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="d9b79-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="d9b79-217">Varsayılan olarak "Geçişler".</span><span class="sxs-lookup"><span data-stu-id="d9b79-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="d9b79-218">DotNet ef geçişleri listesi</span><span class="sxs-lookup"><span data-stu-id="d9b79-218">dotnet ef migrations list</span></span>

<span data-ttu-id="d9b79-219">Kullanılabilir geçişleri listeler.</span><span class="sxs-lookup"><span data-stu-id="d9b79-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="d9b79-220">DotNet ef geçişleri Kaldır</span><span class="sxs-lookup"><span data-stu-id="d9b79-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="d9b79-221">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="d9b79-221">Removes the last migration.</span></span>

<span data-ttu-id="d9b79-222">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="d9b79-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="d9b79-223">-f</span><span class="sxs-lookup"><span data-stu-id="d9b79-223">-f</span></span> | <span data-ttu-id="d9b79-224">--force</span><span class="sxs-lookup"><span data-stu-id="d9b79-224">--force</span></span> | <span data-ttu-id="d9b79-225">Veritabanına uygulanmışsa, geçişi geri alın.</span><span class="sxs-lookup"><span data-stu-id="d9b79-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="d9b79-226">DotNet ef geçişleri betiği</span><span class="sxs-lookup"><span data-stu-id="d9b79-226">dotnet ef migrations script</span></span>

<span data-ttu-id="d9b79-227">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d9b79-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="d9b79-228">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="d9b79-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="d9b79-229">\<GELEN &GT;</span><span class="sxs-lookup"><span data-stu-id="d9b79-229">\<FROM></span></span> | <span data-ttu-id="d9b79-230">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="d9b79-230">The starting migration.</span></span> <span data-ttu-id="d9b79-231">Varsayılan olarak 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="d9b79-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="d9b79-232">\<İÇİN &GT;</span><span class="sxs-lookup"><span data-stu-id="d9b79-232">\<TO></span></span>   | <span data-ttu-id="d9b79-233">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="d9b79-233">The ending migration.</span></span> <span data-ttu-id="d9b79-234">Son varsayılan olarak geçiş.</span><span class="sxs-lookup"><span data-stu-id="d9b79-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="d9b79-235">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="d9b79-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="d9b79-236">-o</span><span class="sxs-lookup"><span data-stu-id="d9b79-236">-o</span></span> | <span data-ttu-id="d9b79-237">--Çıktı \<dosyası ></span><span class="sxs-lookup"><span data-stu-id="d9b79-237">--output \<FILE></span></span> | <span data-ttu-id="d9b79-238">Yazma sonucu dosyası.</span><span class="sxs-lookup"><span data-stu-id="d9b79-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="d9b79-239">-i</span><span class="sxs-lookup"><span data-stu-id="d9b79-239">-i</span></span> | <span data-ttu-id="d9b79-240">ıdempotent--</span><span class="sxs-lookup"><span data-stu-id="d9b79-240">--idempotent</span></span>     | <span data-ttu-id="d9b79-241">Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d9b79-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
