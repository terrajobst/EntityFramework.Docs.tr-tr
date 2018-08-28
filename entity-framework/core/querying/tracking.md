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
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="38a07-102">İzleme ile Hayır-izleme sorguları</span><span class="sxs-lookup"><span data-stu-id="38a07-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="38a07-103">Entity Framework Core, değişiklik İzleyici'varlık örneği hakkında bilgi tutar olup olmadığını davranışı denetimleri izleme.</span><span class="sxs-lookup"><span data-stu-id="38a07-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="38a07-104">Bir varlık izleniyorsa varlık içinde algılanan herhangi bir değişiklik sırasında veritabanını kalıcıdır `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="38a07-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="38a07-105">Entity Framework Core olacaktır ayrıca düzeltme yukarı Gezinti özellikleri izleme sorgudan elde edilen varlıkları ve DbContext örneğine önceden yüklenmiş varlıklar arasındaki.</span><span class="sxs-lookup"><span data-stu-id="38a07-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="38a07-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="38a07-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="38a07-107">İzleme sorguları</span><span class="sxs-lookup"><span data-stu-id="38a07-107">Tracking queries</span></span>

<span data-ttu-id="38a07-108">Varsayılan olarak, varlık türleri döndüren sorgular izlediğiniz.</span><span class="sxs-lookup"><span data-stu-id="38a07-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="38a07-109">Yani bu varlık örneği için değişiklik yapabilirsiniz ve bu değişiklikleri tarafından kalıcı `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="38a07-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="38a07-110">Aşağıdaki örnekte, bloglar derecelendirme değişiklik algılanır ve veritabanı sırasında kalıcı `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="38a07-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="38a07-111">Hayır-izleme sorguları</span><span class="sxs-lookup"><span data-stu-id="38a07-111">No-tracking queries</span></span>

<span data-ttu-id="38a07-112">Hiçbir izleme sorguları sonuçları salt okunur bir senaryoda kullanıldığında yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="38a07-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="38a07-113">Bunlar Kurulum değişiklik izleme bilgilerini gerek yoktur çünkü yürütülecek oluşturulurlar.</span><span class="sxs-lookup"><span data-stu-id="38a07-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="38a07-114">Hayır-izleme için bir sorguyu değiştirebilir:</span><span class="sxs-lookup"><span data-stu-id="38a07-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="38a07-115">Varsayılan davranış bağlam örnek düzeyinde izleme da değiştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="38a07-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="38a07-116">Hiçbir izleme sorguları hala kimlik çözümlemesi yürütmeyi sorgu içinden gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="38a07-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="38a07-117">Sonuç kümesi birden çok kez aynı varlık içeriyorsa, sonuç kümesindeki her örneği için aynı varlık sınıfı örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="38a07-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="38a07-118">Bununla birlikte, zayıf başvurular zaten döndürülen varlıkları izlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="38a07-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="38a07-119">Aynı kimliğe sahip bir önceki sonucu kapsam dışına gider ve atık toplama devam, yeni bir varlık örneği alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38a07-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="38a07-120">Daha fazla bilgi için [nasıl sorgu çalışır](overview.md).</span><span class="sxs-lookup"><span data-stu-id="38a07-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="38a07-121">İzleme ve tahminler</span><span class="sxs-lookup"><span data-stu-id="38a07-121">Tracking and projections</span></span>

<span data-ttu-id="38a07-122">Sonuç varlık türleri içeriyorsa sorgunun sonuç türü bir varlık türü olmasa bile varsayılan olarak hala izlenir.</span><span class="sxs-lookup"><span data-stu-id="38a07-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="38a07-123">Aşağıdaki sorguda örneklerini anonim bir tür döndürür `Blog` sonuç kümesi izlenir.</span><span class="sxs-lookup"><span data-stu-id="38a07-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="38a07-124">Sonuç kümesi herhangi bir varlık türünün içermiyorsa, ardından hiçbir izleme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="38a07-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="38a07-125">Aşağıdaki sorguda, anonim bir tür döndürür varlık (ancak gerçek varlık türü örneği) değerlerden bazıları ile yoktur hiçbir izleme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="38a07-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
