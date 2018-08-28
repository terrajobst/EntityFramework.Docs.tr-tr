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
# <a name="asynchronous-saving"></a><span data-ttu-id="6ca86-102">Zaman uyumsuz kaydetme</span><span class="sxs-lookup"><span data-stu-id="6ca86-102">Asynchronous Saving</span></span>

<span data-ttu-id="6ca86-103">Zaman uyumsuz kaydetme, değişiklikleri veritabanına yazılan sırasında bir iş parçacığını engelleme önler.</span><span class="sxs-lookup"><span data-stu-id="6ca86-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="6ca86-104">Bu, bir kalın istemci uygulamasının kullanıcı arabirimini dondurma önlemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6ca86-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="6ca86-105">Zaman uyumsuz işlemler, ayrıca burada iş parçacığı oluşturan veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilir bir web uygulaması performansını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="6ca86-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="6ca86-106">Daha fazla bilgi için [Asynchronous Programming C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="6ca86-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="6ca86-107">EF Core aynı bağlam örneğinde çalıştırılan birden çok paralel işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="6ca86-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="6ca86-108">Her zaman bir işlemin bir sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ca86-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="6ca86-109">Bu genellikle kullanılarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.</span><span class="sxs-lookup"><span data-stu-id="6ca86-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="6ca86-110">Entity Framework Core sağlar `DbContext.SaveChangesAsync()` zaman uyumsuz bir alternatifi olarak `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="6ca86-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
