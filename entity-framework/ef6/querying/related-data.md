---
title: Varlıklar - EF6 yükleme ilgili
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
caps.latest.revision: 3
ms.openlocfilehash: e7adc9aea11a7a8e9b87b4f9e9120aa7316588db
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912759"
---
# <a name="loading-related-entities"></a><span data-ttu-id="0dbd5-102">İlgili varlıklar yükleniyor</span><span class="sxs-lookup"><span data-stu-id="0dbd5-102">Loading Related Entities</span></span>
<span data-ttu-id="0dbd5-103">Entity Framework, ilgili verileri - yüklenirken eager, yavaş yükleniyor ve açık yükleme yüklemek için üç yol destekler.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="0dbd5-104">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="0dbd5-105">Eagerly yükleniyor</span><span class="sxs-lookup"><span data-stu-id="0dbd5-105">Eagerly Loading</span></span>  

<span data-ttu-id="0dbd5-106">İstekli yükleme alınabildiği bir varlık türü için bir sorgu ayrıca ilgili varlıkları sorgunun bir parçası yükleyen bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="0dbd5-107">İstekli yükleme dahil etme yöntemini kullanarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="0dbd5-108">Örneğin, aşağıdaki sorguları, bloglar ve her blog için ilgili tüm gönderileri yükler.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

<span data-ttu-id="0dbd5-109">INCLUDE System.Data.Entity ad alanındaki bir genişletme yöntemi olduğunu unutmayın kadar bu ad alanı kullanıyorsanız emin olun.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="0dbd5-110">Birden çok düzeyi eagerly yükleniyor</span><span class="sxs-lookup"><span data-stu-id="0dbd5-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="0dbd5-111">İlgili varlıkları birden fazla düzeyde eagerly yüklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="0dbd5-112">Aşağıdaki sorgu, toplama ve başvuru Gezinti özellikleri için bunun nasıl yapılacağını örnekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="0dbd5-113">Şu anda hangi ilgili varlıkları yüklenen filtre mümkün olmadığı anlamına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="0dbd5-114">Dahil ilgili tüm varlıklar her zaman getirin.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="0dbd5-115">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="0dbd5-115">Lazy Loading</span></span>  

<span data-ttu-id="0dbd5-116">Yavaş yükleniyor verebileceğiniz bir varlık veya varlık koleksiyonunu otomatik olarak veritabanından varlık/varlıkları başvuran bir özelliğine erişinceye ilk kez yüklenen bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="0dbd5-117">POCO varlık türleri kullanırken, yavaş yükleniyor proxy türetilen türlerin örneklerini oluşturmak ve ardından yükleme kanca eklemek için sanal özellikleri geçersiz kılma tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="0dbd5-118">Örneğin, aşağıda tanımlanan Blog varlık sınıfı kullanırken gönderileri Gezinti özelliğine erişinceye ilk kez ilgili gönderileri yüklenecektir:</span><span class="sxs-lookup"><span data-stu-id="0dbd5-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="0dbd5-119">Kapatmak için serileştirme yüklenirken yavaş Aç</span><span class="sxs-lookup"><span data-stu-id="0dbd5-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="0dbd5-120">Yavaş yükleniyor ve Serileştirme iyi karıştırmayın ve dikkatli olmazsanız veritabanınız için sorgulama yavaş yükleniyor yalnızca etkin olduğundan yukarı sonlandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="0dbd5-121">Çoğu seri hale getiricileri genişletme türünün bir örneğinde her bir özellik erişerek çalışır.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="0dbd5-122">Daha fazla varlık serileştirilen şekilde yavaş yükleniyor, özellik erişimi tetikler.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="0dbd5-123">Daha fazla varlık yüklendi ve bu varlıkların özellikleri erişilir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="0dbd5-124">Kapalı bir varlık seri hale getirme önce yükleme yavaş etkinleştirmek için iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="0dbd5-125">Aşağıdaki bölümlerde bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="0dbd5-126">Belirli bir gezinti özellikleri yükleniyor yavaş kapatma</span><span class="sxs-lookup"><span data-stu-id="0dbd5-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="0dbd5-127">Yavaş yükleniyor gönderileri koleksiyonun gönderileri özelliği sanal olmayan hale getirerek kapatılabilir:</span><span class="sxs-lookup"><span data-stu-id="0dbd5-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="0dbd5-128">Yükleniyor gönderiler koleksiyon hala istekli yükleme kullanarak gerçekleştirilebilir (bkz *Eagerly Yükleniyor* yukarıda) veya Load yöntemi (bkz *açıkça Yükleniyor* aşağıda).</span><span class="sxs-lookup"><span data-stu-id="0dbd5-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="0dbd5-129">Tüm varlıklar için yükleme yavaş Kapat</span><span class="sxs-lookup"><span data-stu-id="0dbd5-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="0dbd5-130">Gecikmeli yükleme bağlamı tüm varlıklar için yapılandırma özelliği bir bayrak ayarlayarak kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="0dbd5-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0dbd5-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="0dbd5-132">İlgili varlıkları yükleme yine de gerçekleştirilebilir istekli yükleme kullanarak (bkz *Eagerly Yükleniyor* yukarıda) veya Load yöntemi (bkz *açıkça yüklenirken* aşağıda).</span><span class="sxs-lookup"><span data-stu-id="0dbd5-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="0dbd5-133">Açıkça yükleniyor</span><span class="sxs-lookup"><span data-stu-id="0dbd5-133">Explicitly Loading</span></span>  

