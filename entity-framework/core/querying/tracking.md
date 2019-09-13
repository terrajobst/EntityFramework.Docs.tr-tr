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
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="dede9-102">İzleme vs. Izleme sorguları yok</span><span class="sxs-lookup"><span data-stu-id="dede9-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="dede9-103">İzleme davranışı, Entity Framework Core değişiklik izleyicide bir varlık örneği hakkındaki bilgileri tutup tutamayacağını denetler.</span><span class="sxs-lookup"><span data-stu-id="dede9-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="dede9-104">Bir varlık izleniyorsa, varlıkta algılanan tüm değişiklikler sırasında `SaveChanges()`veritabanında kalıcı olur.</span><span class="sxs-lookup"><span data-stu-id="dede9-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="dede9-105">Entity Framework Core Ayrıca, izleme sorgusundan alınan varlıklar ve daha önce DbContext örneğine yüklenmiş varlıklar arasındaki gezinti özelliklerini de düzeltir.</span><span class="sxs-lookup"><span data-stu-id="dede9-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="dede9-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="dede9-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="dede9-107">Sorguları izleme</span><span class="sxs-lookup"><span data-stu-id="dede9-107">Tracking queries</span></span>

<span data-ttu-id="dede9-108">Varsayılan olarak, varlık türleri döndüren sorgular izliyor.</span><span class="sxs-lookup"><span data-stu-id="dede9-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="dede9-109">Bu, bu varlık örneklerinde değişiklik yapabileceğiniz ve bu değişiklikleri tarafından `SaveChanges()`kalıcı hale oluşturabileceğiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="dede9-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="dede9-110">Aşağıdaki örnekte, bloglarda yapılan değişiklik, sırasında `SaveChanges()`veritabanı tarafından algılanır ve kalıcı hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="dede9-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="dede9-111">İzleme sorguları yok</span><span class="sxs-lookup"><span data-stu-id="dede9-111">No-tracking queries</span></span>

<span data-ttu-id="dede9-112">Bir salt okuma senaryosunda sonuçlar kullanıldığında hiçbir izleme sorgusu yararlı değildir.</span><span class="sxs-lookup"><span data-stu-id="dede9-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="dede9-113">Değişiklik izleme bilgilerini ayarlamaya gerek olmadığından, bunlar yürütülmeye daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="dede9-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="dede9-114">Tek bir sorguyu bir izleme olmayacak şekilde takas edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dede9-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="dede9-115">Bağlam örneği düzeyinde varsayılan izleme davranışını da değiştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dede9-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="dede9-116">Yürütülen sorgu içinde hiçbir izleme sorgusu hala kimlik çözümlemesi gerçekleştirmez.</span><span class="sxs-lookup"><span data-stu-id="dede9-116">No tracking queries still perform identity resolution within the executing query.</span></span> <span data-ttu-id="dede9-117">Sonuç kümesi aynı varlığı birden çok kez içeriyorsa, sonuç kümesindeki her bir oluşum için varlık sınıfının aynı örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="dede9-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="dede9-118">Ancak, daha önce döndürülmüş varlıkların izlenmesini sağlamak için zayıf başvurular kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dede9-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="dede9-119">Aynı kimliğe sahip önceki bir sonuç kapsam dışına gittiğinde ve çöp toplama işlemi çalıştırıyorsa, yeni bir varlık örneği alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dede9-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="dede9-120">Daha fazla bilgi için bkz. [sorgunun nasıl çalıştığı](overview.md).</span><span class="sxs-lookup"><span data-stu-id="dede9-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="dede9-121">İzleme ve tahminler</span><span class="sxs-lookup"><span data-stu-id="dede9-121">Tracking and projections</span></span>

<span data-ttu-id="dede9-122">Sorgunun sonuç türü bir varlık türü olmasa bile, sonuç varlık türleri içeriyorsa, bunlar varsayılan olarak yine de izlenir.</span><span class="sxs-lookup"><span data-stu-id="dede9-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="dede9-123">Bir anonim tür döndüren aşağıdaki sorguda, sonuç kümesindeki örnekleri `Blog` izlenir.</span><span class="sxs-lookup"><span data-stu-id="dede9-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="dede9-124">Sonuç kümesi herhangi bir varlık türü içermiyorsa, izleme yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="dede9-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="dede9-125">Aşağıdaki sorguda, varlıktan bazı değerler içeren anonim bir tür döndüren (ancak gerçek varlık türünün örneği olmadığında), hiçbir izleme yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="dede9-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
