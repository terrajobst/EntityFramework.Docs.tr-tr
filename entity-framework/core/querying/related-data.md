---
title: Ilgili verileri yükleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417679"
---
# <a name="loading-related-data"></a><span data-ttu-id="72e6a-102">İlgili Verileri Yükleme</span><span class="sxs-lookup"><span data-stu-id="72e6a-102">Loading Related Data</span></span>

<span data-ttu-id="72e6a-103">Entity Framework Core, ilişkili varlıkları yüklemek için modelinizdeki gezinti özelliklerini kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e6a-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="72e6a-104">İlgili verileri yüklemek için kullanılan üç ortak O/RM deseni vardır.</span><span class="sxs-lookup"><span data-stu-id="72e6a-104">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="72e6a-105">**Eager yüklemesi** , ilgili verilerin ilk sorgunun parçası olarak veritabanından yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="72e6a-106">**Açık yükleme** , ilgili verilerin daha sonra veritabanından açıkça yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="72e6a-107">**Yavaş yükleme** , gezinti özelliğine erişildiğinde ilgili verilerin veritabanından saydam olarak yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="72e6a-108">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="72e6a-109">ekip yükleme</span><span class="sxs-lookup"><span data-stu-id="72e6a-109">Eager loading</span></span>

<span data-ttu-id="72e6a-110">Sorgu sonuçlarına dahil edilecek ilgili verileri belirtmek için `Include` yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="72e6a-111">Aşağıdaki örnekte, sonuçlarda döndürülen blogların `Posts` özelliği ilgili gönderileriyle doldurulmuş olacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e6a-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="72e6a-112">Entity Framework Core, gezinti özelliklerini daha önce bağlam örneğine yüklenmiş diğer varlıklara otomatik olarak düzeltir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="72e6a-113">Bu nedenle, bir gezinti özelliği için verileri açıkça bulundurmasanız bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="72e6a-114">Tek bir sorgudaki birden fazla ilişkilerden ilgili verileri dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="72e6a-115">Birden çok düzey dahil</span><span class="sxs-lookup"><span data-stu-id="72e6a-115">Including multiple levels</span></span>

<span data-ttu-id="72e6a-116">`ThenInclude` yöntemi kullanılarak ilgili verilerin birden fazla düzeyini dahil etmek için ilişkilerde ayrıntıya gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="72e6a-117">Aşağıdaki örnek tüm blogları, ilgili yayınlarını ve her gönderiye ait yazarı yükler.</span><span class="sxs-lookup"><span data-stu-id="72e6a-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="72e6a-118">Daha fazla ilgili veri düzeyi dahil etmek için `ThenInclude` birden çok çağrısı zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="72e6a-119">Aynı sorguda birden çok düzeyden ve birden çok kökten ilgili verileri dahil etmek için tüm bunları birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="72e6a-120">Dahil edilen varlıklardan biri için birden fazla ilgili varlık eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="72e6a-121">Örneğin, `Blogs`sorgulanırken `Posts` ekler ve ardından `Posts``Author` ve `Tags` dahil etmek istersiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="72e6a-122">Bunu yapmak için, kökden başlayarak her bir içerme yolunu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="72e6a-123">Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="72e6a-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="72e6a-124">Bu, gereksiz birleşimler alacağınız anlamına gelmez; Çoğu durumda, SQL oluştururken EF birleştirmeleri birleştirecek.</span><span class="sxs-lookup"><span data-stu-id="72e6a-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> <span data-ttu-id="72e6a-125">Sürüm 3.0.0 bu yana, her `Include` ilişkisel sağlayıcılar tarafından üretilen SQL sorgularına ek bir BIRLEŞIME eklenmesine, ancak önceki sürümlerde ek SQL sorguları oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="72e6a-125">Since version 3.0.0, each `Include` will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries.</span></span> <span data-ttu-id="72e6a-126">Bu, daha iyi veya daha kötü bir şekilde Sorgularınızın performansını önemli ölçüde değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-126">This can significantly change the performance of your queries, for better or worse.</span></span> <span data-ttu-id="72e6a-127">Özellikle, bir ayrık olarak yüksek sayıda `Include` işleçli LINQ sorgularının, Kartezyen açılım sorununa engel olmak için birden fazla ayrı LINQ sorgusuna ayrılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-127">In particular, LINQ queries with an exceedingly high number of `Include` operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.</span></span>

