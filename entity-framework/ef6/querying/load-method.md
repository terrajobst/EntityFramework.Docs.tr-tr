---
title: -EF6 Load yöntemi
author: divega
ms.date: 2016-10-23
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: f7e8410b8fb8b5c3e86c51cd61868604a7566d0c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996657"
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
