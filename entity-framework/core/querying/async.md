---
title: Zaman uyumsuz sorgular-EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921688"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="92067-102">Zaman Uyumsuz Sorgular</span><span class="sxs-lookup"><span data-stu-id="92067-102">Asynchronous Queries</span></span>

<span data-ttu-id="92067-103">Zaman uyumsuz sorgular, sorgu veritabanında yürütüldüğü sırada bir iş parçacığının engellenmesini önler.</span><span class="sxs-lookup"><span data-stu-id="92067-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="92067-104">Bu, bir kalın istemci uygulamasının Kullanıcı arabirimini dondurmaktan kaçınmak için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="92067-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="92067-105">Zaman uyumsuz işlemler, bir Web uygulamasındaki aktarım hızını da artırabilir ve bu durumda iş parçacığı, veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilirler.</span><span class="sxs-lookup"><span data-stu-id="92067-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="92067-106">Daha fazla bilgi için bkz. [zaman uyumsuz C#programlama ](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="92067-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="92067-107">EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="92067-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="92067-108">Sonraki işleme başlamadan önce her zaman bir işlemin tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="92067-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="92067-109">Bu, genellikle her zaman uyumsuz işlemdeki `await` anahtar sözcüğü kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="92067-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="92067-110">Entity Framework Core, bir sorgunun yürütülmesine ve sonuçlar döndürülmesine neden olan LINQ yöntemlerine alternatif olarak kullanılabilecek bir dizi zaman uyumsuz uzantı yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="92067-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="92067-111">Örnekler şunlardır `ToListAsync()` `ToArrayAsync()` ,,vb`SingleAsync()`. `Where(...)` ,`OrderBy(...)`Vb. gibi LINQ işleçlerinin zaman uyumsuz sürümleri yoktur, çünkü bu yöntemler yalnızca LINQ ifade ağacını oluşturur ve sorgunun veritabanında yürütülmesine neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="92067-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="92067-112">EF Core zaman uyumsuz uzantı yöntemleri `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="92067-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="92067-113">Yöntemlerin kullanılabilmesi için bu ad alanı içeri aktarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="92067-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
