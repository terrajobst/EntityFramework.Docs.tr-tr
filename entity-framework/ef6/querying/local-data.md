---
title: Yerel veri-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182426"
---
# <a name="local-data"></a><span data-ttu-id="f1cb8-102">Yerel Veriler</span><span class="sxs-lookup"><span data-stu-id="f1cb8-102">Local Data</span></span>
<span data-ttu-id="f1cb8-103">LINQ sorgusunun doğrudan bir DbSet 'e karşı çalıştırılması veritabanına her zaman bir sorgu gönderir, ancak şu anda DbSet. local özelliğini kullanarak bellekteki verilere erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="f1cb8-104">DbContext. Entry ve DbContext. ChangeTracker. Entries yöntemlerini kullanarak varlıklarınız hakkında daha fazla bilgi için EF 'e de erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="f1cb8-105">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="f1cb8-106">Yerel verilere bakmak için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="f1cb8-106">Using Local to look at local data</span></span>  

<span data-ttu-id="f1cb8-107">DbSet 'in yerel özelliği, şu anda bağlam tarafından izlenmekte olan ve silinmiş olarak işaretlenmeyen, küme varlıklarına basit erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="f1cb8-108">Yerel özelliğe erişim hiçbir şekilde bir sorgunun veritabanına gönderilmesine neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="f1cb8-109">Bu, genellikle bir sorgu daha önce gerçekleştirildikten sonra kullanıldığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="f1cb8-110">Yük uzantısı metodu, içeriğin sonuçları izlemesi için bir sorgu yürütmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="f1cb8-111">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-111">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

<span data-ttu-id="f1cb8-112">' ADO.NET blog ' veritabanında iki blogumuz varsa blogID 1 ve ' a Visual Studio blogu ' ile bir blogID 2-şu çıktıyı bekleyebilir:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="f1cb8-113">Bu üç noktayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-113">This illustrates three points:</span></span>  

- <span data-ttu-id="f1cb8-114">Yeni blog ' yeni blogum ', henüz veritabanına kaydedilmese bile yerel koleksiyona dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="f1cb8-115">Veritabanı henüz varlık için gerçek bir anahtar oluşturulmadığı için bu blogun birincil anahtarı sıfır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="f1cb8-116">' ADO.NET blogu ', bağlam tarafından izlenmekte olmasına rağmen yerel koleksiyona dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="f1cb8-117">Bunun nedeni, bunu DbSet 'ten kaldırdığımız ve bu nedenle silinmiş olarak işaretliyoruz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="f1cb8-118">DbSet bir sorgu gerçekleştirmek için kullanıldığında, silinmek üzere işaretlenen blog (ADO.NET blogu) sonuçlara dahil edilir ve henüz veritabanına kaydedilmemiş olan yeni blog (yeni blogum) sonuçlara dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="f1cb8-119">Bunun nedeni, DbSet 'in veritabanına karşı bir sorgu gerçekleştirme ve döndürülen sonuçların her zaman veritabanında neler olduğunu yansıtır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="f1cb8-120">Bağlamdan varlık eklemek ve kaldırmak için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="f1cb8-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="f1cb8-121">DbSet üzerindeki yerel özellik, olayların içeriğiyle eşitlenmiş olarak kalacak şekilde, olayları bağlayan bir [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) döndürür.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="f1cb8-122">Bu, varlıkların yerel koleksiyona ya da DbSet 'e eklenebileceği veya kaldırılabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="f1cb8-123">Ayrıca, yeni varlıkları içeriğine getiren sorguların, yerel koleksiyonun bu varlıklarla güncelleştirilmesine neden olacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="f1cb8-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework")).Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

