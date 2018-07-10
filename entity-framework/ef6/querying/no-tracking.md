---
title: Hayır-izleme sorguları - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
caps.latest.revision: 3
ms.openlocfilehash: 8310f2dab9e7ed9197a8c3e875e47e4f7b72d279
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912735"
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
