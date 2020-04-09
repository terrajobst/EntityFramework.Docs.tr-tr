---
title: InMemory Veritabanı Sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417221"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="4afde-102">EF Core Bellek Veritabanı Sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="4afde-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="4afde-103">Bu veritabanı sağlayıcısı, Entity Framework Core'un bellek içi bir veritabanıyla kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="4afde-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="4afde-104">Bellek modundaSQLite sağlayıcısı ilişkisel veritabanları için daha uygun bir test yerine olabilir, ancak bu sınama için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4afde-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="4afde-105">Sağlayıcı, Varlık Çerçeve Çekirdek [Projesi'nin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="4afde-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="4afde-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="4afde-106">Install</span></span>

<span data-ttu-id="4afde-107">[Microsoft.EntityFrameworkCore.InMemory NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4afde-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="4afde-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4afde-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="4afde-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4afde-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="4afde-110">Başlarken</span><span class="sxs-lookup"><span data-stu-id="4afde-110">Get Started</span></span>

<span data-ttu-id="4afde-111">Aşağıdaki kaynaklar, bu sağlayıcıyla işe başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4afde-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="4afde-112">InMemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="4afde-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="4afde-113">UnicornStore Örnek Uygulama Testleri</span><span class="sxs-lookup"><span data-stu-id="4afde-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="4afde-114">Desteklenen Veritabanı Motorları</span><span class="sxs-lookup"><span data-stu-id="4afde-114">Supported Database Engines</span></span>

<span data-ttu-id="4afde-115">İşlem içi bellek veritabanı (yalnızca sınama amacıyla tasarlanmıştır)</span><span class="sxs-lookup"><span data-stu-id="4afde-115">In-process memory database (designed for testing purposes only)</span></span>
