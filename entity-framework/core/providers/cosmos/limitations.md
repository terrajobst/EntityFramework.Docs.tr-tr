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
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>EF Core Azure Cosmos DB sağlayıcısı sınırlamaları

Cosmos sağlayıcısında bir dizi sınırlama vardır. Bu sınırlamaların birçoğu, temel Cosmos veritabanı altyapısındaki kısıtlamaların bir sonucudur ve EF 'e özgü değildir. Ancak [henüz uygulanmadı](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Geçici sınırlamalar

- Devralma olmadan yalnızca bir varlık türü olsa bile, bir kapsayıcıya eşlenmiş bir Ayrıştırıcı özelliği hala var.
- Bölüm anahtarları olan varlık türleri bazı senaryolarda düzgün çalışmıyor
- `Include`çağrılar desteklenmez
- `Join`çağrılar desteklenmez

## <a name="azure-cosmos-db-sdk-limitations"></a>Azure Cosmos DB SDK sınırlamaları

- Yalnızca zaman uyumsuz yöntemler sağlanır

> [!WARNING]
> Düşük düzey yöntemlerin EF Core eşitleme sürümü olmadığından, ilgili işlev şu anda döndürülen `.Wait()` `Task`üzerinde çağırarak uygulanır. Bu, zaman uyumsuz karşılıkları yerine `SaveChanges`veya `ToList` gibi yöntemlerin kullanılması uygulamanızda bir kilitlenmeyle yol açabilecek anlamına gelir

## <a name="azure-cosmos-db-limitations"></a>Azure Cosmos DB Sınırlamaları

[Desteklenen Azure Cosmos DB özelliklere](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data)genel bakış ' ı görebilirsiniz, bunlar ilişkisel veritabanıyla karşılaştırıldığında en önemli farklardır:

- İstemci tarafından başlatılan işlemler desteklenmez
- Bazı çapraz bölüm sorguları, dahil edilen işleçlere bağlı olarak desteklenmez veya çok daha yavaştır