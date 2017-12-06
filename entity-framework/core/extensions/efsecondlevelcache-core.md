---
title: "-Araçlar ve uzantılar - EFSecondLevelCache.Core EF çekirdek"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: 6021D246-151D-4634-B0CB-413BAAA82D17
ms.technology: entity-framework-core
uid: core/extensions/efsecondlevelcache-core
ms.openlocfilehash: d54a770bae7c769613cfc14306880a28e923cb05
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="efsecondlevelcachecore-extension"></a><span data-ttu-id="e11d3-102">EFSecondLevelCache.Core uzantısı</span><span class="sxs-lookup"><span data-stu-id="e11d3-102">EFSecondLevelCache.Core Extension</span></span>

> [!NOTE]  
> <span data-ttu-id="e11d3-103">Bu uzantı Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="e11d3-103">This extension is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="e11d3-104">Bir üçüncü taraf uzantı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="e11d3-104">When considering a third party extension, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

<span data-ttu-id="e11d3-105">İkinci düzey önbelleğe alma kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="e11d3-105">Second Level Caching Library.</span></span> <span data-ttu-id="e11d3-106">İkinci düzey önbelleğe alma sorgusu bir önbelleğidir.</span><span class="sxs-lookup"><span data-stu-id="e11d3-106">Second level caching is a query cache.</span></span> <span data-ttu-id="e11d3-107">EF komutları sonuçlarını önbelleğinde aynı EF komutlar kendi veri önbelleği yerine yeniden veritabanına karşı çalıştırma alacak depolanan.</span><span class="sxs-lookup"><span data-stu-id="e11d3-107">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

<span data-ttu-id="e11d3-108">Aşağıdaki kaynak EFSecondLevelCache.Core ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="e11d3-108">The following resource will help you get started with EFSecondLevelCache.Core.</span></span>
* [<span data-ttu-id="e11d3-109">EFSecondLevelCache.Core GitHub deposunu</span><span class="sxs-lookup"><span data-stu-id="e11d3-109">EFSecondLevelCache.Core GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)
