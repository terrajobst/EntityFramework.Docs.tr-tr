---
title: Eşzamanlı Sorgular - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417755"
---
# <a name="asynchronous-queries"></a>Zaman Uyumsuz Sorgular

Eşiş sorguları veritabanında yürütülürken iş parçacığı engellemekaçının. Async sorguları kalın istemci uygulamalarında duyarlı bir Kullanıcı Arabirimi tutmak için önemlidir. Ayrıca, web uygulamalarındaki diğer isteklere hizmet vermek için iş parçacığı serbest web uygulamalarında iş parçacığı artırabilir. Daha fazla bilgi için [C# içinde Asynchronous Programming 'e](/dotnet/csharp/async)bakın.

> [!WARNING]  
> EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez. Her zaman bir sonraki işlem başlamadan önce tamamlanması için bir işlem için beklemeniz gerekir. Bu genellikle her async `await` işleminde anahtar kelime kullanılarak yapılır.

Entity Framework Core, bir sorgu ve return sonuçları yürütmek LINQ yöntemlerine benzer bir async uzantı yöntemleri kümesi sağlar. Örnekler `ToListAsync()`arasında `ToArrayAsync()` `SingleAsync()`, , . Bazı LINQ işleçlerinin yalnızca LINQ `Where(...)` `OrderBy(...)` ifade ağacını oluşturması ve sorgunun veritabanında yürütülmesine neden olmaması gibi async sürümleri yoktur.

> [!IMPORTANT]  
> EF Core async uzantı yöntemleri `Microsoft.EntityFrameworkCore` ad alanında tanımlanır. Bu ad alanı, kullanılabilir yöntemler için içe aktarılmalıdır.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
