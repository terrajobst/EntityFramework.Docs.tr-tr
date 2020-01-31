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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB sağlayıcısında yapılandırılmamış verilerle çalışma

EF Core modelde tanımlanan bir şemayı izleyen verilerle çalışmayı kolaylaştırmak için tasarlanmıştır. Ancak Azure Cosmos DB güçlerinden biri, depolanan verilerin şeklinin esnekliğinden biridir.

## <a name="accessing-the-raw-json"></a>Ham JSON 'a erişme

Mağaza 'dan alınan verileri ve depolanacak verileri temsil eden bir `JObject` içeren `"__jObject"` adlı [Gölge durumda](../../modeling/shadow-properties.md) EF Core tarafından izlenmeyen özelliklere erişmek mümkündür:

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
> `"__jObject"` özelliği EF Core altyapısının bir parçasıdır ve yalnızca gelecekteki sürümlerde farklı davranışları olması olası bir son çare olarak kullanılmalıdır.

> [!NOTE]
> Varlıkta yapılan değişiklikler, `SaveChanges`sırasında `"__jObject"` depolanan değerleri geçersiz kılar.

## <a name="using-cosmosclient"></a>CosmosClient kullanma

EF Core tamamen ayırmak için, `DbContext`[Azure Cosmos DB SDK 'sının parçası](/azure/cosmos-db/sql-api-get-started) olan [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) nesnesini alın:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Özellik değerleri eksik

Önceki örnekte `"TrackingNumber"` özelliğini siparişten kaldırdık. Dizin oluşturmanın Cosmos DB çalışma biçimi nedeniyle, projeksiyondan farklı bir yerde eksik özelliğe başvuruda bulunan sorgular beklenmeyen sonuçlar döndürebilir. Örneğin:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Sıralanmış sorgu aslında sonuç döndürmez. Bu, bir tane, mağaza ile doğrudan çalışırken EF Core tarafından eşlenen özelliklerin her zaman doldurulmasıdır.

> [!NOTE]
> Bu davranış, Cosmos 'nin gelecek sürümlerinde değişebilir. Örneğin, şu anda dizin oluşturma ilkesi Birleşik Dizin {ID/? ' i tanımlıyorsa ASC, TrackingNumber/? ASC)}, sonra ' ORDER BY c.Id ASC, c. ayrıştırıcı ASC ' olan bir sorgu `"TrackingNumber"` özelliği eksik olan __öğeleri döndürür.__
