---
title: İzleme ile Hayır-izleme sorguları - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 985adc795f379199a3bacc985843f32f3168cf64
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993361"
---
# <a name="tracking-vs-no-tracking-queries"></a>İzleme ile Hayır-izleme sorguları

Entity Framework Core, değişiklik İzleyici'varlık örneği hakkında bilgi tutar olup olmadığını davranışı denetimleri izleme. Bir varlık izleniyorsa varlık içinde algılanan herhangi bir değişiklik sırasında veritabanını kalıcıdır `SaveChanges()`. Entity Framework Core olacaktır ayrıca düzeltme yukarı Gezinti özellikleri izleme sorgudan elde edilen varlıkları ve DbContext örneğine önceden yüklenmiş varlıklar arasındaki.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="tracking-queries"></a>İzleme sorguları

Varsayılan olarak, varlık türleri döndüren sorgular izlediğiniz. Yani bu varlık örneği için değişiklik yapabilirsiniz ve bu değişiklikleri tarafından kalıcı `SaveChanges()`.

Aşağıdaki örnekte, bloglar derecelendirme değişiklik algılanır ve veritabanı sırasında kalıcı `SaveChanges()`.

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

Hiçbir izleme sorguları sonuçları salt okunur bir senaryoda kullanıldığında yararlıdır. Bunlar Kurulum değişiklik izleme bilgilerini gerek yoktur çünkü yürütülecek oluşturulurlar.

Hayır-izleme için bir sorguyu değiştirebilir:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Varsayılan davranış bağlam örnek düzeyinde izleme da değiştirebilirsiniz:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Hiçbir izleme sorguları hala kimlik çözümlemesi yürütmeyi sorgu içinden gerçekleştirin. Sonuç kümesi birden çok kez aynı varlık içeriyorsa, sonuç kümesindeki her örneği için aynı varlık sınıfı örneği döndürülür. Bununla birlikte, zayıf başvurular zaten döndürülen varlıkları izlemek için kullanılır. Aynı kimliğe sahip bir önceki sonucu kapsam dışına gider ve atık toplama devam, yeni bir varlık örneği alabilirsiniz. Daha fazla bilgi için [nasıl sorgu çalışır](overview.md).

## <a name="tracking-and-projections"></a>İzleme ve tahminler

Sonuç varlık türleri içeriyorsa sorgunun sonuç türü bir varlık türü olmasa bile varsayılan olarak hala izlenir. Aşağıdaki sorguda örneklerini anonim bir tür döndürür `Blog` sonuç kümesi izlenir.

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

Sonuç kümesi herhangi bir varlık türünün içermiyorsa, ardından hiçbir izleme gerçekleştirilir. Aşağıdaki sorguda, anonim bir tür döndürür varlık (ancak gerçek varlık türü örneği) değerlerden bazıları ile yoktur hiçbir izleme gerçekleştirilir.

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
