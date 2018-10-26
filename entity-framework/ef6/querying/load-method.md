---
title: -EF6 Load yöntemi
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: 4a795285004612ac03ab26532ac09ca333cb4c8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123823"
---
# <a name="the-load-method"></a><span data-ttu-id="980ff-102">Load yöntemi</span><span class="sxs-lookup"><span data-stu-id="980ff-102">The Load Method</span></span>
<span data-ttu-id="980ff-103">Burada bağlamına veritabanından hemen bu varlıklarla herhangi bir şey yapmadan varlıkları yükleme isteyebilirsiniz birkaç senaryo mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="980ff-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="980ff-104">Bunun iyi bir örnek veri bağlama için varlıklar açıklandığı Yükleniyor [yerel veri](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="980ff-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="980ff-105">Bunu yapmanın yaygın bir yolu, LINQ sorgusu yazın ve ardından ToList üzerinde yalnızca oluşturulan listenin hemen atmak için çağrısından sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="980ff-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="980ff-106">Yük genişletme yöntemi yalnızca ToList gibi çalışır listesinin oluşturulması tamamen ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="980ff-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="980ff-107">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="980ff-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="980ff-108">Yük kullanarak iki örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="980ff-108">Here are two examples of using Load.</span></span> <span data-ttu-id="980ff-109">İlk yük kullanıldığı bir Windows Forms veri bağlama uygulamasından alınan varlıklar yerel koleksiyonuna bağlama önce açıklanan şekilde sorgulamak için [yerel veri](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="980ff-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="980ff-110">Bölümünde anlatıldığı gibi ilgili varlıkları filtrelenmiş koleksiyonu yüklemek için yük kullanarak ikinci örnekte gösterildiği [ilgili varlıkları yükleme](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="980ff-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

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
