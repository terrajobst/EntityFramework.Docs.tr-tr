---
title: Zaman uyumsuz kaydetme-EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197825"
---
# <a name="asynchronous-saving"></a>Zaman Uyumsuz Kaydetme

Zaman uyumsuz kaydetme, değişiklikler veritabanına yazılırken bir iş parçacığının engellenmesini önler. Bu, bir kalın istemci uygulamasının Kullanıcı arabirimini dondurmaktan kaçınmak için yararlı olabilir. Zaman uyumsuz işlemler, bir Web uygulamasındaki aktarım hızını da artırabilir ve bu durumda iş parçacığı, veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilirler. Daha fazla bilgi için bkz. [zaman uyumsuz C#programlama ](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez. Sonraki işleme başlamadan önce her zaman bir işlemin tamamlanmasını beklemeniz gerekir. Bu, genellikle her zaman uyumsuz işlemdeki `await` anahtar sözcüğü kullanılarak yapılır.

Entity Framework Core, `DbContext.SaveChangesAsync()` için `DbContext.SaveChanges()`bir zaman uyumsuz alternatif olarak sunulur.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
