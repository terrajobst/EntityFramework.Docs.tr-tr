---
title: .NET core CLI - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="9f84d-102">EF çekirdek .NET komut satırı araçları</span><span class="sxs-lookup"><span data-stu-id="9f84d-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="9f84d-103">Platformlar arası uzantısı Entity Framework Çekirdek .NET komut satırı araçları olan **dotnet** parçasıdır komutu, [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="9f84d-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="9f84d-104">Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.</span><span class="sxs-lookup"><span data-stu-id="9f84d-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="9f84d-105">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="9f84d-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="9f84d-106">.NET Core SDK'sı sürüm 2.1.300 ve daha yeni içerir **dotnet ef** EF çekirdek 2.0 ve sonraki sürümleri ile uyumlu olan komutları.</span><span class="sxs-lookup"><span data-stu-id="9f84d-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="9f84d-107">Bu nedenle .NET Core SDK'sı ve EF çekirdeği çalışma zamanı en güncel sürümlerini kullanıyorsanız, herhangi bir yükleme gereklidir ve bu bölümde rest yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9f84d-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="9f84d-108">Diğer taraftan, **dotnet ef** aracı .NET Core SDK sürüm 2.1.300 içerdiği ve daha yeni sürümü 1.0 ve 1.1 EF çekirdek sürümü ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="9f84d-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="9f84d-109">.NET Core SDK 2.1.300 sahip bir bilgisayarda bu sürümlerde EF çekirdek kullanan bir proje ile çalışabilirsiniz ya da daha yeni yüklü önce sürüm 2.1.200 de yüklemeniz gerekir ya da SDK'ın eski ve uygulamayı değiştirerek, eski sürümünün kullanacak şekilde yapılandırın,  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) dosya.</span><span class="sxs-lookup"><span data-stu-id="9f84d-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="9f84d-110">Bu dosya, normalde çözüm dizini (bir proje üzerinde) bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="9f84d-111">Ardından installlation talimatıyla devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9f84d-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="9f84d-112">.NET Core SDK önceki sürümleri için aşağıdaki adımları kullanarak EF çekirdek .NET komut satırı araçları yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9f84d-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="9f84d-113">Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle</span><span class="sxs-lookup"><span data-stu-id="9f84d-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="9f84d-114">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9f84d-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="9f84d-115">Elde edilen proje aşağıdakine benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="9f84d-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="9f84d-116">Bir paket başvurusuyla `PrivateAssets="All"` değil sunulan genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu proje başvurusu projelerine anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="9f84d-117">Her şeyi doğru kaldırdıysanız, başarılı bir şekilde bir komut isteminde aşağıdaki komutu çalıştırın olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="9f84d-118">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="9f84d-118">Using the tools</span></span>
---------------
<span data-ttu-id="9f84d-119">Bir komut çağırma her iki proje ilgili vardır:</span><span class="sxs-lookup"><span data-stu-id="9f84d-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="9f84d-120">Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="9f84d-121">Hedef projeyi projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **--proje** </nobr> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9f84d-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="9f84d-122">Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="9f84d-123">Ayrıca projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir **--başlangıç projesi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9f84d-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="9f84d-124">Örneğin, web uygulamanızın EF çekirdek farklı projede yüklü olan veritabanını güncelleştirme şuna benzer: `dotnet ef database update --project {project-path}` (dizininizden web uygulaması)</span><span class="sxs-lookup"><span data-stu-id="9f84d-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="9f84d-125">Ortak seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="9f84d-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="9f84d-126">--json</span><span class="sxs-lookup"><span data-stu-id="9f84d-126">--json</span></span>                           | <span data-ttu-id="9f84d-127">JSON çıktısını gösterir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-127">Show JSON output.</span></span>           |
| <span data-ttu-id="9f84d-128">-c</span><span class="sxs-lookup"><span data-stu-id="9f84d-128">-c</span></span> | <span data-ttu-id="9f84d-129">--bağlam \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="9f84d-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="9f84d-130">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="9f84d-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="9f84d-131">-p</span><span class="sxs-lookup"><span data-stu-id="9f84d-131">-p</span></span> | <span data-ttu-id="9f84d-132">--Proje \<Proje ></span><span class="sxs-lookup"><span data-stu-id="9f84d-132">--project \<PROJECT></span></span>             | <span data-ttu-id="9f84d-133">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="9f84d-133">The project to use.</span></span>         |
| <span data-ttu-id="9f84d-134">-s</span><span class="sxs-lookup"><span data-stu-id="9f84d-134">-s</span></span> | <span data-ttu-id="9f84d-135">--başlangıç projesi \<Proje ></span><span class="sxs-lookup"><span data-stu-id="9f84d-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="9f84d-136">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="9f84d-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="9f84d-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="9f84d-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="9f84d-138">Hedef çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="9f84d-138">The target framework.</span></span>       |
|    | <span data-ttu-id="9f84d-139">--configuration \<yapılandırma ></span><span class="sxs-lookup"><span data-stu-id="9f84d-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="9f84d-140">Kullanılacak yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="9f84d-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="9f84d-141">--çalışma zamanı \<TANIMLAYICISI ></span><span class="sxs-lookup"><span data-stu-id="9f84d-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="9f84d-142">Çalışma zamanı.</span><span class="sxs-lookup"><span data-stu-id="9f84d-142">The runtime to use.</span></span>         |
| <span data-ttu-id="9f84d-143">-h</span><span class="sxs-lookup"><span data-stu-id="9f84d-143">-h</span></span> | <span data-ttu-id="9f84d-144">--Yardım</span><span class="sxs-lookup"><span data-stu-id="9f84d-144">--help</span></span>                           | <span data-ttu-id="9f84d-145">Yardım bilgilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-145">Show help information.</span></span>      |
| <span data-ttu-id="9f84d-146">-v</span><span class="sxs-lookup"><span data-stu-id="9f84d-146">-v</span></span> | <span data-ttu-id="9f84d-147">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="9f84d-147">--verbose</span></span>                        | <span data-ttu-id="9f84d-148">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="9f84d-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="9f84d-149">--renk yok</span><span class="sxs-lookup"><span data-stu-id="9f84d-149">--no-color</span></span>                       | <span data-ttu-id="9f84d-150">Çıktı renklendirme yok.</span><span class="sxs-lookup"><span data-stu-id="9f84d-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="9f84d-151">--önek çıktı</span><span class="sxs-lookup"><span data-stu-id="9f84d-151">--prefix-output</span></span>                  | <span data-ttu-id="9f84d-152">Düzeyiyle çıktı öneki.</span><span class="sxs-lookup"><span data-stu-id="9f84d-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="9f84d-153">ASP.NET Core ortam belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="9f84d-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="9f84d-154">Komutlar</span><span class="sxs-lookup"><span data-stu-id="9f84d-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="9f84d-155">DotNet ef veritabanı bırakma</span><span class="sxs-lookup"><span data-stu-id="9f84d-155">dotnet ef database drop</span></span>

