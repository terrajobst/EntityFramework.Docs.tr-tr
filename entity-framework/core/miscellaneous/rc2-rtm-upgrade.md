---
title: EF Core 1,0 RC2 'den RTM 'ye yükseltme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 779caad7883d13684b389dab7515be44bc42e1ef
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416525"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="46a78-102">EF Core 1,0 RC2 'den RTM 'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="46a78-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="46a78-103">Bu makalede RC2 paketleri ile derlenmiş bir uygulamayı 1.0.0 RTM 'ye taşımaya yönelik rehberlik sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="46a78-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="46a78-104">Paket sürümleri</span><span class="sxs-lookup"><span data-stu-id="46a78-104">Package Versions</span></span>

<span data-ttu-id="46a78-105">Genellikle bir uygulamaya yüklediğiniz en üst düzey paketlerin adları RC2 ve RTM arasında değişmemelidir.</span><span class="sxs-lookup"><span data-stu-id="46a78-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="46a78-106">**Yüklü paketleri RTM sürümlerine yükseltmeniz gerekir:**</span><span class="sxs-lookup"><span data-stu-id="46a78-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="46a78-107">Çalışma zamanı paketleri (örneğin, `Microsoft.EntityFrameworkCore.SqlServer`) `1.0.0-rc2-final` `1.0.0`olarak değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="46a78-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="46a78-108">`Microsoft.EntityFrameworkCore.Tools` paketi `1.0.0-preview1-final` `1.0.0-preview2-final`olarak değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="46a78-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="46a78-109">Araç araçlarının hala ön sürüm olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="46a78-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="46a78-110">Mevcut geçişlerde maxLength 'in eklenmesi gerekebilir</span><span class="sxs-lookup"><span data-stu-id="46a78-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="46a78-111">RC2 'de, bir geçişte sütun tanımı `table.Column<string>(nullable: true)` gibi, sütunun uzunluğu ise geçişin arkasındaki kodda depoladığımız bazı meta verilerde aranmıştı.</span><span class="sxs-lookup"><span data-stu-id="46a78-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="46a78-112">RTM 'de, uzunluk artık scafkatli kod `table.Column<string>(maxLength: 450, nullable: true)`dahil edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="46a78-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="46a78-113">RTM kullanılmadan önce iskele alınan mevcut geçişler, `maxLength` bağımsız değişkenine sahip olmaz.</span><span class="sxs-lookup"><span data-stu-id="46a78-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="46a78-114">Bu, veritabanı tarafından desteklenen uzunluk üst sınırının kullanılacağı anlamına gelir (SQL Server`nvarchar(max)`).</span><span class="sxs-lookup"><span data-stu-id="46a78-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="46a78-115">Bu, bazı sütunlarda iyi olabilir, ancak bir anahtarın, yabancı anahtarın veya dizinin parçası olan sütunların en fazla uzunluk içerecek şekilde güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="46a78-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="46a78-116">Kurala göre, 450, anahtar, yabancı anahtar ve dizinli sütunlar için kullanılan en büyük uzunluktadır.</span><span class="sxs-lookup"><span data-stu-id="46a78-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="46a78-117">Modelde bir uzunluğu açık olarak yapılandırdıysanız, bunun yerine bu uzunluğu kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="46a78-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="46a78-118">ASP.NET Kimlik</span><span class="sxs-lookup"><span data-stu-id="46a78-118">ASP.NET Identity</span></span>

<span data-ttu-id="46a78-119">Bu değişiklik ASP.NET Identity kullanan projeleri etkiler ve bir pre-RTM proje şablonundan oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="46a78-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="46a78-120">Proje şablonu, veritabanını oluşturmak için kullanılan bir geçiş içerir.</span><span class="sxs-lookup"><span data-stu-id="46a78-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="46a78-121">Bu geçiş, aşağıdaki sütunlar için en fazla `256` uzunluğunu belirtmek üzere düzenlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="46a78-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

* <span data-ttu-id="46a78-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="46a78-122">**AspNetRoles**</span></span>
  * <span data-ttu-id="46a78-123">Adı</span><span class="sxs-lookup"><span data-stu-id="46a78-123">Name</span></span>
  * <span data-ttu-id="46a78-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="46a78-124">NormalizedName</span></span>
* <span data-ttu-id="46a78-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="46a78-125">**AspNetUsers**</span></span>
  * <span data-ttu-id="46a78-126">Email</span><span class="sxs-lookup"><span data-stu-id="46a78-126">Email</span></span>
  * <span data-ttu-id="46a78-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="46a78-127">NormalizedEmail</span></span>
  * <span data-ttu-id="46a78-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="46a78-128">NormalizedUserName</span></span>
  * <span data-ttu-id="46a78-129">UserName</span><span class="sxs-lookup"><span data-stu-id="46a78-129">UserName</span></span>

<span data-ttu-id="46a78-130">Bir veritabanına ilk geçiş uygulandığında, bu değişikliğin başarısız olması aşağıdaki özel duruma neden olur.</span><span class="sxs-lookup"><span data-stu-id="46a78-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

``` Console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="46a78-131">.NET Core: Project. JSON içinde "Imports" öğesini kaldır</span><span class="sxs-lookup"><span data-stu-id="46a78-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="46a78-132">RC2 ile .NET Core 'u hedefliyorsanız, bazı EF Core bağımlılıklarının .NET Standard desteklememesine yönelik geçici bir geçici çözüm olarak Project. json ' a `imports` eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46a78-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="46a78-133">Bunlar artık kaldırılabilirler.</span><span class="sxs-lookup"><span data-stu-id="46a78-133">These can now be removed.</span></span>

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
> <span data-ttu-id="46a78-134">Sürüm 1,0 RTM itibariyle, [.NET Core SDK](https://www.microsoft.com/net/download/core) artık `project.json` desteklememektedir veya Visual Studio 2015 kullanarak .NET Core uygulamaları geliştiriyor.</span><span class="sxs-lookup"><span data-stu-id="46a78-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="46a78-135">[Project. JSON 'dan csproj 'a geçiş](https://docs.microsoft.com/dotnet/articles/core/migration/)yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="46a78-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="46a78-136">Visual Studio kullanıyorsanız, [Visual studio 2017](https://www.visualstudio.com/downloads/)sürümüne yükseltmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="46a78-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="46a78-137">UWP: bağlama yeniden yönlendirmeleri ekleme</span><span class="sxs-lookup"><span data-stu-id="46a78-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="46a78-138">Evrensel Windows Platformu (UWP) projelerinde EF komutlarının çalıştırılmaya çalışılması aşağıdaki hatayla sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="46a78-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

```output
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

<span data-ttu-id="46a78-139">UWP projesine el ile bağlama yeniden yönlendirmeleri eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46a78-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="46a78-140">Proje kök klasöründe `App.config` adlı bir dosya oluşturun ve doğru derleme sürümlerine yeniden yönlendirmeler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="46a78-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

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
