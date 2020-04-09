---
title: Eşzamanlı Sorgular - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417755"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="01e39-102">Zaman Uyumsuz Sorgular</span><span class="sxs-lookup"><span data-stu-id="01e39-102">Asynchronous Queries</span></span>

<span data-ttu-id="01e39-103">Eşiş sorguları veritabanında yürütülürken iş parçacığı engellemekaçının.</span><span class="sxs-lookup"><span data-stu-id="01e39-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="01e39-104">Async sorguları kalın istemci uygulamalarında duyarlı bir Kullanıcı Arabirimi tutmak için önemlidir.</span><span class="sxs-lookup"><span data-stu-id="01e39-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="01e39-105">Ayrıca, web uygulamalarındaki diğer isteklere hizmet vermek için iş parçacığı serbest web uygulamalarında iş parçacığı artırabilir.</span><span class="sxs-lookup"><span data-stu-id="01e39-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="01e39-106">Daha fazla bilgi için [C# içinde Asynchronous Programming 'e](/dotnet/csharp/async)bakın.</span><span class="sxs-lookup"><span data-stu-id="01e39-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="01e39-107">EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="01e39-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="01e39-108">Her zaman bir sonraki işlem başlamadan önce tamamlanması için bir işlem için beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="01e39-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="01e39-109">Bu genellikle her async `await` işleminde anahtar kelime kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="01e39-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="01e39-110">Entity Framework Core, bir sorgu ve return sonuçları yürütmek LINQ yöntemlerine benzer bir async uzantı yöntemleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="01e39-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="01e39-111">Örnekler `ToListAsync()`arasında `ToArrayAsync()` `SingleAsync()`, , .</span><span class="sxs-lookup"><span data-stu-id="01e39-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="01e39-112">Bazı LINQ işleçlerinin yalnızca LINQ `Where(...)` `OrderBy(...)` ifade ağacını oluşturması ve sorgunun veritabanında yürütülmesine neden olmaması gibi async sürümleri yoktur.</span><span class="sxs-lookup"><span data-stu-id="01e39-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="01e39-113">EF Core async uzantı yöntemleri `Microsoft.EntityFrameworkCore` ad alanında tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="01e39-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="01e39-114">Bu ad alanı, kullanılabilir yöntemler için içe aktarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="01e39-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
