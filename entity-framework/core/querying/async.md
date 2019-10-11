---
title: Zaman uyumsuz sorgular-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181818"
---
# <a name="asynchronous-queries"></a>Zaman Uyumsuz Sorgular

Zaman uyumsuz sorgular, sorgu veritabanında yürütüldüğü sırada bir iş parçacığının engellenmesini önler. Zaman uyumsuz sorgular, çok duyarlı bir kullanıcı arabirimini kalın istemci uygulamalarında tutmak için önemlidir. Ayrıca, Web uygulamalarındaki diğer isteklere hizmet vermek için iş parçacığını boşaldıkları Web uygulamalarında üretilen işi de artırabilir. Daha fazla bilgi için bkz. [zaman uyumsuz C#programlama ](/dotnet/csharp/async).

> [!WARNING]  
> EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez. Sonraki işleme başlamadan önce her zaman bir işlemin tamamlanmasını beklemeniz gerekir. Bu, genellikle her zaman uyumsuz işlem üzerinde `await` anahtar sözcüğü kullanılarak yapılır.

Entity Framework Core, bir sorgu ve sonuç döndüren LINQ yöntemlerine benzer bir zaman uyumsuz uzantı yöntemleri kümesi sağlar. Örnekler arasında `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` sayılabilir. @No__t-0 veya `OrderBy(...)` gibi bazı LINQ işleçlerinin zaman uyumsuz bir sürümü yoktur, çünkü bu yöntemler yalnızca LINQ ifade ağacını oluşturur ve sorgunun veritabanında yürütülmesine neden olmaz.

> [!IMPORTANT]  
> EF Core zaman uyumsuz uzantı yöntemleri `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır. Yöntemlerin kullanılabilmesi için bu ad alanı içeri aktarılmalıdır.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
