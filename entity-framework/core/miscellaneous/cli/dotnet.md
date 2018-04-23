---
title: .NET core CLI - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="cfbd6-102">EF çekirdek .NET komut satırı araçları</span><span class="sxs-lookup"><span data-stu-id="cfbd6-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="cfbd6-103">Platformlar arası uzantısı Entity Framework Çekirdek .NET komut satırı araçları olan **dotnet** parçasıdır komutu, [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="cfbd6-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="cfbd6-104">Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="cfbd6-105">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="cfbd6-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="cfbd6-106">EF çekirdek .NET komut satırı araçları şu adımları kullanarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="cfbd6-107">Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle</span><span class="sxs-lookup"><span data-stu-id="cfbd6-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="cfbd6-108">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="cfbd6-109">Elde edilen proje aşağıdakine benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="cfbd6-110">Bir paket başvurusuyla `PrivateAssets="All"` değil sunulan genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu proje başvurusu projelerine anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="cfbd6-111">Her şeyi doğru kaldırdıysanız, başarılı bir şekilde bir komut isteminde aşağıdaki komutu çalıştırın olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="cfbd6-112">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="cfbd6-112">Using the tools</span></span>
---------------
<span data-ttu-id="cfbd6-113">Bir komut çağırma her iki proje ilgili vardır:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="cfbd6-114">Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="cfbd6-115">Hedef projeyi projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **--proje** </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="cfbd6-116">Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="cfbd6-117">Ayrıca projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir **--başlangıç projesi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="cfbd6-118">Örneğin, web uygulamanızın EF çekirdek farklı projede yüklü olan veritabanını güncelleştirme şuna benzer: `dotnet ef database update --project {project-path}` (dizininizden web uygulaması)</span><span class="sxs-lookup"><span data-stu-id="cfbd6-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="cfbd6-119">Ortak seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="cfbd6-120">--json</span><span class="sxs-lookup"><span data-stu-id="cfbd6-120">--json</span></span>                           | <span data-ttu-id="cfbd6-121">JSON çıktısını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-121">Show JSON output.</span></span>           |
| <span data-ttu-id="cfbd6-122">-c</span><span class="sxs-lookup"><span data-stu-id="cfbd6-122">-c</span></span> | <span data-ttu-id="cfbd6-123">--bağlam \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="cfbd6-124">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="cfbd6-125">-p</span><span class="sxs-lookup"><span data-stu-id="cfbd6-125">-p</span></span> | <span data-ttu-id="cfbd6-126">--Proje \<Proje ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-126">--project \<PROJECT></span></span>             | <span data-ttu-id="cfbd6-127">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-127">The project to use.</span></span>         |
| <span data-ttu-id="cfbd6-128">-s</span><span class="sxs-lookup"><span data-stu-id="cfbd6-128">-s</span></span> | <span data-ttu-id="cfbd6-129">--başlangıç projesi \<Proje ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="cfbd6-130">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="cfbd6-131">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="cfbd6-132">Hedef çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-132">The target framework.</span></span>       |
|    | <span data-ttu-id="cfbd6-133">--configuration \<yapılandırma ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="cfbd6-134">Kullanılacak yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="cfbd6-135">--çalışma zamanı \<TANIMLAYICISI ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="cfbd6-136">Çalışma zamanı.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-136">The runtime to use.</span></span>         |
| <span data-ttu-id="cfbd6-137">-h</span><span class="sxs-lookup"><span data-stu-id="cfbd6-137">-h</span></span> | <span data-ttu-id="cfbd6-138">--Yardım</span><span class="sxs-lookup"><span data-stu-id="cfbd6-138">--help</span></span>                           | <span data-ttu-id="cfbd6-139">Yardım bilgilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-139">Show help information.</span></span>      |
| <span data-ttu-id="cfbd6-140">-v</span><span class="sxs-lookup"><span data-stu-id="cfbd6-140">-v</span></span> | <span data-ttu-id="cfbd6-141">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="cfbd6-141">--verbose</span></span>                        | <span data-ttu-id="cfbd6-142">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="cfbd6-143">--renk yok</span><span class="sxs-lookup"><span data-stu-id="cfbd6-143">--no-color</span></span>                       | <span data-ttu-id="cfbd6-144">Çıktı renklendirme yok.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="cfbd6-145">--önek çıktı</span><span class="sxs-lookup"><span data-stu-id="cfbd6-145">--prefix-output</span></span>                  | <span data-ttu-id="cfbd6-146">Düzeyiyle çıktı öneki.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="cfbd6-147">ASP.NET Core ortam belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="cfbd6-148">Komutlar</span><span class="sxs-lookup"><span data-stu-id="cfbd6-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="cfbd6-149">DotNet ef veritabanı bırakma</span><span class="sxs-lookup"><span data-stu-id="cfbd6-149">dotnet ef database drop</span></span>

