---
title: Eşzamanlılık belirteçleri-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: bfeb611f222f7195fe22d920b452b40cc4addf90
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417290"
---
# <a name="concurrency-tokens"></a>Eşzamanlılık Belirteçleri

> [!NOTE]
> Bu sayfa eşzamanlılık belirteçlerinin nasıl yapılandırılacağını belgeler. Eşzamanlılık denetiminin EF Core nasıl çalıştığı hakkında ayrıntılı bir açıklama ve uygulamanızda eşzamanlılık çakışmalarını nasıl işleyeceğinizi gösteren örnekler için bkz. [eşzamanlılık çakışmalarını işleme](../saving/concurrency.md) .

Eşzamanlılık belirteçleri olarak yapılandırılan özellikler, iyimser eşzamanlılık denetimini uygulamak için kullanılır.

## <a name="configuration"></a>Yapılandırma

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Zaman damgası/rowversion

Zaman damgası/ROWVERSION, her satır eklendiğinde veya güncelleştirilirse veritabanı tarafından otomatik olarak oluşturulan yeni bir değer için bir özelliktir. Özelliği aynı zamanda bir eşzamanlılık belirteci olarak değerlendirilir ve güncelleştirmiş olduğunuz bir satır sorgulandıktan sonra değiştirilirse bir özel durum almanızı sağlar. Kesin ayrıntılar, kullanılmakta olan veritabanı sağlayıcısına bağlıdır; SQL Server için, genellikle veritabanında bir *ROWVERSION* sütunu olarak ayarlanacak bir *Byte []* özelliği kullanılır.

Bir özelliği şu şekilde bir zaman damgası/rowversion olacak şekilde yapılandırabilirsiniz:

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs?name=Timestamp&highlight=9,17)]

***
