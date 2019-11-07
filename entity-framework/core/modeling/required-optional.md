---
title: Gerekli ve Isteğe bağlı özellikler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655658"
---
# <a name="required-and-optional-properties"></a>Gerekli ve İsteğe Bağlı Özellikler

Özelliği, `null`içermesi için geçerliyse, isteğe bağlı olarak kabul edilir. `null` bir özelliğe atanacak geçerli bir değer değilse, gerekli bir özellik olarak kabul edilir.

İlişkisel bir veritabanı şemasına eşleme yaparken, gerekli özellikler null yapılamayan sütunlar olarak oluşturulur ve isteğe bağlı özellikler null yapılabilir sütunlar olarak oluşturulur.

## <a name="conventions"></a>Kurallar

Kurala göre, .NET türü null içerebilen bir özellik isteğe bağlı olarak yapılandırılır, ancak .NET türü null değer içermeyen özellikler gerekli olarak yapılandırılır. Örneğin, .NET değer türleri (`int`, `decimal`, `bool`, vb.) olan tüm özellikler gerekli olarak yapılandırılır ve null yapılabilir .NET değer türleri (`int?`, `decimal?`, `bool?`, vb.) tüm özellikler isteğe bağlı olarak yapılandırılır.

C#8, başvuru türlerinin açıklanmasına izin veren, null [olabilen başvuru](/dotnet/csharp/tutorials/nullable-reference-types)türleri olarak adlandırılan yeni bir özellik sunmuştur. Bu özellik varsayılan olarak devre dışıdır ve etkinleştirilirse, EF Core davranışını aşağıdaki şekilde değiştirir:

* Null yapılabilir başvuru türleri devre dışıysa (varsayılan), .NET başvuru türleri olan tüm özellikler, kurala göre isteğe bağlı olarak yapılandırılır (ör. `string`).
* Null yapılabilir başvuru türleri etkinleştirilirse, Özellikler .NET türlerinden C# null değer verilebilirliği temel alınarak yapılandırılır: `string?` isteğe bağlı olarak yapılandırılır, ancak `string` gerekli olarak yapılandırılır.

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
