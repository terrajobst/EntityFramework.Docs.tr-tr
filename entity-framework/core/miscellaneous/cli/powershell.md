---
title: Paket Yöneticisi Konsolu (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490862"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="45168-102">EF Core Paket Yöneticisi konsolu araçları</span><span class="sxs-lookup"><span data-stu-id="45168-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="45168-103">NuGet kullanarak EF Core Paket Yöneticisi Konsolu (PMC) araçları Visual Studio içinde çalıştırma [Paket Yöneticisi Konsolu][2].</span><span class="sxs-lookup"><span data-stu-id="45168-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="45168-104">Bu araçlar, .NET Framework hem de .NET Core projeleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="45168-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="45168-105">Visual Studio kullanmıyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="45168-105">Not using Visual Studio?</span></span> <span data-ttu-id="45168-106">[EF Core komut satırı araçları] [ 1] platformlar arası ve bir komut istemi içinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="45168-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="45168-107">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="45168-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="45168-108">EF Core Paket Yöneticisi konsolu araçları Microsoft.EntityFrameworkCore.Tools NuGet paketini yükleyerek yükleyin.</span><span class="sxs-lookup"><span data-stu-id="45168-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="45168-109">İçinde aşağıdaki komutu çalıştırarak yükleyebilirsiniz [Paket Yöneticisi Konsolu][2].</span><span class="sxs-lookup"><span data-stu-id="45168-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="45168-110">Her şeyin düzgün çalıştığı, bu komutu çalıştırmak kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45168-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="45168-111">Başlangıç projeniz .NET Standard hedefliyorsa [desteklenen framework çapraz hedefleme] [ 3] Araçları'nı kullanmadan önce.</span><span class="sxs-lookup"><span data-stu-id="45168-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45168-112">Kullanıyorsanız **Evrensel Windows** veya **Xamarin**, EF kodunuzu .NET Standard Sınıf Kitaplığı'na taşıyın ve [desteklenen framework çapraz hedefleme] [ 3] Araçları'nı kullanmadan önce.</span><span class="sxs-lookup"><span data-stu-id="45168-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="45168-113">Sınıf kitaplığı, başlangıç projesi olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="45168-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="45168-114">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="45168-114">Using the tools</span></span>
---------------
<span data-ttu-id="45168-115">Bir komut çağırma her iki proje kullanılan vardır:</span><span class="sxs-lookup"><span data-stu-id="45168-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="45168-116">Hedef dosyalar eklenir (veya bazı durumlarda kaldırıldı) projesidir.</span><span class="sxs-lookup"><span data-stu-id="45168-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="45168-117">Hedef Proje Varsayılanları **varsayılan proje** Paket Yöneticisi Konsolu'nda seçildi, ancak kullanılarak da belirtilebilir proje parametresi.</span><span class="sxs-lookup"><span data-stu-id="45168-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="45168-118">Başlangıç projesi, proje kodunuzun yürütülürken araçları tarafından Öykünülen bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="45168-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="45168-119">İçin varsayılan olarak **başlangıç projesi olarak ayarla** Çözüm Gezgini'nde.</span><span class="sxs-lookup"><span data-stu-id="45168-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="45168-120">Bu, - StartupProject parametresi kullanılarak da belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="45168-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="45168-121">Ortak parametreleri:</span><span class="sxs-lookup"><span data-stu-id="45168-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="45168-122">-Bağlam \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-122">-Context \<String></span></span>        | <span data-ttu-id="45168-123">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="45168-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="45168-124">-Project \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-124">-Project \<String></span></span>        | <span data-ttu-id="45168-125">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="45168-125">The project to use.</span></span>         |
| <span data-ttu-id="45168-126">-StartupProject \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-126">-StartupProject \<String></span></span> | <span data-ttu-id="45168-127">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="45168-127">The startup project to use.</span></span> |
| <span data-ttu-id="45168-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="45168-128">-Verbose</span></span>                  | <span data-ttu-id="45168-129">Ayrıntılı çıktıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="45168-129">Show verbose output.</span></span>        |

<span data-ttu-id="45168-130">Bir komut hakkında Yardım bilgilerini görüntülemek için PowerShell'in kullanın `Get-Help` komutu.</span><span class="sxs-lookup"><span data-stu-id="45168-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="45168-131">Bağlam, proje ve StartupProject parametreleri sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="45168-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="45168-132">Ayarlama **env:ASPNETCORE_ENVIRONMENT** ASP.NET Core ortamını belirtmek için çalıştırmadan önce.</span><span class="sxs-lookup"><span data-stu-id="45168-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="45168-133">Komutlar</span><span class="sxs-lookup"><span data-stu-id="45168-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="45168-134">Geçiş Ekle</span><span class="sxs-lookup"><span data-stu-id="45168-134">Add-Migration</span></span>

<span data-ttu-id="45168-135">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="45168-135">Adds a new migration.</span></span>

<span data-ttu-id="45168-136">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="45168-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="45168-137">***-Ad*** \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-137">***-Name*** \<String></span></span>             | <span data-ttu-id="45168-138">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="45168-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="45168-139"><nobr>-OutputDir \<dizesi ></nobr></span><span class="sxs-lookup"><span data-stu-id="45168-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="45168-140">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="45168-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="45168-141">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="45168-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="45168-142">Varsayılan olarak "Geçişler".</span><span class="sxs-lookup"><span data-stu-id="45168-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="45168-143">Parametrelerinde **kalın** gereklidir ve olanları içinde *italik* konumsal olan.</span><span class="sxs-lookup"><span data-stu-id="45168-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="45168-144">Veritabanını silme</span><span class="sxs-lookup"><span data-stu-id="45168-144">Drop-Database</span></span>

<span data-ttu-id="45168-145">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="45168-145">Drops the database.</span></span>

<span data-ttu-id="45168-146">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="45168-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="45168-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="45168-147">-WhatIf</span></span> | <span data-ttu-id="45168-148">Hangi veritabanı bırakılan ancak açılan yoksa gösterir.</span><span class="sxs-lookup"><span data-stu-id="45168-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="45168-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="45168-149">Get-DbContext</span></span>

<span data-ttu-id="45168-150">DbContext tür bilgilerini alır.</span><span class="sxs-lookup"><span data-stu-id="45168-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="45168-151">Remove-geçiş</span><span class="sxs-lookup"><span data-stu-id="45168-151">Remove-Migration</span></span>

<span data-ttu-id="45168-152">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="45168-152">Removes the last migration.</span></span>

<span data-ttu-id="45168-153">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="45168-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="45168-154">-Force</span><span class="sxs-lookup"><span data-stu-id="45168-154">-Force</span></span> | <span data-ttu-id="45168-155">Veritabanına uygulanmışsa, geçişi geri alın.</span><span class="sxs-lookup"><span data-stu-id="45168-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="45168-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="45168-156">Scaffold-DbContext</span></span>

<span data-ttu-id="45168-157">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="45168-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="45168-158">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="45168-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="45168-159"><nobr>***-Bağlantı*** \<dizesi ></nobr></span><span class="sxs-lookup"><span data-stu-id="45168-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="45168-160">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="45168-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="45168-161">***-Provider*** \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="45168-162">Kullanılacak sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="45168-162">The provider to use.</span></span> <span data-ttu-id="45168-163">(örneğin, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="45168-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="45168-164">-OutputDir \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="45168-165">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="45168-165">The directory to put files in.</span></span> <span data-ttu-id="45168-166">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="45168-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="45168-167">-ContextDir \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="45168-168">DbContext dosya yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="45168-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="45168-169">Proje dizinine göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="45168-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="45168-170">-Bağlam \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-170">-Context \<String></span></span>                       | <span data-ttu-id="45168-171">Oluşturulacak DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="45168-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="45168-172">-Şemaları \<String [] ></span><span class="sxs-lookup"><span data-stu-id="45168-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="45168-173">Şemaları için varlık türleri oluşturmak için tablo.</span><span class="sxs-lookup"><span data-stu-id="45168-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="45168-174">-Tablolar \<String [] ></span><span class="sxs-lookup"><span data-stu-id="45168-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="45168-175">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="45168-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="45168-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="45168-176">-DataAnnotations</span></span>                         | <span data-ttu-id="45168-177">Öznitelikler, model (mümkünse) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="45168-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="45168-178">Atlanırsa, yalnızca fluent API'si kullanılır.</span><span class="sxs-lookup"><span data-stu-id="45168-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="45168-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="45168-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="45168-180">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="45168-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="45168-181">-Force</span><span class="sxs-lookup"><span data-stu-id="45168-181">-Force</span></span>                                   | <span data-ttu-id="45168-182">Varolan dosyaların üzerine yaz.</span><span class="sxs-lookup"><span data-stu-id="45168-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="45168-183">Geçiş betiği</span><span class="sxs-lookup"><span data-stu-id="45168-183">Script-Migration</span></span>

<span data-ttu-id="45168-184">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45168-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="45168-185">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="45168-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="45168-186">*-From* \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-186">*-From* \<String></span></span> | <span data-ttu-id="45168-187">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="45168-187">The starting migration.</span></span> <span data-ttu-id="45168-188">Varsayılan olarak 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="45168-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="45168-189">*-Çok* \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-189">*-To* \<String></span></span>   | <span data-ttu-id="45168-190">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="45168-190">The ending migration.</span></span> <span data-ttu-id="45168-191">Son varsayılan olarak geçiş.</span><span class="sxs-lookup"><span data-stu-id="45168-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="45168-192">-Bir kez etkili</span><span class="sxs-lookup"><span data-stu-id="45168-192">-Idempotent</span></span>       | <span data-ttu-id="45168-193">Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45168-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="45168-194">-Çıkış \<dizesi ></span><span class="sxs-lookup"><span data-stu-id="45168-194">-Output \<String></span></span> | <span data-ttu-id="45168-195">Yazma sonucu dosyası.</span><span class="sxs-lookup"><span data-stu-id="45168-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="45168-196">Kime, ve çıkış parametreleri sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="45168-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="45168-197">Veritabanını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="45168-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="45168-198"><nobr>*-Geçiş* \<dizesi ></nobr></span><span class="sxs-lookup"><span data-stu-id="45168-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="45168-199">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="45168-199">The target migration.</span></span> <span data-ttu-id="45168-200">'0 'ise, tüm geçişler döndürülecek.</span><span class="sxs-lookup"><span data-stu-id="45168-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="45168-201">Son varsayılan olarak geçiş.</span><span class="sxs-lookup"><span data-stu-id="45168-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="45168-202">Geçiş parametresi sekme genişletmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="45168-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
