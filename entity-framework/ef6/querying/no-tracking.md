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
# <a name="no-tracking-queries"></a>Izleme sorguları yok
Bazen, varlıkları bir sorgudan geri almak isteyebilirsiniz, ancak bu varlıkların bağlam tarafından izlenmesi gerekmez. Bu, salt okuma senaryolarında çok sayıda varlık sorgulanırken daha iyi performans oluşmasına neden olabilir. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

Yeni bir genişletme yöntemi olan bir sorgu, herhangi bir sorgunun bu şekilde çalıştırılmasına izin verir. Örnek:  

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
