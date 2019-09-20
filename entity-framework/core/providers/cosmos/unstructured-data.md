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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB sağlayıcısında yapılandırılmamış verilerle çalışma

EF Core modelde tanımlanan bir şemayı izleyen verilerle çalışmayı kolaylaştırmak için tasarlanmıştır. Ancak Azure Cosmos DB güçlerinden biri, depolanan verilerin şeklinin esnekliğinden biridir.

## <a name="accessing-the-raw-json"></a>Ham JSON 'a erişme

Mağaza 'dan alınan verileri ve depolanacak verileri temsil eden adlı `"__jObject"` [Gölge durumundaki](../../modeling/shadow-properties.md) özel bir `JObject` özellik aracılığıyla EF Core tarafından izlenmeyen özelliklere erişmek mümkündür:

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
> `"__jObject"` Özelliği EF Core altyapısının bir parçasıdır ve bu özellik, gelecekteki sürümlerde farklı davranışa sahip olmak büyük olasılıkla son çare olarak kullanılmalıdır.

> [!NOTE]
> Varlıktaki değişiklikler, içinde `"__jObject"` `SaveChanges`depolanan değerleri geçersiz kılar.

## <a name="using-cosmosclient"></a>CosmosClient kullanma

EF Core tamamen [ayırmak için Azure Cosmos DB SDK 'sının parçası](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) `DbContext`olan `CosmosClient` nesneyi şuradan alın:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Özellik değerleri eksik

Önceki örnekte, `"TrackingNumber"` özelliği siparişten kaldırdık. Dizin oluşturmanın Cosmos DB çalışma biçimi nedeniyle, projeksiyondan farklı bir yerde eksik özelliğe başvuruda bulunan sorgular beklenmeyen sonuçlar döndürebilir. Örneğin:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Sıralanmış sorgu aslında sonuç döndürmez. Bu, bir tane, mağaza ile doğrudan çalışırken EF Core tarafından eşlenen özelliklerin her zaman doldurulmasıdır.

> [!NOTE]
> Bu davranış, Cosmos 'nin gelecek sürümlerinde değişebilir. Örneğin, şu anda dizin oluşturma ilkesi Birleşik Dizin {ID/? ' i tanımlıyorsa ASC, TrackingNumber/? ASC)}, sonra ' order by c.ID ASC, c. ayrıştırıcı ASC ' olan bir sorgu, `"TrackingNumber"` özelliği eksik olan öğeleri döndürür.