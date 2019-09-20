---
title: Azure Cosmos DB sağlayıcı-yapılandırılmamış verilerle çalışma-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150818"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="82c54-102">EF Core Azure Cosmos DB sağlayıcısında yapılandırılmamış verilerle çalışma</span><span class="sxs-lookup"><span data-stu-id="82c54-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="82c54-103">EF Core modelde tanımlanan bir şemayı izleyen verilerle çalışmayı kolaylaştırmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="82c54-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="82c54-104">Ancak Azure Cosmos DB güçlerinden biri, depolanan verilerin şeklinin esnekliğinden biridir.</span><span class="sxs-lookup"><span data-stu-id="82c54-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="82c54-105">Ham JSON 'a erişme</span><span class="sxs-lookup"><span data-stu-id="82c54-105">Accessing the raw JSON</span></span>

<span data-ttu-id="82c54-106">Mağaza 'dan alınan verileri ve depolanacak verileri temsil eden adlı `"__jObject"` [Gölge durumundaki](../../modeling/shadow-properties.md) özel bir `JObject` özellik aracılığıyla EF Core tarafından izlenmeyen özelliklere erişmek mümkündür:</span><span class="sxs-lookup"><span data-stu-id="82c54-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
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
> <span data-ttu-id="82c54-107">`"__jObject"` Özelliği EF Core altyapısının bir parçasıdır ve bu özellik, gelecekteki sürümlerde farklı davranışa sahip olmak büyük olasılıkla son çare olarak kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="82c54-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="82c54-108">Varlıktaki değişiklikler, içinde `"__jObject"` `SaveChanges`depolanan değerleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="82c54-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="82c54-109">CosmosClient kullanma</span><span class="sxs-lookup"><span data-stu-id="82c54-109">Using CosmosClient</span></span>

<span data-ttu-id="82c54-110">EF Core tamamen [ayırmak için Azure Cosmos DB SDK 'sının parçası](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) `DbContext`olan `CosmosClient` nesneyi şuradan alın:</span><span class="sxs-lookup"><span data-stu-id="82c54-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="82c54-111">Özellik değerleri eksik</span><span class="sxs-lookup"><span data-stu-id="82c54-111">Missing property values</span></span>

<span data-ttu-id="82c54-112">Önceki örnekte, `"TrackingNumber"` özelliği siparişten kaldırdık.</span><span class="sxs-lookup"><span data-stu-id="82c54-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="82c54-113">Dizin oluşturmanın Cosmos DB çalışma biçimi nedeniyle, projeksiyondan farklı bir yerde eksik özelliğe başvuruda bulunan sorgular beklenmeyen sonuçlar döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="82c54-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="82c54-114">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="82c54-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="82c54-115">Sıralanmış sorgu aslında sonuç döndürmez.</span><span class="sxs-lookup"><span data-stu-id="82c54-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="82c54-116">Bu, bir tane, mağaza ile doğrudan çalışırken EF Core tarafından eşlenen özelliklerin her zaman doldurulmasıdır.</span><span class="sxs-lookup"><span data-stu-id="82c54-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="82c54-117">Bu davranış, Cosmos 'nin gelecek sürümlerinde değişebilir.</span><span class="sxs-lookup"><span data-stu-id="82c54-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="82c54-118">Örneğin, şu anda dizin oluşturma ilkesi Birleşik Dizin {ID/? ' i tanımlıyorsa</span><span class="sxs-lookup"><span data-stu-id="82c54-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="82c54-119">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="82c54-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="82c54-120">ASC)}, sonra ' order by c.ID ASC, c. ayrıştırıcı ASC ' olan bir sorgu, `"TrackingNumber"` özelliği eksik olan öğeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="82c54-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>