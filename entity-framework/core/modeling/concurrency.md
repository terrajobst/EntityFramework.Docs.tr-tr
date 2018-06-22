---
title: Eşzamanlılık belirteçleri - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
ms.locfileid: "29745499"
---
# <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri

> [!NOTE]
> Bu sayfayı eşzamanlılık belirteçleri yapılandırma konusunda belgelenmiştir. Bkz: [eşzamanlılık çakışmalarını işleme](../saving/concurrency.md) EF çekirdek ve uygulamanızda eşzamanlılık çakışmaları nasıl ele alınacağını örnekleri eşzamanlılık denetimi nasıl çalışır, ayrıntılı bir açıklama için.

Eşzamanlılık belirteçleri yapılandırılmış özellikleri iyimser eşzamanlılık denetimini uygulamak için kullanılır.

## <a name="conventions"></a>Kurallar

Kurala göre hiçbir zaman eşzamanlılık belirteçleri yapılandırılır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özelliği bir eşzamanlılık belirteci olarak yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent API'si

Bir özelliği bir eşzamanlılık belirteci olarak yapılandırmak için Fluent API kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Zaman damgası/satır sürümü

Bir zaman damgası, yeni bir değer satır eklenmesi veya güncelleştirilmesi her zaman veritabanı tarafından oluşturulduğu bir özelliktir. Özelliği, ayrıca bir eşzamanlılık belirteci olarak kabul edilir. Bu, başka birisi için verileri sorgulanan beri güncelleştirmeye çalıştığınız bir satır değiştirmişse bir özel durum alırsınız sağlar.

Bu, nasıl sağlanır kullanılan veritabanı kadar sağlayıcıdır. SQL Server için zaman damgası genellikle kullanılan bir *byte []* olacaktır özelliği kurulumunu olarak bir *ROWVERSION* veritabanındaki sütun.

### <a name="conventions"></a>Kurallar

Kurala göre hiçbir zaman zaman damgaları yapılandırılır.

### <a name="data-annotations"></a>Veri ek açıklamaları

Bir özelliği bir zaman damgası yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent API'si

Fluent API bir zaman damgası bir özelliğini yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
