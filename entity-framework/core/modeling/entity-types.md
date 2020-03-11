---
title: Varlık türleri-EF Core
description: Entity Framework Core kullanarak varlık türlerini yapılandırma ve eşleme
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417235"
---
# <a name="entity-types"></a>Varlık Türleri

Bağlam üzerindeki bir DbSet türü de dahil olmak, EF Core modeline dahil olduğu anlamına gelir; genellikle *varlık*olarak böyle bir türe başvurduk. EF Core, ve veritabanından varlık örnekleri okuyup yazabilir ve ilişkisel bir veritabanı kullanıyorsanız, geçişler aracılığıyla varlıklarınız için tablolar oluşturabilir EF Core.

## <a name="including-types-in-the-model"></a>Modeldeki türleri ekleme

Kurala göre, bağlamdaki DbSet özelliklerinde sunulan türler, modele varlık olarak dahil edilir. `OnModelCreating` yönteminde belirtilen varlık türleri, diğer keşfedilen varlık türlerinin gezinti özelliklerini yinelemeli olarak inceleyerek bulunan her türlü tür olduğu gibi da dahil edilmiştir.

Aşağıdaki kod örneğinde, tüm türler dahil edilmiştir:

* `Blog`, bağlamdaki bir DbSet özelliğinde kullanıma sunulduğundan dahil edilir.
* `Post`, `Blog.Posts` gezinti özelliği aracılığıyla bulunduğundan dahil edilir.
* `OnModelCreating`belirtildiğinden `AuditEntry`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a>Modeldeki türler dışlanıyor

Modele bir türün dahil edilmesini istemiyorsanız, bu tür dışında bırakabilirsiniz:

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a>Tablo adı

Kural gereği, her varlık türü, varlığı sunan DbSet özelliğiyle aynı ada sahip bir veritabanı tablosuna eşlenecek şekilde ayarlanır. Belirtilen varlık için hiçbir DbSet yoksa, sınıf adı kullanılır.

Tablo adını el ile yapılandırabilirsiniz:

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a>Tablo şeması

İlişkisel bir veritabanı kullanılırken, tablolar veritabanınızın varsayılan şemasında oluşturulan kurala göre yapılır. Örneğin, Microsoft SQL Server `dbo` şemasını kullanır (SQLite şemaları desteklemez).

Belirli bir şemada oluşturulacak tabloları aşağıdaki gibi yapılandırabilirsiniz:

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

Her tablo için şema belirtmek yerine, Fluent API model düzeyinde varsayılan şemayı de tanımlayabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

Varsayılan şemanın ayarlanması, diziler gibi diğer veritabanı nesnelerini de etkileyecek şekilde değişir.
