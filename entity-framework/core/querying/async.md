---
title: Zaman uyumsuz sorguları - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>Zaman Uyumsuz Sorgular

Zaman uyumsuz sorgular veritabanında sorgu yürütülür sırada bir iş parçacığı engelleme kaçının. Kalın istemci uygulamasının UI'ını dondurma önlemek yararlı olabilir. Zaman uyumsuz işlemleri de burada iş parçacığı veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için önbellekten silinebilir bir web uygulaması performansını artırabilir. Daha fazla bilgi için bkz: [zaman uyumsuz programlama C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF çekirdek aynı bağlam örneğine üzerinde çalıştırılan birden çok paralel işlemleri desteklemez. Her zaman bir işlemin sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir. Bu genellikle kullanarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.

Entity Framework Çekirdek alternatif döndürülen sonuçları ve yürütülecek sorgu neden LINQ yöntemler olarak kullanılan zaman uyumsuz genişletme yöntemleri sağlar. Örnekler `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`vb. Olmadığından LINQ işleçleri zaman uyumsuz sürümlerini gibi `Where(...)`, `OrderBy(...)`vb. çünkü bu yöntem yalnızca LINQ ifadesi ağacı oluşturmak ve veritabanında yürütülecek sorgu neden olmaz.

> [!IMPORTANT]  
> EF çekirdek zaman uyumsuz genişletme yöntemleri tanımlanan `Microsoft.EntityFrameworkCore` ad alanı. Bu ad alanı yöntemleri kullanılabilir olması içeri aktarılmalıdır.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
