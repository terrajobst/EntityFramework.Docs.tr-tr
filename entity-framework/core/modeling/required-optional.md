---
title: Gerekli ve Isteğe bağlı özellikler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197840"
---
# <a name="required-and-optional-properties"></a>Gerekli ve Isteğe bağlı özellikler

Özelliği, içermesi `null`için geçerliyse, isteğe bağlı olarak kabul edilir. `null` Bir özelliğe atanacak geçerli bir değer değilse, gerekli bir özellik olarak kabul edilir.

İlişkisel bir veritabanı şemasına eşleme yaparken, gerekli özellikler null yapılamayan sütunlar olarak oluşturulur ve isteğe bağlı özellikler null yapılabilir sütunlar olarak oluşturulur.

## <a name="conventions"></a>Kurallar

Kurala göre, .NET türü null içerebilen bir özellik isteğe bağlı olarak yapılandırılır, ancak .NET türü null değer içermeyen özellikler gerekli olarak yapılandırılır. Örneğin, .net değer türleri (`int`, `decimal`, `bool`vb.) olan tüm özellikler gerekli olarak yapılandırılır ve Nullable .net değer türleri (`int?`, `decimal?`, `bool?`vb.) olan tüm özellikler isteğe bağlı olarak yapılandırılır.

C#8, başvuru türlerinin açıklanmasına izin veren, null [olabilen başvuru](/dotnet/csharp/tutorials/nullable-reference-types)türleri olarak adlandırılan yeni bir özellik sunmuştur. Bu özellik varsayılan olarak devre dışıdır ve etkinleştirilirse, EF Core davranışını aşağıdaki şekilde değiştirir:

* Null yapılabilir başvuru türleri devre dışıysa (varsayılan), .NET başvuru türleri olan tüm özellikler, kurala göre isteğe bağlı olarak yapılandırılır (ör. `string`).
* Null yapılabilir başvuru türleri etkinleştirilirse, Özellikler .net türlerinden C# null değer verilebilirliği temel alınarak yapılandırılır: `string?` isteğe bağlı olarak yapılandırılır, ancak `string` gerekli olarak yapılandırılır.

Aşağıdaki örnek, null olabilen başvuru özelliği devre dışı (varsayılan) ve etkin olarak, gerekli ve isteğe bağlı özelliklerle bir varlık türü gösterir.

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Nullable başvuru türleri olmadan (varsayılan)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[Null yapılabilir başvuru türleri](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Kod içinde C# ifade edilen null değer alabilme durumunu EF Core modeli ve veritabanına akıdığından ve aynı kavramı iki kez ifade etmek Için, akıcı API veya veri ek açıklamalarını obviates, Nullable başvuru türlerini kullanmak önerilir.

> [!NOTE]
> Mevcut bir projede null yapılabilir başvuru türlerini etkinleştirirken dikkatli olun: daha önce isteğe bağlı olarak yapılandırılmış olan başvuru türü özellikleri, açıkça null olarak açıklanmadığı sürece artık gerekli olarak yapılandırılır. İlişkisel bir veritabanı şemasını yönetirken, bu, veritabanı sütununun null değer alabilme durumu belirleyen geçişlerin oluşturulmasına neden olabilir.

Null yapılabilir başvuru türleri hakkında daha fazla bilgi ve bunların EF Core ile nasıl kullanılacağı hakkında daha fazla bilgi için, [Bu özellik için adanmış belgeler sayfasına bakın](xref:core/miscellaneous/nullable-reference-types).

## <a name="configuration"></a>Yapılandırma

Kurala göre isteğe bağlı olacak bir özellik, aşağıdaki gibi gerekli olacak şekilde yapılandırılabilir:

# <a name="data-annotationstabdata-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[Akıcı API](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***