---
title: Paket Yöneticisi Konsolu (Visual Studio) - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="4737a-102">EF çekirdek Paket Yöneticisi konsolu araçları</span><span class="sxs-lookup"><span data-stu-id="4737a-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="4737a-103">NuGet kullanarak Visual Studio içinde EF çekirdek Paket Yöneticisi Konsolu (PMC) araçları çalıştırmak [Paket Yöneticisi Konsolu][2].</span><span class="sxs-lookup"><span data-stu-id="4737a-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="4737a-104">Bu araçlar, .NET Framework ve .NET Core projeleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="4737a-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="4737a-105">Visual Studio kullanmıyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="4737a-105">Not using Visual Studio?</span></span> <span data-ttu-id="4737a-106">[EF çekirdek komut satırı araçları] [ 1] platformlar arası ve bir komut istemi içinde çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="4737a-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="4737a-107">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="4737a-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="4737a-108">EF çekirdek Paket Yöneticisi konsolu araçları Microsoft.EntityFrameworkCore.Tools NuGet paketini yükleyerek yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4737a-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="4737a-109">İçinde aşağıdaki komutu çalıştırarak yükleyebilirsiniz [Paket Yöneticisi Konsolu][2].</span><span class="sxs-lookup"><span data-stu-id="4737a-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="4737a-110">Her şeyin doğru şekilde çalışan, bu komutu çalıştırmak yapabiliyor olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="4737a-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="4737a-111">Başlangıç projeniz .NET standart hedefliyorsa [arası hedef desteklenen çerçevelerden] [ 3] araçları kullanmadan önce.</span><span class="sxs-lookup"><span data-stu-id="4737a-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4737a-112">Kullanıyorsanız, **Evrensel Windows** veya **Xamarin**, .NET standart bir sınıf kitaplığı'na EF kodunuzu taşıyın ve [arası hedef desteklenen çerçevelerden] [ 3] araçları kullanmadan önce.</span><span class="sxs-lookup"><span data-stu-id="4737a-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="4737a-113">Sınıf kitaplığı başlangıç projesi olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="4737a-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="4737a-114">Araçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="4737a-114">Using the tools</span></span>
---------------
<span data-ttu-id="4737a-115">Bir komut çağırma her iki proje ilgili vardır:</span><span class="sxs-lookup"><span data-stu-id="4737a-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="4737a-116">Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.</span><span class="sxs-lookup"><span data-stu-id="4737a-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="4737a-117">Hedef Proje Varsayılanları için **varsayılan proje** Paket Yöneticisi Konsolu'nda seçili ancak kullanılarak da belirtilebilir proje parametresi.</span><span class="sxs-lookup"><span data-stu-id="4737a-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="4737a-118">Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.</span><span class="sxs-lookup"><span data-stu-id="4737a-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="4737a-119">Bir varsayılan olarak **başlangıç projesi olarak ayarla** Çözüm Gezgini'nde.</span><span class="sxs-lookup"><span data-stu-id="4737a-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="4737a-120">Bu, - StartupProject parametresi kullanılarak da belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="4737a-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="4737a-121">Ortak Parametreler:</span><span class="sxs-lookup"><span data-stu-id="4737a-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="4737a-122">-Context \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-122">-Context \<String></span></span>        | <span data-ttu-id="4737a-123">Kullanılacak DbContext.</span><span class="sxs-lookup"><span data-stu-id="4737a-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="4737a-124">-Proje \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-124">-Project \<String></span></span>        | <span data-ttu-id="4737a-125">Kullanmak için proje.</span><span class="sxs-lookup"><span data-stu-id="4737a-125">The project to use.</span></span>         |
| <span data-ttu-id="4737a-126">-StartupProject \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-126">-StartupProject \<String></span></span> | <span data-ttu-id="4737a-127">Kullanılacak başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="4737a-127">The startup project to use.</span></span> |
| <span data-ttu-id="4737a-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="4737a-128">-Verbose</span></span>                  | <span data-ttu-id="4737a-129">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="4737a-129">Show verbose output.</span></span>        |

