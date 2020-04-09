---
title: Asynchronous Kaydetme - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417644"
---
# <a name="asynchronous-saving"></a>Zaman Uyumsuz Kaydetme

Eşiş yarası kaydetme, değişiklikler veritabanına yazılırken iş parçacığının engellenmesini önler. Bu, kalın istemci uygulamasının kullanıcı kullanıcı ayını dondurmayı önlemek için yararlı olabilir. Eşiş işlemleri, veritabanı işlemi tamamlarken iş parçacığının diğer isteklere hizmet vermek üzere serbest bırakılabildiği bir web uygulamasında iş parçacığının da artırılabilir. Daha fazla bilgi için [C# içinde Asynchronous Programming 'e](https://docs.microsoft.com/dotnet/csharp/async)bakın.

> [!WARNING]  
> EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez. Her zaman bir sonraki işlem başlamadan önce tamamlanması için bir işlem için beklemeniz gerekir. Bu genellikle her eşzamanlı `await` işlemde anahtar kelime kullanılarak yapılır.

Varlık Framework `DbContext.SaveChangesAsync()` Core' a asynchronous alternatif olarak `DbContext.SaveChanges()`sağlar.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