<span data-ttu-id="cfbd6-150">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-150">Drops the database.</span></span>

<span data-ttu-id="cfbd6-151">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="cfbd6-152">-f</span><span class="sxs-lookup"><span data-stu-id="cfbd6-152">-f</span></span> | <span data-ttu-id="cfbd6-153">--zorla</span><span class="sxs-lookup"><span data-stu-id="cfbd6-153">--force</span></span>   | <span data-ttu-id="cfbd6-154">Onaylayın yok.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="cfbd6-155">--çalıştırma</span><span class="sxs-lookup"><span data-stu-id="cfbd6-155">--dry-run</span></span> | <span data-ttu-id="cfbd6-156">Hangi veritabanı olduğundan bırakılamaz, ancak bırakma yok gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="cfbd6-157">DotNet ef veritabanı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="cfbd6-157">dotnet ef database update</span></span>

<span data-ttu-id="cfbd6-158">Belirtilen geçiş için veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="cfbd6-159">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="cfbd6-160">\<GEÇİŞ &GT;</span><span class="sxs-lookup"><span data-stu-id="cfbd6-160">\<MIGRATION></span></span> | <span data-ttu-id="cfbd6-161">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-161">The target migration.</span></span> <span data-ttu-id="cfbd6-162">0 ise, tüm geçişler geri alınacak.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="cfbd6-163">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="cfbd6-164">DotNet ef dbcontext bilgisi</span><span class="sxs-lookup"><span data-stu-id="cfbd6-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="cfbd6-165">DbContext türü hakkındaki bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="cfbd6-166">DotNet ef dbcontext listesi</span><span class="sxs-lookup"><span data-stu-id="cfbd6-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="cfbd6-167">Kullanılabilir DbContext türleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="cfbd6-168">DotNet ef dbcontext iskele</span><span class="sxs-lookup"><span data-stu-id="cfbd6-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="cfbd6-169">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="cfbd6-170">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="cfbd6-171">\<BAĞLANTI &GT;</span><span class="sxs-lookup"><span data-stu-id="cfbd6-171">\<CONNECTION></span></span> | <span data-ttu-id="cfbd6-172">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="cfbd6-173">\<SAĞLAYICI &GT;</span><span class="sxs-lookup"><span data-stu-id="cfbd6-173">\<PROVIDER></span></span>   | <span data-ttu-id="cfbd6-174">Kullanılacak sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-174">The provider to use.</span></span> <span data-ttu-id="cfbd6-175">(Örn.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-175">(E.g.</span></span> <span data-ttu-id="cfbd6-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="cfbd6-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="cfbd6-177">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cfbd6-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="cfbd6-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="cfbd6-179">--Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="cfbd6-179">--data-annotations</span></span>                      | <span data-ttu-id="cfbd6-180">Öznitelikler, model (mümkün olduğunda) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="cfbd6-181">Atlanırsa, yalnızca fluent API kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="cfbd6-182">-c</span><span class="sxs-lookup"><span data-stu-id="cfbd6-182">-c</span></span>              | <span data-ttu-id="cfbd6-183">--bağlam \<adı ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-183">--context \<NAME></span></span>                       | <span data-ttu-id="cfbd6-184">DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="cfbd6-185">--bağlam dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="cfbd6-186">DbContext dosyasını yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="cfbd6-187">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="cfbd6-188">-f</span><span class="sxs-lookup"><span data-stu-id="cfbd6-188">-f</span></span>              | <span data-ttu-id="cfbd6-189">--zorla</span><span class="sxs-lookup"><span data-stu-id="cfbd6-189">--force</span></span>                                 | <span data-ttu-id="cfbd6-190">Var olan dosyaların üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="cfbd6-191">-o</span><span class="sxs-lookup"><span data-stu-id="cfbd6-191">-o</span></span>              | <span data-ttu-id="cfbd6-192">--Çıkış dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="cfbd6-193">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-193">The directory to put files in.</span></span> <span data-ttu-id="cfbd6-194">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="cfbd6-195"><nobr>--şema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="cfbd6-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="cfbd6-196">Varlık türleri oluşturmak için tabloları şemalar.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="cfbd6-197">-t</span><span class="sxs-lookup"><span data-stu-id="cfbd6-197">-t</span></span>              | <span data-ttu-id="cfbd6-198">--Tablo \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="cfbd6-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="cfbd6-199">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="cfbd6-200">--adları veritabanı kullan</span><span class="sxs-lookup"><span data-stu-id="cfbd6-200">--use-database-names</span></span>                    | <span data-ttu-id="cfbd6-201">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="cfbd6-202">DotNet ef geçişler ekleme</span><span class="sxs-lookup"><span data-stu-id="cfbd6-202">dotnet ef migrations add</span></span>

<span data-ttu-id="cfbd6-203">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-203">Adds a new migration.</span></span>

<span data-ttu-id="cfbd6-204">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="cfbd6-205">\<ADI &GT;</span><span class="sxs-lookup"><span data-stu-id="cfbd6-205">\<NAME></span></span> | <span data-ttu-id="cfbd6-206">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-206">The name of the migration.</span></span> |

<span data-ttu-id="cfbd6-207">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cfbd6-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="cfbd6-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="cfbd6-209"><nobr>--Çıkış dir \<yolu ></nobr></span><span class="sxs-lookup"><span data-stu-id="cfbd6-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="cfbd6-210">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="cfbd6-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="cfbd6-211">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="cfbd6-212">Varsayılan olarak "Geçiş".</span><span class="sxs-lookup"><span data-stu-id="cfbd6-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="cfbd6-213">DotNet ef geçişler listesi</span><span class="sxs-lookup"><span data-stu-id="cfbd6-213">dotnet ef migrations list</span></span>

<span data-ttu-id="cfbd6-214">Kullanılabilir geçişler listeler.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="cfbd6-215">DotNet ef geçişler Kaldır</span><span class="sxs-lookup"><span data-stu-id="cfbd6-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="cfbd6-216">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-216">Removes the last migration.</span></span>

<span data-ttu-id="cfbd6-217">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="cfbd6-218">-f</span><span class="sxs-lookup"><span data-stu-id="cfbd6-218">-f</span></span> | <span data-ttu-id="cfbd6-219">--zorla</span><span class="sxs-lookup"><span data-stu-id="cfbd6-219">--force</span></span> | <span data-ttu-id="cfbd6-220">Veritabanına uyguladıysanız geçişi geri alın.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="cfbd6-221">DotNet ef geçişler komut dosyası</span><span class="sxs-lookup"><span data-stu-id="cfbd6-221">dotnet ef migrations script</span></span>

<span data-ttu-id="cfbd6-222">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="cfbd6-223">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="cfbd6-224">\<GELEN &GT;</span><span class="sxs-lookup"><span data-stu-id="cfbd6-224">\<FROM></span></span> | <span data-ttu-id="cfbd6-225">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-225">The starting migration.</span></span> <span data-ttu-id="cfbd6-226">Varsayılan ayar: 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="cfbd6-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="cfbd6-227">\<İÇİN &GT;</span><span class="sxs-lookup"><span data-stu-id="cfbd6-227">\<TO></span></span>   | <span data-ttu-id="cfbd6-228">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-228">The ending migration.</span></span> <span data-ttu-id="cfbd6-229">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="cfbd6-230">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="cfbd6-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="cfbd6-231">-o</span><span class="sxs-lookup"><span data-stu-id="cfbd6-231">-o</span></span> | <span data-ttu-id="cfbd6-232">--Çıkış \<dosyası ></span><span class="sxs-lookup"><span data-stu-id="cfbd6-232">--output \<FILE></span></span> | <span data-ttu-id="cfbd6-233">Sonucu yazmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="cfbd6-234">-i</span><span class="sxs-lookup"><span data-stu-id="cfbd6-234">-i</span></span> | <span data-ttu-id="cfbd6-235">--ıdempotent</span><span class="sxs-lookup"><span data-stu-id="cfbd6-235">--idempotent</span></span>     | <span data-ttu-id="cfbd6-236">Bir veritabanında hiçbir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfbd6-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
