---
title: Ilgili verileri yükleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4bf9598f9b7e74c2835d3926215de9a7ef4e6f96
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921799"
---
# <a name="loading-related-data"></a><span data-ttu-id="6a6e5-102">Ilgili verileri yükleme</span><span class="sxs-lookup"><span data-stu-id="6a6e5-102">Loading Related Data</span></span>

<span data-ttu-id="6a6e5-103">Entity Framework Core, ilişkili varlıkları yüklemek için modelinizdeki gezinti özelliklerini kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="6a6e5-104">İlgili verileri yüklemek için kullanılan üç ortak O/RM deseni vardır.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="6a6e5-105">**Eager yüklemesi** , ilgili verilerin ilk sorgunun parçası olarak veritabanından yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="6a6e5-106">**Açık yükleme** , ilgili verilerin daha sonra veritabanından açıkça yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="6a6e5-107">**Yavaş yükleme** , gezinti özelliğine erişildiğinde ilgili verilerin veritabanından saydam olarak yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="6a6e5-108">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="6a6e5-109">ekip yükleme</span><span class="sxs-lookup"><span data-stu-id="6a6e5-109">Eager loading</span></span>

<span data-ttu-id="6a6e5-110">Sorgu sonuçlarına dahil edilecek `Include` ilgili verileri belirtmek için yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="6a6e5-111">Aşağıdaki örnekte, sonuçlarda döndürülen blogların `Posts` özelliği ilgili gönderileriyle doldurulmuş olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="6a6e5-112">Entity Framework Core, gezinti özelliklerini daha önce bağlam örneğine yüklenmiş diğer varlıklara otomatik olarak düzeltir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="6a6e5-113">Bu nedenle, bir gezinti özelliği için verileri açıkça bulundurmasanız bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="6a6e5-114">Tek bir sorgudaki birden fazla ilişkilerden ilgili verileri dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="6a6e5-115">Birden çok düzey dahil</span><span class="sxs-lookup"><span data-stu-id="6a6e5-115">Including multiple levels</span></span>

<span data-ttu-id="6a6e5-116">`ThenInclude` Yöntemini kullanarak ilgili verilerin birden fazla düzeyini dahil etmek için ilişkilerde ayrıntıya gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="6a6e5-117">Aşağıdaki örnek tüm blogları, ilgili yayınlarını ve her gönderiye ait yazarı yükler.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="6a6e5-118">Visual Studio 'nun geçerli sürümleri yanlış kod tamamlama seçenekleri sunar ve bir koleksiyon gezintisi özelliğinden sonra `ThenInclude` yöntemi kullanılırken, doğru ifadelerin sözdizimi hatalarıyla işaretlenmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="6a6e5-119">Bu, tarihinde https://github.com/dotnet/roslyn/issues/8237 izlenen bir IntelliSense hata belirtisidir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="6a6e5-120">Kodun doğru olduğu ve başarıyla derlenemediği sürece bu, söz dizimi hatalarının yoksayılıp yoksayılması güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="6a6e5-121">Daha fazla ilgili veri düzeyi dahil `ThenInclude` devam etmek için birden çok çağrısı zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="6a6e5-122">Aynı sorguda birden çok düzeyden ve birden çok kökten ilgili verileri dahil etmek için tüm bunları birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="6a6e5-123">Dahil edilen varlıklardan biri için birden fazla ilgili varlık eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="6a6e5-124">Örneğin `Blogs`, sorgulama `Author` `Tags` yaparken, ve ' nin`Posts`her ikisini de dahil etmek istersiniz. `Posts`</span><span class="sxs-lookup"><span data-stu-id="6a6e5-124">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="6a6e5-125">Bunu yapmak için, kökden başlayarak her bir içerme yolunu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="6a6e5-126">Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="6a6e5-127">Bu, gereksiz birleşimler alacağınız anlamına gelmez; Çoğu durumda, SQL oluştururken EF birleştirmeleri birleştirecek.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="6a6e5-128">Türetilmiş türlere dahil et</span><span class="sxs-lookup"><span data-stu-id="6a6e5-128">Include on derived types</span></span>