<span data-ttu-id="f1cb8-125">' Entity-Framework ' ve ' asp.net ' ile etiketlenmiş birkaç gönderi olduğunu varsayarsak, çıktı şuna benzer olabilir:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```console
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

<span data-ttu-id="f1cb8-126">Bu üç noktayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-126">This illustrates three points:</span></span>  

- <span data-ttu-id="f1cb8-127">Yerel koleksiyona eklenen yeni gönderi ' EF 'teki yenilikler ', eklenen durumdaki bağlam tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="f1cb8-128">Bu, SaveChanges çağrıldığında veritabanına eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="f1cb8-129">Yerel koleksiyondan kaldırılan gönderi (EF son kullanım kılavuzu) artık bağlamda silinmiş olarak işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="f1cb8-130">Bu, SaveChanges çağrıldığında veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="f1cb8-131">İkinci sorgu ile bağlam içine yüklenen ek gönderi (ASP.NET yeni başlayanlar kılavuzu), yerel koleksiyona otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="f1cb8-132">Yerel hakkında aklınızda bir son şey, ObservableCollection bir performans olduğundan çok sayıda varlık için harika değildir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="f1cb8-133">Bu nedenle, bağlamdaki binlerce varlıkla uğraşıyorsanız yerel kullanılması önerilmez.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="f1cb8-134">WPF veri bağlaması için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="f1cb8-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="f1cb8-135">DbSet üzerindeki Local özelliği, bir WPF uygulamasında bir ObservableCollection örneği olduğundan doğrudan veri bağlama için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="f1cb8-136">Önceki bölümlerde açıklandığı gibi bu, içeriğin içeriğiyle eşitlenmiş olarak kalacağı ve bağlam içeriğinin otomatik olarak onunla eşitlenmiş kalacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="f1cb8-137">Yerel koleksiyonun, bir veritabanı sorgusuna hiçbir şey neden olmadığı için bağlanacak bir şey olması için verilerle önceden doldurulması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="f1cb8-138">Bu, tam bir WPF veri bağlama örneği için uygun bir yer değildir ancak anahtar öğeleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="f1cb8-139">Bağlama kaynağı ayarlama</span><span class="sxs-lookup"><span data-stu-id="f1cb8-139">Setup a binding source</span></span>  
- <span data-ttu-id="f1cb8-140">Bu dosyayı, kendi ayarladığınız yerel özelliğine bağlayın</span><span class="sxs-lookup"><span data-stu-id="f1cb8-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="f1cb8-141">Veritabanını bir sorgu kullanarak yerel olarak doldurun.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="f1cb8-142">Gezinti özelliklerine WPF bağlama</span><span class="sxs-lookup"><span data-stu-id="f1cb8-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="f1cb8-143">Ana/ayrıntı veri bağlama yapıyorsanız, ayrıntı görünümünü varlıklarınızın bir gezinti özelliğine bağlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="f1cb8-144">Bu işi yapmanın kolay bir yolu, gezinti özelliği için bir ObservableCollection kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="f1cb8-145">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-145">For example:</span></span>  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="f1cb8-146">SaveChanges 'da varlıkları temizlemek için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="f1cb8-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="f1cb8-147">Çoğu durumda, bir gezinti özelliğinden kaldırılan varlıklar bağlamda silinmiş olarak otomatik olarak işaretlenmez.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="f1cb8-148">Örneğin, blog. gönderimleri koleksiyonundan bir post nesnesini kaldırırsanız, SaveChanges çağrıldığında bu gönderi otomatik olarak silinmez.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="f1cb8-149">Bunun silinmeli olması gerekiyorsa, bu salgze varlıklarını bulmanız ve SaveChanges 'ı veya geçersiz kılınan SaveChanges 'un bir parçası olarak bu varlıkları silinmek üzere işaretlemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="f1cb8-150">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-150">For example:</span></span>  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

<span data-ttu-id="f1cb8-151">Yukarıdaki kod, tüm gönderileri bulmak için yerel koleksiyonu kullanır ve silinen bir blog başvurusu olmayan her türlü işareti işaretler.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="f1cb8-152">ToList çağrısı gereklidir çünkü aksi takdirde koleksiyon, numaralandırılmakta olan kaldırma çağrısı tarafından değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="f1cb8-153">Çoğu durumda, ToList kullanmadan doğrudan yerel özelliğe karşı sorgulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="f1cb8-154">Windows Forms veri bağlama için yerel ve ToBindingList kullanma</span><span class="sxs-lookup"><span data-stu-id="f1cb8-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="f1cb8-155">Windows Forms doğrudan ObservableCollection kullanarak tam aslına uygunluk verileri bağlamayı desteklemez.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="f1cb8-156">Ancak, önceki bölümlerde açıklanan tüm avantajları almak için veri bağlama için DbSet yerel özelliğini kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="f1cb8-157">Bu, yerel ObservableCollection tarafından desteklenen bir [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) uygulamasını oluşturan ToBindingList Extension yöntemi aracılığıyla elde edilir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="f1cb8-158">Bu, tam Windows Forms veri bağlama örneği için uygun bir yer değildir ancak anahtar öğeleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="f1cb8-159">Nesne bağlama kaynağı ayarlama</span><span class="sxs-lookup"><span data-stu-id="f1cb8-159">Setup an object binding source</span></span>  
- <span data-ttu-id="f1cb8-160">Yerel. ToBindingList () kullanarak kendi ayarladığınız Yerel özelliğe bağlayın</span><span class="sxs-lookup"><span data-stu-id="f1cb8-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="f1cb8-161">Veritabanını bir sorgu kullanarak yerel olarak doldur</span><span class="sxs-lookup"><span data-stu-id="f1cb8-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="f1cb8-162">İzlenen varlıklar hakkında ayrıntılı bilgi alma</span><span class="sxs-lookup"><span data-stu-id="f1cb8-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="f1cb8-163">Bu dizideki birçok örnek, bir varlık için DbEntityEntry örneği döndürmek üzere entry metodunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="f1cb8-164">Bu giriş nesnesi daha sonra varlık hakkında geçerli durumu gibi bilgi toplamak için başlangıç noktası olarak, varlık üzerinde de ilgili bir varlığı açıkça yükleme gibi işlemler gerçekleştirmek için de çalışır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="f1cb8-165">Giriş yöntemleri, bağlam tarafından izlenen birçok veya tüm varlık için DbEntityEntry nesneleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="f1cb8-166">Bu, yalnızca tek bir giriş yerine birçok varlık üzerinde bilgi toplamanıza veya işlem gerçekleştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="f1cb8-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

<span data-ttu-id="f1cb8-168">Örneğin, bu sınıfların her ikisi de IPerson arabirimini uygulayan bir yazar ve okuyucu sınıfı sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

<span data-ttu-id="f1cb8-169">Veritabanında aşağıdaki verilere sahip olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="f1cb8-170">BlogID = 1 ve ad = ' ADO.NET blogu ' ile blog</span><span class="sxs-lookup"><span data-stu-id="f1cb8-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="f1cb8-171">BlogID = 2 ve ad = ' Visual Studio blogu ' ile blog</span><span class="sxs-lookup"><span data-stu-id="f1cb8-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="f1cb8-172">BlogID = 3 ve ad = ' .NET Framework blog ' ile blog</span><span class="sxs-lookup"><span data-stu-id="f1cb8-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="f1cb8-173">AuthorId = 1 ve Name = ' ali Bloggs ' olan yazar</span><span class="sxs-lookup"><span data-stu-id="f1cb8-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="f1cb8-174">Readerıd = 1 ve Name = ' John tikan ' ile okuyucu</span><span class="sxs-lookup"><span data-stu-id="f1cb8-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="f1cb8-175">Kodu çalıştırmanın çıkışı şöyle olacaktır:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-175">The output from running the code would be:</span></span>  

```console
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

