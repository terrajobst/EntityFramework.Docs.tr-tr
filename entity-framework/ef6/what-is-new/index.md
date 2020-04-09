---
title: Yenilikler - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136133"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="4df29-102">EF6'daki yenilikler</span><span class="sxs-lookup"><span data-stu-id="4df29-102">What's new in EF6</span></span>

<span data-ttu-id="4df29-103">En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için Entity Framework'ün en son yayımlanan sürümünü kullanmanızı şiddetle öneririz.</span><span class="sxs-lookup"><span data-stu-id="4df29-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="4df29-104">Ancak, önceki bir sürümü kullanmanız gerekebileceğini veya en son sürüm öncesi yeni geliştirmelerle denemeler yapmak isteyebileceğinibiliyoruz.</span><span class="sxs-lookup"><span data-stu-id="4df29-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="4df29-105">EF'nin belirli sürümlerini yüklemek için Varlık [Çerçevesi'ni al'a](~/ef6/fundamentals/install.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="4df29-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="4df29-106">EF 6.4.0</span><span class="sxs-lookup"><span data-stu-id="4df29-106">EF 6.4.0</span></span>

<span data-ttu-id="4df29-107">EF 6.4.0 çalışma süresi Aralık 2019'da NuGet'de yayınlandı.</span><span class="sxs-lookup"><span data-stu-id="4df29-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="4df29-108">EF 6.4'ün birincil hedefi, EF 6.3'te teslim edilen özellikleri ve senaryoları parlatmaktır.</span><span class="sxs-lookup"><span data-stu-id="4df29-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="4df29-109">Github'daki [önemli düzeltmelerin listesine](https://github.com/dotnet/ef6/milestone/14?closed=1) bakın.</span><span class="sxs-lookup"><span data-stu-id="4df29-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="4df29-110">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="4df29-110">EF 6.3.0</span></span>

<span data-ttu-id="4df29-111">EF 6.3.0 çalışma süresi Eylül 2019'da NuGet'de yayınlandı.</span><span class="sxs-lookup"><span data-stu-id="4df29-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="4df29-112">Bu sürümün temel amacı, EF 6'yı .NET Core 3.0'a geçiren varolan uygulamaların geçişini kolaylaştırmaktı.</span><span class="sxs-lookup"><span data-stu-id="4df29-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="4df29-113">Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeler katkıda bulunmuştur.</span><span class="sxs-lookup"><span data-stu-id="4df29-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="4df29-114">Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın.</span><span class="sxs-lookup"><span data-stu-id="4df29-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="4df29-115">Burada daha önemli olanlardan bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4df29-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="4df29-116">.NET Core 3.0 desteği</span><span class="sxs-lookup"><span data-stu-id="4df29-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="4df29-117">EntityFramework paketi artık .NET Framework 4.x'e ek olarak .NET Standard 2.1'i hedeflemektedir.</span><span class="sxs-lookup"><span data-stu-id="4df29-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="4df29-118">Bu, EF 6.3'ün çapraz platform olduğu ve Linux ve macOS gibi Windows'un yanı sıra diğer işletim sistemlerinde de desteklenerek desteklendirilen anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4df29-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="4df29-119">Geçiş komutları, işlem dışında yürütülmesi ve SDK tarzı projelerle çalışmak üzere yeniden yazıldı.</span><span class="sxs-lookup"><span data-stu-id="4df29-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="4df29-120">SQL Server HierarchyId desteği.</span><span class="sxs-lookup"><span data-stu-id="4df29-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="4df29-121">Roslyn ve NuGet PackageReference ile geliştirilmiş uyumluluk.</span><span class="sxs-lookup"><span data-stu-id="4df29-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="4df29-122">Derlemelerden geçişleri etkinleştirmek, eklemek, komut dosyası oluşturmak ve uygulamak için yardımcı program eklendi. `ef6.exe`</span><span class="sxs-lookup"><span data-stu-id="4df29-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="4df29-123">Bu, `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="4df29-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="4df29-124">EF tasarımcı desteği</span><span class="sxs-lookup"><span data-stu-id="4df29-124">EF designer support</span></span>

<span data-ttu-id="4df29-125">Şu anda EF tasarımcısını doğrudan .NET Core veya .NET Standard projelerinde veya SDK tarzı bir .NET Framework projesinde kullanmak için destek yoktur.</span><span class="sxs-lookup"><span data-stu-id="4df29-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="4df29-126">EdMX dosyasını ve varlıklar ve DbContext için oluşturulan sınıfları aynı çözüme bağlı bir .NET Core 3.0 veya .NET Standard 2.1 projesine bağlı dosyalar olarak ekleyerek bu sınırlamayı çözebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4df29-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="4df29-127">Bağlı dosyalar proje dosyasında aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="4df29-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="4df29-128">EDMX dosyasının EntityDeploy yapı eylemiyle bağlantılı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4df29-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="4df29-129">Bu özel bir MSBuild görev (şimdi EF 6.3 paketine dahil) gömülü kaynaklar olarak hedef derleme içine EF modeli ekleyerek ilgilenir (veya EDMX Metadata Artifact Processing ayarına bağlı olarak çıktı klasöründe dosya olarak kopyalama).</span><span class="sxs-lookup"><span data-stu-id="4df29-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="4df29-130">Bu kurulumu nasıl yaptırılabildiğini öğrenmek için [EDMX .NET Core örneğimize](https://aka.ms/EdmxDotNetCoreSample)bakın.</span><span class="sxs-lookup"><span data-stu-id="4df29-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="4df29-131">Uyarı: "Gerçek" .edmx dosyasını tanımlayan eski stilin (yani SDK tarzı olmayan) .NET Framework projesinin .sln dosyasının içindeki bağlantıyı tanımlayan projeden _önce_ geldiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="4df29-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="4df29-132">Aksi takdirde, tasarımcıda .edmx dosyasını açtığınızda, "Varlık Çerçevesi proje için şu anda belirtilen hedef çerçevede kullanılamaz" hata iletisini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4df29-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="4df29-133">Projenin hedef çerçevesini değiştirebilir veya XmlEditor'da modeli değiştirebilirsiniz".</span><span class="sxs-lookup"><span data-stu-id="4df29-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="4df29-134">Geçmiş Yayınlar</span><span class="sxs-lookup"><span data-stu-id="4df29-134">Past Releases</span></span>

<span data-ttu-id="4df29-135">[Geçmiş Sürümler](past-releases.md) sayfası, EF'nin önceki tüm sürümlerinin ve her sürümde tanıtılan önemli özelliklerin bir arşivini içerir.</span><span class="sxs-lookup"><span data-stu-id="4df29-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
