---
title: Eşzamanlılık belirteçleri - EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994232"
---
# <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri

> [!NOTE]
> Bu sayfa, eşzamanlılık belirteçleri yapılandırma belgeler. Bkz: [eşzamanlılık çakışmalarını işleme](../saving/concurrency.md) EF Core ve uygulamanızdaki eşzamanlılık çakışmalarını nasıl ele alınacağını örnekleri eşzamanlılık denetimi nasıl çalışır, ayrıntılı bir açıklama.

Eşzamanlılık belirteçleri yapılandırılmış özellikleri, iyimser eşzamanlılık denetimi uygulamak için kullanılır.

## <a name="conventions"></a>Kurallar

Kural gereği, özellikleri, hiçbir zaman eşzamanlılık belirteçleri yapılandırılır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özelliğin eşzamanlı bir simge yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir özelliğin eşzamanlı bir simge yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Zaman damgası/satır sürümü

Bir zaman damgası, yeni bir değer her zaman bir satır eklendiğinde veya veritabanı tarafından oluşturulduğu bir özelliktir. Özelliği, ayrıca bir eşzamanlılık belirteci olarak kabul edilir. Bu, başka hiç kimse verileri sorguladı güncelleştirilemiyor çalıştığınız bir satır değiştirildi, bir özel durum alırsınız sağlar.

Bunu nasıl elde edildiğini kullanılan veritabanı kadar sağlayıcısıdır. SQL Server için zaman damgası genellikle üzerinde kullanılan bir *byte []* olacağı özelliği kurulum olarak bir *ROWVERSION* veritabanındaki sütunu.

### <a name="conventions"></a>Kurallar

Kural gereği, özellikleri, hiçbir zaman zaman damgaları yapılandırılır.

### <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek olarak bir zaman damgası bir özelliğini yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent API'si

Fluent API'si olarak bir zaman damgası bir özelliğini yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