<span data-ttu-id="4737a-130">Bir komutla ilgili Yardım bilgilerini göstermek için PowerShell'ın kullanın `Get-Help` komutu.</span><span class="sxs-lookup"><span data-stu-id="4737a-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="4737a-131">Bağlam, proje ve StartupProject parametreleri sekmesi genişletme destekler.</span><span class="sxs-lookup"><span data-stu-id="4737a-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="4737a-132">Ayarlama **env:ASPNETCORE_ENVIRONMENT** ASP.NET Core ortam belirtmek için çalıştırmadan önce.</span><span class="sxs-lookup"><span data-stu-id="4737a-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="4737a-133">Komutlar</span><span class="sxs-lookup"><span data-stu-id="4737a-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="4737a-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="4737a-134">Add-Migration</span></span>

<span data-ttu-id="4737a-135">Yeni bir geçiş ekler.</span><span class="sxs-lookup"><span data-stu-id="4737a-135">Adds a new migration.</span></span>

<span data-ttu-id="4737a-136">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="4737a-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4737a-137">***-Ad*** \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-137">***-Name*** \<String></span></span>             | <span data-ttu-id="4737a-138">Geçiş adı.</span><span class="sxs-lookup"><span data-stu-id="4737a-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="4737a-139"><nobr>-OutputDir \<dize ></nobr></span><span class="sxs-lookup"><span data-stu-id="4737a-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="4737a-140">Kullanılacak dizin (ve alt ad alanı).</span><span class="sxs-lookup"><span data-stu-id="4737a-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="4737a-141">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="4737a-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="4737a-142">Varsayılan olarak "Geçiş".</span><span class="sxs-lookup"><span data-stu-id="4737a-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="4737a-143">Parametrelerde **kalın** gereklidir ve olanları içinde *italik* konumsal şunlardır.</span><span class="sxs-lookup"><span data-stu-id="4737a-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="4737a-144">Veritabanını silme</span><span class="sxs-lookup"><span data-stu-id="4737a-144">Drop-Database</span></span>

<span data-ttu-id="4737a-145">Veritabanı bırakır.</span><span class="sxs-lookup"><span data-stu-id="4737a-145">Drops the database.</span></span>

<span data-ttu-id="4737a-146">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="4737a-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="4737a-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="4737a-147">-WhatIf</span></span> | <span data-ttu-id="4737a-148">Hangi veritabanı olduğundan bırakılamaz, ancak bırakma yok gösterir.</span><span class="sxs-lookup"><span data-stu-id="4737a-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="4737a-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="4737a-149">Get-DbContext</span></span>

<span data-ttu-id="4737a-150">DbContext türü hakkındaki bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="4737a-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="4737a-151">Remove-geçiş</span><span class="sxs-lookup"><span data-stu-id="4737a-151">Remove-Migration</span></span>

<span data-ttu-id="4737a-152">Son geçiş kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4737a-152">Removes the last migration.</span></span>

<span data-ttu-id="4737a-153">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="4737a-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="4737a-154">-Force</span><span class="sxs-lookup"><span data-stu-id="4737a-154">-Force</span></span> | <span data-ttu-id="4737a-155">Veritabanına uyguladıysanız geçişi geri alın.</span><span class="sxs-lookup"><span data-stu-id="4737a-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="4737a-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="4737a-156">Scaffold-DbContext</span></span>

