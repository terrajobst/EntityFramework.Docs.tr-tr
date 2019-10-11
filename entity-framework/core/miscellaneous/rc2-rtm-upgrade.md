---
title: EF Core 1,0 RC2 'den RTM 'ye yükseltme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: e7f121d18931e26e7b5d11842da6da4a9b789efe
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181357"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="46781-102">EF Core 1,0 RC2 'den RTM 'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="46781-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="46781-103">Bu makalede RC2 paketleri ile derlenmiş bir uygulamayı 1.0.0 RTM 'ye taşımaya yönelik rehberlik sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="46781-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="46781-104">Paket sürümleri</span><span class="sxs-lookup"><span data-stu-id="46781-104">Package Versions</span></span>

<span data-ttu-id="46781-105">Genellikle bir uygulamaya yüklediğiniz en üst düzey paketlerin adları RC2 ve RTM arasında değişmemelidir.</span><span class="sxs-lookup"><span data-stu-id="46781-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="46781-106">**Yüklü paketleri RTM sürümlerine yükseltmeniz gerekir:**</span><span class="sxs-lookup"><span data-stu-id="46781-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="46781-107">Çalışma zamanı paketleri (örneğin, `Microsoft.EntityFrameworkCore.SqlServer`) `1.0.0-rc2-final` ' den `1.0.0` ' ye değişti.</span><span class="sxs-lookup"><span data-stu-id="46781-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="46781-108">@No__t-0 paketi, `1.0.0-preview1-final` ' den `1.0.0-preview2-final` ' ye değişti.</span><span class="sxs-lookup"><span data-stu-id="46781-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="46781-109">Araç araçlarının hala ön sürüm olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="46781-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="46781-110">Mevcut geçişlerde maxLength 'in eklenmesi gerekebilir</span><span class="sxs-lookup"><span data-stu-id="46781-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="46781-111">RC2 'de, bir geçişte sütun tanımı `table.Column<string>(nullable: true)` gibi görünüyor ve sütunun uzunluğu, geçişin arkasındaki kodda depoladığımız bazı meta verilerde aranmıştı.</span><span class="sxs-lookup"><span data-stu-id="46781-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="46781-112">RTM 'de, uzunluk artık iskele `table.Column<string>(maxLength: 450, nullable: true)` ' a dahildir.</span><span class="sxs-lookup"><span data-stu-id="46781-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="46781-113">RTM kullanılmadan önce iskele alınan mevcut geçişlerde `maxLength` bağımsız değişkeni belirtilecektir.</span><span class="sxs-lookup"><span data-stu-id="46781-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="46781-114">Bu, veritabanı tarafından desteklenen uzunluk üst sınırının kullanılacağı anlamına gelir (SQL Server üzerinde `nvarchar(max)`).</span><span class="sxs-lookup"><span data-stu-id="46781-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="46781-115">Bu, bazı sütunlarda iyi olabilir, ancak bir anahtarın, yabancı anahtarın veya dizinin parçası olan sütunların en fazla uzunluk içerecek şekilde güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="46781-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="46781-116">Kurala göre, 450, anahtar, yabancı anahtar ve dizinli sütunlar için kullanılan en büyük uzunluktadır.</span><span class="sxs-lookup"><span data-stu-id="46781-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="46781-117">Modelde bir uzunluğu açık olarak yapılandırdıysanız, bunun yerine bu uzunluğu kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="46781-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="46781-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="46781-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="46781-119">Bu değişiklik ASP.NET Identity kullanan projeleri etkiler ve bir pre-RTM proje şablonundan oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="46781-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="46781-120">Proje şablonu, veritabanını oluşturmak için kullanılan bir geçiş içerir.</span><span class="sxs-lookup"><span data-stu-id="46781-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="46781-121">Bu geçiş, aşağıdaki sütunlar için en fazla `256` değerini belirtmek üzere düzenlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="46781-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="46781-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="46781-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="46781-123">Name</span><span class="sxs-lookup"><span data-stu-id="46781-123">Name</span></span>

    * <span data-ttu-id="46781-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="46781-124">NormalizedName</span></span>

*  <span data-ttu-id="46781-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="46781-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="46781-126">Email</span><span class="sxs-lookup"><span data-stu-id="46781-126">Email</span></span>

   * <span data-ttu-id="46781-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="46781-127">NormalizedEmail</span></span>

   * <span data-ttu-id="46781-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="46781-128">NormalizedUserName</span></span>

   * <span data-ttu-id="46781-129">UserName</span><span class="sxs-lookup"><span data-stu-id="46781-129">UserName</span></span>

<span data-ttu-id="46781-130">Bir veritabanına ilk geçiş uygulandığında, bu değişikliğin başarısız olması aşağıdaki özel duruma neden olur.</span><span class="sxs-lookup"><span data-stu-id="46781-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

```console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="46781-131">.NET Core: Project. JSON içindeki "Imports" öğesini kaldır</span><span class="sxs-lookup"><span data-stu-id="46781-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="46781-132">RC2 ile .NET Core 'u hedefliyorsanız, bazı EF Core bağımlılıklarının .NET Standard desteklememe için geçici bir geçici çözüm olarak Project. json ' a `imports` eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46781-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="46781-133">Bunlar artık kaldırılabilirler.</span><span class="sxs-lookup"><span data-stu-id="46781-133">These can now be removed.</span></span>

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
> <span data-ttu-id="46781-134">Sürüm 1,0 RTM itibariyle, [.NET Core SDK](https://www.microsoft.com/net/download/core) artık `project.json` ' i desteklemiyor veya Visual Studio 2015 kullanarak .NET Core uygulamaları geliştiriyor.</span><span class="sxs-lookup"><span data-stu-id="46781-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="46781-135">[Project. JSON 'dan csproj 'a geçiş](https://docs.microsoft.com/dotnet/articles/core/migration/)yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="46781-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="46781-136">Visual Studio kullanıyorsanız, [Visual studio 2017](https://www.visualstudio.com/downloads/)sürümüne yükseltmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="46781-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="46781-137">UWP Bağlama yeniden yönlendirmeleri ekleme</span><span class="sxs-lookup"><span data-stu-id="46781-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="46781-138">Evrensel Windows Platformu (UWP) projelerinde EF komutlarının çalıştırılmaya çalışılması aşağıdaki hatayla sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="46781-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

```console
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

<span data-ttu-id="46781-139">UWP projesine el ile bağlama yeniden yönlendirmeleri eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46781-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="46781-140">Proje kök klasöründe `App.config` adlı bir dosya oluşturun ve doğru derleme sürümlerine yeniden yönlendirmeler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="46781-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

```xml
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
