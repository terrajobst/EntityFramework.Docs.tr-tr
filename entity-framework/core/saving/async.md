---
title: "Zaman uyumsuz kaydetme - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a>Zaman uyumsuz kaydetme

Zaman uyumsuz kaydetme değişiklikleri veritabanına yazıldığı sırada bir iş parçacığı engelleme önler. Kalın istemci uygulamasının UI'ını dondurma önlemek yararlı olabilir. Zaman uyumsuz işlemleri de burada iş parçacığı veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için önbellekten silinebilir bir web uygulaması performansını artırabilir. Daha fazla bilgi için bkz: [zaman uyumsuz programlama C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF çekirdek aynı bağlam örneğine üzerinde çalıştırılan birden çok paralel işlemleri desteklemez. Her zaman bir işlemin sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir. Bu genellikle kullanarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.

Entity Framework Çekirdek sağlar `DbContext.SaveChangesAsync()` zaman uyumsuz alternatif olarak `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
