---
title: Izleme sorguları yok-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417104"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="db75e-102">Izleme sorguları yok</span><span class="sxs-lookup"><span data-stu-id="db75e-102">No-Tracking Queries</span></span>
<span data-ttu-id="db75e-103">Bazen, varlıkları bir sorgudan geri almak isteyebilirsiniz, ancak bu varlıkların bağlam tarafından izlenmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="db75e-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="db75e-104">Bu, salt okuma senaryolarında çok sayıda varlık sorgulanırken daha iyi performans oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="db75e-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="db75e-105">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="db75e-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="db75e-106">Yeni bir genişletme yöntemi olan bir sorgu, herhangi bir sorgunun bu şekilde çalıştırılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="db75e-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="db75e-107">Örnek:</span><span class="sxs-lookup"><span data-stu-id="db75e-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
