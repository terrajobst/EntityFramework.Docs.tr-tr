---
title: "Zaman uyumsuz sorguları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="a6322-102">Zaman Uyumsuz Sorgular</span><span class="sxs-lookup"><span data-stu-id="a6322-102">Asynchronous Queries</span></span>

<span data-ttu-id="a6322-103">Zaman uyumsuz sorgular veritabanında sorgu yürütülür sırada bir iş parçacığı engelleme kaçının.</span><span class="sxs-lookup"><span data-stu-id="a6322-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="a6322-104">Kalın istemci uygulamasının UI'ını dondurma önlemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="a6322-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="a6322-105">Zaman uyumsuz işlemleri de burada iş parçacığı veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için önbellekten silinebilir bir web uygulaması performansını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="a6322-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="a6322-106">Daha fazla bilgi için bkz: [zaman uyumsuz programlama C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="a6322-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="a6322-107">EF çekirdek aynı bağlam örneğine üzerinde çalıştırılan birden çok paralel işlemleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="a6322-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="a6322-108">Her zaman bir işlemin sonraki işlemi başlamadan önce tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a6322-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="a6322-109">Bu genellikle kullanarak yapılır `await` anahtar sözcüğü her zaman uyumsuz işlem.</span><span class="sxs-lookup"><span data-stu-id="a6322-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="a6322-110">Entity Framework Çekirdek alternatif döndürülen sonuçları ve yürütülecek sorgu neden LINQ yöntemler olarak kullanılan zaman uyumsuz genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a6322-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="a6322-111">Örnekler `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`vb. Olmadığından LINQ işleçleri zaman uyumsuz sürümlerini gibi `Where(...)`, `OrderBy(...)`vb. çünkü bu yöntem yalnızca LINQ ifadesi ağacı oluşturmak ve veritabanında yürütülecek sorgu neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="a6322-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="a6322-112">EF çekirdek zaman uyumsuz genişletme yöntemleri tanımlanan `Microsoft.EntityFrameworkCore` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="a6322-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="a6322-113">Bu ad alanı yöntemleri kullanılabilir olması içeri aktarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a6322-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
