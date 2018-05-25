---
title: İzleme vs. Hayır-izleme sorguları - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>İzleme vs. Hayır-izleme sorguları

Entity Framework Çekirdek kendi değişikliği İzleyicisi bir varlık örneği hakkında bilgi tutar olup olmadığına bakılmaksızın davranışı denetimleri izleme. Bir varlık izleniyorsa varlıkta algılanan değişiklikleri sırasında veritabanı kalıcıdır `SaveChanges()`. Entity Framework Çekirdek aynı zamanda düzeltme yukarı Gezinti özellikleri izleme sorgudan alınan varlıkları ve DbContext örneğine önceden yüklenmiş varlıklar arasındaki.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.

## <a name="tracking-queries"></a>İzleme sorguları

Varsayılan olarak, varlık türleri döndüren sorgular izlemekte olduğunuz. Yani bu varlık örneklerini değişiklik yapabilirsiniz ve bu değişiklikleri tarafından kalıcı `SaveChanges()`.

Aşağıdaki örnekte, bloglar derecesi değişikliği algılanır ve sırasında veritabanına kalıcı `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Hayır-izleme sorguları

Sonuçları bir salt okunur senaryosunda kullanıldığında hiçbir izleme sorguları yararlı olur. Bunlar, Kurulum değişiklik izleme bilgilerinin gerek olduğundan yürütmek oluşturulurlar.

Hayır-izleme için tek bir sorguya takas edebilirsiniz:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Varsayılan davranış bağlam örnek düzeyinde izleme de değiştirebilirsiniz:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Hiçbir izleme sorguları hala yürütmeyi sorgusundan kimlik çözümleme gerçekleştirin. Sonuç kümesi birden çok kez aynı varlık içeriyorsa, sonuç kümesinde her örneği için aynı varlık sınıfı örneği döndürülür. Ancak, zayıf başvurular zaten döndürülen varlıkları izlemek için kullanılır. Aynı kimliğe sahip bir önceki sonuç kapsamının dışına gider ve atık toplama devam, yeni bir varlık örneği elde edebilirsiniz. Daha fazla bilgi için bkz: [nasıl sorgu çalışır](overview.md).

## <a name="tracking-and-projections"></a>İzleme ve projeksiyonu

Varlık türleri sonucu içeriyorsa, sorgunun sonuç türü bir varlık türü olmasa bile varsayılan olarak hala izlenecektir. Aşağıdaki sorguda döndüğü örneklerini anonim bir tür `Blog` sonuç kümesi izlenir.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Sonuç kümesi herhangi bir varlık türünün içermiyorsa, hiçbir izleme gerçekleştirilir. Aşağıdaki sorguda döndüğü anonim bir tür varlık (ancak gerçek bir varlık türü örneği) değerlerinden bazıları ile yoktur hiçbir izleme gerçekleştirilir.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
