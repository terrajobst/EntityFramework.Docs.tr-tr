---
title: Değişiklikleri otomatik algıla-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416979"
---
# <a name="automatic-detect-changes"></a>Değişiklikleri otomatik algıla
En fazla POCO varlıklarını kullanırken, bir varlığın nasıl değiştiğini belirleme (ve bu nedenle veritabanına gönderilmesi gereken güncelleştirmeler), değişiklikleri Algıla algoritması tarafından işlenir. Varlığın geçerli özellik değerleri ve varlık sorgulanırken ya da eklendiğinde anlık görüntüde depolanan özgün özellik değerleri arasındaki farkları algılayarak değişiklikleri tespit edin. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

Varsayılan olarak, Entity Framework aşağıdaki Yöntemler çağrıldığında değişiklikleri otomatik olarak algılamaya çalışır:  

- DbSet. Find  
- DbSet. Local  
- DbSet. Add  
- DbSet. AddRange
- DbSet. Remove  
- DbSet. RemoveRange
- DbSet. Attach  
- DbContext. SaveChanges  
- DbContext. GetValidationErrors  
- DbContext. Entry  
- DbChangeTracker. Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Değişikliklerin otomatik olarak algılanmasını devre dışı bırakma  

Bağlamdaki çok sayıda varlığı izliyorsanız ve bu yöntemlerden birini bir döngüde çok kez çağırırsanız, döngü süresince değişikliklerin algılanmasını devre dışı bırakarak önemli performans iyileştirmeleri alabilirsiniz. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

Döngüden sonra değişiklik algılamayı yeniden etkinleştirmeyi unutmayın. döngüdeki kod bir özel durum oluşturursa bile her zaman yeniden etkinleştirildiğinden emin olmak için bir try/finally kullandık.  

Devre dışı bırakmaya ve yeniden etkinleştirmeye alternatif olarak, değişikliklerin otomatik olarak algılanmasını her zaman devre dışı bırakmamanız ve her iki durumda da bağlam çağırmasıdır. ChangeTracker. DetectChanges açık veya değişiklik izleme proxy 'leri kullanır. Bu seçeneklerin her ikisi de gelişmiş bir uygulamadır ve uygulamanıza kolayca hafif hatalar getirebilir, böylece bunları dikkatli bir şekilde kullanın.  

Bağlamdan çok sayıda nesne eklemeniz veya kaldırmanız gerekirse, DbSet. AddRange ve DbSet. RemoveRange kullanmayı düşünün. Bu yöntemler, değişiklikleri yalnızca ekleme veya kaldırma işlemleri tamamlandıktan sonra otomatik olarak algılar. 
