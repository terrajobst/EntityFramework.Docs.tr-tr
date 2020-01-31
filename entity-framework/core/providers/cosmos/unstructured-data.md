---
title: Azure Cosmos DB sağlayıcı-yapılandırılmamış verilerle çalışma-EF Core
description: Entity Framework Core kullanarak Azure Cosmos DB yapılandırılmamış verilerle çalışma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888102"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="b1c6f-103">EF Core Azure Cosmos DB sağlayıcısında yapılandırılmamış verilerle çalışma</span><span class="sxs-lookup"><span data-stu-id="b1c6f-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="b1c6f-104">EF Core modelde tanımlanan bir şemayı izleyen verilerle çalışmayı kolaylaştırmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="b1c6f-105">Ancak Azure Cosmos DB güçlerinden biri, depolanan verilerin şeklinin esnekliğinden biridir.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="b1c6f-106">Ham JSON 'a erişme</span><span class="sxs-lookup"><span data-stu-id="b1c6f-106">Accessing the raw JSON</span></span>

<span data-ttu-id="b1c6f-107">Mağaza 'dan alınan verileri ve depolanacak verileri temsil eden bir `JObject` içeren `"__jObject"` adlı [Gölge durumda](../../modeling/shadow-properties.md) EF Core tarafından izlenmeyen özelliklere erişmek mümkündür:</span><span class="sxs-lookup"><span data-stu-id="b1c6f-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="b1c6f-108">`"__jObject"` özelliği EF Core altyapısının bir parçasıdır ve yalnızca gelecekteki sürümlerde farklı davranışları olması olası bir son çare olarak kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="b1c6f-109">Varlıkta yapılan değişiklikler, `SaveChanges`sırasında `"__jObject"` depolanan değerleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="b1c6f-110">CosmosClient kullanma</span><span class="sxs-lookup"><span data-stu-id="b1c6f-110">Using CosmosClient</span></span>

<span data-ttu-id="b1c6f-111">EF Core tamamen ayırmak için, `DbContext`[Azure Cosmos DB SDK 'sının parçası](/azure/cosmos-db/sql-api-get-started) olan [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) nesnesini alın:</span><span class="sxs-lookup"><span data-stu-id="b1c6f-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="b1c6f-112">Özellik değerleri eksik</span><span class="sxs-lookup"><span data-stu-id="b1c6f-112">Missing property values</span></span>

<span data-ttu-id="b1c6f-113">Önceki örnekte `"TrackingNumber"` özelliğini siparişten kaldırdık.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="b1c6f-114">Dizin oluşturmanın Cosmos DB çalışma biçimi nedeniyle, projeksiyondan farklı bir yerde eksik özelliğe başvuruda bulunan sorgular beklenmeyen sonuçlar döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="b1c6f-115">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b1c6f-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="b1c6f-116">Sıralanmış sorgu aslında sonuç döndürmez.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="b1c6f-117">Bu, bir tane, mağaza ile doğrudan çalışırken EF Core tarafından eşlenen özelliklerin her zaman doldurulmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="b1c6f-118">Bu davranış, Cosmos 'nin gelecek sürümlerinde değişebilir.</span><span class="sxs-lookup"><span data-stu-id="b1c6f-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="b1c6f-119">Örneğin, şu anda dizin oluşturma ilkesi Birleşik Dizin {ID/? ' i tanımlıyorsa</span><span class="sxs-lookup"><span data-stu-id="b1c6f-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="b1c6f-120">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="b1c6f-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="b1c6f-121">ASC)}, sonra ' ORDER BY c.Id ASC, c. ayrıştırıcı ASC ' olan bir sorgu `"TrackingNumber"` özelliği eksik olan __öğeleri döndürür.__</span><span class="sxs-lookup"><span data-stu-id="b1c6f-121">ASC)}, then a query that has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