<span data-ttu-id="4737a-157">Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="4737a-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="4737a-158">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="4737a-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4737a-159"><nobr>***-Bağlantı*** \<dize ></nobr></span><span class="sxs-lookup"><span data-stu-id="4737a-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="4737a-160">Veritabanı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="4737a-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="4737a-161">***-Sağlayıcısı*** \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="4737a-162">Kullanılacak sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="4737a-162">The provider to use.</span></span> <span data-ttu-id="4737a-163">(Örn.</span><span class="sxs-lookup"><span data-stu-id="4737a-163">(E.g.</span></span> <span data-ttu-id="4737a-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="4737a-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="4737a-165">-OutputDir \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="4737a-166">Dosyaları yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="4737a-166">The directory to put files in.</span></span> <span data-ttu-id="4737a-167">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="4737a-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="4737a-168">-ContextDir \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="4737a-169">DbContext dosyasını yerleştirmek için dizin.</span><span class="sxs-lookup"><span data-stu-id="4737a-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="4737a-170">Proje dizininin göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="4737a-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="4737a-171">-Context \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-171">-Context \<String></span></span>                       | <span data-ttu-id="4737a-172">Oluşturulacak DbContext adı.</span><span class="sxs-lookup"><span data-stu-id="4737a-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="4737a-173">-Şemaları \<String [] ></span><span class="sxs-lookup"><span data-stu-id="4737a-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="4737a-174">Varlık türleri oluşturmak için tabloları şemalar.</span><span class="sxs-lookup"><span data-stu-id="4737a-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="4737a-175">-Tabloları \<String [] ></span><span class="sxs-lookup"><span data-stu-id="4737a-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="4737a-176">Varlık türleri için oluşturmak üzere tablolara.</span><span class="sxs-lookup"><span data-stu-id="4737a-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="4737a-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="4737a-177">-DataAnnotations</span></span>                         | <span data-ttu-id="4737a-178">Öznitelikler, model (mümkün olduğunda) yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="4737a-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="4737a-179">Atlanırsa, yalnızca fluent API kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4737a-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="4737a-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="4737a-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="4737a-181">Doğrudan veritabanından tablo ve sütun adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="4737a-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="4737a-182">-Force</span><span class="sxs-lookup"><span data-stu-id="4737a-182">-Force</span></span>                                   | <span data-ttu-id="4737a-183">Var olan dosyaların üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="4737a-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="4737a-184">Komut dosyası geçişi</span><span class="sxs-lookup"><span data-stu-id="4737a-184">Script-Migration</span></span>

<span data-ttu-id="4737a-185">Bir SQL betiği geçişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4737a-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="4737a-186">Parametreler:</span><span class="sxs-lookup"><span data-stu-id="4737a-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="4737a-187">*-From* \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-187">*-From* \<String></span></span> | <span data-ttu-id="4737a-188">Başlangıç geçiş.</span><span class="sxs-lookup"><span data-stu-id="4737a-188">The starting migration.</span></span> <span data-ttu-id="4737a-189">Varsayılan ayar: 0 (ilk veritabanı).</span><span class="sxs-lookup"><span data-stu-id="4737a-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="4737a-190">*-Çok* \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-190">*-To* \<String></span></span>   | <span data-ttu-id="4737a-191">Bitiş geçiş.</span><span class="sxs-lookup"><span data-stu-id="4737a-191">The ending migration.</span></span> <span data-ttu-id="4737a-192">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4737a-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="4737a-193">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="4737a-193">-Idempotent</span></span>       | <span data-ttu-id="4737a-194">Bir veritabanında hiçbir geçiş sırasında kullanılan bir komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4737a-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="4737a-195">-Çıktı \<dize ></span><span class="sxs-lookup"><span data-stu-id="4737a-195">-Output \<String></span></span> | <span data-ttu-id="4737a-196">Sonucu yazmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="4737a-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="4737a-197">Kime, ve çıkış parametreleri sekmesi genişletme desteği.</span><span class="sxs-lookup"><span data-stu-id="4737a-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="4737a-198">Update-Database</span><span class="sxs-lookup"><span data-stu-id="4737a-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="4737a-199"><nobr>*-Geçiş* \<dize ></nobr></span><span class="sxs-lookup"><span data-stu-id="4737a-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="4737a-200">Hedef geçiş.</span><span class="sxs-lookup"><span data-stu-id="4737a-200">The target migration.</span></span> <span data-ttu-id="4737a-201">'0 'ise, tüm geçişler geri alınacak.</span><span class="sxs-lookup"><span data-stu-id="4737a-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="4737a-202">Son geçiş varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4737a-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="4737a-203">Geçiş parametre sekmesini genişletme destekler.</span><span class="sxs-lookup"><span data-stu-id="4737a-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
