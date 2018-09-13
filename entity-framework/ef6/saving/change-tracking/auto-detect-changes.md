---
title: Değişiklikleri - EF6 otomatik algılama
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490996"
---
# <a name="automatic-detect-changes"></a>Değişiklikleri otomatik algıla
Çoğu POCO varlık kullanırken belirlenmesi varlığın nasıl değiştiğini (ve bu nedenle hangi güncelleştirmelerin veritabanına gönderilmesi gerekir) değişiklikleri algılama algoritması tarafından işlenir. Geçerli özellik değerlerini varlığın varlık sorgulanan ya da ekli anlık görüntüde depolanan özgün özellik değerleri arasındaki farkları algılayarak değişiklikleri works algılayın. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

Varsayılan olarak Entity Framework algılama değişiklikler aşağıdaki yöntemler çağrıldığında otomatik olarak gerçekleştirir:  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Değişiklikleri otomatik olarak algılanmasını devre dışı bırakma  

Bağlamınızı içinde çok sayıda varlık izleme ve, aşağıdaki yöntemlerden birini bir döngüde birçok kez çağrılması, değişikliklerin algılanması için döngü süresini devre dışı bırakarak önemli performans geliştirmeleri alabilirsiniz. Örneğin:  

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

Döngü sonra değişiklikleri algılama yeniden etkinleştirmek unutmayın; biz bunu her zaman yeniden etkin döngüde kod bir özel durum oluşturursa bile emin olmak için bir try/finally kullandınız.  

Alternatif devre dışı bırakma ve yeniden etkinleştirerek otomatik algılanması değişiklikleri her zaman ve her iki arama bağlamı devre dışı bırakmak için. ChangeTracker.DetectChanges açıkça veya kullanım izleme proxy'leri yapıyorduk değiştirin. Bu seçeneklerin ikisi de Gelişmiş ve küçük hatalar uygulamanıza kolayca çıkarabilir bunları dikkatli şekilde kullanın.  

Çok sayıda nesne bir bağlamdan ekleyip gerekiyorsa DbSet.AddRange ve DbSet.RemoveRange kullanarak göz önünde bulundurun. Ekleme veya kaldırma işlemleri tamamlandıktan sonra bu yöntemleri otomatik olarak değişiklikleri yalnızca bir kez algılar. 
