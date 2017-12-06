---
title: "Microsoft SQL Server Compact Edition veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="900db-102">Microsoft SQL Server Compact Edition EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="900db-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="900db-103">Bu veritabanı sağlayıcısı, SQL Server Compact Edition ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="900db-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="900db-104">Sağlayıcı bir parçası olarak korunur [ErikEJ/EntityFramework.SqlServerCompact GitHub proje](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="900db-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="900db-105">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="900db-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="900db-106">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="900db-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="900db-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="900db-107">Install</span></span>

<span data-ttu-id="900db-108">SQL Server Compact Edition 4.0 ile çalışmak için yükleme [EntityFrameworkCore.SqlServerCompact40 NuGet paketi](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="900db-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="900db-109">SQL Server Compact Edition 3.5 ile çalışmak için yükleme [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="900db-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="900db-110">Başlarken</span><span class="sxs-lookup"><span data-stu-id="900db-110">Get Started</span></span>

<span data-ttu-id="900db-111">Bkz: [Başlarken belgeleri proje sitesinde](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span><span class="sxs-lookup"><span data-stu-id="900db-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="900db-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="900db-112">Supported Database Engines</span></span>

* <span data-ttu-id="900db-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="900db-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="900db-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="900db-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="900db-115">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="900db-115">Supported Platforms</span></span>

* <span data-ttu-id="900db-116">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="900db-116">.NET Framework (4.5.1 onwards)</span></span>
