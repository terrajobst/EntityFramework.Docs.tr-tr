---
title: Yenilikler-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: bb7038764644682c2149a8a500f342804d01f3d2
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198042"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="2ef95-102">EF6 'deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="2ef95-102">What's new in EF6</span></span>

<span data-ttu-id="2ef95-103">En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.</span><span class="sxs-lookup"><span data-stu-id="2ef95-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="2ef95-104">Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="2ef95-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="2ef95-105">EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="2ef95-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="2ef95-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="2ef95-106">EF 6.3.0</span></span>

<span data-ttu-id="2ef95-107">EF 6.3.0 Runtime, Eylül 2019 ' de NuGet 'e yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="2ef95-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="2ef95-108">Bu sürümün ana amacı, EF 6 kullanan mevcut uygulamaların .NET Core 3,0 ' e geçirilmesini kolaylaştırmıştı.</span><span class="sxs-lookup"><span data-stu-id="2ef95-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="2ef95-109">Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeleri de katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="2ef95-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="2ef95-110">Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın.</span><span class="sxs-lookup"><span data-stu-id="2ef95-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="2ef95-111">Burada, daha fazla bilgi yer verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="2ef95-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="2ef95-112">.NET Core 3,0 desteği</span><span class="sxs-lookup"><span data-stu-id="2ef95-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="2ef95-113">EntityFramework paketi artık .NET Framework 4. x ' e ek olarak .NET Standard 2,1 ' i hedefliyor</span><span class="sxs-lookup"><span data-stu-id="2ef95-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="2ef95-114">Geçişler komutları, işlem dışında yürütülecek ve SDK stili projelerle çalışacak şekilde yeniden yazıldı</span><span class="sxs-lookup"><span data-stu-id="2ef95-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="2ef95-115">SQL Server HierarchyId desteği</span><span class="sxs-lookup"><span data-stu-id="2ef95-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="2ef95-116">Roslyn ve NuGet PackageReference ile iyileştirilmiş uyumluluk</span><span class="sxs-lookup"><span data-stu-id="2ef95-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="2ef95-117">Derlemelerden `ef6.exe` geçişleri etkinleştirmek, eklemek, betik oluşturmak ve uygulamak için yardımcı program eklendi.</span><span class="sxs-lookup"><span data-stu-id="2ef95-117">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="2ef95-118">Bunun yerini alır`migrate.exe`</span><span class="sxs-lookup"><span data-stu-id="2ef95-118">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="2ef95-119">EF Designer desteği</span><span class="sxs-lookup"><span data-stu-id="2ef95-119">EF designer support</span></span>

<span data-ttu-id="2ef95-120">Şu anda EF Designer 'ı doğrudan .NET Core veya .NET Standard projelerinde kullanma desteği yoktur.</span><span class="sxs-lookup"><span data-stu-id="2ef95-120">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="2ef95-121">Aynı çözümde bir .NET Core 3,0 veya .NET Standard 2,1 projesine, öğeler için EDMX dosyasını ve üretilen sınıfları ve DbContext 'i bağlı dosyalar olarak ekleyerek bu sınırlamaya geçici bir çözüm bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ef95-121">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="2ef95-122">Bağlantılı dosyalar proje dosyasında şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="2ef95-122">The linked files will look like this in the project file:</span></span>

``` csproj 
&lt;ItemGroup&gt;
  &lt;EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" /&gt;
&lt;/ItemGroup&gt;
```

<span data-ttu-id="2ef95-123">EDMX dosyasının EntityDeploy derleme eylemiyle bağlantılı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2ef95-123">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="2ef95-124">Bu, EF modelinin hedef derlemeye gömülü kaynaklar olarak eklenmesi (veya EDMX 'in meta veri yapıt Işleme ayarına bağlı olarak çıktı klasörüne dosya olarak kopyalamak) için özel bir MSBuild görevi (artık EF 6,3 paketine eklenmiştir).</span><span class="sxs-lookup"><span data-stu-id="2ef95-124">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="2ef95-125">Bu kurulumu nasıl alacağınız hakkında daha fazla bilgi için bkz. [edmx .NET Core örneği](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="2ef95-125">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="2ef95-126">Geçmiş Yayınlar</span><span class="sxs-lookup"><span data-stu-id="2ef95-126">Past Releases</span></span>

<span data-ttu-id="2ef95-127">[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.</span><span class="sxs-lookup"><span data-stu-id="2ef95-127">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
