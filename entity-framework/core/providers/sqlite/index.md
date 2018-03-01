---
title: "SQLite veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="8866f-102">SQLite EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="8866f-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="8866f-103">Bu veritabanı sağlayıcısı ile SQLite kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="8866f-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="8866f-104">Sağlayıcı bir parçası olarak korunur [Entity Framework Çekirdek proje](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="8866f-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="8866f-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="8866f-105">Install</span></span>

<span data-ttu-id="8866f-106">Yükleme [Microsoft.EntityFrameworkCore.Sqlite NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="8866f-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="8866f-107">Başlarken</span><span class="sxs-lookup"><span data-stu-id="8866f-107">Get Started</span></span>

<span data-ttu-id="8866f-108">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="8866f-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="8866f-109">UWP üzerinde yerel SQLite</span><span class="sxs-lookup"><span data-stu-id="8866f-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="8866f-110">.NET core uygulamasına yeni SQLite veritabanı</span><span class="sxs-lookup"><span data-stu-id="8866f-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="8866f-111">Unicorn Clicker örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="8866f-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="8866f-112">Unicorn Packer örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="8866f-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="8866f-113">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="8866f-113">Supported Database Engines</span></span>

* <span data-ttu-id="8866f-114">SQLite (3.7 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="8866f-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="8866f-115">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="8866f-115">Supported Platforms</span></span>

* <span data-ttu-id="8866f-116">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="8866f-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="8866f-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="8866f-117">.NET Core</span></span>

* <span data-ttu-id="8866f-118">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="8866f-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="8866f-119">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="8866f-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="8866f-120">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="8866f-120">Limitations</span></span>

<span data-ttu-id="8866f-121">Bkz: [SQLite sınırlamalar](limitations.md) bazı önemli sınırlamalar SQLite sağlayıcısı için.</span><span class="sxs-lookup"><span data-stu-id="8866f-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
