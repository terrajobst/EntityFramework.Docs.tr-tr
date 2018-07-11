---
title: Temel sorgular - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: eceac81546b23157611edd530b8b71f71e970c1f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949066"
---
# <a name="basic-queries"></a><span data-ttu-id="f94cd-102">Temel sorgular</span><span class="sxs-lookup"><span data-stu-id="f94cd-102">Basic Queries</span></span>

<span data-ttu-id="f94cd-103">Dil tümleşik sorgu (LINQ) kullanarak bir veritabanından varlıklar yükleme hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="f94cd-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="f94cd-104">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="f94cd-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="f94cd-105">101 LINQ örneği</span><span class="sxs-lookup"><span data-stu-id="f94cd-105">101 LINQ samples</span></span>

<span data-ttu-id="f94cd-106">Bu sayfa, Entity Framework Core ile ortak görevler elde etmek için bazı örnekler göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="f94cd-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="f94cd-107">LINQ ile neler yapılabileceğini gösteren örnekleri kapsamlı bir dizi için bkz: [101 LINQ örnekleri](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="f94cd-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="f94cd-108">Tüm veri yükleme</span><span class="sxs-lookup"><span data-stu-id="f94cd-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="f94cd-109">Tek bir varlık yüklenirken</span><span class="sxs-lookup"><span data-stu-id="f94cd-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="f94cd-110">Filtreleme</span><span class="sxs-lookup"><span data-stu-id="f94cd-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
