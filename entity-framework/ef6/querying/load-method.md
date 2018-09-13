---
title: -EF6 Load yöntemi
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: 3a0d11552b6bfd8b83f15c58c6cb9f945d9d4536
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490902"
---
# <a name="the-load-method"></a>Load yöntemi
Burada bağlamına veritabanından hemen bu varlıklarla herhangi bir şey yapmadan varlıkları yükleme isteyebilirsiniz birkaç senaryo mevcuttur. Bunun iyi bir örnek veri bağlama için varlıklar açıklandığı Yükleniyor [yerel veri](~/ef6/querying/local-data.md). Bunu yapmanın yaygın bir yolu, LINQ sorgusu yazın ve ardından ToList üzerinde yalnızca oluşturulan listenin hemen atmak için çağrısından sağlamaktır. Yük genişletme yöntemi yalnızca ToList gibi çalışır listesinin oluşturulması tamamen ortadan kaldırır.  

Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

Yük kullanarak iki örnek aşağıda verilmiştir. İlk yük kullanıldığı bir Windows Forms veri bağlama uygulamasından alınan varlıklar yerel koleksiyonuna bağlama önce açıklanan şekilde sorgulamak için [yerel veri](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Bölümünde anlatıldığı gibi ilgili varlıkları filtrelenmiş koleksiyonu yüklemek için yük kullanarak ikinci örnekte gösterildiği [ilgili varlıkları yükleme](~/ef6/querying/related-data.md):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
