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
# <a name="the-load-method"></a><span data-ttu-id="6a310-102">Load Yöntemi</span><span class="sxs-lookup"><span data-stu-id="6a310-102">The Load Method</span></span>
<span data-ttu-id="6a310-103">Bu varlıklarla her şeyi hemen yapmadan, varlıkları veritabanından bağlamına yüklemek isteyebileceğiniz birkaç senaryo vardır.</span><span class="sxs-lookup"><span data-stu-id="6a310-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="6a310-104">Bunun iyi bir örneği, [yerel verilerde](~/ef6/querying/local-data.md)açıklandığı gibi veri bağlama için varlıkları yüklemedir.</span><span class="sxs-lookup"><span data-stu-id="6a310-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="6a310-105">Bunu yapmanın yaygın bir yolu, bir LINQ sorgusu yazmak ve sonra yalnızca oluşturulan listeyi hemen atmak için üzerinde ToList çağırmanız olur.</span><span class="sxs-lookup"><span data-stu-id="6a310-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="6a310-106">Load genişletme yöntemi, yalnızca ToList gibi çalışarak, listenin tamamen oluşturulmasını önler.</span><span class="sxs-lookup"><span data-stu-id="6a310-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="6a310-107">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6a310-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="6a310-108">Yükleme kullanmanın iki örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6a310-108">Here are two examples of using Load.</span></span> <span data-ttu-id="6a310-109">Birincisi, [yerel verilerde](~/ef6/querying/local-data.md)açıklandığı gibi yerel koleksiyona bağlanmadan önce varlık sorgulamak için yük kullanıldığı Windows Forms veri bağlama uygulamasından alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="6a310-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="6a310-110">İkinci örnek, [Ilgili varlıkların yüklenmesi](~/ef6/querying/related-data.md)bölümünde açıklandığı gibi, ilgili varlıkların filtrelenmiş bir koleksiyonunu yüklemek için Load kullanılarak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6a310-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

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
