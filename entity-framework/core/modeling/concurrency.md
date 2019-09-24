---
title: Eşzamanlılık belirteçleri-EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197459"
---
# <a name="concurrency-tokens"></a>Eşzamanlılık Belirteçleri

> [!NOTE]
> Bu sayfa eşzamanlılık belirteçlerinin nasıl yapılandırılacağını belgeler. Eşzamanlılık denetiminin EF Core nasıl çalıştığı hakkında ayrıntılı bir açıklama ve uygulamanızda eşzamanlılık çakışmalarını nasıl işleyeceğinizi gösteren örnekler için bkz. [eşzamanlılık çakışmalarını işleme](../saving/concurrency.md) .

Eşzamanlılık belirteçleri olarak yapılandırılan özellikler, iyimser eşzamanlılık denetimini uygulamak için kullanılır.

## <a name="conventions"></a>Kurallar

Kurala göre, Özellikler hiçbir şekilde eşzamanlılık belirteçleri olarak yapılandırılmamıştır.

## <a name="data-annotations"></a>Veri Açıklamaları

Bir özelliği eşzamanlılık belirteci olarak yapılandırmak için veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Akıcı API

Bir özelliği eşzamanlılık belirteci olarak yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Zaman damgası/satır sürümü

Zaman damgası, her satır eklendiğinde veya güncelleştirilirken veritabanı tarafından yeni bir değerin oluşturulduğu bir özelliktir. Özelliği de eşzamanlılık belirteci olarak değerlendirilir. Bu, verileri sorguladığınız tarihten sonra güncelleştirmeye çalıştığınız bir satırı değiştirmişse bir özel durum almanızı sağlar.

Bu nasıl elde edilir, kullanılan veritabanı sağlayıcısına kadar olur. SQL Server, zaman damgası genellikle veritabanında *ROWVERSION* sütunu olarak oluşturulacak *Byte []* özelliğinde kullanılır.

### <a name="conventions"></a>Kurallar

Kurala göre, Özellikler hiçbir zaman zaman damgası olarak yapılandırılmamıştır.

### <a name="data-annotations"></a>Veri Açıklamaları

Bir özelliği zaman damgası olarak yapılandırmak için, veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Akıcı API

Bir özelliği zaman damgası olarak yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