<span data-ttu-id="0dbd5-134">Devre dışı bile yavaş yükleyerek, gevşek ilgili varlıkları yükleme yine de mümkündür, ancak açık bir çağrı ile yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="0dbd5-135">Bunu yapmak için ilgili varlığın girişinde yükleme yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="0dbd5-136">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0dbd5-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="0dbd5-137">Bir varlık başka bir tek bir varlık Gezinti özelliğine sahip olduğunda başvuru yöntemi kullanılması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="0dbd5-138">Öte yandan, bir varlığın bir gezinti özelliği için diğer varlıklar koleksiyonu olduğunda koleksiyon yöntemi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="0dbd5-139">Açıkça ilgili varlıkları yüklenirken uygulanan filtreler</span><span class="sxs-lookup"><span data-stu-id="0dbd5-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="0dbd5-140">Sorgu yöntemine, Entity Framework, ilgili varlıkları yüklerken kullanacağınız temel alınan sorgunun erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="0dbd5-141">Ardından LINQ ToList, yük, vb. gibi LINQ genişletme yöntemine bir çağrıyla yürütmeden önce sorguya filtre uygulamak için de kullanabilirsiniz. Sorgu yöntemi ile başvuru hem koleksiyon Gezinti özellikleri kullanılabilir, ancak burada bu koleksiyonun parçası yalnızca yüklenecek kullanılabilir koleksiyonlar için en kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="0dbd5-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0dbd5-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

<span data-ttu-id="0dbd5-143">Sorgu yöntemini kullanırken gezinti özelliği için yükleme yavaş kapatmak en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="0dbd5-144">Bu durum, aksi takdirde tüm koleksiyon otomatik olarak yavaş yükleme mekanizması tarafından önce veya filtrelenmiş sorgu yürütüldükten sonra yüklenir çünkü.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="0dbd5-145">İlişki yerine bir lambda ifadesi bir dize olarak belirtilebilir, ancak döndürülen Iqueryable dize kullanıldığında ve faydalı bir şey ile yapılabilir önce Cast yöntemi genellikle gerekli genel olmadığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="0dbd5-146">İlgili varlıkları yükleme olmadan saymak için sorgu kullanma</span><span class="sxs-lookup"><span data-stu-id="0dbd5-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="0dbd5-147">Bazen kaç varlıkları veritabanında başka bir varlık yüklenirken bu varlıkların maliyeti olmaksızın ilgili bilmek de yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="0dbd5-148">Bunu yapmak için sorgu yöntemi ile LINQ Count yöntemi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0dbd5-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="0dbd5-149">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0dbd5-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
