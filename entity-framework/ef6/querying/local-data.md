---
title: Yerel veriler - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: 400b9e1337edac1b9fa4f0ec9e1384ca58aa2fbc
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490460"
---
# <a name="local-data"></a><span data-ttu-id="eb514-102">Yerel veriler</span><span class="sxs-lookup"><span data-stu-id="eb514-102">Local Data</span></span>
<span data-ttu-id="eb514-103">Doğrudan olan DB karşı çalışan bir LINQ Sorgu her zaman bir sorgu için veritabanı gönderir, ancak şu anda DbSet.Local özelliğini kullanarak bellek içi verileri erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb514-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="eb514-104">Varlıklarınızı DbContext.Entry ve DbContext.ChangeTracker.Entries yöntemleri kullanma hakkında ek bilgi EF izleme de erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb514-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="eb514-105">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="eb514-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="eb514-106">Yerel verilere bakmak için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="eb514-106">Using Local to look at local data</span></span>  

<span data-ttu-id="eb514-107">Yerel bir özellik olan DB, şu anda bağlam izlendiğini ve silinmiş ' işaretlenmemiş varlıkları kümesinin basit erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb514-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="eb514-108">Yerel özellik erişim asla veritabanına gönderilmek üzere bir sorgu neden olur.</span><span class="sxs-lookup"><span data-stu-id="eb514-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="eb514-109">Bu, bir sorgu zaten gerçekleştirildikten sonra genellikle kullanıldığını anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="eb514-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="eb514-110">Yük genişletme yöntemi, böylece sonuçları içerik izleyen bir sorgu yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eb514-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="eb514-111">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="eb514-111">For example:</span></span>  

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

<span data-ttu-id="eb514-112">-'ADO.NET Blog' bir BlogId 1 ile - ve 'Visual Studio Blog' 2'in bir BlogId ile veritabanında iki blog vardı, aşağıdaki çıktıyı bekliyoruz:</span><span class="sxs-lookup"><span data-stu-id="eb514-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="eb514-113">Bu, üç nokta gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="eb514-113">This illustrates three points:</span></span>  

- <span data-ttu-id="eb514-114">Bunu henüz veritabanına kaydedilmedi olsa bile yeni blog 'My Yeni Blog' yerel koleksiyona dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="eb514-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="eb514-115">Veritabanı henüz varlık için gerçek bir anahtar oluşturmamıştır için bu blog sıfır birincil bir anahtar içeriyor.</span><span class="sxs-lookup"><span data-stu-id="eb514-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="eb514-116">Hala bağlam tarafından izleniyor olsa bile 'ADO.NET Blog' yerel koleksiyona dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="eb514-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="eb514-117">Böylece bu silindi olarak işaretleme olan DB kaldırdık olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="eb514-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="eb514-118">Bir sorguyu gerçekleştirmek için olan DB kullanıldığında (ADO.NET Blog) silinmek üzere işaretlenmiş blog sonuçlarda yer ve veritabanına kaydedilmedi yeni blog (My Yeni Blog) sonuçlarda yer almaz.</span><span class="sxs-lookup"><span data-stu-id="eb514-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="eb514-119">Olan DB veritabanında bir sorgu gerçekleştiriyor ve her zaman döndürülen sonuçları veritabanında nedir yansıtmak için budur.</span><span class="sxs-lookup"><span data-stu-id="eb514-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="eb514-120">Varlıkları bağlamdan ekleyip için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="eb514-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="eb514-121">Yerel özelliği olan DB döndürür bir [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) eşitleme bağlamı içeriğini kalacak şekilde ölçekledikçe olayları.</span><span class="sxs-lookup"><span data-stu-id="eb514-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="eb514-122">Bu varlıklar eklenebilir veya yerel koleksiyon veya olan DB kaldırılması gerektiğini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="eb514-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="eb514-123">Ayrıca, bu varlıklarla güncelleştirilen yerel koleksiyonu bağlamına yeni varlıkları getiren sorgular sonuçlanır anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="eb514-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="eb514-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="eb514-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

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

<span data-ttu-id="eb514-125">'Entity framework' ve 'asp.net' çıkışı ile etiketlenmiş birkaç gönderileri vardı varsayarsak şunun gibi görünebilir:</span><span class="sxs-lookup"><span data-stu-id="eb514-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```  
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

<span data-ttu-id="eb514-126">Bu, üç nokta gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="eb514-126">This illustrates three points:</span></span>  

