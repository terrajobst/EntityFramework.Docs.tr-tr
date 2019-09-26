---
title: Yenilikler-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: c49f4cba0066d1e218f11c3959d96f9cafa913f4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266789"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="1fe0b-102">EF6 'deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="1fe0b-102">What's new in EF6</span></span>

<span data-ttu-id="1fe0b-103">En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="1fe0b-104">Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="1fe0b-105">EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="1fe0b-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="1fe0b-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="1fe0b-106">EF 6.3.0</span></span>

<span data-ttu-id="1fe0b-107">EF 6.3.0 Runtime, Eylül 2019 ' de NuGet 'e yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="1fe0b-108">Bu sürümün ana amacı, EF 6 kullanan mevcut uygulamaların .NET Core 3,0 ' e geçirilmesini kolaylaştırmıştı.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="1fe0b-109">Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeleri de katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="1fe0b-110">Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="1fe0b-111">Burada, daha fazla bilgi yer verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="1fe0b-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="1fe0b-112">.NET Core 3,0 desteği</span><span class="sxs-lookup"><span data-stu-id="1fe0b-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="1fe0b-113">EntityFramework paketi artık .NET Framework 4. x ' e ek olarak .NET Standard 2,1 ' i hedefliyor.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="1fe0b-114">Bu, EF 6,3 ' nin platformlar arası olduğu ve Linux ve macOS gibi Windows dışındaki diğer işletim sistemlerinde desteklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="1fe0b-115">Geçişler komutları, işlem dışında yürütülecek ve SDK stili projelerle çalışacak şekilde yeniden yazıldı.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="1fe0b-116">SQL Server HierarchyId desteği</span><span class="sxs-lookup"><span data-stu-id="1fe0b-116">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="1fe0b-117">Roslyn ve NuGet PackageReference ile iyileştirilmiş uyumluluk</span><span class="sxs-lookup"><span data-stu-id="1fe0b-117">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="1fe0b-118">Derlemelerden `ef6.exe` geçişleri etkinleştirmek, eklemek, betik oluşturmak ve uygulamak için yardımcı program eklendi.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="1fe0b-119">Bunun yerini alır`migrate.exe`</span><span class="sxs-lookup"><span data-stu-id="1fe0b-119">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="1fe0b-120">EF Designer desteği</span><span class="sxs-lookup"><span data-stu-id="1fe0b-120">EF designer support</span></span>

<span data-ttu-id="1fe0b-121">Şu anda EF Designer 'ı doğrudan .NET Core veya .NET Standard projelerinde kullanma desteği yoktur.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="1fe0b-122">Aynı çözümde bir .NET Core 3,0 veya .NET Standard 2,1 projesine, öğeler için EDMX dosyasını ve üretilen sınıfları ve DbContext 'i bağlı dosyalar olarak ekleyerek bu sınırlamaya geçici bir çözüm bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="1fe0b-123">Bağlantılı dosyalar proje dosyasında şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="1fe0b-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="1fe0b-124">EDMX dosyasının EntityDeploy derleme eylemiyle bağlantılı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="1fe0b-125">Bu, EF modelinin hedef derlemeye gömülü kaynaklar olarak eklenmesi (veya EDMX 'in meta veri yapıt Işleme ayarına bağlı olarak çıktı klasörüne dosya olarak kopyalamak) için özel bir MSBuild görevi (artık EF 6,3 paketine eklenmiştir).</span><span class="sxs-lookup"><span data-stu-id="1fe0b-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="1fe0b-126">Bu kurulumu nasıl alacağınız hakkında daha fazla bilgi için bkz. [edmx .NET Core örneği](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="1fe0b-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="1fe0b-127">Geçmiş Yayınlar</span><span class="sxs-lookup"><span data-stu-id="1fe0b-127">Past Releases</span></span>

<span data-ttu-id="1fe0b-128">[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.</span><span class="sxs-lookup"><span data-stu-id="1fe0b-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
