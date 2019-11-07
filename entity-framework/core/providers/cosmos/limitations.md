---
title: Azure Cosmos DB sağlayıcı-sınırlamalar-EF Core
description: Entity Framework Core Azure Cosmos DB sağlayıcının sınırlamaları
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655981"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="fb95a-103">EF Core Azure Cosmos DB sağlayıcısı sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="fb95a-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="fb95a-104">Cosmos sağlayıcısında bir dizi sınırlama vardır.</span><span class="sxs-lookup"><span data-stu-id="fb95a-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="fb95a-105">Bu sınırlamaların birçoğu, temel Cosmos veritabanı altyapısındaki kısıtlamaların bir sonucudur ve EF 'e özgü değildir.</span><span class="sxs-lookup"><span data-stu-id="fb95a-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="fb95a-106">Ancak [henüz uygulanmadı](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="fb95a-106">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="fb95a-107">Geçici sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="fb95a-107">Temporary limitations</span></span>

- <span data-ttu-id="fb95a-108">Devralma olmadan yalnızca bir varlık türü olsa bile, bir kapsayıcıya eşlenmiş bir Ayrıştırıcı özelliği hala var.</span><span class="sxs-lookup"><span data-stu-id="fb95a-108">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="fb95a-109">Bölüm anahtarları olan varlık türleri bazı senaryolarda düzgün çalışmıyor</span><span class="sxs-lookup"><span data-stu-id="fb95a-109">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="fb95a-110">`Include` çağrıları desteklenmez</span><span class="sxs-lookup"><span data-stu-id="fb95a-110">`Include` calls are not supported</span></span>
- <span data-ttu-id="fb95a-111">`Join` çağrıları desteklenmez</span><span class="sxs-lookup"><span data-stu-id="fb95a-111">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="fb95a-112">Azure Cosmos DB SDK sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="fb95a-112">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="fb95a-113">Yalnızca zaman uyumsuz yöntemler sağlanır</span><span class="sxs-lookup"><span data-stu-id="fb95a-113">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="fb95a-114">Düşük düzey yöntemlerin EF Core eşitleme sürümü olmadığından, bu işlev şu anda döndürülen `Task``.Wait()` çağırarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fb95a-114">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="fb95a-115">Yani, zaman uyumsuz karşılıkları yerine `SaveChanges`veya `ToList` gibi yöntemlerin kullanılması uygulamanızdaki bir kilitlenmeyle sonuçlanabilir</span><span class="sxs-lookup"><span data-stu-id="fb95a-115">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="fb95a-116">Azure Cosmos DB Sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="fb95a-116">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="fb95a-117">[Desteklenen Azure Cosmos DB özelliklere](/azure/cosmos-db/modeling-data)genel bakış ' ı görebilirsiniz, bunlar ilişkisel veritabanıyla karşılaştırıldığında en önemli farklardır:</span><span class="sxs-lookup"><span data-stu-id="fb95a-117">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="fb95a-118">İstemci tarafından başlatılan işlemler desteklenmez</span><span class="sxs-lookup"><span data-stu-id="fb95a-118">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="fb95a-119">Bazı çapraz bölüm sorguları, dahil edilen işleçlere bağlı olarak desteklenmez veya çok daha yavaştır</span><span class="sxs-lookup"><span data-stu-id="fb95a-119">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>
