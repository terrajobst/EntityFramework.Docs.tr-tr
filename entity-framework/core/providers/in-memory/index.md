---
title: "Inmemory veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="dfd67-102">EF çekirdek bellek içi veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="dfd67-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="dfd67-103">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfd67-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="dfd67-104">Bu, Entity Framework Çekirdek kullanan kodu sınarken kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="dfd67-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="dfd67-105">Sağlayıcı bir parçası olarak korunur [EntityFramework GitHub proje](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="dfd67-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="dfd67-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="dfd67-106">Install</span></span>

<span data-ttu-id="dfd67-107">Yükleme [Microsoft.EntityFrameworkCore.InMemory NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="dfd67-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="dfd67-108">Başlarken</span><span class="sxs-lookup"><span data-stu-id="dfd67-108">Get Started</span></span>

<span data-ttu-id="dfd67-109">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="dfd67-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="dfd67-110">Inmemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="dfd67-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="dfd67-111">UnicornStore örnek uygulaması testleri</span><span class="sxs-lookup"><span data-stu-id="dfd67-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="dfd67-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="dfd67-112">Supported Database Engines</span></span>

* <span data-ttu-id="dfd67-113">(Yalnızca sınama amacıyla tasarlanmış) yerleşik bellek içi veritabanı</span><span class="sxs-lookup"><span data-stu-id="dfd67-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="dfd67-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="dfd67-114">Supported Platforms</span></span>

* <span data-ttu-id="dfd67-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="dfd67-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="dfd67-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfd67-116">.NET Core</span></span>

* <span data-ttu-id="dfd67-117">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="dfd67-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="dfd67-118">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="dfd67-118">Universal Windows Platform</span></span>
