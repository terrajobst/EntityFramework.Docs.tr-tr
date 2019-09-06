---
title: Yenilikler-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271439"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="3637a-102">EF6 'deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="3637a-102">What's New in EF6</span></span>

<span data-ttu-id="3637a-103">En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.</span><span class="sxs-lookup"><span data-stu-id="3637a-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="3637a-104">Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="3637a-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="3637a-105">EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="3637a-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="3637a-106">Bu sayfa, her yeni sürümde bulunan özellikleri belgeler.</span><span class="sxs-lookup"><span data-stu-id="3637a-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="3637a-107">Son yayınlar</span><span class="sxs-lookup"><span data-stu-id="3637a-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="3637a-108">Visual Studio 'da EF araçları güncelleştirmesi 2017 15,7</span><span class="sxs-lookup"><span data-stu-id="3637a-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="3637a-109">Mayıs 2018 ' de, Visual Studio 2017 15,7 kapsamında EF araçlarının güncelleştirilmiş bir sürümünü yayımladık.</span><span class="sxs-lookup"><span data-stu-id="3637a-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="3637a-110">Bu, bazı yaygın sorun noktaları için iyileştirmeler içerir:</span><span class="sxs-lookup"><span data-stu-id="3637a-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="3637a-111">Birkaç kullanıcı arabirimi erişilebilirlik hatası düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="3637a-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="3637a-112">Mevcut veritabanlarından model oluştururken SQL Server performans gerileme için geçici çözüm [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="3637a-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="3637a-113">SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185) daha büyük modellerin modellerini güncelleştirme desteği</span><span class="sxs-lookup"><span data-stu-id="3637a-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="3637a-114">EF araçları 'nın bu yeni sürümündeki diğer bir geliştirme, yeni bir projede model oluştururken EF 6,2 çalışma zamanını yüklemektedir.</span><span class="sxs-lookup"><span data-stu-id="3637a-114">Another improvement in this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="3637a-115">Visual Studio 'nun eski sürümlerinde, NuGet paketinin karşılık gelen sürümünü yükleyerek EF 6,2 çalışma zamanının (EF 'in geçmiş sürümleri) kullanılması mümkündür.</span><span class="sxs-lookup"><span data-stu-id="3637a-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="3637a-116">EF 6,2 çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="3637a-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="3637a-117">EF 6,2 çalışma zamanı, Ekim 2017 ' de NuGet 'e yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="3637a-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="3637a-118">Açık kaynak katkıda bulunanlar topluluğumuza yönelik harika bir bölümde Teşekkür ederiz, EF 6,2 birçok [hata](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) düzeltmesi ve [ürün geliştirmesi](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)içerir.</span><span class="sxs-lookup"><span data-stu-id="3637a-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="3637a-119">EF 6,2 çalışma zamanını etkileyen en önemli değişikliklerin kısa bir listesi aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="3637a-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="3637a-120">Kalıcı bir önbellekten tamamlanan kod ilk modellerini yükleyerek başlangıç süresini düşürün [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="3637a-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="3637a-121">Dizinleri tanımlamak için akıcı API [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="3637a-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="3637a-122">SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241) gıbı çeviren LINQ sorguları yazmayı etkinleştirmek Için dbfunctions. like ()</span><span class="sxs-lookup"><span data-stu-id="3637a-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="3637a-123">Migrate. exe artık-Script seçeneğini destekliyor [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="3637a-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="3637a-124">EF6, artık SQL Server bir sıra tarafından oluşturulan anahtar değerleriyle çalışabilir [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="3637a-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="3637a-125">SQL Azure yürütme stratejisi için geçici hataların listesini güncelleştirin [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="3637a-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="3637a-126">Tiva Sorgular veya SQL komutlarının yeniden denenme işlemi "SqlParameter zaten başka bir SqlParameterCollection tarafından bulunuyor" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="3637a-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="3637a-127">Tiva DbQuery. ToString (), hata ayıklayıcıda sıklıkla zaman aşımına uğruyor [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="3637a-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="3637a-128">Gelecek yayınlar</span><span class="sxs-lookup"><span data-stu-id="3637a-128">Future Releases</span></span>

<span data-ttu-id="3637a-129">EF6 'in gelecek sürümü hakkında daha fazla bilgi için lütfen [yol haritası](roadmap.md)' na bakın.</span><span class="sxs-lookup"><span data-stu-id="3637a-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="3637a-130">Geçmiş yayınlar</span><span class="sxs-lookup"><span data-stu-id="3637a-130">Past Releases</span></span>

<span data-ttu-id="3637a-131">[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.</span><span class="sxs-lookup"><span data-stu-id="3637a-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
