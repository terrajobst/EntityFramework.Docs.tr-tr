---
title: EF Core ve EF6 Karşılaştırması
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997130"
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="e76ab-102">EF Core ve EF6 Karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="e76ab-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="e76ab-103">Entity Framework, Entity Framework Core ve Entity Framework 6 farklı iki sürümü vardır.</span><span class="sxs-lookup"><span data-stu-id="e76ab-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="e76ab-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e76ab-104">Entity Framework 6</span></span>

<span data-ttu-id="e76ab-105">Entity Framework 6 (EF6), yıllar boyunca özellikleri ve sabitleme veri erişimi teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="e76ab-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="e76ab-106">Bu ilk 2008'de, .NET Framework 3.5 SP1 ve Visual Studio 2008 SP1'in bir parçası olarak yayımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e76ab-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="e76ab-107">Sevk olarak EF4.1 sürüm ile başlayarak [EntityFramework NuGet paketini](https://www.nuget.org/packages/EntityFramework/) -en popüler paketleri NuGet.org üzerinde şu anda biri.</span><span class="sxs-lookup"><span data-stu-id="e76ab-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="e76ab-108">EF6 desteklenen bir ürün olmaya devam eder ve hata düzeltmeleri ve süre için küçük iyileştirmeler görmeye devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="e76ab-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="e76ab-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e76ab-109">Entity Framework Core</span></span>

<span data-ttu-id="e76ab-110">Entity Framework Core (EF Core), Entity Framework'ün hafif, genişletilebilir ve platformlar arası bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="e76ab-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="e76ab-111">EF Core birçok geliştirme ve EF6 ile karşılaştırıldığında yeni özellikleri tanıtır.</span><span class="sxs-lookup"><span data-stu-id="e76ab-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="e76ab-112">Aynı zamanda, EF Core bir yeni temel ve EF6 olarak kadar olgun koddur.</span><span class="sxs-lookup"><span data-stu-id="e76ab-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="e76ab-113">EF Core Geliştirici deneyimini EF6'dan tutar ve EF Core EF6 kullanmış istemeyenler için çok tanıdık gelmeli şekilde en üst düzey API'lerinin aynı de kalır.</span><span class="sxs-lookup"><span data-stu-id="e76ab-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="e76ab-114">Aynı zamanda, EF Core temel bileşenleri tamamen yeni bir küme üzerinde oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e76ab-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="e76ab-115">Başka bir deyişle, EF Core EF6'dan tüm özelliklerini otomatik olarak devralmaz.</span><span class="sxs-lookup"><span data-stu-id="e76ab-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="e76ab-116">Gelecek sürümlerde bu özelliklerin bazıları gösterilir, ancak daha az kullanılan diğer özellikleri de EF Core uygulanan değil.</span><span class="sxs-lookup"><span data-stu-id="e76ab-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="e76ab-117">Yeni, genişletilebilir ve basit çekirdek ayrıca bazı özellikler EF6 ' (örneğin, alternatif anahtarlar, toplu güncelleştirmeler ve karma istemci/veritabanı değerlendirme LINQ sorgularında) gerçekleştirilmez EF Core eklemek etmemizi de sağladı.</span><span class="sxs-lookup"><span data-stu-id="e76ab-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
