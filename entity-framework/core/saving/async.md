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
# <a name="asynchronous-saving"></a><span data-ttu-id="97467-102">Zaman Uyumsuz Kaydetme</span><span class="sxs-lookup"><span data-stu-id="97467-102">Asynchronous Saving</span></span>

<span data-ttu-id="97467-103">Eşiş yarası kaydetme, değişiklikler veritabanına yazılırken iş parçacığının engellenmesini önler.</span><span class="sxs-lookup"><span data-stu-id="97467-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="97467-104">Bu, kalın istemci uygulamasının kullanıcı kullanıcı ayını dondurmayı önlemek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="97467-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="97467-105">Eşiş işlemleri, veritabanı işlemi tamamlarken iş parçacığının diğer isteklere hizmet vermek üzere serbest bırakılabildiği bir web uygulamasında iş parçacığının da artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="97467-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="97467-106">Daha fazla bilgi için [C# içinde Asynchronous Programming 'e](https://docs.microsoft.com/dotnet/csharp/async)bakın.</span><span class="sxs-lookup"><span data-stu-id="97467-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="97467-107">EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="97467-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="97467-108">Her zaman bir sonraki işlem başlamadan önce tamamlanması için bir işlem için beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="97467-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="97467-109">Bu genellikle her eşzamanlı `await` işlemde anahtar kelime kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="97467-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="97467-110">Varlık Framework `DbContext.SaveChangesAsync()` Core' a asynchronous alternatif olarak `DbContext.SaveChanges()`sağlar.</span><span class="sxs-lookup"><span data-stu-id="97467-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
