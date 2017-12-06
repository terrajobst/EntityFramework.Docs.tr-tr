---
title: "Temel sorguları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="basic-queries"></a>Temel sorguları

Dil tümleştirmek sorgu (LINQ) kullanarak veritabanından varlıklar yük öğrenin.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.

## <a name="101-linq-samples"></a>101 LINQ örnekleri

Bu sayfa Entity Framework Çekirdek ile ortak görevler elde etmek için birkaç örnek gösterir. LINQ ile olası gösteren örnekleri kapsamlı bir kümesini için bkz: [101 LINQ örnekleri](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

## <a name="loading-all-data"></a>Tüm veri yükleme

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a>Tek bir varlık yükleniyor

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a>Filtreleme

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
