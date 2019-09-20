---
title: InMemory veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 28f5f262b41cbc1f196e41d75c8b88ca60e678fe
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149242"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="fc2ce-102">Bellek Içi veritabanı sağlayıcısını EF Core</span><span class="sxs-lookup"><span data-stu-id="fc2ce-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="fc2ce-103">Bu veritabanı sağlayıcısı, Entity Framework Core bellek içi veritabanıyla birlikte kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc2ce-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="fc2ce-104">Bu, sınama için faydalı olabilir, ancak bellek içi moddaki SQLite sağlayıcı, ilişkisel veritabanları için daha uygun bir test değişikliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc2ce-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="fc2ce-105">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="fc2ce-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="fc2ce-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="fc2ce-106">Install</span></span>

<span data-ttu-id="fc2ce-107">[Microsoft. EntityFrameworkCore. InMemory NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)yükler.</span><span class="sxs-lookup"><span data-stu-id="fc2ce-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="fc2ce-108">Başlarken</span><span class="sxs-lookup"><span data-stu-id="fc2ce-108">Get Started</span></span>

<span data-ttu-id="fc2ce-109">Aşağıdaki kaynaklar, bu sağlayıcıyı kullanmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fc2ce-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="fc2ce-110">InMemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="fc2ce-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="fc2ce-111">UnicornStore örnek uygulama testleri</span><span class="sxs-lookup"><span data-stu-id="fc2ce-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="fc2ce-112">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="fc2ce-112">Supported Database Engines</span></span>

* <span data-ttu-id="fc2ce-113">Yerleşik bellek içi veritabanı (yalnızca test amaçlı olarak tasarlanmıştır)</span><span class="sxs-lookup"><span data-stu-id="fc2ce-113">Built-in in-memory database (designed for testing purposes only)</span></span>
