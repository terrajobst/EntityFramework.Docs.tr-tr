---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489615"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="9b0f8-102">EF Core .NET komut satırı araçları</span><span class="sxs-lookup"><span data-stu-id="9b0f8-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="9b0f8-103">Entity Framework Core ve .NET komut satırı araçları, platformlar arası bir uzantısı olan **dotnet** parçası olan komut, [.NET Core SDK'sı][2].</span><span class="sxs-lookup"><span data-stu-id="9b0f8-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="9b0f8-104">Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="9b0f8-105">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="9b0f8-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="9b0f8-106">.NET Core SDK'sı sürüm 2.1.300 ve yeni içerir **dotnet ef** EF Core 2.0 ve sonraki sürümler ile uyumlu olan komutları.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="9b0f8-107">Bu nedenle .NET Core SDK'sını ve EF Core çalışma zamanı son sürümlerini kullanıyorsanız, herhangi bir yükleme gereklidir ve bu bölümün geri kalanında yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="9b0f8-108">Öte yandan, **dotnet ef** aracı .NET Core SDK'sı sürüm 2.1.300 içerdiği ve yeni EF Core 1.0 ve 1.1 sürümüyle uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="9b0f8-109">.NET Core SDK'sı 2.1.300 olan bir bilgisayarda bu sürümlerde EF Core kullanan bir proje ile çalışabilir veya üzerinin yüklü önce sürüm 2.1.200 da yüklemeniz gerekir ya da SDK'ın eski ve değiştirerek, eski sürümünün kullanmak için uygulamayı yapılandırma,  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) dosya.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="9b0f8-110">Normalde bu dosya çözüm dizini (bir proje üzerinde) dahildir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="9b0f8-111">Ardından installlation talimatıyla geçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="9b0f8-112">.NET Core SDK'sının önceki sürümleri için aşağıdaki adımları kullanarak EF Core .NET komut satırı araçları yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="9b0f8-113">Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle</span><span class="sxs-lookup"><span data-stu-id="9b0f8-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="9b0f8-114">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="9b0f8-115">Elde edilen proje şunun gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="9b0f8-116">Bir paket başvurusu ile `PrivateAssets="All"` değilse ifşa genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu projeye başvuran projeler için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="9b0f8-117">Her şeyi doğru kaldırdıysanız, başarıyla bir komut isteminde aşağıdaki komutu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="9b0f8-118">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="9b0f8-118">Using the tools</span></span>
---------------
<span data-ttu-id="9b0f8-119">Bir komut çağırma her iki proje kullanılan vardır:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="9b0f8-120">Hedef dosyalar eklenir (veya bazı durumlarda kaldırıldı) projesidir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="9b0f8-121">Hedef proje geçerli dizinde proje için varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **`--project`** </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="9b0f8-122">Başlangıç projesi, proje kodunuzun yürütülürken araçları tarafından Öykünülen bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="9b0f8-123">Ayrıca geçerli dizinde proje için varsayılan olarak, ancak kullanılarak değiştirilebilir **`--startup-project`** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="9b0f8-124">Örneğin, web uygulamanızın yüklü farklı bir projede EF Core sahip veritabanını güncelleme şuna benzer: `dotnet ef database update --project {project-path}` (dizininizden web uygulaması)</span><span class="sxs-lookup"><span data-stu-id="9b0f8-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="9b0f8-125">Genel Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="9b0f8-126">JSON çıktıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-126">Show JSON output.</span></span>           |
| <span data-ttu-id="9b0f8-127">-c</span><span class="sxs-lookup"><span data-stu-id="9b0f8-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="9b0f8-128">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="9b0f8-129">-p</span><span class="sxs-lookup"><span data-stu-id="9b0f8-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="9b0f8-130">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-130">The project to use.</span></span>         |
| <span data-ttu-id="9b0f8-131">-s</span><span class="sxs-lookup"><span data-stu-id="9b0f8-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="9b0f8-132">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="9b0f8-133">Hedef Framework'ü.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="9b0f8-134">Kullanılacak yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="9b0f8-135">Kullanmak için çalışma zamanı.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-135">The runtime to use.</span></span>         |
| <span data-ttu-id="9b0f8-136">-h</span><span class="sxs-lookup"><span data-stu-id="9b0f8-136">-h</span></span> | `--help`                           | <span data-ttu-id="9b0f8-137">Yardım bilgilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-137">Show help information.</span></span>      |
| <span data-ttu-id="9b0f8-138">-v</span><span class="sxs-lookup"><span data-stu-id="9b0f8-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="9b0f8-139">Ayrıntılı çıktıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="9b0f8-140">Çıkış renklendirmeye yok.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="9b0f8-141">Düzeyiyle çıkış öneki.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="9b0f8-142">ASP.NET Core ortamını belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="9b0f8-143">Komutlar</span><span class="sxs-lookup"><span data-stu-id="9b0f8-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="9b0f8-144">DotNet ef veritabanını bırak</span><span class="sxs-lookup"><span data-stu-id="9b0f8-144">dotnet ef database drop</span></span>

<span data-ttu-id="9b0f8-145">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-145">Drops the database.</span></span>