<span data-ttu-id="6a6e5-129">Yalnızca ve `Include` `ThenInclude`kullanarak türetilmiş bir tür üzerinde tanımlanan gezintilerden ilgili verileri dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="6a6e5-130">Aşağıdaki model veriliyor:</span><span class="sxs-lookup"><span data-stu-id="6a6e5-130">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="6a6e5-131">Öğrenciler olan tüm kişilerin gezinmesininiçerikleri,çeşitlidesenlerkullanılarakoluşturulabilir:`School`</span><span class="sxs-lookup"><span data-stu-id="6a6e5-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="6a6e5-132">cast kullanma</span><span class="sxs-lookup"><span data-stu-id="6a6e5-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="6a6e5-133">Using `as` işleci</span><span class="sxs-lookup"><span data-stu-id="6a6e5-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="6a6e5-134">türü parametre alan `Include` aşırı yüklemesini kullanma`string`</span><span class="sxs-lookup"><span data-stu-id="6a6e5-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="6a6e5-135">Yoksayılan ekler</span><span class="sxs-lookup"><span data-stu-id="6a6e5-135">Ignored includes</span></span>

<span data-ttu-id="6a6e5-136">Sorguyu artık sorgunun başladığı varlık türünün örneklerini döndürmez şekilde değiştirirseniz, içerme işleçleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="6a6e5-137">Aşağıdaki örnekte, Include işleçleri öğesine dayalıdır `Blog`, ancak `Select` sonra işleci, anonim bir tür döndürecek şekilde sorgu değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="6a6e5-138">Bu durumda, içerme işleçlerinin hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="6a6e5-139">Varsayılan olarak, içerme işleçleri göz ardı edildiğinde EF Core bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="6a6e5-140">Günlüğe kaydetme çıkışını görüntüleme hakkında daha fazla bilgi için [günlüğe](../miscellaneous/logging.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="6a6e5-141">Ekleme veya hiçbir şey yapma için bir içerme işlecinin yoksayılması durumunda davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="6a6e5-142">Bu işlem, bağlam için seçenekleri ayarlarken yapılır-içinde `DbContext.OnConfiguring` `Startup.cs` veya ASP.NET Core kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="6a6e5-143">açık yükleme</span><span class="sxs-lookup"><span data-stu-id="6a6e5-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="6a6e5-144">Bu özellik EF Core 1,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="6a6e5-145">`DbContext.Entry(...)` API aracılığıyla bir gezinti özelliğini açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="6a6e5-146">Ayrıca, ilişkili varlıkları döndüren ayrı bir sorgu yürüterek bir gezinti özelliğini açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="6a6e5-147">Değişiklik izleme etkinse, bir varlık yüklenirken EF Core, önceden yüklenmiş olan varlıkların gezinti özelliklerini otomatik olarak ayarlar ve bu, önceden yüklenmiş varlıkların gezinti özelliklerini, daha önce yüklenmiş varlıklara başvuracak şekilde ayarlar. Yeni yüklenen varlık.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="6a6e5-148">İlgili varlıkları sorgulama</span><span class="sxs-lookup"><span data-stu-id="6a6e5-148">Querying related entities</span></span>

<span data-ttu-id="6a6e5-149">Ayrıca, bir gezinti özelliğinin içeriğini temsil eden bir LINQ sorgusu da edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="6a6e5-150">Bu, ilgili varlıklar üzerinde bir toplama işlecini belleğe yüklemeden çalıştırmak gibi işlemleri yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="6a6e5-151">Ayrıca, hangi ilgili varlıkların belleğe yükleneceğini de filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="6a6e5-152">geç yükleme</span><span class="sxs-lookup"><span data-stu-id="6a6e5-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="6a6e5-153">Bu özellik EF Core 2,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="6a6e5-154">Geç yüklemeyi kullanmanın en basit yolu, [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paketini yüklemek ve ' a çağrı `UseLazyLoadingProxies`yaparak bunu yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="6a6e5-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6a6e5-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="6a6e5-156">AddDbContext kullanırken:</span><span class="sxs-lookup"><span data-stu-id="6a6e5-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="6a6e5-157">EF Core, geçersiz kılınabilen herhangi bir gezinti özelliği için yavaş yüklemeyi etkinleştirir-Yani, `virtual` ve öğesinden devralınabilen bir sınıf üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="6a6e5-158">Örneğin, aşağıdaki varlıklarda `Post.Blog` ve `Blog.Posts` gezinti özellikleri yavaş yüklenir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="6a6e5-159">Proxy olmadan yavaş yükleme</span><span class="sxs-lookup"><span data-stu-id="6a6e5-159">Lazy loading without proxies</span></span>

<span data-ttu-id="6a6e5-160">Yavaş yükleme proxy 'leri, `ILazyLoader` [varlık türü oluşturucuları](../modeling/constructors.md)bölümünde açıklandığı gibi hizmeti bir varlığa ekleme.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="6a6e5-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6a6e5-161">For example:</span></span>
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="6a6e5-162">Bu, varlık türlerinin veya gezinti özelliklerinden devralınmasını gerektirmez ve ile `new` oluşturulan varlık örneklerinin bir içeriğe eklendikten sonra geç yükleme yapmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="6a6e5-163">Ancak, `ILazyLoader` [Microsoft. entityframeworkcore. soyutlamalar](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) paketinde tanımlanan hizmete bir başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="6a6e5-164">Bu paket, buna bağlı olarak çok az etkisi olması için en az bir tür kümesi içerir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="6a6e5-165">Ancak, varlık türlerindeki tüm EF Core paketlerine bağlı olarak tamamen kaçınmak için, `ILazyLoader.Load` yöntemi bir temsilci olarak eklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="6a6e5-166">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6a6e5-166">For example:</span></span>
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="6a6e5-167">Yukarıdaki kod, temsilciyi bir `Load` bit temizleyici kullanarak yapmak için bir genişletme yöntemi kullanır:</span><span class="sxs-lookup"><span data-stu-id="6a6e5-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="6a6e5-168">Yavaş yükleme temsilcisinin Oluşturucu parametresine "lazyLoader" adı verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="6a6e5-169">Daha sonraki bir sürüm için bu değerden farklı bir ad kullanacak şekilde yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="6a6e5-170">İlgili veriler ve serileştirme</span><span class="sxs-lookup"><span data-stu-id="6a6e5-170">Related data and serialization</span></span>

<span data-ttu-id="6a6e5-171">EF Core, gezinti özelliklerini otomatik olarak düzeltiğinden, nesne grafiğinizde döngülerle bitilecektir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="6a6e5-172">Örneğin, bir blog ve ilgili gönderimler yükleme, bir gönderi koleksiyonuna başvuran bir blog nesnesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="6a6e5-173">Bu gönderilerin her birinin bloguna bir başvurusu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="6a6e5-174">Bazı serileştirme çerçeveleri bu tür döngülerine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="6a6e5-175">Örneğin, bir döngüyle karşılaşılırsa Json.NET aşağıdaki özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="6a6e5-176">Newtonsoft. JSON. JsonSerializationException: ' MyApplication. modeller. blog ' türündeki ' blog ' özelliği için kendine başvuran bir döngü algılandı.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="6a6e5-177">ASP.NET Core kullanıyorsanız, Json.NET 'ı nesne grafiğinde bulduğu döngüleri yoksayacak şekilde yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="6a6e5-178">Bu, içindeki `ConfigureServices(...)` `Startup.cs`yönteminde yapılır.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

<span data-ttu-id="6a6e5-179">Diğer bir seçenek de, bir gezinti özelliklerinden `[JsonIgnore]` birini, JSON.net, serileştirilirken Bu gezinti özelliğinde çapraz geçiş yapmasına yönlendiren özniteliğiyle süslemesine yönelik bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="6a6e5-179">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