### <a name="include-on-derived-types"></a><span data-ttu-id="72e6a-128">Türetilmiş türlere dahil et</span><span class="sxs-lookup"><span data-stu-id="72e6a-128">Include on derived types</span></span>

<span data-ttu-id="72e6a-129">Yalnızca `Include` ve `ThenInclude`kullanarak türetilmiş bir tür üzerinde tanımlanan gezintilerden ilgili verileri dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="72e6a-130">Aşağıdaki model veriliyor:</span><span class="sxs-lookup"><span data-stu-id="72e6a-130">Given the following model:</span></span>

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

<span data-ttu-id="72e6a-131">Öğrenciler olan tüm kişilerin `School` gezintisi içerikleri, çeşitli desenler kullanılarak oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="72e6a-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="72e6a-132">cast kullanma</span><span class="sxs-lookup"><span data-stu-id="72e6a-132">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="72e6a-133">`as` işleci kullanma</span><span class="sxs-lookup"><span data-stu-id="72e6a-133">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="72e6a-134">`string` türünde parametre alan `Include` aşırı yüklemesini kullanma</span><span class="sxs-lookup"><span data-stu-id="72e6a-134">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="72e6a-135">açık yükleme</span><span class="sxs-lookup"><span data-stu-id="72e6a-135">Explicit loading</span></span>

<span data-ttu-id="72e6a-136">`DbContext.Entry(...)` API 'SI aracılığıyla bir gezinti özelliğini açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="72e6a-137">Ayrıca, ilişkili varlıkları döndüren ayrı bir sorgu yürüterek bir gezinti özelliğini açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-137">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="72e6a-138">Değişiklik izleme etkinse, bir varlık yüklenirken EF Core, önceden yüklenmiş olan varlıkların gezinti özelliklerini otomatik olarak ayarlar ve bu, önceden yüklenmiş varlıkların gezinti özelliklerini, daha önce yüklenmiş varlıklara başvuracak şekilde ayarlar. Yeni yüklenen varlık.</span><span class="sxs-lookup"><span data-stu-id="72e6a-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="72e6a-139">İlgili varlıkları sorgulama</span><span class="sxs-lookup"><span data-stu-id="72e6a-139">Querying related entities</span></span>

<span data-ttu-id="72e6a-140">Ayrıca, bir gezinti özelliğinin içeriğini temsil eden bir LINQ sorgusu da edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="72e6a-141">Bu, ilgili varlıklar üzerinde bir toplama işlecini belleğe yüklemeden çalıştırmak gibi işlemleri yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e6a-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="72e6a-142">Ayrıca, hangi ilgili varlıkların belleğe yükleneceğini de filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="72e6a-143">geç yükleme</span><span class="sxs-lookup"><span data-stu-id="72e6a-143">Lazy loading</span></span>

