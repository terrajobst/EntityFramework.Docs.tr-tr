---
title: Hayır-izleme sorguları - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994246"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="c70d3-102">Hayır-izleme sorguları</span><span class="sxs-lookup"><span data-stu-id="c70d3-102">No-Tracking Queries</span></span>
<span data-ttu-id="c70d3-103">Bazen varlıklar bir sorgudan geri almak ancak bağlam tarafından izlenmesi kişilikleri bulunmuyor isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c70d3-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="c70d3-104">Bu daha iyi performans için çok sayıda varlık salt okunur senaryolarda sorgulanırken neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c70d3-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="c70d3-105">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c70d3-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="c70d3-106">Yeni bir genişletme yöntemi bu şekilde çalıştırılması herhangi bir sorgu AsNoTracking sağlar.</span><span class="sxs-lookup"><span data-stu-id="c70d3-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="c70d3-107">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c70d3-107">For example:</span></span>  

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
