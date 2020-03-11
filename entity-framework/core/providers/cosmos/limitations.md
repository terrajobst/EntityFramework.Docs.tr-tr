---
title: Azure Cosmos DB sağlayıcı-sınırlamalar-EF Core
description: Entity Framework Core Azure Cosmos DB sağlayıcının sınırlamaları
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417211"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>EF Core Azure Cosmos DB sağlayıcısı sınırlamaları

Cosmos sağlayıcısında bir dizi sınırlama vardır. Bu sınırlamaların birçoğu, temel Cosmos veritabanı altyapısındaki kısıtlamaların bir sonucudur ve EF 'e özgü değildir. Ancak [henüz uygulanmadı](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Geçici sınırlamalar

- Devralma olmadan yalnızca bir varlık türü olsa bile, bir kapsayıcıya eşlenmiş bir Ayrıştırıcı özelliği hala var.
- Bölüm anahtarları olan varlık türleri bazı senaryolarda düzgün çalışmıyor
- `Include` çağrıları desteklenmez
- `Join` çağrıları desteklenmez

## <a name="azure-cosmos-db-sdk-limitations"></a>Azure Cosmos DB SDK sınırlamaları

- Yalnızca zaman uyumsuz yöntemler sağlanır

> [!WARNING]
> Düşük düzey yöntemlerin EF Core eşitleme sürümü olmadığından, bu işlev şu anda döndürülen `Task``.Wait()` çağırarak uygulanır. Yani, zaman uyumsuz karşılıkları yerine `SaveChanges`veya `ToList` gibi yöntemlerin kullanılması uygulamanızdaki bir kilitlenmeyle sonuçlanabilir

## <a name="azure-cosmos-db-limitations"></a>Azure Cosmos DB Sınırlamaları

[Desteklenen Azure Cosmos DB özelliklere](/azure/cosmos-db/modeling-data)genel bakış ' ı görebilirsiniz, bunlar ilişkisel veritabanıyla karşılaştırıldığında en önemli farklardır:

- İstemci tarafından başlatılan işlemler desteklenmez
- Bazı çapraz bölüm sorguları, dahil edilen işleçlere bağlı olarak desteklenmez veya çok daha yavaştır
