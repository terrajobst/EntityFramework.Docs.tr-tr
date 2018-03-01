---
title: "FirebirdSQL veritabanı sağlayıcısı - EF çekirdek"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="35b72-102">Firebird EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="35b72-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="35b72-103">Bu veritabanı sağlayıcısı ile FirebirdSQL kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="35b72-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="35b72-104">Sağlayıcı bir parçası olarak korunur [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub proje](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="35b72-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="35b72-105">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="35b72-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="35b72-106">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="35b72-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="35b72-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="35b72-107">Install</span></span>

<span data-ttu-id="35b72-108">Yükleme [EntityFrameworkCore.FirebirdSQL NuGet paketi](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="35b72-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="35b72-109">Başlarken</span><span class="sxs-lookup"><span data-stu-id="35b72-109">Get Started</span></span>

<span data-ttu-id="35b72-110">Bkz: [Başlarken belgeleri proje sitesinde](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span><span class="sxs-lookup"><span data-stu-id="35b72-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="35b72-111">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="35b72-111">Supported Database Engines</span></span>

* <span data-ttu-id="35b72-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="35b72-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="35b72-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="35b72-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="35b72-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="35b72-114">Supported Platforms</span></span>

* <span data-ttu-id="35b72-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="35b72-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="35b72-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="35b72-116">.NET Core</span></span>

* <span data-ttu-id="35b72-117">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="35b72-117">Mono (4.2.0 onwards)</span></span>
