---
title: "SQLite veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="e3e17-102">SQLite EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="e3e17-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="e3e17-103">Bu veritabanı sağlayıcısı ile SQLite kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e3e17-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="e3e17-104">Sağlayıcı bir parçası olarak korunur [EntityFramework GitHub proje](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="e3e17-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="e3e17-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="e3e17-105">Install</span></span>

<span data-ttu-id="e3e17-106">Yükleme [Microsoft.EntityFrameworkCore.Sqlite NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="e3e17-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="e3e17-107">Başlarken</span><span class="sxs-lookup"><span data-stu-id="e3e17-107">Get Started</span></span>

<span data-ttu-id="e3e17-108">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="e3e17-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="e3e17-109">UWP üzerinde yerel SQLite</span><span class="sxs-lookup"><span data-stu-id="e3e17-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="e3e17-110">.NET core uygulamasına yeni SQLite veritabanı</span><span class="sxs-lookup"><span data-stu-id="e3e17-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="e3e17-111">Unicorn Clicker örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="e3e17-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="e3e17-112">Unicorn Packer örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="e3e17-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="e3e17-113">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="e3e17-113">Supported Database Engines</span></span>

* <span data-ttu-id="e3e17-114">SQLite (3.7 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="e3e17-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e3e17-115">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="e3e17-115">Supported Platforms</span></span>

* <span data-ttu-id="e3e17-116">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="e3e17-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="e3e17-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3e17-117">.NET Core</span></span>

* <span data-ttu-id="e3e17-118">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="e3e17-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="e3e17-119">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="e3e17-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="e3e17-120">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="e3e17-120">Limitations</span></span>

<span data-ttu-id="e3e17-121">Bkz: [SQLite sınırlamalar](limitations.md) bazı önemli sınırlamalar SQLite sağlayıcısı için.</span><span class="sxs-lookup"><span data-stu-id="e3e17-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
