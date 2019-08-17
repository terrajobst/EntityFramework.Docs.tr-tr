---
title: Ilgili varlıkları yükleme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: f40034475ed6659b60ab4317605fd1d802218d69
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565324"
---
# <a name="loading-related-entities"></a><span data-ttu-id="65998-102">Ilgili varlıkları yükleme</span><span class="sxs-lookup"><span data-stu-id="65998-102">Loading Related Entities</span></span>
<span data-ttu-id="65998-103">Entity Framework, ilgili veri yükleme, yavaş yükleme ve açık yükleme gibi üç yolu destekler.</span><span class="sxs-lookup"><span data-stu-id="65998-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="65998-104">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="65998-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="65998-105">Ekip yükleme</span><span class="sxs-lookup"><span data-stu-id="65998-105">Eagerly Loading</span></span>  

<span data-ttu-id="65998-106">Eager yüklemesi, bir varlık türü için bir sorgunun aynı zamanda ilgili varlıkları sorgunun bir parçası olarak yüklediği işlemdir.</span><span class="sxs-lookup"><span data-stu-id="65998-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="65998-107">Eager yüklemesi, Include yönteminin kullanımı ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="65998-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="65998-108">Örneğin, aşağıdaki sorgular blogların ve her blogun ilgili tüm gönderilerin yükünü yükler.</span><span class="sxs-lookup"><span data-stu-id="65998-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts
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

<span data-ttu-id="65998-109">Include 'in System. Data. Entity ad alanındaki bir genişletme yöntemi olduğunu unutmayın, bu nedenle bu ad alanını kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="65998-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="65998-110">Birden çok düzey yükleme</span><span class="sxs-lookup"><span data-stu-id="65998-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="65998-111">Ayrıca, birden çok ilgili varlık düzeyini de yüklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="65998-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="65998-112">Aşağıdaki sorgularda, hem koleksiyon hem de başvuru gezinti özellikleri için bunu nasıl yapabilinin örnekleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="65998-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="65998-113">Şu anda hangi ilgili varlıkların yüklendiğini filtrelemek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="65998-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="65998-114">Dahil etme her zaman ilgili varlıkların tümünü alacak.</span><span class="sxs-lookup"><span data-stu-id="65998-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="65998-115">Geç yükleme</span><span class="sxs-lookup"><span data-stu-id="65998-115">Lazy Loading</span></span>  

<span data-ttu-id="65998-116">Geç yükleme, varlığa/varlıklara başvuran bir özelliğe ilk kez erişildiğinde bir varlık veya varlık koleksiyonunun veritabanından otomatik olarak yüklendiğine yönelik bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="65998-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="65998-117">POCO varlık türlerini kullanırken, yavaş yükleme türetilmiş proxy türlerinin örnekleri oluşturularak ve sonra yükleme kancasını eklemek için sanal özellikleri geçersiz kılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="65998-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="65998-118">Örneğin, aşağıda tanımlanan blog varlık sınıfı kullanılırken, gönderi gezintisi özelliği ilk kez erişildiğinde ilgili postalar yüklenir:</span><span class="sxs-lookup"><span data-stu-id="65998-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

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

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="65998-119">Serileştirme için yavaş yüklemeyi kapat</span><span class="sxs-lookup"><span data-stu-id="65998-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="65998-120">Geç yükleme ve serileştirme iyi bir şekilde karıştırmayın ve unutmayın, yavaş yükleme etkin olduğundan, veritabanınızın tamamına yönelik sorgulama yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="65998-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="65998-121">Çoğu serileştiriciler, bir tür örneğindeki her özelliğe erişerek çalışır.</span><span class="sxs-lookup"><span data-stu-id="65998-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="65998-122">Özellik erişimi, yavaş yüklemeyi tetikler, bu nedenle daha fazla varlık serileştirilmiş hale alınır.</span><span class="sxs-lookup"><span data-stu-id="65998-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="65998-123">Bu varlıklar özelliklerine erişilir, hatta daha fazla varlık yüklenir.</span><span class="sxs-lookup"><span data-stu-id="65998-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="65998-124">Bir varlığı seri hale gelmeden önce yavaş yüklemeyi kapatmak iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="65998-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="65998-125">Aşağıdaki bölümlerde bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="65998-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="65998-126">Belirli gezinti özellikleri için yavaş yüklemeyi kapatma</span><span class="sxs-lookup"><span data-stu-id="65998-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="65998-127">Gönderi özelliğinin geç yüklemesi, gönderimler özelliği sanal olmayan bir hale getirilerek kapatılabilir:</span><span class="sxs-lookup"><span data-stu-id="65998-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

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

