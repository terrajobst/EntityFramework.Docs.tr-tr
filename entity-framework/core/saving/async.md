---
title: Zaman uyumsuz kaydetme - EF çekirdek
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
ms.locfileid: "26054135"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="16421-102">Zaman uyumsuz kaydetme</span><span class="sxs-lookup"><span data-stu-id="16421-102">Asynchronous Saving</span></span>

<span data-ttu-id="16421-103">Zaman uyumsuz kaydetme değişiklikleri veritabanına yazıldığı sırada bir iş parçacığı engelleme önler.</span><span class="sxs-lookup"><span data-stu-id="16421-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="16421-104">Kalın istemci uygulamasının UI'ını dondurma önlemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="16421-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="16421-105">Zaman uyumsuz işlemleri de burada iş parçacığı veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için önbellekten silinebilir bir web uygulaması performansını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="16421-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="16421-106">Daha fazla bilgi için bkz: [zaman uyumsuz programlama C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="16421-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="16421-107">EF çekirdek aynı bağlam örneğine üzerinde çalıştırılan birden çok paralel işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="16421-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="16421-108">Her zaman bir işlemin sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="16421-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="16421-109">Bu genellikle kullanarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.</span><span class="sxs-lookup"><span data-stu-id="16421-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="16421-110">Entity Framework Çekirdek sağlar `DbContext.SaveChangesAsync()` zaman uyumsuz alternatif olarak `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="16421-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
