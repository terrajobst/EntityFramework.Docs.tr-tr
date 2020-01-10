---
title: Varlık özellikleri-EF Core
description: Entity Framework Core kullanarak varlık özelliklerini yapılandırma ve eşleme
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502469"
---
# <a name="entity-properties"></a>Varlık Özellikleri

Modelinizdeki her varlık türü bir özellikler kümesine sahiptir ve bu EF Core veritabanından okur ve yazar. İlişkisel bir veritabanı kullanıyorsanız, varlık özellikleri tablo sütunlarıyla eşlenir.

## <a name="included-and-excluded-properties"></a>Dahil edilen ve dışlanan Özellikler

Kurala göre, bir alıcı ve ayarlayıcı içeren tüm ortak özellikler modele dahil edilir.

Belirli özellikler aşağıdaki gibi dışarıda bırakılabilirler:

### <a name="data-annotationstabdata-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a>Sütun adları

Kurala göre, ilişkisel bir veritabanı kullanılırken varlık özellikleri, özelliği ile aynı ada sahip tablo sütunlarına eşlenir.

Sütunlarınızı farklı adlarla yapılandırmayı tercih ediyorsanız, bunu aşağıdaki şekilde yapabilirsiniz:

### <a name="data-annotationstabdata-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a>Sütun veri türleri

İlişkisel bir veritabanı kullanılırken, veritabanı sağlayıcısı özelliğin .NET türüne göre bir veri türü seçer. Ayrıca, yapılandırılmış [Maksimum uzunluk](#maximum-length), özelliğin birincil bir anahtarın parçası olup olmadığı gibi diğer meta veriler de hesaba girer.

Örneğin, SQL Server `DateTime` özelliklerini `datetime2(7)` sütunlara ve `string` özelliklerini `nvarchar(max)` sütunlara (veya anahtar olarak kullanılan özellikler için `nvarchar(450)`) eşler.

Sütunları, sütun için tam bir veri türü belirtmek üzere de yapılandırabilirsiniz. Örneğin, aşağıdaki kod, `Url` en fazla `200` olan Unicode olmayan bir dize olarak ve `2``5` ve ölçeği ölçeklendirerek ondalık olarak `Rating`:

### <a name="data-annotationstabdata-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a>Maksimum uzunluk

En büyük uzunluk yapılandırması, belirli bir özellik için seçim yapmak üzere uygun sütun veri türü hakkında veritabanı sağlayıcısına bir ipucu sağlar. Maksimum uzunluk yalnızca `string` ve `byte[]`gibi dizi veri türleri için geçerlidir.

> [!NOTE]
> Entity Framework, verileri sağlayıcıya geçirmeden önce en fazla uzunluk doğrulaması yapmaz. Uygun olup olmadığını doğrulamak için sağlayıcıya veya veri deposuna kadar olur. Örneğin, SQL Server hedeflenirken, en fazla uzunluğu aşarak temel alınan sütunun veri türü fazla verilerin depolanmasına izin vermez.

Aşağıdaki örnekte, 500 uzunluk üst sınırını yapılandırmak SQL Server üzerinde `nvarchar(500)` türünde bir sütunun oluşturulmasına neden olur:

#### <a name="data-annotationstabdata-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a>Gerekli ve isteğe bağlı özellikler

Özelliği, `null`içermesi için geçerliyse, isteğe bağlı olarak kabul edilir. `null` bir özelliğe atanacak geçerli bir değer değilse, gerekli bir özellik olarak kabul edilir. İlişkisel bir veritabanı şemasına eşleme yaparken, gerekli özellikler null yapılamayan sütunlar olarak oluşturulur ve isteğe bağlı özellikler null yapılabilir sütunlar olarak oluşturulur.

### <a name="conventions"></a>Kurallar

Kurala göre, .NET türü null içerebilen bir özellik isteğe bağlı olarak yapılandırılır, ancak .NET türü null değer içermeyen özellikler gerekli olarak yapılandırılır. Örneğin, .NET değer türleri (`int`, `decimal`, `bool`, vb.) olan tüm özellikler gerekli olarak yapılandırılır ve null yapılabilir .NET değer türleri (`int?`, `decimal?`, `bool?`, vb.) tüm özellikler isteğe bağlı olarak yapılandırılır.

C#8, başvuru türlerinin açıklanmasına izin veren, null [olabilen başvuru](/dotnet/csharp/tutorials/nullable-reference-types)türleri olarak adlandırılan yeni bir özellik sunmuştur. Bu özellik varsayılan olarak devre dışıdır ve etkinleştirilirse, EF Core davranışını aşağıdaki şekilde değiştirir:

* Null yapılabilir başvuru türleri devre dışıysa (varsayılan), .NET başvuru türleri olan tüm özellikler, kurala göre isteğe bağlı olarak yapılandırılır (ör. `string`).
* Null yapılabilir başvuru türleri etkinleştirilirse, Özellikler .NET türlerinden C# null değer verilebilirliği temel alınarak yapılandırılır: `string?` isteğe bağlı olarak yapılandırılır, ancak `string` gerekli olarak yapılandırılır.

Aşağıdaki örnek, null olabilen başvuru özelliği devre dışı (varsayılan) ve etkin olarak, gerekli ve isteğe bağlı özelliklerle bir varlık türü gösterir.

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Nullable başvuru türleri olmadan (varsayılan)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[Null yapılabilir başvuru türleri](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Kod içinde C# ifade edilen null değer alabilme durumunu EF Core modeli ve veritabanına akıdığından ve aynı kavramı iki kez ifade etmek Için, akıcı API veya veri ek açıklamalarını obviates, Nullable başvuru türlerini kullanmak önerilir.

> [!NOTE]
> Mevcut bir projede null yapılabilir başvuru türlerini etkinleştirirken dikkatli olun: daha önce isteğe bağlı olarak yapılandırılmış olan başvuru türü özellikleri, açıkça null olarak açıklanmadığı sürece artık gerekli olarak yapılandırılır. İlişkisel bir veritabanı şemasını yönetirken, bu, veritabanı sütununun null değer alabilme durumu belirleyen geçişlerin oluşturulmasına neden olabilir.

Null yapılabilir başvuru türleri hakkında daha fazla bilgi ve bunların EF Core ile nasıl kullanılacağı hakkında daha fazla bilgi için, [Bu özellik için adanmış belgeler sayfasına bakın](xref:core/miscellaneous/nullable-reference-types).

### <a name="explicit-configuration"></a>Açık yapılandırma

Kurala göre isteğe bağlı olacak bir özellik, aşağıdaki gibi gerekli olacak şekilde yapılandırılabilir:

#### <a name="data-annotationstabdata-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
