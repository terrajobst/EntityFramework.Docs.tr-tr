---
title: Zaman uyumsuz kaydetme-EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417644"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="5c9f3-102">Zaman Uyumsuz Kaydetme</span><span class="sxs-lookup"><span data-stu-id="5c9f3-102">Asynchronous Saving</span></span>

<span data-ttu-id="5c9f3-103">Zaman uyumsuz kaydetme, değişiklikler veritabanına yazılırken bir iş parçacığının engellenmesini önler.</span><span class="sxs-lookup"><span data-stu-id="5c9f3-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="5c9f3-104">Bu, bir kalın istemci uygulamasının Kullanıcı arabirimini dondurmaktan kaçınmak için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5c9f3-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="5c9f3-105">Zaman uyumsuz işlemler, bir Web uygulamasındaki aktarım hızını da artırabilir ve bu durumda iş parçacığı, veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilirler.</span><span class="sxs-lookup"><span data-stu-id="5c9f3-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="5c9f3-106">Daha fazla bilgi için bkz. [zaman uyumsuz C#programlama ](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="5c9f3-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="5c9f3-107">EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="5c9f3-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="5c9f3-108">Sonraki işleme başlamadan önce her zaman bir işlemin tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5c9f3-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="5c9f3-109">Bu, genellikle her zaman uyumsuz işlem üzerinde `await` anahtar sözcüğü kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="5c9f3-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="5c9f3-110">Entity Framework Core, `DbContext.SaveChanges()`için zaman uyumsuz bir alternatif olarak `DbContext.SaveChangesAsync()` sağlar.</span><span class="sxs-lookup"><span data-stu-id="5c9f3-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
