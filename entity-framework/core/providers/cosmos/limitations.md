---
title: Azure Cosmos DB sağlayıcı-sınırlamalar-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150811"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="87d71-102">EF Core Azure Cosmos DB sağlayıcısı sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="87d71-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="87d71-103">Cosmos sağlayıcısında bir dizi sınırlama vardır.</span><span class="sxs-lookup"><span data-stu-id="87d71-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="87d71-104">Bu sınırlamaların birçoğu, temel Cosmos veritabanı altyapısındaki kısıtlamaların bir sonucudur ve EF 'e özgü değildir.</span><span class="sxs-lookup"><span data-stu-id="87d71-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="87d71-105">Ancak [henüz uygulanmadı](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="87d71-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="87d71-106">Geçici sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="87d71-106">Temporary limitations</span></span>

- <span data-ttu-id="87d71-107">Devralma olmadan yalnızca bir varlık türü olsa bile, bir kapsayıcıya eşlenmiş bir Ayrıştırıcı özelliği hala var.</span><span class="sxs-lookup"><span data-stu-id="87d71-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="87d71-108">Bölüm anahtarları olan varlık türleri bazı senaryolarda düzgün çalışmıyor</span><span class="sxs-lookup"><span data-stu-id="87d71-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="87d71-109">`Include`çağrılar desteklenmez</span><span class="sxs-lookup"><span data-stu-id="87d71-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="87d71-110">`Join`çağrılar desteklenmez</span><span class="sxs-lookup"><span data-stu-id="87d71-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="87d71-111">Azure Cosmos DB SDK sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="87d71-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="87d71-112">Yalnızca zaman uyumsuz yöntemler sağlanır</span><span class="sxs-lookup"><span data-stu-id="87d71-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="87d71-113">Düşük düzey yöntemlerin EF Core eşitleme sürümü olmadığından, ilgili işlev şu anda döndürülen `.Wait()` `Task`üzerinde çağırarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="87d71-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="87d71-114">Bu, zaman uyumsuz karşılıkları yerine `SaveChanges`veya `ToList` gibi yöntemlerin kullanılması uygulamanızda bir kilitlenmeyle yol açabilecek anlamına gelir</span><span class="sxs-lookup"><span data-stu-id="87d71-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="87d71-115">Azure Cosmos DB Sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="87d71-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="87d71-116">[Desteklenen Azure Cosmos DB özelliklere](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data)genel bakış ' ı görebilirsiniz, bunlar ilişkisel veritabanıyla karşılaştırıldığında en önemli farklardır:</span><span class="sxs-lookup"><span data-stu-id="87d71-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="87d71-117">İstemci tarafından başlatılan işlemler desteklenmez</span><span class="sxs-lookup"><span data-stu-id="87d71-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="87d71-118">Bazı çapraz bölüm sorguları, dahil edilen işleçlere bağlı olarak desteklenmez veya çok daha yavaştır</span><span class="sxs-lookup"><span data-stu-id="87d71-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>