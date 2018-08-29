---
title: Zaman uyumsuz sorgular - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993571"
---
# <a name="asynchronous-queries"></a>Zaman Uyumsuz Sorgular

Sorgu veritabanında yürütüldüğü sırada bir iş parçacığını engelleyen zaman uyumsuz sorgular kullanmayın. Bu, bir kalın istemci uygulamasının kullanıcı arabirimini dondurma önlemek yararlı olabilir. Zaman uyumsuz işlemler, ayrıca burada iş parçacığı oluşturan veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilir bir web uygulaması performansını artırabilir. Daha fazla bilgi için [Asynchronous Programming C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core aynı bağlam örneğinde çalıştırılan birden çok paralel işlemleri desteklemez. Her zaman bir işlemin bir sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir. Bu genellikle kullanılarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.

Entity Framework Core bir dizi yürütülecek sorgu neden LINQ yöntemleri ve döndürülen sonuçların bir alternatifi olarak kullanılan zaman uyumsuz bir genişletme yöntemleri sağlar. Örnekler `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`vb. Yoksa zaman uyumsuz sürümlerini LINQ işleçleri gibi `Where(...)`, `OrderBy(...)`vb. çünkü bu yöntem yalnızca LINQ ifade ağacı oluşturmak ve veritabanında yürütülecek sorgu neden olmaz.

> [!IMPORTANT]  
> EF Core zaman uyumsuz genişletme yöntemleri, şurada tanımlanan `Microsoft.EntityFrameworkCore` ad alanı. Bu ad, kullanılabilir olması için yöntemleri aktarılmalıdır.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
