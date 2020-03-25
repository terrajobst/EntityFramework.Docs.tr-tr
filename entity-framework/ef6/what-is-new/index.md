---
title: Yenilikler-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136133"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="d00a6-102">EF6 'deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="d00a6-102">What's new in EF6</span></span>

<span data-ttu-id="d00a6-103">En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.</span><span class="sxs-lookup"><span data-stu-id="d00a6-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="d00a6-104">Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="d00a6-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="d00a6-105">EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="d00a6-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="d00a6-106">EF 6.4.0</span><span class="sxs-lookup"><span data-stu-id="d00a6-106">EF 6.4.0</span></span>

<span data-ttu-id="d00a6-107">EF 6.4.0 Runtime, Aralık 2019 ' de NuGet 'e yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="d00a6-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="d00a6-108">EF 6,4 ' in birincil amacı, EF 6,3 ' de sunulan özellikler ve senaryolar için Lehçe ' dır.</span><span class="sxs-lookup"><span data-stu-id="d00a6-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="d00a6-109">GitHub 'daki [önemli düzeltmelerin listesini](https://github.com/dotnet/ef6/milestone/14?closed=1) görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="d00a6-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="d00a6-110">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="d00a6-110">EF 6.3.0</span></span>

<span data-ttu-id="d00a6-111">EF 6.3.0 Runtime, Eylül 2019 ' de NuGet 'e yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="d00a6-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="d00a6-112">Bu sürümün ana amacı, EF 6 kullanan mevcut uygulamaların .NET Core 3,0 ' e geçirilmesini kolaylaştırmıştı.</span><span class="sxs-lookup"><span data-stu-id="d00a6-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="d00a6-113">Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeleri de katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="d00a6-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="d00a6-114">Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın.</span><span class="sxs-lookup"><span data-stu-id="d00a6-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="d00a6-115">Burada, daha fazla bilgi yer verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d00a6-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="d00a6-116">.NET Core 3,0 desteği</span><span class="sxs-lookup"><span data-stu-id="d00a6-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="d00a6-117">EntityFramework paketi artık .NET Framework 4. x ' e ek olarak .NET Standard 2,1 ' i hedefliyor.</span><span class="sxs-lookup"><span data-stu-id="d00a6-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="d00a6-118">Bu, EF 6,3 ' nin platformlar arası olduğu ve Linux ve macOS gibi Windows dışındaki diğer işletim sistemlerinde desteklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d00a6-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="d00a6-119">Geçişler komutları, işlem dışında yürütülecek ve SDK stili projelerle çalışacak şekilde yeniden yazıldı.</span><span class="sxs-lookup"><span data-stu-id="d00a6-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="d00a6-120">SQL Server HierarchyId desteği.</span><span class="sxs-lookup"><span data-stu-id="d00a6-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="d00a6-121">Roslyn ve NuGet PackageReference ile iyileştirilmiş uyumluluk.</span><span class="sxs-lookup"><span data-stu-id="d00a6-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="d00a6-122">Derlemelerden geçişleri etkinleştirmek, eklemek, betik oluşturmak ve uygulamak için `ef6.exe` yardımcı program eklendi.</span><span class="sxs-lookup"><span data-stu-id="d00a6-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="d00a6-123">Bu `migrate.exe`yerini alır.</span><span class="sxs-lookup"><span data-stu-id="d00a6-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="d00a6-124">EF Designer desteği</span><span class="sxs-lookup"><span data-stu-id="d00a6-124">EF designer support</span></span>

<span data-ttu-id="d00a6-125">Şu anda EF Designer 'ı doğrudan .NET Core veya .NET Standard projelerinde ya da bir SDK stili .NET Framework projesinde kullanma desteği yoktur.</span><span class="sxs-lookup"><span data-stu-id="d00a6-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="d00a6-126">Aynı çözümde bir .NET Core 3,0 veya .NET Standard 2,1 projesine, öğeler için EDMX dosyasını ve üretilen sınıfları ve DbContext 'i bağlı dosyalar olarak ekleyerek bu sınırlamaya geçici bir çözüm bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d00a6-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="d00a6-127">Bağlantılı dosyalar proje dosyasında şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="d00a6-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="d00a6-128">EDMX dosyasının EntityDeploy derleme eylemiyle bağlantılı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d00a6-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="d00a6-129">Bu, EF modelinin hedef derlemeye gömülü kaynaklar olarak eklenmesi (veya EDMX 'in meta veri yapıt Işleme ayarına bağlı olarak çıktı klasörüne dosya olarak kopyalamak) için özel bir MSBuild görevi (artık EF 6,3 paketine eklenmiştir).</span><span class="sxs-lookup"><span data-stu-id="d00a6-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="d00a6-130">Bu kurulumu nasıl alacağınız hakkında daha fazla bilgi için bkz. [edmx .NET Core örneği](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="d00a6-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="d00a6-131">Uyarı: eski stilin (yani SDK olmayan), "Real". edmx dosyasını tanımlayan .NET Framework proje, bağlantıyı. sln dosyası içinde tanımlayan projeden _önce_ geldiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="d00a6-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="d00a6-132">Aksi halde, tasarımcıda. edmx dosyasını açtığınızda, "Entity Framework proje için geçerli olarak belirtilen hedef çerçevede kullanılamıyor" hata iletisini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="d00a6-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="d00a6-133">Projenin hedef çerçevesini değiştirebilir veya modeli Xmtaditor 'da düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d00a6-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="d00a6-134">Geçmiş Yayınlar</span><span class="sxs-lookup"><span data-stu-id="d00a6-134">Past Releases</span></span>

<span data-ttu-id="d00a6-135">[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.</span><span class="sxs-lookup"><span data-stu-id="d00a6-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
