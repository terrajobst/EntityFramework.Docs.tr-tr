---
title: ".NET core CLI - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="e1570-102">EF çekirdek .NET komut satırı araçları</span><span class="sxs-lookup"><span data-stu-id="e1570-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="e1570-103">Platformlar arası uzantısı Entity Framework Çekirdek .NET komut satırı araçları olan **dotnet** parçasıdır komutu, [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="e1570-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="e1570-104">Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.</span><span class="sxs-lookup"><span data-stu-id="e1570-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="e1570-105">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="e1570-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="e1570-106">EF çekirdek .NET komut satırı araçları şu adımları kullanarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="e1570-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="e1570-107">Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle</span><span class="sxs-lookup"><span data-stu-id="e1570-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="e1570-108">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e1570-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="e1570-109">Elde edilen proje aşağıdakine benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="e1570-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="e1570-110">Bir paket başvurusuyla `PrivateAssets="All"` değil sunulan genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu proje başvurusu projelerine anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e1570-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="e1570-111">Her şeyi doğru kaldırdıysanız, başarılı bir şekilde bir komut isteminde aşağıdaki komutu çalıştırın olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e1570-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="e1570-112">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="e1570-112">Using the tools</span></span>
---------------
<span data-ttu-id="e1570-113">Bir komut çağırma her iki proje ilgili vardır:</span><span class="sxs-lookup"><span data-stu-id="e1570-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="e1570-114">Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.</span><span class="sxs-lookup"><span data-stu-id="e1570-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="e1570-115">Hedef projeyi projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **--proje** </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="e1570-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="e1570-116">Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.</span><span class="sxs-lookup"><span data-stu-id="e1570-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="e1570-117">Ayrıca projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir **--başlangıç projesi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="e1570-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="e1570-118">Ortak seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="e1570-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="e1570-119">--json</span><span class="sxs-lookup"><span data-stu-id="e1570-119">--json</span></span>                           | <span data-ttu-id="e1570-120">JSON çıktısını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e1570-120">Show JSON output.</span></span>           |
| <span data-ttu-id="e1570-121">-c</span><span class="sxs-lookup"><span data-stu-id="e1570-121">-c</span></span> | <span data-ttu-id="e1570-122">--bağlam \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="e1570-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="e1570-123">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="e1570-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="e1570-124">-p</span><span class="sxs-lookup"><span data-stu-id="e1570-124">-p</span></span> | <span data-ttu-id="e1570-125">--Proje \<Proje ></span><span class="sxs-lookup"><span data-stu-id="e1570-125">--project \<PROJECT></span></span>             | <span data-ttu-id="e1570-126">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="e1570-126">The project to use.</span></span>         |
| <span data-ttu-id="e1570-127">-s</span><span class="sxs-lookup"><span data-stu-id="e1570-127">-s</span></span> | <span data-ttu-id="e1570-128">--başlangıç projesi \<Proje ></span><span class="sxs-lookup"><span data-stu-id="e1570-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="e1570-129">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="e1570-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="e1570-130">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="e1570-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="e1570-131">Hedef çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="e1570-131">The target framework.</span></span>       |
|    | <span data-ttu-id="e1570-132">--configuration \<yapılandırma ></span><span class="sxs-lookup"><span data-stu-id="e1570-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="e1570-133">Kullanılacak yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="e1570-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="e1570-134">--çalışma zamanı \<TANIMLAYICISI ></span><span class="sxs-lookup"><span data-stu-id="e1570-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="e1570-135">Çalışma zamanı.</span><span class="sxs-lookup"><span data-stu-id="e1570-135">The runtime to use.</span></span>         |
| <span data-ttu-id="e1570-136">-h</span><span class="sxs-lookup"><span data-stu-id="e1570-136">-h</span></span> | <span data-ttu-id="e1570-137">--Yardım</span><span class="sxs-lookup"><span data-stu-id="e1570-137">--help</span></span>                           | <span data-ttu-id="e1570-138">Yardım bilgilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e1570-138">Show help information.</span></span>      |
| <span data-ttu-id="e1570-139">-v</span><span class="sxs-lookup"><span data-stu-id="e1570-139">-v</span></span> | <span data-ttu-id="e1570-140">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="e1570-140">--verbose</span></span>                        | <span data-ttu-id="e1570-141">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="e1570-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="e1570-142">--renk yok</span><span class="sxs-lookup"><span data-stu-id="e1570-142">--no-color</span></span>                       | <span data-ttu-id="e1570-143">Çıktı renklendirme yok.</span><span class="sxs-lookup"><span data-stu-id="e1570-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="e1570-144">--önek çıktı</span><span class="sxs-lookup"><span data-stu-id="e1570-144">--prefix-output</span></span>                  | <span data-ttu-id="e1570-145">Düzeyiyle çıktı öneki.</span><span class="sxs-lookup"><span data-stu-id="e1570-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="e1570-146">ASP.NET Core ortam belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e1570-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="e1570-147">Komutları</span><span class="sxs-lookup"><span data-stu-id="e1570-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="e1570-148">DotNet ef veritabanı bırakma</span><span class="sxs-lookup"><span data-stu-id="e1570-148">dotnet ef database drop</span></span>

<span data-ttu-id="e1570-149">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="e1570-149">Drops the database.</span></span>

<span data-ttu-id="e1570-150">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="e1570-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="e1570-151">-f</span><span class="sxs-lookup"><span data-stu-id="e1570-151">-f</span></span> | <span data-ttu-id="e1570-152">--zorla</span><span class="sxs-lookup"><span data-stu-id="e1570-152">--force</span></span>   | <span data-ttu-id="e1570-153">Onaylayın yok.</span><span class="sxs-lookup"><span data-stu-id="e1570-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="e1570-154">--çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e1570-154">--dry-run</span></span> | <span data-ttu-id="e1570-155">Hangi veritabanı olduğundan bırakılamaz, ancak bırakma yok gösterir.</span><span class="sxs-lookup"><span data-stu-id="e1570-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="e1570-156">DotNet ef veritabanı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e1570-156">dotnet ef database update</span></span>

<span data-ttu-id="e1570-157">Belirtilen geçiş için veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e1570-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="e1570-158">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="e1570-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="e1570-159">\<GEÇİŞ ></span><span class="sxs-lookup"><span data-stu-id="e1570-159">\<MIGRATION></span></span> | <span data-ttu-id="e1570-160">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="e1570-160">The target migration.</span></span> <span data-ttu-id="e1570-161">0 ise, tüm geçişler geri alınacak.</span><span class="sxs-lookup"><span data-stu-id="e1570-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="e1570-162">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="e1570-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="e1570-163">DotNet ef dbcontext bilgisi</span><span class="sxs-lookup"><span data-stu-id="e1570-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="e1570-164">DbContext türü hakkındaki bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="e1570-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="e1570-165">DotNet ef dbcontext listesi</span><span class="sxs-lookup"><span data-stu-id="e1570-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="e1570-166">Kullanılabilir DbContext türleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="e1570-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="e1570-167">DotNet ef dbcontext iskele</span><span class="sxs-lookup"><span data-stu-id="e1570-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="e1570-168">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="e1570-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="e1570-169">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="e1570-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="e1570-170">\<BAĞLANTI ></span><span class="sxs-lookup"><span data-stu-id="e1570-170">\<CONNECTION></span></span> | <span data-ttu-id="e1570-171">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="e1570-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="e1570-172">\<SAĞLAYICI ></span><span class="sxs-lookup"><span data-stu-id="e1570-172">\<PROVIDER></span></span>   | <span data-ttu-id="e1570-173">Kullanılacak sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="e1570-173">The provider to use.</span></span> <span data-ttu-id="e1570-174">(Örn.</span><span class="sxs-lookup"><span data-stu-id="e1570-174">(E.g.</span></span> <span data-ttu-id="e1570-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="e1570-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="e1570-176">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="e1570-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="e1570-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="e1570-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="e1570-178">--Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="e1570-178">--data-annotations</span></span>                | <span data-ttu-id="e1570-179">Öznitelikler, model (mümkün olduğunda) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="e1570-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="e1570-180">Atlanırsa, yalnızca fluent API kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e1570-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="e1570-181">-c</span><span class="sxs-lookup"><span data-stu-id="e1570-181">-c</span></span>        |       <span data-ttu-id="e1570-182">--bağlam \<adı ></span><span class="sxs-lookup"><span data-stu-id="e1570-182">--context \<NAME></span></span>                 | <span data-ttu-id="e1570-183">DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="e1570-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="e1570-184">-f</span><span class="sxs-lookup"><span data-stu-id="e1570-184">-f</span></span>        |       <span data-ttu-id="e1570-185">--zorla</span><span class="sxs-lookup"><span data-stu-id="e1570-185">--force</span></span>                           | <span data-ttu-id="e1570-186">Var olan dosyaların üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="e1570-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="e1570-187">-o</span><span class="sxs-lookup"><span data-stu-id="e1570-187">-o</span></span>        |       <span data-ttu-id="e1570-188">--Çıkış dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="e1570-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="e1570-189">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="e1570-189">The directory to put files in.</span></span> <span data-ttu-id="e1570-190">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="e1570-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="e1570-191"><nobr>--şema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="e1570-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="e1570-192">Varlık türleri oluşturmak için tabloları şemalar.</span><span class="sxs-lookup"><span data-stu-id="e1570-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="e1570-193">-t</span><span class="sxs-lookup"><span data-stu-id="e1570-193">-t</span></span>        |       <span data-ttu-id="e1570-194">--Tablo \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="e1570-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="e1570-195">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="e1570-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="e1570-196">--adları veritabanı kullan</span><span class="sxs-lookup"><span data-stu-id="e1570-196">--use-database-names</span></span>              | <span data-ttu-id="e1570-197">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="e1570-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="e1570-198">DotNet ef geçişler ekleme</span><span class="sxs-lookup"><span data-stu-id="e1570-198">dotnet ef migrations add</span></span>

<span data-ttu-id="e1570-199">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="e1570-199">Adds a new migration.</span></span>

<span data-ttu-id="e1570-200">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="e1570-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="e1570-201">\<ADI ></span><span class="sxs-lookup"><span data-stu-id="e1570-201">\<NAME></span></span> | <span data-ttu-id="e1570-202">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="e1570-202">The name of the migration.</span></span> |

<span data-ttu-id="e1570-203">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="e1570-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="e1570-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="e1570-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="e1570-205"><nobr>--Çıkış dir \<yolu ></nobr></span><span class="sxs-lookup"><span data-stu-id="e1570-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="e1570-206">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="e1570-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="e1570-207">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="e1570-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="e1570-208">Varsayılan olarak "Geçiş".</span><span class="sxs-lookup"><span data-stu-id="e1570-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="e1570-209">DotNet ef geçişler listesi</span><span class="sxs-lookup"><span data-stu-id="e1570-209">dotnet ef migrations list</span></span>

<span data-ttu-id="e1570-210">Kullanılabilir geçişler listeler.</span><span class="sxs-lookup"><span data-stu-id="e1570-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="e1570-211">DotNet ef geçişler Kaldır</span><span class="sxs-lookup"><span data-stu-id="e1570-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="e1570-212">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="e1570-212">Removes the last migration.</span></span>

<span data-ttu-id="e1570-213">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="e1570-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="e1570-214">-f</span><span class="sxs-lookup"><span data-stu-id="e1570-214">-f</span></span> | <span data-ttu-id="e1570-215">--zorla</span><span class="sxs-lookup"><span data-stu-id="e1570-215">--force</span></span> | <span data-ttu-id="e1570-216">Geçiş veritabanına uygulanıp uygulanmadığını denetleyin yok.</span><span class="sxs-lookup"><span data-stu-id="e1570-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="e1570-217">DotNet ef geçişler komut dosyası</span><span class="sxs-lookup"><span data-stu-id="e1570-217">dotnet ef migrations script</span></span>

<span data-ttu-id="e1570-218">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e1570-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="e1570-219">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="e1570-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="e1570-220">\<GELEN ></span><span class="sxs-lookup"><span data-stu-id="e1570-220">\<FROM></span></span> | <span data-ttu-id="e1570-221">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="e1570-221">The starting migration.</span></span> <span data-ttu-id="e1570-222">Varsayılan ayar: 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="e1570-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="e1570-223">\<İÇİN ></span><span class="sxs-lookup"><span data-stu-id="e1570-223">\<TO></span></span>   | <span data-ttu-id="e1570-224">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="e1570-224">The ending migration.</span></span> <span data-ttu-id="e1570-225">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="e1570-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="e1570-226">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="e1570-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="e1570-227">-o</span><span class="sxs-lookup"><span data-stu-id="e1570-227">-o</span></span> | <span data-ttu-id="e1570-228">--Çıkış \<dosyası ></span><span class="sxs-lookup"><span data-stu-id="e1570-228">--output \<FILE></span></span> | <span data-ttu-id="e1570-229">Sonucu yazmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="e1570-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="e1570-230">-i</span><span class="sxs-lookup"><span data-stu-id="e1570-230">-i</span></span> | <span data-ttu-id="e1570-231">--ıdempotent</span><span class="sxs-lookup"><span data-stu-id="e1570-231">--idempotent</span></span>     | <span data-ttu-id="e1570-232">Bir veritabanında hiçbir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e1570-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
