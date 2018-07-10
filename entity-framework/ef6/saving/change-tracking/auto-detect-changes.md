---
title: Değişiklikleri - EF6 otomatik algılama
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
caps.latest.revision: 3
ms.openlocfilehash: 62f2f026426346fc1230a2f5743c8cb7d232ec7f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912003"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="b2f77-102">Değişiklikleri otomatik algıla</span><span class="sxs-lookup"><span data-stu-id="b2f77-102">Automatic detect changes</span></span>
<span data-ttu-id="b2f77-103">Çoğu POCO varlık kullanırken belirlenmesi varlığın nasıl değiştiğini (ve bu nedenle hangi güncelleştirmelerin veritabanına gönderilmesi gerekir) değişiklikleri algılama algoritması tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="b2f77-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="b2f77-104">Geçerli özellik değerlerini varlığın varlık sorgulanan ya da ekli anlık görüntüde depolanan özgün özellik değerleri arasındaki farkları algılayarak değişiklikleri works algılayın.</span><span class="sxs-lookup"><span data-stu-id="b2f77-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="b2f77-105">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b2f77-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="b2f77-106">Varsayılan olarak Entity Framework algılama değişiklikler aşağıdaki yöntemler çağrıldığında otomatik olarak gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="b2f77-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="b2f77-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="b2f77-107">DbSet.Find</span></span>  
- <span data-ttu-id="b2f77-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="b2f77-108">DbSet.Local</span></span>  
- <span data-ttu-id="b2f77-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="b2f77-109">DbSet.Add</span></span>  
- <span data-ttu-id="b2f77-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="b2f77-110">DbSet.AddRange</span></span>
- <span data-ttu-id="b2f77-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="b2f77-111">DbSet.Remove</span></span>  
- <span data-ttu-id="b2f77-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="b2f77-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="b2f77-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="b2f77-113">DbSet.Attach</span></span>  
- <span data-ttu-id="b2f77-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="b2f77-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="b2f77-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="b2f77-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="b2f77-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="b2f77-116">DbContext.Entry</span></span>  
- <span data-ttu-id="b2f77-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="b2f77-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="b2f77-118">Değişiklikleri otomatik olarak algılanmasını devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="b2f77-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="b2f77-119">Bağlamınızı içinde çok sayıda varlık izleme ve, aşağıdaki yöntemlerden birini bir döngüde birçok kez çağrılması, değişikliklerin algılanması için döngü süresini devre dışı bırakarak önemli performans geliştirmeleri alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2f77-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="b2f77-120">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b2f77-120">For example:</span></span>  

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

<span data-ttu-id="b2f77-121">Döngü sonra değişiklikleri algılama yeniden etkinleştirmek unutmayın; biz bunu her zaman yeniden etkin döngüde kod bir özel durum oluşturursa bile emin olmak için bir try/finally kullandınız.</span><span class="sxs-lookup"><span data-stu-id="b2f77-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="b2f77-122">Alternatif devre dışı bırakma ve yeniden etkinleştirerek otomatik algılanması değişiklikleri her zaman ve her iki arama bağlamı devre dışı bırakmak için. ChangeTracker.DetectChanges açıkça veya kullanım izleme proxy'leri yapıyorduk değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b2f77-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="b2f77-123">Bu seçeneklerin ikisi de Gelişmiş ve küçük hatalar uygulamanıza kolayca çıkarabilir bunları dikkatli şekilde kullanın.</span><span class="sxs-lookup"><span data-stu-id="b2f77-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="b2f77-124">Çok sayıda nesne bir bağlamdan ekleyip gerekiyorsa DbSet.AddRange ve DbSet.RemoveRange kullanarak göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="b2f77-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="b2f77-125">Ekleme veya kaldırma işlemleri tamamlandıktan sonra bu yöntemleri otomatik olarak değişiklikleri yalnızca bir kez algılar.</span><span class="sxs-lookup"><span data-stu-id="b2f77-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
