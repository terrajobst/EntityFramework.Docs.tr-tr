---
title: "Inmemory veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="8f168-102">EF çekirdek bellek içi veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="8f168-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="8f168-103">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="8f168-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="8f168-104">Bellek içi modunda SQLite sağlayıcısı test yerine ilişkisel veritabanları için daha uygun olabilir ancak bu test etmek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8f168-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="8f168-105">Sağlayıcı bir parçası olarak korunur [Entity Framework Çekirdek proje](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="8f168-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="8f168-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="8f168-106">Install</span></span>

<span data-ttu-id="8f168-107">Yükleme [Microsoft.EntityFrameworkCore.InMemory NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="8f168-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="8f168-108">Başlarken</span><span class="sxs-lookup"><span data-stu-id="8f168-108">Get Started</span></span>

<span data-ttu-id="8f168-109">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="8f168-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="8f168-110">InMemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="8f168-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="8f168-111">UnicornStore örnek uygulaması testleri</span><span class="sxs-lookup"><span data-stu-id="8f168-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="8f168-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="8f168-112">Supported Database Engines</span></span>

* <span data-ttu-id="8f168-113">(Yalnızca sınama amacıyla tasarlanmış) yerleşik bellek içi veritabanı</span><span class="sxs-lookup"><span data-stu-id="8f168-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="8f168-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="8f168-114">Supported Platforms</span></span>

* <span data-ttu-id="8f168-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="8f168-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="8f168-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="8f168-116">.NET Core</span></span>

* <span data-ttu-id="8f168-117">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="8f168-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="8f168-118">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="8f168-118">Universal Windows Platform</span></span>
