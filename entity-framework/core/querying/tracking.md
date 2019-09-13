---
title: İzleme vs. Izleme sorguları yok-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921650"
---
# <a name="tracking-vs-no-tracking-queries"></a>İzleme vs. Izleme sorguları yok

İzleme davranışı, Entity Framework Core değişiklik izleyicide bir varlık örneği hakkındaki bilgileri tutup tutamayacağını denetler. Bir varlık izleniyorsa, varlıkta algılanan tüm değişiklikler sırasında `SaveChanges()`veritabanında kalıcı olur. Entity Framework Core Ayrıca, izleme sorgusundan alınan varlıklar ve daha önce DbContext örneğine yüklenmiş varlıklar arasındaki gezinti özelliklerini de düzeltir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="tracking-queries"></a>Sorguları izleme

Varsayılan olarak, varlık türleri döndüren sorgular izliyor. Bu, bu varlık örneklerinde değişiklik yapabileceğiniz ve bu değişiklikleri tarafından `SaveChanges()`kalıcı hale oluşturabileceğiniz anlamına gelir.

Aşağıdaki örnekte, bloglarda yapılan değişiklik, sırasında `SaveChanges()`veritabanı tarafından algılanır ve kalıcı hale getirilir.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>İzleme sorguları yok

Bir salt okuma senaryosunda sonuçlar kullanıldığında hiçbir izleme sorgusu yararlı değildir. Değişiklik izleme bilgilerini ayarlamaya gerek olmadığından, bunlar yürütülmeye daha hızlıdır.

Tek bir sorguyu bir izleme olmayacak şekilde takas edebilirsiniz:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Bağlam örneği düzeyinde varsayılan izleme davranışını da değiştirebilirsiniz:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Yürütülen sorgu içinde hiçbir izleme sorgusu hala kimlik çözümlemesi gerçekleştirmez. Sonuç kümesi aynı varlığı birden çok kez içeriyorsa, sonuç kümesindeki her bir oluşum için varlık sınıfının aynı örneği döndürülür. Ancak, daha önce döndürülmüş varlıkların izlenmesini sağlamak için zayıf başvurular kullanılır. Aynı kimliğe sahip önceki bir sonuç kapsam dışına gittiğinde ve çöp toplama işlemi çalıştırıyorsa, yeni bir varlık örneği alabilirsiniz. Daha fazla bilgi için bkz. [sorgunun nasıl çalıştığı](overview.md).

## <a name="tracking-and-projections"></a>İzleme ve tahminler

Sorgunun sonuç türü bir varlık türü olmasa bile, sonuç varlık türleri içeriyorsa, bunlar varsayılan olarak yine de izlenir. Bir anonim tür döndüren aşağıdaki sorguda, sonuç kümesindeki örnekleri `Blog` izlenir.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
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

Sonuç kümesi herhangi bir varlık türü içermiyorsa, izleme yapılmaz. Aşağıdaki sorguda, varlıktan bazı değerler içeren anonim bir tür döndüren (ancak gerçek varlık türünün örneği olmadığında), hiçbir izleme yapılmaz.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
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
