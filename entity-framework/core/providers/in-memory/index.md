---
title: InMemory veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 2aed8db79157f994bcc259acdf1f7c161017a50a
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824394"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="540f7-102">Bellek Içi veritabanı sağlayıcısını EF Core</span><span class="sxs-lookup"><span data-stu-id="540f7-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="540f7-103">Bu veritabanı sağlayıcısı, Entity Framework Core bellek içi veritabanıyla birlikte kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="540f7-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="540f7-104">Bu, sınama için faydalı olabilir, ancak bellek içi moddaki SQLite sağlayıcı, ilişkisel veritabanları için daha uygun bir test değişikliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="540f7-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="540f7-105">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="540f7-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="540f7-106">yükleme</span><span class="sxs-lookup"><span data-stu-id="540f7-106">Install</span></span>

<span data-ttu-id="540f7-107">[Microsoft. EntityFrameworkCore. InMemory NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)yükler.</span><span class="sxs-lookup"><span data-stu-id="540f7-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="540f7-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="540f7-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="540f7-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f7-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="540f7-110">Başlarken</span><span class="sxs-lookup"><span data-stu-id="540f7-110">Get Started</span></span>

<span data-ttu-id="540f7-111">Aşağıdaki kaynaklar, bu sağlayıcıyı kullanmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="540f7-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="540f7-112">InMemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="540f7-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="540f7-113">UnicornStore örnek uygulama testleri</span><span class="sxs-lookup"><span data-stu-id="540f7-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="540f7-114">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="540f7-114">Supported Database Engines</span></span>

<span data-ttu-id="540f7-115">İşlem içi bellek veritabanı (yalnızca test amaçlı olarak tasarlanmıştır)</span><span class="sxs-lookup"><span data-stu-id="540f7-115">In-process memory database (designed for testing purposes only)</span></span>
