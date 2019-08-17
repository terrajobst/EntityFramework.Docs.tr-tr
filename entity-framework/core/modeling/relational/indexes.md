---
title: Dizinler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: dfada7446f812f3c277572cc1338441272e8f448
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565362"
---
# <a name="indexes"></a>Dizinlerde

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

İlişkisel veritabanındaki bir dizin, Entity Framework çekirdekdeki bir dizinle aynı kavram ile eşlenir.

## <a name="conventions"></a>Kurallar

Kurala göre, dizinler adlandırılır `IX_<type name>_<property name>`. Bileşik dizinler `<property name>` için özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.

## <a name="data-annotations"></a>Veri açıklamaları

Dizinler, veri açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Bir dizinin adını yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Ayrıca, bir filtre de belirtebilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

SQL Server sağlayıcısı EF kullanıldığında, benzersiz bir dizinin parçası olan tüm null yapılabilen sütunlar için bir ' NOT NULL ' filtresi ekler. Bu kuralı geçersiz kılmak için bir `null` değer sağlayabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>SQL Server dizinlerine sütun Ekle

Sorgudaki tüm sütunlar dizine anahtar veya anahtar olmayan sütunlar olarak dahil edildiğinde sorgu performansını önemli ölçüde artırmak için, [dahil edilen sütunlara sahip dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) yapılandırabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/ForSqlServerHasIndex.cs?name=Model)]