<span data-ttu-id="72e6a-144">Geç yüklemeyi kullanmanın en basit yolu, [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paketini yükleyip `UseLazyLoadingProxies`çağrısı yaparak bunu yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e6a-144">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="72e6a-145">Örnek:</span><span class="sxs-lookup"><span data-stu-id="72e6a-145">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="72e6a-146">AddDbContext kullanırken:</span><span class="sxs-lookup"><span data-stu-id="72e6a-146">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="72e6a-147">EF Core, geçersiz kılınabilen herhangi bir gezinti özelliği için yavaş yüklemeyi etkinleştirir, yani bu, `virtual` ve içinden devralınabilen bir sınıfta yer almalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e6a-147">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="72e6a-148">Örneğin, aşağıdaki varlıklarda `Post.Blog` ve `Blog.Posts` gezinti özellikleri yavaş yüklenir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-148">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

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

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="72e6a-149">Proxy olmadan yavaş yükleme</span><span class="sxs-lookup"><span data-stu-id="72e6a-149">Lazy loading without proxies</span></span>

<span data-ttu-id="72e6a-150">Yavaş yükleme proxy 'leri, [varlık türü oluşturucuları](../modeling/constructors.md)bölümünde açıklandığı gibi `ILazyLoader` hizmetini bir varlığa ekleme.</span><span class="sxs-lookup"><span data-stu-id="72e6a-150">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="72e6a-151">Örnek:</span><span class="sxs-lookup"><span data-stu-id="72e6a-151">For example:</span></span>

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

<span data-ttu-id="72e6a-152">Bu, varlık türlerinin devralınacağı veya gezinti özelliklerinden sanal olmasına gerek yoktur ve `new` oluşturulan varlık örneklerinin bir içeriğe eklendikten sonra yavaş yükleme yapmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-152">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="72e6a-153">Ancak, [Microsoft. EntityFrameworkCore. soyutlamalar](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) paketinde tanımlanan `ILazyLoader` hizmetine bir başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-153">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="72e6a-154">Bu paket, buna bağlı olarak çok az etkisi olması için en az bir tür kümesi içerir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-154">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="72e6a-155">Ancak, varlık türlerindeki tüm EF Core paketlerine bağlı olarak tamamen kaçınmak için, `ILazyLoader.Load` metodunu bir temsilci olarak eklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="72e6a-155">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="72e6a-156">Örnek:</span><span class="sxs-lookup"><span data-stu-id="72e6a-156">For example:</span></span>

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

<span data-ttu-id="72e6a-157">Yukarıdaki kod, temsilciyi bir bit temizleyici kullanarak yapmak için bir `Load` genişletme yöntemi kullanır:</span><span class="sxs-lookup"><span data-stu-id="72e6a-157">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

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
> <span data-ttu-id="72e6a-158">Yavaş yükleme temsilcisinin Oluşturucu parametresine "lazyLoader" adı verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-158">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="72e6a-159">Daha sonraki bir sürüm için bu değerden farklı bir ad kullanacak şekilde yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="72e6a-159">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="72e6a-160">İlgili veriler ve serileştirme</span><span class="sxs-lookup"><span data-stu-id="72e6a-160">Related data and serialization</span></span>

<span data-ttu-id="72e6a-161">EF Core, gezinti özelliklerini otomatik olarak düzeltiğinden, nesne grafiğinizde döngülerle bitilecektir.</span><span class="sxs-lookup"><span data-stu-id="72e6a-161">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="72e6a-162">Örneğin, bir blog ve ilgili gönderimler yükleme, bir gönderi koleksiyonuna başvuran bir blog nesnesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="72e6a-162">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="72e6a-163">Bu gönderilerin her birinin bloguna bir başvurusu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e6a-163">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="72e6a-164">Bazı serileştirme çerçeveleri bu tür döngülerine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="72e6a-164">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="72e6a-165">Örneğin, bir döngüyle karşılaşılırsa Json.NET aşağıdaki özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="72e6a-165">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="72e6a-166">Newtonsoft. JSON. JsonSerializationException: ' MyApplication. modeller. blog ' türündeki ' blog ' özelliği için kendine başvuran bir döngü algılandı.</span><span class="sxs-lookup"><span data-stu-id="72e6a-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="72e6a-167">ASP.NET Core kullanıyorsanız, Json.NET 'ı nesne grafiğinde bulduğu döngüleri yoksayacak şekilde yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e6a-167">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="72e6a-168">Bu, `Startup.cs``ConfigureServices(...)` yönteminde yapılır.</span><span class="sxs-lookup"><span data-stu-id="72e6a-168">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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

<span data-ttu-id="72e6a-169">Diğer bir seçenek de `[JsonIgnore]` özniteliği ile gezinti özelliklerinden birini süslemek, bu da Json.NET ' i serileştirirken Bu gezinti özelliğini çapraz tutmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e6a-169">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