<span data-ttu-id="9f84d-156">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-156">Drops the database.</span></span>

<span data-ttu-id="9f84d-157">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="9f84d-158">-f</span><span class="sxs-lookup"><span data-stu-id="9f84d-158">-f</span></span> | <span data-ttu-id="9f84d-159">--zorla</span><span class="sxs-lookup"><span data-stu-id="9f84d-159">--force</span></span>   | <span data-ttu-id="9f84d-160">Onaylayın yok.</span><span class="sxs-lookup"><span data-stu-id="9f84d-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="9f84d-161">--çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9f84d-161">--dry-run</span></span> | <span data-ttu-id="9f84d-162">Hangi veritabanı olduğundan bırakılamaz, ancak bırakma yok gösterir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="9f84d-163">DotNet ef veritabanı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="9f84d-163">dotnet ef database update</span></span>

<span data-ttu-id="9f84d-164">Belirtilen geçiş için veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="9f84d-165">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="9f84d-166">\<GEÇİŞ &GT;</span><span class="sxs-lookup"><span data-stu-id="9f84d-166">\<MIGRATION></span></span> | <span data-ttu-id="9f84d-167">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="9f84d-167">The target migration.</span></span> <span data-ttu-id="9f84d-168">0 ise, tüm geçişler geri alınacak.</span><span class="sxs-lookup"><span data-stu-id="9f84d-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="9f84d-169">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="9f84d-170">DotNet ef dbcontext bilgisi</span><span class="sxs-lookup"><span data-stu-id="9f84d-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="9f84d-171">DbContext türü hakkındaki bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="9f84d-172">DotNet ef dbcontext listesi</span><span class="sxs-lookup"><span data-stu-id="9f84d-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="9f84d-173">Kullanılabilir DbContext türleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="9f84d-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="9f84d-174">DotNet ef dbcontext iskele</span><span class="sxs-lookup"><span data-stu-id="9f84d-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="9f84d-175">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="9f84d-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="9f84d-176">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-176">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="9f84d-177">\<BAĞLANTI &GT;</span><span class="sxs-lookup"><span data-stu-id="9f84d-177">\<CONNECTION></span></span> | <span data-ttu-id="9f84d-178">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="9f84d-178">The connection string to the database.</span></span>                              |
| <span data-ttu-id="9f84d-179">\<SAĞLAYICI &GT;</span><span class="sxs-lookup"><span data-stu-id="9f84d-179">\<PROVIDER></span></span>   | <span data-ttu-id="9f84d-180">Kullanılacak sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="9f84d-180">The provider to use.</span></span> <span data-ttu-id="9f84d-181">(Örn.</span><span class="sxs-lookup"><span data-stu-id="9f84d-181">(E.g.</span></span> <span data-ttu-id="9f84d-182">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="9f84d-182">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="9f84d-183">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-183">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9f84d-184"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="9f84d-184"><nobr>-d</nobr></span></span> | <span data-ttu-id="9f84d-185">--Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="9f84d-185">--data-annotations</span></span>                      | <span data-ttu-id="9f84d-186">Öznitelikler, model (mümkün olduğunda) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="9f84d-186">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="9f84d-187">Atlanırsa, yalnızca fluent API kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-187">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="9f84d-188">-c</span><span class="sxs-lookup"><span data-stu-id="9f84d-188">-c</span></span>              | <span data-ttu-id="9f84d-189">--bağlam \<adı ></span><span class="sxs-lookup"><span data-stu-id="9f84d-189">--context \<NAME></span></span>                       | <span data-ttu-id="9f84d-190">DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="9f84d-190">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="9f84d-191">--bağlam dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="9f84d-191">--context-dir \<PATH></span></span>                   | <span data-ttu-id="9f84d-192">DbContext dosyasını yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="9f84d-192">The directory to put DbContext file in.</span></span> <span data-ttu-id="9f84d-193">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-193">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="9f84d-194">-f</span><span class="sxs-lookup"><span data-stu-id="9f84d-194">-f</span></span>              | <span data-ttu-id="9f84d-195">--zorla</span><span class="sxs-lookup"><span data-stu-id="9f84d-195">--force</span></span>                                 | <span data-ttu-id="9f84d-196">Var olan dosyaların üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="9f84d-196">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="9f84d-197">-o</span><span class="sxs-lookup"><span data-stu-id="9f84d-197">-o</span></span>              | <span data-ttu-id="9f84d-198">--Çıkış dir \<yolu ></span><span class="sxs-lookup"><span data-stu-id="9f84d-198">--output-dir \<PATH></span></span>                    | <span data-ttu-id="9f84d-199">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="9f84d-199">The directory to put files in.</span></span> <span data-ttu-id="9f84d-200">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-200">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="9f84d-201"><nobr>--şema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="9f84d-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="9f84d-202">Varlık türleri oluşturmak için tabloları şemalar.</span><span class="sxs-lookup"><span data-stu-id="9f84d-202">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="9f84d-203">-t</span><span class="sxs-lookup"><span data-stu-id="9f84d-203">-t</span></span>              | <span data-ttu-id="9f84d-204">--Tablo \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="9f84d-204">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="9f84d-205">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="9f84d-205">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="9f84d-206">--adları veritabanı kullan</span><span class="sxs-lookup"><span data-stu-id="9f84d-206">--use-database-names</span></span>                    | <span data-ttu-id="9f84d-207">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="9f84d-207">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="9f84d-208">DotNet ef geçişler ekleme</span><span class="sxs-lookup"><span data-stu-id="9f84d-208">dotnet ef migrations add</span></span>

<span data-ttu-id="9f84d-209">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="9f84d-209">Adds a new migration.</span></span>

<span data-ttu-id="9f84d-210">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-210">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="9f84d-211">\<ADI &GT;</span><span class="sxs-lookup"><span data-stu-id="9f84d-211">\<NAME></span></span> | <span data-ttu-id="9f84d-212">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="9f84d-212">The name of the migration.</span></span> |

<span data-ttu-id="9f84d-213">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-213">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9f84d-214"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="9f84d-214"><nobr>-o</nobr></span></span> | <span data-ttu-id="9f84d-215"><nobr>--Çıkış dir \<yolu ></nobr></span><span class="sxs-lookup"><span data-stu-id="9f84d-215"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="9f84d-216">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="9f84d-216">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="9f84d-217">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-217">Paths are relative to the project directory.</span></span> <span data-ttu-id="9f84d-218">Varsayılan olarak "Geçiş".</span><span class="sxs-lookup"><span data-stu-id="9f84d-218">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="9f84d-219">DotNet ef geçişler listesi</span><span class="sxs-lookup"><span data-stu-id="9f84d-219">dotnet ef migrations list</span></span>

<span data-ttu-id="9f84d-220">Kullanılabilir geçişler listeler.</span><span class="sxs-lookup"><span data-stu-id="9f84d-220">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="9f84d-221">DotNet ef geçişler Kaldır</span><span class="sxs-lookup"><span data-stu-id="9f84d-221">dotnet ef migrations remove</span></span>

<span data-ttu-id="9f84d-222">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-222">Removes the last migration.</span></span>

<span data-ttu-id="9f84d-223">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-223">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="9f84d-224">-f</span><span class="sxs-lookup"><span data-stu-id="9f84d-224">-f</span></span> | <span data-ttu-id="9f84d-225">--zorla</span><span class="sxs-lookup"><span data-stu-id="9f84d-225">--force</span></span> | <span data-ttu-id="9f84d-226">Veritabanına uyguladıysanız geçişi geri alın.</span><span class="sxs-lookup"><span data-stu-id="9f84d-226">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="9f84d-227">DotNet ef geçişler komut dosyası</span><span class="sxs-lookup"><span data-stu-id="9f84d-227">dotnet ef migrations script</span></span>

<span data-ttu-id="9f84d-228">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9f84d-228">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="9f84d-229">Bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-229">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="9f84d-230">\<GELEN &GT;</span><span class="sxs-lookup"><span data-stu-id="9f84d-230">\<FROM></span></span> | <span data-ttu-id="9f84d-231">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="9f84d-231">The starting migration.</span></span> <span data-ttu-id="9f84d-232">Varsayılan ayar: 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="9f84d-232">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="9f84d-233">\<İÇİN &GT;</span><span class="sxs-lookup"><span data-stu-id="9f84d-233">\<TO></span></span>   | <span data-ttu-id="9f84d-234">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="9f84d-234">The ending migration.</span></span> <span data-ttu-id="9f84d-235">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9f84d-235">Defaults to the last migration.</span></span>         |

<span data-ttu-id="9f84d-236">Seçenekler:</span><span class="sxs-lookup"><span data-stu-id="9f84d-236">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="9f84d-237">-o</span><span class="sxs-lookup"><span data-stu-id="9f84d-237">-o</span></span> | <span data-ttu-id="9f84d-238">--Çıkış \<dosyası ></span><span class="sxs-lookup"><span data-stu-id="9f84d-238">--output \<FILE></span></span> | <span data-ttu-id="9f84d-239">Sonucu yazmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="9f84d-239">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="9f84d-240">-i</span><span class="sxs-lookup"><span data-stu-id="9f84d-240">-i</span></span> | <span data-ttu-id="9f84d-241">--ıdempotent</span><span class="sxs-lookup"><span data-stu-id="9f84d-241">--idempotent</span></span>     | <span data-ttu-id="9f84d-242">Bir veritabanında hiçbir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9f84d-242">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
