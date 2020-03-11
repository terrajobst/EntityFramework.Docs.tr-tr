---
title: Load yöntemi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417125"
---
# <a name="the-load-method"></a>Load Yöntemi
Bu varlıklarla her şeyi hemen yapmadan, varlıkları veritabanından bağlamına yüklemek isteyebileceğiniz birkaç senaryo vardır. Bunun iyi bir örneği, [yerel verilerde](~/ef6/querying/local-data.md)açıklandığı gibi veri bağlama için varlıkları yüklemedir. Bunu yapmanın yaygın bir yolu, bir LINQ sorgusu yazmak ve sonra yalnızca oluşturulan listeyi hemen atmak için üzerinde ToList çağırmanız olur. Load genişletme yöntemi, yalnızca ToList gibi çalışarak, listenin tamamen oluşturulmasını önler.  

Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

Yükleme kullanmanın iki örneği aşağıda verilmiştir. Birincisi, [yerel verilerde](~/ef6/querying/local-data.md)açıklandığı gibi yerel koleksiyona bağlanmadan önce varlık sorgulamak için yük kullanıldığı Windows Forms veri bağlama uygulamasından alınmıştır:  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

İkinci örnek, [Ilgili varlıkların yüklenmesi](~/ef6/querying/related-data.md)bölümünde açıklandığı gibi, ilgili varlıkların filtrelenmiş bir koleksiyonunu yüklemek için Load kullanılarak gösterilmektedir:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
