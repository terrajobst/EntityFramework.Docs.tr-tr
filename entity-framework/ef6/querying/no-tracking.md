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
# <a name="no-tracking-queries"></a>Hayır-izleme sorguları
Bazen varlıklar bir sorgudan geri almak ancak bağlam tarafından izlenmesi kişilikleri bulunmuyor isteyebilirsiniz. Bu daha iyi performans için çok sayıda varlık salt okunur senaryolarda sorgulanırken neden olabilir. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

Yeni bir genişletme yöntemi bu şekilde çalıştırılması herhangi bir sorgu AsNoTracking sağlar. Örneğin:  

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