<span data-ttu-id="f1cb8-176">Bu örneklerde çeşitli noktaları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f1cb8-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="f1cb8-177">Giriş yöntemleri, silinen dahil olmak üzere tüm durumlarda varlıkların girişlerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="f1cb8-178">Bunu, silinen varlıkları dışlayan yerel ile karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="f1cb8-179">Tüm varlık türlerinin girişleri, genel olmayan girişler yöntemi kullanıldığında döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="f1cb8-180">Genel girişler yöntemi kullanılan girişler yalnızca genel türün örnekleri olan varlıklar için döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="f1cb8-181">Bu, tüm blogların girişlerini almak için yukarıda kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="f1cb8-182">Ayrıca, IPerson uygulayan tüm varlıkların girişlerini almak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="f1cb8-183">Bu, genel türün gerçek bir varlık türü olması gerektiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="f1cb8-184">LINQ to Objects döndürülen sonuçları filtrelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="f1cb8-185">Bu, değiştirildikleri sürece herhangi bir türdeki varlıkları bulmak için yukarıda kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="f1cb8-186">DbEntityEntry örneklerinin her zaman null olmayan bir varlık içerdiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="f1cb8-187">İlişki girişleri ve saplama girdileri, DbEntityEntry örnekleri olarak temsil edilmez, bu nedenle bunlara filtre uygulamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f1cb8-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
