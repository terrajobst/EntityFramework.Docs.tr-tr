---
title: Zaman uyumsuz kaydetme - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997288"
---
# <a name="asynchronous-saving"></a>Zaman uyumsuz kaydetme

Zaman uyumsuz kaydetme, değişiklikleri veritabanına yazılan sırasında bir iş parçacığını engelleme önler. Bu, bir kalın istemci uygulamasının kullanıcı arabirimini dondurma önlemek yararlı olabilir. Zaman uyumsuz işlemler, ayrıca burada iş parçacığı oluşturan veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilir bir web uygulaması performansını artırabilir. Daha fazla bilgi için [Asynchronous Programming C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core aynı bağlam örneğinde çalıştırılan birden çok paralel işlemleri desteklemez. Her zaman bir işlemin bir sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir. Bu genellikle kullanılarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.

Entity Framework Core sağlar `DbContext.SaveChangesAsync()` zaman uyumsuz bir alternatifi olarak `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
