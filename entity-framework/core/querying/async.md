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
# <a name="asynchronous-queries"></a><span data-ttu-id="46854-102">Zaman Uyumsuz Sorgular</span><span class="sxs-lookup"><span data-stu-id="46854-102">Asynchronous Queries</span></span>

<span data-ttu-id="46854-103">Sorgu veritabanında yürütüldüğü sırada bir iş parçacığını engelleyen zaman uyumsuz sorgular kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="46854-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="46854-104">Bu, bir kalın istemci uygulamasının kullanıcı arabirimini dondurma önlemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="46854-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="46854-105">Zaman uyumsuz işlemler, ayrıca burada iş parçacığı oluşturan veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilir bir web uygulaması performansını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="46854-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="46854-106">Daha fazla bilgi için [Asynchronous Programming C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="46854-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="46854-107">EF Core aynı bağlam örneğinde çalıştırılan birden çok paralel işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="46854-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="46854-108">Her zaman bir işlemin bir sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46854-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="46854-109">Bu genellikle kullanılarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.</span><span class="sxs-lookup"><span data-stu-id="46854-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="46854-110">Entity Framework Core bir dizi yürütülecek sorgu neden LINQ yöntemleri ve döndürülen sonuçların bir alternatifi olarak kullanılan zaman uyumsuz bir genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="46854-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="46854-111">Örnekler `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`vb. Yoksa zaman uyumsuz sürümlerini LINQ işleçleri gibi `Where(...)`, `OrderBy(...)`vb. çünkü bu yöntem yalnızca LINQ ifade ağacı oluşturmak ve veritabanında yürütülecek sorgu neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="46854-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="46854-112">EF Core zaman uyumsuz genişletme yöntemleri, şurada tanımlanan `Microsoft.EntityFrameworkCore` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="46854-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="46854-113">Bu ad, kullanılabilir olması için yöntemleri aktarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="46854-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
