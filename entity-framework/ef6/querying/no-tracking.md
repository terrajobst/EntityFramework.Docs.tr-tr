---
title: Hayır-izleme sorguları - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490133"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="2a380-102">Hayır-izleme sorguları</span><span class="sxs-lookup"><span data-stu-id="2a380-102">No-Tracking Queries</span></span>
<span data-ttu-id="2a380-103">Bazen varlıklar bir sorgudan geri almak ancak bağlam tarafından izlenmesi kişilikleri bulunmuyor isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2a380-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="2a380-104">Bu daha iyi performans için çok sayıda varlık salt okunur senaryolarda sorgulanırken neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2a380-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="2a380-105">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2a380-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="2a380-106">Yeni bir genişletme yöntemi bu şekilde çalıştırılması herhangi bir sorgu AsNoTracking sağlar.</span><span class="sxs-lookup"><span data-stu-id="2a380-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="2a380-107">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2a380-107">For example:</span></span>  

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
