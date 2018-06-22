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
ms.locfileid: "26054786"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="4af40-102">İzleme vs. Hayır-izleme sorguları</span><span class="sxs-lookup"><span data-stu-id="4af40-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="4af40-103">Entity Framework Çekirdek kendi değişikliği İzleyicisi bir varlık örneği hakkında bilgi tutar olup olmadığına bakılmaksızın davranışı denetimleri izleme.</span><span class="sxs-lookup"><span data-stu-id="4af40-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="4af40-104">Bir varlık izleniyorsa varlıkta algılanan değişiklikleri sırasında veritabanı kalıcıdır `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="4af40-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="4af40-105">Entity Framework Çekirdek aynı zamanda düzeltme yukarı Gezinti özellikleri izleme sorgudan alınan varlıkları ve DbContext örneğine önceden yüklenmiş varlıklar arasındaki.</span><span class="sxs-lookup"><span data-stu-id="4af40-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="4af40-106">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.</span><span class="sxs-lookup"><span data-stu-id="4af40-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="4af40-107">İzleme sorguları</span><span class="sxs-lookup"><span data-stu-id="4af40-107">Tracking queries</span></span>

<span data-ttu-id="4af40-108">Varsayılan olarak, varlık türleri döndüren sorgular izlemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="4af40-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="4af40-109">Yani bu varlık örneklerini değişiklik yapabilirsiniz ve bu değişiklikleri tarafından kalıcı `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="4af40-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="4af40-110">Aşağıdaki örnekte, bloglar derecesi değişikliği algılanır ve sırasında veritabanına kalıcı `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="4af40-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="4af40-111">Hayır-izleme sorguları</span><span class="sxs-lookup"><span data-stu-id="4af40-111">No-tracking queries</span></span>

<span data-ttu-id="4af40-112">Sonuçları bir salt okunur senaryosunda kullanıldığında hiçbir izleme sorguları yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="4af40-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="4af40-113">Bunlar, Kurulum değişiklik izleme bilgilerinin gerek olduğundan yürütmek oluşturulurlar.</span><span class="sxs-lookup"><span data-stu-id="4af40-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="4af40-114">Hayır-izleme için tek bir sorguya takas edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4af40-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="4af40-115">Varsayılan davranış bağlam örnek düzeyinde izleme de değiştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4af40-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="4af40-116">Hiçbir izleme sorguları hala yürütmeyi sorgusundan kimlik çözümleme gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="4af40-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="4af40-117">Sonuç kümesi birden çok kez aynı varlık içeriyorsa, sonuç kümesinde her örneği için aynı varlık sınıfı örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4af40-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="4af40-118">Ancak, zayıf başvurular zaten döndürülen varlıkları izlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4af40-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="4af40-119">Aynı kimliğe sahip bir önceki sonuç kapsamının dışına gider ve atık toplama devam, yeni bir varlık örneği elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4af40-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="4af40-120">Daha fazla bilgi için bkz: [nasıl sorgu çalışır](overview.md).</span><span class="sxs-lookup"><span data-stu-id="4af40-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="4af40-121">İzleme ve projeksiyonu</span><span class="sxs-lookup"><span data-stu-id="4af40-121">Tracking and projections</span></span>

<span data-ttu-id="4af40-122">Varlık türleri sonucu içeriyorsa, sorgunun sonuç türü bir varlık türü olmasa bile varsayılan olarak hala izlenecektir.</span><span class="sxs-lookup"><span data-stu-id="4af40-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="4af40-123">Aşağıdaki sorguda döndüğü örneklerini anonim bir tür `Blog` sonuç kümesi izlenir.</span><span class="sxs-lookup"><span data-stu-id="4af40-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="4af40-124">Sonuç kümesi herhangi bir varlık türünün içermiyorsa, hiçbir izleme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4af40-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="4af40-125">Aşağıdaki sorguda döndüğü anonim bir tür varlık (ancak gerçek bir varlık türü örneği) değerlerinden bazıları ile yoktur hiçbir izleme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4af40-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
