---
title: -Yenilikler EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: dba6403fa341e1abfe8da488a19cf8520e3ea574
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914182"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="79eac-102">EF6 yenilikler</span><span class="sxs-lookup"><span data-stu-id="79eac-102">What's New in EF6</span></span>

<span data-ttu-id="79eac-103">En son özellikleri ve kararlılığı en yüksek alma emin olmak için Entity Framework ' son yayınlanan sürümünü kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="79eac-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="79eac-104">Ancak, önceki bir sürümü kullanmanız gerekebilir veya yeni yayın öncesi en son yenilikleri denemek isteyebilirsiniz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="79eac-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="79eac-105">EF belirli sürümlerini yüklemek için bkz: [alma Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="79eac-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="79eac-106">Bu sayfa, her yeni yayın üzerinde dahil özelliklerini içermektedir.</span><span class="sxs-lookup"><span data-stu-id="79eac-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="79eac-107">Yeni sürümler</span><span class="sxs-lookup"><span data-stu-id="79eac-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="79eac-108">Visual Studio 2017 15.7 EF araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="79eac-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="79eac-109">Mayıs 2018'de Visual Studio 2017 15.7 bir parçası olarak EF Araçları'nın güncelleştirilmiş bir sürümünü çıkardık.</span><span class="sxs-lookup"><span data-stu-id="79eac-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="79eac-110">Sık karşılaşılan bazı sorunlu noktaları için geliştirmeler içerir:</span><span class="sxs-lookup"><span data-stu-id="79eac-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="79eac-111">Birkaç kullanıcı arabirimi erişilebilirliği hata düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="79eac-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="79eac-112">Geçici çözüm için mevcut veritabanlarından modelleri oluştururken, SQL Server performans regresyon [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="79eac-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="79eac-113">SQL Server'da büyük modeller için modelleri güncelleştirme desteği [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="79eac-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="79eac-114">Başka bir geliştirme bu EF Araçları'nın bu yeni sürümün bu EF 6.2 çalışma zamanı bir modeli içerisinde yeni bir proje oluştururken yüklemesidir.</span><span class="sxs-lookup"><span data-stu-id="79eac-114">Another improvement in this this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="79eac-115">Visual Studio'nun eski sürümleriyle ilgili NuGet paketi sürümünü yükleyerek EF 6.2 çalışma zamanı (aynı zamanda EF'ın önceki bir sürümü) kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="79eac-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="79eac-116">EF 6.2 çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="79eac-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="79eac-117">EF 6.2 çalışma zamanı için NuGet'ın Ekim 2017'de yayınlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="79eac-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="79eac-118">Harika, teşekkür ederiz. topluluğumuza açık kaynağa katkıda bulunanlar, çalışmalarınızda kullanmanız için bölüm, çok sayıda EF 6.2 içerir [düzeltmeleri hataları](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) ve [Ürün geliştirmeleri](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="79eac-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="79eac-119">EF 6.2 çalışma zamanı etkileyen en önemli değişikliklerin kısa bir listesi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="79eac-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="79eac-120">Başlatma süresi yükleyerek azaltmak tamamlandı ilk kod modelleri kalıcı önbellekten [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="79eac-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="79eac-121">Dizinleri tanımlamak için Fluent API'si [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="79eac-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="79eac-122">SQL BENZERİ Çevir LINQ sorguları yazma etkinleştirmek için DbFunctions.Like() [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="79eac-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="79eac-123">Migrate.exe artık destekliyor - komut dosyası seçeneği [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="79eac-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="79eac-124">EF6 bir sırada bir SQL Server tarafından oluşturulan anahtar değerleri artık çalışabilir [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="79eac-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="79eac-125">SQL Azure yürütme stratejisi için geçici hataları güncelleştirme listesi [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="79eac-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="79eac-126">Hata: "SqlParameter zaten başka bir SqlParameterCollection tarafından yer ile" sorguları veya SQL komutları yeniden deneme başarısız [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="79eac-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="79eac-127">Hata: DbQuery.ToString() değerlendirmesini sık hata ayıklayıcıda zaman aşımına [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="79eac-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="79eac-128">Gelecek sürümler</span><span class="sxs-lookup"><span data-stu-id="79eac-128">Future Releases</span></span>

<span data-ttu-id="79eac-129">EF6'ın gelecek sürümünde hakkında daha fazla bilgi için lütfen bakmak bizim [yol haritası](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="79eac-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="79eac-130">Eski sürümler</span><span class="sxs-lookup"><span data-stu-id="79eac-130">Past Releases</span></span>

<span data-ttu-id="79eac-131">[Geçmiş sunumlar](past-releases.md) sayfası bir arşiv EF ve her bir yayının sunulan büyük özelliklerin tüm önceki sürümlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="79eac-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
