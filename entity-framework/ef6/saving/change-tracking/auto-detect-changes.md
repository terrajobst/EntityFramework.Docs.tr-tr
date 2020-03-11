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
# <a name="automatic-detect-changes"></a><span data-ttu-id="d8aa6-102">Değişiklikleri otomatik algıla</span><span class="sxs-lookup"><span data-stu-id="d8aa6-102">Automatic detect changes</span></span>
<span data-ttu-id="d8aa6-103">En fazla POCO varlıklarını kullanırken, bir varlığın nasıl değiştiğini belirleme (ve bu nedenle veritabanına gönderilmesi gereken güncelleştirmeler), değişiklikleri Algıla algoritması tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="d8aa6-104">Varlığın geçerli özellik değerleri ve varlık sorgulanırken ya da eklendiğinde anlık görüntüde depolanan özgün özellik değerleri arasındaki farkları algılayarak değişiklikleri tespit edin.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="d8aa6-105">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="d8aa6-106">Varsayılan olarak, Entity Framework aşağıdaki Yöntemler çağrıldığında değişiklikleri otomatik olarak algılamaya çalışır:</span><span class="sxs-lookup"><span data-stu-id="d8aa6-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="d8aa6-107">DbSet. Find</span><span class="sxs-lookup"><span data-stu-id="d8aa6-107">DbSet.Find</span></span>  
- <span data-ttu-id="d8aa6-108">DbSet. Local</span><span class="sxs-lookup"><span data-stu-id="d8aa6-108">DbSet.Local</span></span>  
- <span data-ttu-id="d8aa6-109">DbSet. Add</span><span class="sxs-lookup"><span data-stu-id="d8aa6-109">DbSet.Add</span></span>  
- <span data-ttu-id="d8aa6-110">DbSet. AddRange</span><span class="sxs-lookup"><span data-stu-id="d8aa6-110">DbSet.AddRange</span></span>
- <span data-ttu-id="d8aa6-111">DbSet. Remove</span><span class="sxs-lookup"><span data-stu-id="d8aa6-111">DbSet.Remove</span></span>  
- <span data-ttu-id="d8aa6-112">DbSet. RemoveRange</span><span class="sxs-lookup"><span data-stu-id="d8aa6-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="d8aa6-113">DbSet. Attach</span><span class="sxs-lookup"><span data-stu-id="d8aa6-113">DbSet.Attach</span></span>  
- <span data-ttu-id="d8aa6-114">DbContext. SaveChanges</span><span class="sxs-lookup"><span data-stu-id="d8aa6-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="d8aa6-115">DbContext. GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="d8aa6-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="d8aa6-116">DbContext. Entry</span><span class="sxs-lookup"><span data-stu-id="d8aa6-116">DbContext.Entry</span></span>  
- <span data-ttu-id="d8aa6-117">DbChangeTracker. Entries</span><span class="sxs-lookup"><span data-stu-id="d8aa6-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="d8aa6-118">Değişikliklerin otomatik olarak algılanmasını devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="d8aa6-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="d8aa6-119">Bağlamdaki çok sayıda varlığı izliyorsanız ve bu yöntemlerden birini bir döngüde çok kez çağırırsanız, döngü süresince değişikliklerin algılanmasını devre dışı bırakarak önemli performans iyileştirmeleri alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="d8aa6-120">Örnek:</span><span class="sxs-lookup"><span data-stu-id="d8aa6-120">For example:</span></span>  

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

<span data-ttu-id="d8aa6-121">Döngüden sonra değişiklik algılamayı yeniden etkinleştirmeyi unutmayın. döngüdeki kod bir özel durum oluşturursa bile her zaman yeniden etkinleştirildiğinden emin olmak için bir try/finally kullandık.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="d8aa6-122">Devre dışı bırakmaya ve yeniden etkinleştirmeye alternatif olarak, değişikliklerin otomatik olarak algılanmasını her zaman devre dışı bırakmamanız ve her iki durumda da bağlam çağırmasıdır. ChangeTracker. DetectChanges açık veya değişiklik izleme proxy 'leri kullanır.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="d8aa6-123">Bu seçeneklerin her ikisi de gelişmiş bir uygulamadır ve uygulamanıza kolayca hafif hatalar getirebilir, böylece bunları dikkatli bir şekilde kullanın.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="d8aa6-124">Bağlamdan çok sayıda nesne eklemeniz veya kaldırmanız gerekirse, DbSet. AddRange ve DbSet. RemoveRange kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="d8aa6-125">Bu yöntemler, değişiklikleri yalnızca ekleme veya kaldırma işlemleri tamamlandıktan sonra otomatik olarak algılar.</span><span class="sxs-lookup"><span data-stu-id="d8aa6-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
