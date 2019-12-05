---
title: Dizinler (Ilişkisel veritabanı)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824569"
---
# <a name="indexes-relational-database"></a>Dizinler (Ilişkisel veritabanı)

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

İlişkisel veritabanındaki bir dizin, Entity Framework çekirdekdeki bir dizinle aynı kavram ile eşlenir.

## <a name="conventions"></a>Kurallar

Kurala göre, dizinler `IX_<type name>_<property name>`olarak adlandırılır. Bileşik dizinler için `<property name>`, özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.

## <a name="data-annotations"></a>Veri Açıklamaları

Dizinler, veri açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Bir dizinin adını yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Ayrıca, bir filtre de belirtebilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

SQL Server sağlayıcısı EF kullanıldığında, benzersiz bir dizinin parçası olan tüm null yapılabilen sütunlar için bir `'IS NOT NULL'` filtresi ekler. Bu kuralı geçersiz kılmak için `null` bir değer sağlayabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>SQL Server dizinlerine sütun Ekle

Sorgudaki tüm sütunlar dizine anahtar veya anahtar olmayan sütunlar olarak dahil edildiğinde sorgu performansını önemli ölçüde artırmak için, [dahil edilen sütunlara sahip dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) yapılandırabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
