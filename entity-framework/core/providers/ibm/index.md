---
title: "IBM veri sunucu (DB2) veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="a4b90-102">IBM veri sunucu (DB2) EF çekirdek veritabanı sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="a4b90-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="a4b90-103">Bu veritabanı sağlayıcısı IBM veri sunucusu ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4b90-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="a4b90-104">Sağlayıcı IBM tarafından korunur.</span><span class="sxs-lookup"><span data-stu-id="a4b90-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="a4b90-105">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="a4b90-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="a4b90-106">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="a4b90-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="a4b90-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="a4b90-107">Install</span></span>

<span data-ttu-id="a4b90-108">IBM veri sunucusuyla Windows üzerinde çalışacak şekilde yükleme [IBM. EntityFrameworkCore NuGet paketi](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="a4b90-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="a4b90-109">IBM veri sunucu Linux üzerinde çalışmak için yükleme [IBM. EntityFrameworkCore lnx NuGet paketi](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="a4b90-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="a4b90-110">Başlarken</span><span class="sxs-lookup"><span data-stu-id="a4b90-110">Get Started</span></span>

[<span data-ttu-id="a4b90-111">.NET Core için IBM .NET sağlayıcısını kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a4b90-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="a4b90-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="a4b90-112">Supported Database Engines</span></span>

* <span data-ttu-id="a4b90-113">zOS</span><span class="sxs-lookup"><span data-stu-id="a4b90-113">zOS</span></span>
* <span data-ttu-id="a4b90-114">IBM dashDB dahil olmak üzere LUW</span><span class="sxs-lookup"><span data-stu-id="a4b90-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="a4b90-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="a4b90-115">IBM I</span></span>
* <span data-ttu-id="a4b90-116">Informix</span><span class="sxs-lookup"><span data-stu-id="a4b90-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a4b90-117">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="a4b90-117">Supported Platforms</span></span>

* <span data-ttu-id="a4b90-118">.NET framework (veya sonraki sürümleri 4.6)</span><span class="sxs-lookup"><span data-stu-id="a4b90-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="a4b90-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4b90-119">.NET Core</span></span>