- <span data-ttu-id="eb514-127">'EF yenilikler' yeni gönderiye eklenen yerel koleksiyonu eklendi durumu bağlamda tarafından izlenen olur.</span><span class="sxs-lookup"><span data-stu-id="eb514-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="eb514-128">SaveChanges çağrıldığında bu nedenle veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb514-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="eb514-129">(EF Başlangıç Kılavuzu) yerel koleksiyondan kaldırıldı post artık bağlam içinde silindi olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="eb514-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="eb514-130">SaveChanges çağrıldığında bu nedenle veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="eb514-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="eb514-131">İkinci sorgu bağlamla yüklenen ek post (ASP.NET Başlangıç Kılavuzu) otomatik olarak yerel koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="eb514-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="eb514-132">ObservableCollection performans varlıklar çok sayıda harika değil, çünkü yerel hakkında dikkat edilecek son bir şey olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="eb514-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="eb514-133">Bu nedenle, bağlam içinde binlerce varlığın ile uğraşıyorsanız yerel bildirilmesi olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="eb514-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="eb514-134">WPF veri bağlama için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="eb514-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="eb514-135">ObservableCollection örneği olduğundan doğrudan WPF uygulamasında veri bağlama için olan DB yerel özelliği kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eb514-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="eb514-136">Bu otomatik olarak kullanacağı anlamına gelir önceki bölümlerde açıklandığı gibi eşitleme bağlamı içeriğinden haberdar olun ve bağlama içeriğini otomatik olarak ile eşitlenmiş durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="eb514-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="eb514-137">Yerel koleksiyon için yerel veritabanı sorgusu hiçbir zaman neden olduğundan bağlamak için herhangi bir şey olması burada verilerle önceden doldurmak gereken unutmayın.</span><span class="sxs-lookup"><span data-stu-id="eb514-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="eb514-138">Bu tam bir WPF verilerini bağlama örneği için uygun bir yere değildir ancak temel öğeleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="eb514-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="eb514-139">Kurulum bağlama kaynağı</span><span class="sxs-lookup"><span data-stu-id="eb514-139">Setup a binding source</span></span>  
- <span data-ttu-id="eb514-140">Yerel kümenize özelliği için bağlama</span><span class="sxs-lookup"><span data-stu-id="eb514-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="eb514-141">Yerel veritabanına bir sorgu kullanarak doldurun.</span><span class="sxs-lookup"><span data-stu-id="eb514-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="eb514-142">WPF bağlama için Gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="eb514-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="eb514-143">Sizin yapmanız durumunda, ana/ayrıntı veri ayrıntılı görünüm birinin varlıklarınızdaki her gezinti özelliği için bağlama isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb514-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="eb514-144">Bunun çalışmasını sağlamak için kolay bir yolu, gezinti özelliği için ObservableCollection kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="eb514-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="eb514-145">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="eb514-145">For example:</span></span>  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="eb514-146">SaveChanges varlıklarda temizlemek için yerel kullanma</span><span class="sxs-lookup"><span data-stu-id="eb514-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="eb514-147">Çoğu durumda bir gezinti özelliği kaldırıldı varlıkları otomatik olarak bağlamında silindi olarak işaretlenmez.</span><span class="sxs-lookup"><span data-stu-id="eb514-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="eb514-148">Gönderin, ardından bir Post nesnesi Blog.Posts koleksiyondan kaldırırsanız SaveChanges çağrıldığında Örneğin, otomatik olarak silinmez.</span><span class="sxs-lookup"><span data-stu-id="eb514-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="eb514-149">Ardından silinmesi gerekiyorsa bu Sallantıdaki varlıkları bulun ve bunları SaveChanges çağırmadan önce veya geçersiz kılınan bir SaveChanges bir parçası olarak silindi olarak işaretle gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="eb514-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="eb514-150">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="eb514-150">For example:</span></span>  

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

