---
title: 1.0 RC2 - RTM için çekirdek EF yükseltme EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949063"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="52fb7-102">EF Core 1.0 RC2'den RTM'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="52fb7-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="52fb7-103">Bu makale 1.0.0 RC2 paketler ile oluşturulmuş bir uygulama taşıma konusunda rehberlik yapmaktadır RTM.</span><span class="sxs-lookup"><span data-stu-id="52fb7-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="52fb7-104">Paket sürümü</span><span class="sxs-lookup"><span data-stu-id="52fb7-104">Package Versions</span></span>

<span data-ttu-id="52fb7-105">Bir uygulamaya normal olarak yüklenir üst düzey paketlerin adlarını RC2 ile RTM arasında değişmemiştir.</span><span class="sxs-lookup"><span data-stu-id="52fb7-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="52fb7-106">**Yüklü paketleri RTM sürümüne yükseltme yapmanız gerekir:**</span><span class="sxs-lookup"><span data-stu-id="52fb7-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="52fb7-107">Çalışma zamanı paketlerini (örneğin, `Microsoft.EntityFrameworkCore.SqlServer`) değiştirildi `1.0.0-rc2-final` için `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="52fb7-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="52fb7-108">`Microsoft.EntityFrameworkCore.Tools` Paket değiştirildi `1.0.0-preview1-final` için `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="52fb7-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="52fb7-109">Araç hala yayın öncesi olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="52fb7-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="52fb7-110">Mevcut geçişleri eklenen maxLength gerekebilir</span><span class="sxs-lookup"><span data-stu-id="52fb7-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="52fb7-111">RC2'deki gibi bir geçiş sütun tanımında baktığı `table.Column<string>(nullable: true)` ve sütun uzunluğu depolarız geçiş arkasındaki kodda bazı meta verilerinde aranabilir.</span><span class="sxs-lookup"><span data-stu-id="52fb7-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="52fb7-112">RTM sürümündeki uzunluğu iskele kurulmuş kodu artık dahil `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="52fb7-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="52fb7-113">RTM kullanılmadan önce iskele kurulmuş mevcut tüm geçişler olmayacaktır `maxLength` bağımsız değişken belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="52fb7-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="52fb7-114">Bu veritabanı tarafından desteklenen en fazla uzunluk kullanılacak anlamına gelir (`nvarchar(max)` SQL Server).</span><span class="sxs-lookup"><span data-stu-id="52fb7-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="52fb7-115">Bu bazı sütunları, ancak bir anahtar, yabancı anahtar parçası olan sütunlar için iyi olabilir veya dizin uzunluğu en fazla içerecek şekilde güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="52fb7-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="52fb7-116">Kural gereği, 450 uzunluğu en fazla kullanılan anahtarları, yabancı anahtarlar ve sütunlar dizine var.</span><span class="sxs-lookup"><span data-stu-id="52fb7-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="52fb7-117">Ardından model içinde açıkça bir uzunluk yapılandırdıysanız, bu uzunluğu yerine kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="52fb7-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="52fb7-118">**ASP.NET kimlik**</span><span class="sxs-lookup"><span data-stu-id="52fb7-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="52fb7-119">Bu değişiklik, ASP.NET Identity kullanan ve bir önceden oluşturulmuş projeleri etkiler-RTM proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="52fb7-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="52fb7-120">Veritabanını oluşturmak için kullanılan bir geçiş proje şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="52fb7-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="52fb7-121">Bu geçiş, maksimum uzunluğunu belirtmek üzere düzenlenmelidir `256` şu sütunlar için.</span><span class="sxs-lookup"><span data-stu-id="52fb7-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="52fb7-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="52fb7-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="52fb7-123">Ad</span><span class="sxs-lookup"><span data-stu-id="52fb7-123">Name</span></span>

    * <span data-ttu-id="52fb7-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="52fb7-124">NormalizedName</span></span>

*  <span data-ttu-id="52fb7-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="52fb7-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="52fb7-126">E-posta</span><span class="sxs-lookup"><span data-stu-id="52fb7-126">Email</span></span>

   * <span data-ttu-id="52fb7-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="52fb7-127">NormalizedEmail</span></span>

   * <span data-ttu-id="52fb7-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="52fb7-128">NormalizedUserName</span></span>

   * <span data-ttu-id="52fb7-129">UserName</span><span class="sxs-lookup"><span data-stu-id="52fb7-129">UserName</span></span>

<span data-ttu-id="52fb7-130">Bir veritabanına ilk geçiş uygulandığında hata bu değişikliği yapmak için aşağıdaki özel duruma neden olur.</span><span class="sxs-lookup"><span data-stu-id="52fb7-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="52fb7-131">.NET core: "içeri aktarmalar" project.json kaldırın.</span><span class="sxs-lookup"><span data-stu-id="52fb7-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="52fb7-132">.NET Core RC2 ile hedefleyen, eklemek için gereken `imports` project.json .NET Standard desteklemediğinden EF Core'nın bağımlılıkları bazıları için geçici bir çözüm olarak için.</span><span class="sxs-lookup"><span data-stu-id="52fb7-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="52fb7-133">Bunlar artık kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="52fb7-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="52fb7-134">1.0 sürümünden itibaren RTM [.NET Core SDK'sı](https://www.microsoft.com/net/download/core) artık `project.json` veya Visual Studio 2015'i kullanarak .NET Core uygulamaları geliştirmek.</span><span class="sxs-lookup"><span data-stu-id="52fb7-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="52fb7-135">Öneririz [Project.json'dan csproj'a geçirilmesi geçirme](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="52fb7-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="52fb7-136">Visual Studio kullanıyorsanız, yükseltme öneririz [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="52fb7-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="52fb7-137">UWP: bağlama yeniden yönlendirmeleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="52fb7-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="52fb7-138">Evrensel Windows Platformu (UWP) projeleri şu hata sonuçlarında EF komutları çalıştırmak çalışılıyor:</span><span class="sxs-lookup"><span data-stu-id="52fb7-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="52fb7-139">UWP projesi için bağlama yeniden yönlendirmelerini el ile eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="52fb7-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="52fb7-140">Adlı bir dosya oluşturun `App.config` projesinde kök klasörü ve için doğru derleme sürümlerini yeniden yönlendirmeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="52fb7-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

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
