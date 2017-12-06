---
title: "Çekirdek 1.0 RC2 - RTM için EF yükseltme EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="520e9-102">RTM'ye EF çekirdek 1.0 RC2 ' yükseltme</span><span class="sxs-lookup"><span data-stu-id="520e9-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="520e9-103">Bu makalede 1.0.0 RC2 paketler ile oluşturulan bir uygulamayı taşıma için kılavuz sağlar RTM.</span><span class="sxs-lookup"><span data-stu-id="520e9-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="520e9-104">Paketi sürümleri</span><span class="sxs-lookup"><span data-stu-id="520e9-104">Package Versions</span></span>

<span data-ttu-id="520e9-105">Genellikle bir uygulamayı yükler en üst düzey paketleri adlarını RC2 ve RTM arasında değişmemiştir.</span><span class="sxs-lookup"><span data-stu-id="520e9-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="520e9-106">**Yüklü paketler RTM sürümüne yükseltme yapmanız gerekir:**</span><span class="sxs-lookup"><span data-stu-id="520e9-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="520e9-107">Çalışma zamanı paketleri (örneğin `Microsoft.EntityFrameworkCore.SqlServer`) değiştirildi `1.0.0-rc2-final` için `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="520e9-107">Runtime packages (e.g. `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="520e9-108">`Microsoft.EntityFrameworkCore.Tools` Paket değiştirildi `1.0.0-preview1-final` için `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="520e9-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="520e9-109">Araç hala yayın öncesi olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="520e9-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="520e9-110">Varolan geçişleri eklenen maxLength gerekebilir</span><span class="sxs-lookup"><span data-stu-id="520e9-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="520e9-111">Gibi bir geçiş sütun tanımında RC2 içinde arama `table.Column<string>(nullable: true)` ve sütun uzunluğu depolarız geçiş arkasındaki kodda bazı meta verilerde arama.</span><span class="sxs-lookup"><span data-stu-id="520e9-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="520e9-112">RTM içinde uzunluğu kurulmuş kod artık dahil `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="520e9-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="520e9-113">RTM kullanılmadan önce iskele kurulmuş tüm mevcut geçişler sahip olmamak `maxLength` belirtilen bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="520e9-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="520e9-114">Bu veritabanı tarafından desteklenen en fazla uzunluk kullanılacak anlamına gelir (`nvarchar(max)` SQL Server üzerinde).</span><span class="sxs-lookup"><span data-stu-id="520e9-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="520e9-115">Bu bazı sütunları, ancak bir anahtar, yabancı anahtar parçası olan sütunlar için ince olabilir veya dizin en fazla içerecek şekilde güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="520e9-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="520e9-116">Kurala göre 450 en fazla uzunluk yabancı anahtarlar anahtarları için kullanılan sütun dizini ise.</span><span class="sxs-lookup"><span data-stu-id="520e9-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="520e9-117">Ardından bir uzunluk modelde yapılandırdıysanız bu uzunluğu yerine kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="520e9-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="520e9-118">**ASP.NET kimliği**</span><span class="sxs-lookup"><span data-stu-id="520e9-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="520e9-119">Bu değişiklik öncesi oluşturulmuş ve ASP.NET Identity kullanan projeleri etkiler-proje şablonu RTM.</span><span class="sxs-lookup"><span data-stu-id="520e9-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="520e9-120">Proje şablonu veritabanı oluşturmak için kullanılan bir geçiş içerir.</span><span class="sxs-lookup"><span data-stu-id="520e9-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="520e9-121">En büyük uzunluğunu belirtmek için bu geçiş düzenlenmesi `256` şu sütunlar için.</span><span class="sxs-lookup"><span data-stu-id="520e9-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="520e9-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="520e9-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="520e9-123">Ad</span><span class="sxs-lookup"><span data-stu-id="520e9-123">Name</span></span>

    * <span data-ttu-id="520e9-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="520e9-124">NormalizedName</span></span>

*  <span data-ttu-id="520e9-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="520e9-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="520e9-126">E-posta</span><span class="sxs-lookup"><span data-stu-id="520e9-126">Email</span></span>

   * <span data-ttu-id="520e9-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="520e9-127">NormalizedEmail</span></span>

   * <span data-ttu-id="520e9-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="520e9-128">NormalizedUserName</span></span>

   * <span data-ttu-id="520e9-129">Kullanıcı adı</span><span class="sxs-lookup"><span data-stu-id="520e9-129">UserName</span></span>

<span data-ttu-id="520e9-130">Bir veritabanına ilk geçiş uygulandığında hata bu değişikliği yapmak için aşağıdaki özel durum neden olur.</span><span class="sxs-lookup"><span data-stu-id="520e9-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="520e9-131">.NET core: "Imports" project.json kaldırın.</span><span class="sxs-lookup"><span data-stu-id="520e9-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="520e9-132">.NET Core RC2 ile hedefleme, eklemek için gereken `imports` .NET standart desteklemediğinden EF çekirdek'ın bağımlılıkları bazıları için geçici bir çözüm olarak project.json için.</span><span class="sxs-lookup"><span data-stu-id="520e9-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="520e9-133">Bunlar artık kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="520e9-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="520e9-134">UWP: bağlama yeniden yönlendirmeleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="520e9-134">UWP: Add binding redirects</span></span>

<span data-ttu-id="520e9-135">Evrensel Windows Platformu (UWP) projeleri sonuçlarını aşağıdaki hata EF komutları çalıştırma denemesi:</span><span class="sxs-lookup"><span data-stu-id="520e9-135">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="520e9-136">UWP projesini bağlama yeniden yönlendirmeleri el ile eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="520e9-136">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="520e9-137">Adlı bir dosya oluşturun `App.config` projesinde kök klasör ve doğru derleme sürümlerini yeniden yönlendirmeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="520e9-137">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
