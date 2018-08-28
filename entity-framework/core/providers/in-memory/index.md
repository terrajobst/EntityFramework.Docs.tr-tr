---
title: Inmemory veritabanı sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993326"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="08ff8-102">EF Core bellek içi veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="08ff8-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="08ff8-103">Bu veritabanı sağlayıcısı, bir bellek içi veritabanı ile kullanılacak Entity Framework Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="08ff8-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="08ff8-104">Bellek içi modunda SQLite sağlayıcısı test yerine ilişkisel veritabanları için daha uygun olabilir ancak bu test etmek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="08ff8-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="08ff8-105">Sağlayıcı bir parçası olarak korunur [Entity Framework Core projesi](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="08ff8-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="08ff8-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="08ff8-106">Install</span></span>

<span data-ttu-id="08ff8-107">Yükleme [Microsoft.EntityFrameworkCore.InMemory NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="08ff8-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="08ff8-108">Başlarken</span><span class="sxs-lookup"><span data-stu-id="08ff8-108">Get Started</span></span>

<span data-ttu-id="08ff8-109">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="08ff8-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="08ff8-110">InMemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="08ff8-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="08ff8-111">UnicornStore örnek uygulama testi</span><span class="sxs-lookup"><span data-stu-id="08ff8-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="08ff8-112">Veritabanı altyapıları desteklenir</span><span class="sxs-lookup"><span data-stu-id="08ff8-112">Supported Database Engines</span></span>

* <span data-ttu-id="08ff8-113">Yerleşik bellek içi veritabanına (yalnızca test amacıyla tasarlanmış)</span><span class="sxs-lookup"><span data-stu-id="08ff8-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="08ff8-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="08ff8-114">Supported Platforms</span></span>

* <span data-ttu-id="08ff8-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="08ff8-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="08ff8-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="08ff8-116">.NET Core</span></span>

* <span data-ttu-id="08ff8-117">Mono (4.2.0 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="08ff8-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="08ff8-118">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="08ff8-118">Universal Windows Platform</span></span>