<span data-ttu-id="9b0f8-146">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="9b0f8-147">-f</span><span class="sxs-lookup"><span data-stu-id="9b0f8-147">-f</span></span> | `--force`   | <span data-ttu-id="9b0f8-148">Onay isteme.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="9b0f8-149">Hangi veritabanı bırakılan ancak açılan yoksa gösterir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="9b0f8-150">DotNet ef veritabanı güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="9b0f8-150">dotnet ef database update</span></span>

<span data-ttu-id="9b0f8-151">Belirtilen geçiş veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="9b0f8-152">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="9b0f8-153">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-153">The target migration.</span></span> <span data-ttu-id="9b0f8-154">0 ise, tüm geçişler döndürülecek.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="9b0f8-155">Son varsayılan olarak geçiş.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="9b0f8-156">DotNet ef dbcontext bilgileri</span><span class="sxs-lookup"><span data-stu-id="9b0f8-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="9b0f8-157">DbContext tür bilgilerini alır.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="9b0f8-158">DotNet ef dbcontext listesi</span><span class="sxs-lookup"><span data-stu-id="9b0f8-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="9b0f8-159">Kullanılabilir DbContext türlerini listeler.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="9b0f8-160">DotNet ef dbcontext iskele</span><span class="sxs-lookup"><span data-stu-id="9b0f8-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="9b0f8-161">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="9b0f8-162">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="9b0f8-163">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="9b0f8-164">Kullanılacak sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-164">The provider to use.</span></span> <span data-ttu-id="9b0f8-165">(örneğin, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="9b0f8-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="9b0f8-166">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9b0f8-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="9b0f8-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="9b0f8-168">Öznitelikler, model (mümkünse) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="9b0f8-169">Atlanırsa, yalnızca fluent API'si kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="9b0f8-170">-c</span><span class="sxs-lookup"><span data-stu-id="9b0f8-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="9b0f8-171">DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="9b0f8-172">DbContext dosya yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="9b0f8-173">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="9b0f8-174">-f</span><span class="sxs-lookup"><span data-stu-id="9b0f8-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="9b0f8-175">Varolan dosyaların üzerine yaz.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="9b0f8-176">-o</span><span class="sxs-lookup"><span data-stu-id="9b0f8-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="9b0f8-177">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-177">The directory to put files in.</span></span> <span data-ttu-id="9b0f8-178">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="9b0f8-179">Şemaları için varlık türleri oluşturmak için tablo.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="9b0f8-180">-t</span><span class="sxs-lookup"><span data-stu-id="9b0f8-180">-t</span></span>              | <span data-ttu-id="9b0f8-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="9b0f8-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="9b0f8-182">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="9b0f8-183">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="9b0f8-184">DotNet ef geçişleri Ekle</span><span class="sxs-lookup"><span data-stu-id="9b0f8-184">dotnet ef migrations add</span></span>

<span data-ttu-id="9b0f8-185">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-185">Adds a new migration.</span></span>

<span data-ttu-id="9b0f8-186">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="9b0f8-187">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-187">The name of the migration.</span></span> |

<span data-ttu-id="9b0f8-188">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9b0f8-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="9b0f8-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="9b0f8-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="9b0f8-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="9b0f8-191">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="9b0f8-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="9b0f8-192">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="9b0f8-193">Varsayılan olarak "Geçişler".</span><span class="sxs-lookup"><span data-stu-id="9b0f8-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="9b0f8-194">DotNet ef geçişleri listesi</span><span class="sxs-lookup"><span data-stu-id="9b0f8-194">dotnet ef migrations list</span></span>

<span data-ttu-id="9b0f8-195">Kullanılabilir geçişleri listeler.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="9b0f8-196">DotNet ef geçişleri Kaldır</span><span class="sxs-lookup"><span data-stu-id="9b0f8-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="9b0f8-197">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-197">Removes the last migration.</span></span>

<span data-ttu-id="9b0f8-198">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="9b0f8-199">-f</span><span class="sxs-lookup"><span data-stu-id="9b0f8-199">-f</span></span> | `--force` | <span data-ttu-id="9b0f8-200">Veritabanına uygulanmışsa, geçişi geri alın.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="9b0f8-201">DotNet ef geçişleri betiği</span><span class="sxs-lookup"><span data-stu-id="9b0f8-201">dotnet ef migrations script</span></span>

<span data-ttu-id="9b0f8-202">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="9b0f8-203">Bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="9b0f8-204">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-204">The starting migration.</span></span> <span data-ttu-id="9b0f8-205">Varsayılan olarak 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="9b0f8-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="9b0f8-206">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-206">The ending migration.</span></span> <span data-ttu-id="9b0f8-207">Son varsayılan olarak geçiş.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="9b0f8-208">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9b0f8-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="9b0f8-209">-o</span><span class="sxs-lookup"><span data-stu-id="9b0f8-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="9b0f8-210">Yazma sonucu dosyası.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="9b0f8-211">-i</span><span class="sxs-lookup"><span data-stu-id="9b0f8-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="9b0f8-212">Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9b0f8-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
