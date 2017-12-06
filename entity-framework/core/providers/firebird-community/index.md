---
title: "FirebirdSQL veritabanı sağlayıcısı - EF çekirdek"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 1cbd3d3cd92bdaf8223b8821c58200bcfed10ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="82b69-102">Firebird EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="82b69-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="82b69-103">Bu veritabanı sağlayıcısı ile FirebirdSQL kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="82b69-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="82b69-104">Sağlayıcı bir parçası olarak korunur [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub proje](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="82b69-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="82b69-105">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="82b69-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="82b69-106">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="82b69-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="82b69-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="82b69-107">Install</span></span>

<span data-ttu-id="82b69-108">Yükleme [EntityFrameworkCore.FirebirdSQL NuGet paketi](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="82b69-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="82b69-109">Başlarken</span><span class="sxs-lookup"><span data-stu-id="82b69-109">Get Started</span></span>

<span data-ttu-id="82b69-110">Bkz: [Başlarken belgeleri proje sitesinde](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span><span class="sxs-lookup"><span data-stu-id="82b69-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="82b69-111">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="82b69-111">Supported Database Engines</span></span>

* <span data-ttu-id="82b69-112">FirebirdSql 2,5</span><span class="sxs-lookup"><span data-stu-id="82b69-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="82b69-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="82b69-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="82b69-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="82b69-114">Supported Platforms</span></span>

* <span data-ttu-id="82b69-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="82b69-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="82b69-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="82b69-116">.NET Core</span></span>

* <span data-ttu-id="82b69-117">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="82b69-117">Mono (4.2.0 onwards)</span></span>
