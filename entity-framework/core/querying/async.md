---
title: Zaman uyumsuz sorgular-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417755"
---
# <a name="asynchronous-queries"></a>Zaman Uyumsuz Sorgular

Zaman uyumsuz sorgular, sorgu veritabanında yürütüldüğü sırada bir iş parçacığının engellenmesini önler. Zaman uyumsuz sorgular, çok duyarlı bir kullanıcı arabirimini kalın istemci uygulamalarında tutmak için önemlidir. Ayrıca, Web uygulamalarındaki diğer isteklere hizmet vermek için iş parçacığını boşaldıkları Web uygulamalarında üretilen işi de artırabilir. Daha fazla bilgi için bkz. [zaman uyumsuz C#programlama ](/dotnet/csharp/async).

> [!WARNING]  
> EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez. Sonraki işleme başlamadan önce her zaman bir işlemin tamamlanmasını beklemeniz gerekir. Bu, genellikle her zaman uyumsuz işlem üzerinde `await` anahtar sözcüğü kullanılarak yapılır.

Entity Framework Core, bir sorgu ve sonuç döndüren LINQ yöntemlerine benzer bir zaman uyumsuz uzantı yöntemleri kümesi sağlar. `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`sayılabilir. `Where(...)` veya `OrderBy(...)` gibi bazı LINQ işleçlerinin zaman uyumsuz sürümleri yoktur, çünkü bu yöntemler yalnızca LINQ ifade ağacını oluşturur ve sorgunun veritabanında yürütülmesine neden olmaz.

> [!IMPORTANT]  
> EF Core zaman uyumsuz uzantı yöntemleri `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır. Yöntemlerin kullanılabilmesi için bu ad alanı içeri aktarılmalıdır.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