<span data-ttu-id="65998-128">Gönderi koleksiyonu yüklemesi yine de Eager yüklemesi (bkz. yukarıya *yükleme* ) veya Load yöntemi (bkz. aşağıda *açıkça yükleme* ) kullanılarak elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="65998-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="65998-129">Tüm varlıklar için yavaş yüklemeyi kapat</span><span class="sxs-lookup"><span data-stu-id="65998-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="65998-130">Yapılandırma özelliğinde bir bayrak ayarlanarak, bağlam içindeki tüm varlıklar için yavaş yükleme kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="65998-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="65998-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="65998-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="65998-132">İlgili varlıkların yüklenmesi yine de Eager yüklemesi (bkz. yukarıya *yükleme* ) veya Load yöntemi (bkz. aşağıda *açıkça yükleme* ) kullanılarak elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="65998-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="65998-133">Açıkça yükleme</span><span class="sxs-lookup"><span data-stu-id="65998-133">Explicitly Loading</span></span>  

<span data-ttu-id="65998-134">Yavaş yükleme devre dışı bırakılmış olsa bile, ilgili varlıkların geç yüklenmeye devam edebilir, ancak açık bir çağrıyla yapılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="65998-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="65998-135">Bunu yapmak için, ilgili varlığın girişinde Load yöntemini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="65998-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="65998-136">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="65998-136">For example:</span></span>  

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

<span data-ttu-id="65998-137">Bir varlığın başka bir varlığa bir gezinti özelliği olduğunda başvuru yönteminin kullanılması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="65998-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="65998-138">Diğer taraftan, bir varlık başka varlıkların koleksiyonuna bir gezinti özelliği olduğunda, koleksiyon yöntemi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="65998-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="65998-139">İlgili varlıkları açıkça yüklerken filtre uygulama</span><span class="sxs-lookup"><span data-stu-id="65998-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="65998-140">Sorgu yöntemi, ilgili varlıklar yüklenirken Entity Framework kullanacağı temel sorguya erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="65998-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="65998-141">Daha sonra, ToList, Load, vb. gibi bir LINQ genişletme yöntemine yapılan bir çağrıyla yürütmeden önce sorguya filtre uygulamak için LINQ kullanabilirsiniz. Sorgu yöntemi hem başvuru hem de koleksiyon gezinme özellikleriyle birlikte kullanılabilir, ancak koleksiyonun yalnızca bir bölümünü yüklemek için kullanılabilecek olan koleksiyonlar için en yararlı seçenektir.</span><span class="sxs-lookup"><span data-stu-id="65998-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="65998-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="65998-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```  

<span data-ttu-id="65998-143">Sorgu yöntemi kullanılırken, gezinti özelliği için yavaş yüklemeyi devre dışı bırakmak genellikle en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="65998-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="65998-144">Bunun nedeni, tersi durumda, filtre uygulanmış Sorgu yürütüldükten önce veya sonra yavaş yükleme mekanizması tarafından otomatik olarak yüklenemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="65998-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="65998-145">İlişki bir lambda ifadesi yerine bir dize olarak belirtilebildiği sürece, döndürülen IQueryable bir dize kullanıldığında genel değildir ve bu nedenle, bu durumda herhangi bir şeyin yararlı olması için atama yöntemi gerekir.</span><span class="sxs-lookup"><span data-stu-id="65998-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="65998-146">Sorgu kullanarak ilgili varlıkları yüklemeden Sayın</span><span class="sxs-lookup"><span data-stu-id="65998-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="65998-147">Bazen, tüm bu varlıkları yükleme maliyetini karşılamak zorunda kalmadan veritabanında bulunan başka bir varlıkla ilgili kaç varlık olduğunu öğrenmek faydalı olur.</span><span class="sxs-lookup"><span data-stu-id="65998-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="65998-148">Bunu yapmak için LINQ Count yöntemine sahip sorgu yöntemi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="65998-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="65998-149">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="65998-149">For example:</span></span>  

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
