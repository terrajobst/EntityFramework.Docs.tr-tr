---
title: Zaman uyumsuz sorgular-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417755"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="3b432-102">Zaman Uyumsuz Sorgular</span><span class="sxs-lookup"><span data-stu-id="3b432-102">Asynchronous Queries</span></span>

<span data-ttu-id="3b432-103">Zaman uyumsuz sorgular, sorgu veritabanında yürütüldüğü sırada bir iş parçacığının engellenmesini önler.</span><span class="sxs-lookup"><span data-stu-id="3b432-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="3b432-104">Zaman uyumsuz sorgular, çok duyarlı bir kullanıcı arabirimini kalın istemci uygulamalarında tutmak için önemlidir.</span><span class="sxs-lookup"><span data-stu-id="3b432-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="3b432-105">Ayrıca, Web uygulamalarındaki diğer isteklere hizmet vermek için iş parçacığını boşaldıkları Web uygulamalarında üretilen işi de artırabilir.</span><span class="sxs-lookup"><span data-stu-id="3b432-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="3b432-106">Daha fazla bilgi için bkz. [zaman uyumsuz C#programlama ](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="3b432-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="3b432-107">EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3b432-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="3b432-108">Sonraki işleme başlamadan önce her zaman bir işlemin tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3b432-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="3b432-109">Bu, genellikle her zaman uyumsuz işlem üzerinde `await` anahtar sözcüğü kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="3b432-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="3b432-110">Entity Framework Core, bir sorgu ve sonuç döndüren LINQ yöntemlerine benzer bir zaman uyumsuz uzantı yöntemleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="3b432-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="3b432-111">`ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`sayılabilir.</span><span class="sxs-lookup"><span data-stu-id="3b432-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="3b432-112">`Where(...)` veya `OrderBy(...)` gibi bazı LINQ işleçlerinin zaman uyumsuz sürümleri yoktur, çünkü bu yöntemler yalnızca LINQ ifade ağacını oluşturur ve sorgunun veritabanında yürütülmesine neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="3b432-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="3b432-113">EF Core zaman uyumsuz uzantı yöntemleri `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3b432-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="3b432-114">Yöntemlerin kullanılabilmesi için bu ad alanı içeri aktarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3b432-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