<span data-ttu-id="eb514-151">Yukarıdaki kod, tüm gönderileri ve herhangi bir blog başvurusu silindi olarak yok işaretleri bulmak için yerel koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="eb514-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="eb514-152">ToList çağrı gereklidir çünkü koleksiyon Kaldır tarafından aksi durumda değiştirilecek numaralandırılan çağırın.</span><span class="sxs-lookup"><span data-stu-id="eb514-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="eb514-153">Diğer çoğu durumda ToList kullanmadan önce yerel özellik karşı doğrudan sorgulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb514-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="eb514-154">Yerel ve ToBindingList için Windows Forms veri bağlama işlemini kullanma</span><span class="sxs-lookup"><span data-stu-id="eb514-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="eb514-155">Windows Forms kullanarak doğrudan ObservableCollection tam uygunlukta veri bağlamayı desteklemez.</span><span class="sxs-lookup"><span data-stu-id="eb514-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="eb514-156">Ancak, önceki bölümlerde açıklanan tüm avantajlarından yararlanabilmek için veri bağlama için olan DB yerel özelliği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb514-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="eb514-157">Bu oluşturan ToBindingList uzantı yöntemiyle yaratılabilir sağlanır bir [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) alacağından yerel ObservableCollection tarafından.</span><span class="sxs-lookup"><span data-stu-id="eb514-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="eb514-158">Bu tam bir Windows Forms veri bağlama örneği için uygun bir yere değildir, ancak temel öğeleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="eb514-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="eb514-159">Bir nesne bağlama kaynağı ayarla</span><span class="sxs-lookup"><span data-stu-id="eb514-159">Setup an object binding source</span></span>  
- <span data-ttu-id="eb514-160">Yerel kümenize Local.ToBindingList() özelliğine bağlama</span><span class="sxs-lookup"><span data-stu-id="eb514-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="eb514-161">Veritabanına bir sorgu kullanarak yerel Doldur</span><span class="sxs-lookup"><span data-stu-id="eb514-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="eb514-162">İzlenen varlıkları hakkında ayrıntılı bilgi alma</span><span class="sxs-lookup"><span data-stu-id="eb514-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="eb514-163">Bu serideki örneklerin çoğu DbEntityEntry örneği bir varlık için döndürülecek giriş yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb514-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="eb514-164">Bu giriş nesnesi, ardından geçerli durumu gibi varlık hakkında bilgi toplamak için yanı sıra bir ilgili varlığa açıkça yükleme gibi varlık üzerinde işlem gerçekleştirmek için başlangıç noktası olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="eb514-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="eb514-165">Giriş yöntemleri, bağlam tarafından izleniyor çoğu veya tüm varlıklar için DbEntityEntry nesneleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="eb514-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="eb514-166">Bu, bilgi toplamak veya birçok varlığın yalnızca tek bir giriş yazmak yerine işlemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb514-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="eb514-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="eb514-167">For example:</span></span>  

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

<span data-ttu-id="eb514-168">Bir yazar ve okuyucu sınıfı örnek sunuyoruz - bu sınıfların her ikisi de IPerson arabirimini uygulayan göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="eb514-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

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

<span data-ttu-id="eb514-169">Aşağıdaki veriler bir veritabanında sahibiz varsayalım:</span><span class="sxs-lookup"><span data-stu-id="eb514-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="eb514-170">Blog BlogId ile = 1 ve adı 'ADO.NET Blog' =</span><span class="sxs-lookup"><span data-stu-id="eb514-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="eb514-171">Blog BlogId ile = 2 ve adı 'Visual Studio Blog' =</span><span class="sxs-lookup"><span data-stu-id="eb514-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="eb514-172">Blog BlogId ile 3 ve adı = '.NET Framework blogu' =</span><span class="sxs-lookup"><span data-stu-id="eb514-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="eb514-173">Yazar ile AuthorId = 1 ve adı 'ALi Bloggs' =</span><span class="sxs-lookup"><span data-stu-id="eb514-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="eb514-174">Okuyucu ile ReaderId = 1 ve ad = 'Turgay elmas'</span><span class="sxs-lookup"><span data-stu-id="eb514-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="eb514-175">Bir kod çalıştırmasını çıktı aşağıdaki gibi olur:</span><span class="sxs-lookup"><span data-stu-id="eb514-175">The output from running the code would be:</span></span>  

```  
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

<span data-ttu-id="eb514-176">Bu örnekler birkaç nokta gösterir:</span><span class="sxs-lookup"><span data-stu-id="eb514-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="eb514-177">Giriş yöntemleri girişleri varlıklar için silinen dahil olmak üzere tüm durumlarda döndürür.</span><span class="sxs-lookup"><span data-stu-id="eb514-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="eb514-178">Karşılaştırmak için bu dışlar yerel varlıkları silindi.</span><span class="sxs-lookup"><span data-stu-id="eb514-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="eb514-179">Genel olmayan girişler yöntemi kullanıldığında tüm varlık türlerini girişlerinde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="eb514-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="eb514-180">Genel girişleri yöntemi kullanıldığında girişleri genel tür örneği olan varlıklar için yalnızca döndürülür.</span><span class="sxs-lookup"><span data-stu-id="eb514-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="eb514-181">Bu yukarıdaki tüm blogları girişlerini almak için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="eb514-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="eb514-182">Ayrıca, girişlerini IPerson uygulamak için tüm varlıkları almak için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="eb514-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="eb514-183">Bu, genel tür, gerçek varlık türü yok gösterir.</span><span class="sxs-lookup"><span data-stu-id="eb514-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="eb514-184">Nesnelere LINQ, döndürülen sonuçları filtrelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eb514-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="eb514-185">Bunu yukarıda değiştirilmeden sürece herhangi bir türde varlıkları bulmak için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="eb514-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="eb514-186">DbEntityEntry örneklerinin her zaman null olmayan bir varlık içerdiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="eb514-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="eb514-187">Bu filtre gerekmez. Bu nedenle ilişki girişlerinin ve saplama girdileri DbEntityEntry örnekleri olarak temsil edilmez.</span><span class="sxs-lookup"><span data-stu-id="eb514-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
