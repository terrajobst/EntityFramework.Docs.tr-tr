---
title: SQLite veritabanı sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994008"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="03654-102">SQLite EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="03654-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="03654-103">Bu veritabanı sağlayıcısı SQLite ile kullanılacak Entity Framework Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="03654-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="03654-104">Sağlayıcı bir parçası olarak korunur [Entity Framework Core proje](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="03654-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="03654-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="03654-105">Install</span></span>

<span data-ttu-id="03654-106">Yükleme [Microsoft.EntityFrameworkCore.Sqlite NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="03654-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="03654-107">Başlarken</span><span class="sxs-lookup"><span data-stu-id="03654-107">Get Started</span></span>

<span data-ttu-id="03654-108">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="03654-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="03654-109">UWP üzerinde yerel bir SQLite</span><span class="sxs-lookup"><span data-stu-id="03654-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="03654-110">.NET core uygulaması için yeni bir SQLite veritabanı</span><span class="sxs-lookup"><span data-stu-id="03654-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="03654-111">Unicorn Clicker örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="03654-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="03654-112">Unicorn Packer örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="03654-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="03654-113">Veritabanı altyapıları desteklenir</span><span class="sxs-lookup"><span data-stu-id="03654-113">Supported Database Engines</span></span>

* <span data-ttu-id="03654-114">SQLite (3.7 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="03654-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="03654-115">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="03654-115">Supported Platforms</span></span>

* <span data-ttu-id="03654-116">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="03654-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="03654-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="03654-117">.NET Core</span></span>

* <span data-ttu-id="03654-118">Mono (4.2.0 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="03654-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="03654-119">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="03654-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="03654-120">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="03654-120">Limitations</span></span>

<span data-ttu-id="03654-121">Bkz: [SQLite sınırlamaları](limitations.md) bazılarında önemli sınırlamalar SQLite sağlayıcısı için.</span><span class="sxs-lookup"><span data-stu-id="03654-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
