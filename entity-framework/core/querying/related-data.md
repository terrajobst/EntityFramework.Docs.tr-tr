---
title: İlgili Verilerin Yüklenmesi - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417679"
---
# <a name="loading-related-data"></a><span data-ttu-id="da2fc-102">İlgili Verileri Yükleme</span><span class="sxs-lookup"><span data-stu-id="da2fc-102">Loading Related Data</span></span>

<span data-ttu-id="da2fc-103">Entity Framework Core, ilgili varlıkları yüklemek için modelinizdeki gezinti özelliklerini kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="da2fc-104">İlgili verileri yüklemek için kullanılan üç yaygın O/RM delemi vardır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-104">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="da2fc-105">**Eager yükleme,** ilgili verilerin ilk sorgunun bir parçası olarak veritabanından yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="da2fc-106">**Açık yükleme,** ilgili verilerin daha sonra veritabanından açıkça yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="da2fc-107">**Tembel yükleme,** gezinti özelliğine erişildiğinde ilgili verilerin veritabanından saydam olarak yüklendiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="da2fc-108">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="da2fc-109">Istekli yükleme</span><span class="sxs-lookup"><span data-stu-id="da2fc-109">Eager loading</span></span>

<span data-ttu-id="da2fc-110">Sorgu sonuçlarına `Include` dahil edilecek ilgili verileri belirtmek için yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="da2fc-111">Aşağıdaki örnekte, sonuçlarda döndürülen blogların `Posts` özelliği ilgili gönderilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="da2fc-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="da2fc-112">Entity Framework Core, daha önce bağlam örneğine yüklenen diğer varlıklara gezinme özelliklerini otomatik olarak sabitler.</span><span class="sxs-lookup"><span data-stu-id="da2fc-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="da2fc-113">Bu nedenle, bir gezinti özelliğinin verilerini açıkça eklemeseniz bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse, özellik yine de doldurulabilir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="da2fc-114">Tek bir sorguda birden çok ilişkiden ilgili verileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="da2fc-115">Birden çok düzey dahil</span><span class="sxs-lookup"><span data-stu-id="da2fc-115">Including multiple levels</span></span>

<span data-ttu-id="da2fc-116">Yöntemi kullanarak ilişkili verilerin birden çok düzeylerini içerecek `ThenInclude` şekilde ilişkiler arasında ayrıntılı bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="da2fc-117">Aşağıdaki örnek, tüm blogları, ilgili gönderileri ve her gönderinin yazarını yükler.</span><span class="sxs-lookup"><span data-stu-id="da2fc-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="da2fc-118">İlgili verilerin daha `ThenInclude` fazla düzeyleri de dahil olmak üzere devam etmek için birden çok çağrı zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="da2fc-119">Tüm bunları, aynı sorguya birden çok düzeyden ve birden çok kökten ilgili verileri içerecek şekilde birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="da2fc-120">Dahil edilen varlıklardan biri için birden çok ilgili varlık eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="da2fc-121">Örneğin, `Blogs`sorgularken, `Posts` hem de `Author` `Tags` . `Posts`</span><span class="sxs-lookup"><span data-stu-id="da2fc-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="da2fc-122">Bunu yapmak için, kökten başlayan her bir include yolu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="da2fc-123">Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="da2fc-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="da2fc-124">Bu, gereksiz birleştirmeler alacağınız anlamına gelmez; çoğu durumda, EF SQL oluştururken birleştirmeleri birleştirir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> <span data-ttu-id="da2fc-125">Sürüm 3.0.0'dan `Include` bu yana, her biri ilişkisel sağlayıcılar tarafından üretilen SQL sorgularına ek bir JOIN eklenmesine neden olurken, önceki sürümler ek SQL sorguları oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-125">Since version 3.0.0, each `Include` will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries.</span></span> <span data-ttu-id="da2fc-126">Bu, sorgularınızın performansını iyi veya kötü önemli ölçüde değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-126">This can significantly change the performance of your queries, for better or worse.</span></span> <span data-ttu-id="da2fc-127">Özellikle, çok yüksek sayıda `Include` işleç içeren LINQ sorgularının kartezyen patlama sorununu önlemek için birden çok ayrı LINQ sorgusuna ayrılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-127">In particular, LINQ queries with an exceedingly high number of `Include` operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.</span></span>

### <a name="include-on-derived-types"></a><span data-ttu-id="da2fc-128">Türemiş türlere ekleme</span><span class="sxs-lookup"><span data-stu-id="da2fc-128">Include on derived types</span></span>

<span data-ttu-id="da2fc-129">Yalnızca türetilmiş bir türde tanımlanan gezintilerden `Include` `ThenInclude`ilgili verileri ekleyebilirsiniz ve.</span><span class="sxs-lookup"><span data-stu-id="da2fc-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="da2fc-130">Aşağıdaki model göz önüne alındığında:</span><span class="sxs-lookup"><span data-stu-id="da2fc-130">Given the following model:</span></span>

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

<span data-ttu-id="da2fc-131">Öğrenci `School` olan tüm kişilerin gezinme içeriği bir dizi desen kullanılarak hevesle yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="da2fc-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="da2fc-132">döküm kullanma</span><span class="sxs-lookup"><span data-stu-id="da2fc-132">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="da2fc-133">operatör `as` kullanma</span><span class="sxs-lookup"><span data-stu-id="da2fc-133">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="da2fc-134">bu tür `Include` parametre alır aşırı yük kullanarak`string`</span><span class="sxs-lookup"><span data-stu-id="da2fc-134">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="da2fc-135">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="da2fc-135">Explicit loading</span></span>

<span data-ttu-id="da2fc-136">API üzerinden bir gezinti özelliğini `DbContext.Entry(...)` açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="da2fc-137">Ayrıca, ilgili varlıkları döndüren ayrı bir sorgu gerçekleştirerek bir gezinti özelliğini açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-137">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="da2fc-138">Değişiklik izleme etkinse, bir varlık yüklenirken, EF Core otomatik olarak yeni yüklenen entitiy'nin gezinme özelliklerini zaten yüklenmiş olan varlıklara başvurmak üzere ayarlar ve zaten yüklenmiş varlıkların gezinti özelliklerini yeni yüklenen varlığa başvurmak üzere ayarlar.</span><span class="sxs-lookup"><span data-stu-id="da2fc-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="da2fc-139">İlgili varlıkları sorgulama</span><span class="sxs-lookup"><span data-stu-id="da2fc-139">Querying related entities</span></span>

<span data-ttu-id="da2fc-140">Ayrıca, bir gezinti özelliğinin içeriğini temsil eden bir LINQ sorgusu da alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="da2fc-141">Bu, ilgili varlıklar üzerinde bir toplu işleç çalıştırma gibi şeyleri belleğe yüklemeden yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="da2fc-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="da2fc-142">Ayrıca, hangi ilgili varlıkların belleğe yüklendiğini de filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="da2fc-143">Tembel yükleme</span><span class="sxs-lookup"><span data-stu-id="da2fc-143">Lazy loading</span></span>

<span data-ttu-id="da2fc-144">Tembel yükleme kullanmak için en basit yolu [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paketi yükleyerek ve `UseLazyLoadingProxies`bir çağrı ile etkinleştirerek.</span><span class="sxs-lookup"><span data-stu-id="da2fc-144">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="da2fc-145">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da2fc-145">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="da2fc-146">Veya AddDbContext kullanırken:</span><span class="sxs-lookup"><span data-stu-id="da2fc-146">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="da2fc-147">EF Core daha sonra geçersiz kılınabilen herhangi bir gezinme özelliği için `virtual` tembel yüklemeye olanak sağlayacaktır- yani, olması gereken ve devralınabilecek bir sınıf üzerinde.</span><span class="sxs-lookup"><span data-stu-id="da2fc-147">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="da2fc-148">Örneğin, aşağıdaki varlıklarda, `Post.Blog` ve `Blog.Posts` gezinti özellikleri tembel yüklü olacaktır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-148">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

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

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="da2fc-149">Vekiller olmadan tembel yükleme</span><span class="sxs-lookup"><span data-stu-id="da2fc-149">Lazy loading without proxies</span></span>

<span data-ttu-id="da2fc-150">Tembel yükleme vekilleri, Entity Type `ILazyLoader` [Constructors'da](../modeling/constructors.md)açıklandığı gibi hizmeti bir varlığa enjekte ederek çalışır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-150">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="da2fc-151">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da2fc-151">For example:</span></span>

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

<span data-ttu-id="da2fc-152">Bu, varlık türlerinin devralınmasını veya gezinme özelliklerinin sanal olmasını gerektirmez ve `new` bir bağlama iliştirildikten sonra tembel liğe bağlı olarak oluşturulan varlık örneklerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-152">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="da2fc-153">Ancak, `ILazyLoader` [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) paketinde tanımlanan hizmete başvuru gerektirir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-153">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="da2fc-154">Bu paket, bağlı olarak çok az etkisi olacak şekilde en az sayıda tür içerir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-154">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="da2fc-155">Ancak, varlık türlerindeki EF Core paketlerine bağlı kalmaktan tamamen kaçınmak `ILazyLoader.Load` için, yöntemi temsilci olarak enjekte etmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="da2fc-155">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="da2fc-156">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da2fc-156">For example:</span></span>

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

<span data-ttu-id="da2fc-157">Yukarıdaki kod, `Load` temsilciyi biraz daha temiz kullanmak için bir uzantı yöntemi kullanır:</span><span class="sxs-lookup"><span data-stu-id="da2fc-157">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

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
> <span data-ttu-id="da2fc-158">Tembel yükleme temsilcisi için yapıcı parametre "lazyLoader" olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-158">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="da2fc-159">Yapılandırma bundan farklı bir ad kullanmak için gelecekteki bir sürüm için planlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-159">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="da2fc-160">İlgili veriler ve serileştirme</span><span class="sxs-lookup"><span data-stu-id="da2fc-160">Related data and serialization</span></span>

<span data-ttu-id="da2fc-161">EF Core navigasyon özelliklerini otomatik olarak düzelteceği için, nesne grafiğinizde döngüler olabilir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-161">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="da2fc-162">Örneğin, bir blogun ve ilgili gönderilerin yüklenmesi, bir gönderi koleksiyonuna başvuran bir blog nesnesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="da2fc-162">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="da2fc-163">Bu mesajların her biri geri bloga bir referans olacaktır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-163">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="da2fc-164">Bazı serileştirme çerçeveleri bu tür döngülere izin vermez.</span><span class="sxs-lookup"><span data-stu-id="da2fc-164">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="da2fc-165">Örneğin, bir döngü yle karşılaşılırsa, Json.NET aşağıdaki özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="da2fc-165">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="da2fc-166">Newtonsoft.Json.JsonSerializationException: 'MyApplication.Models.Blog' türü ile özellik 'Blog' için algılanan Self referencing döngüsü.</span><span class="sxs-lookup"><span data-stu-id="da2fc-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="da2fc-167">ASP.NET Core kullanıyorsanız, Json.NET nesne grafiğinde bulduğu döngüleri yoksayacak şekilde yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da2fc-167">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="da2fc-168">Bu `ConfigureServices(...)` yöntemde `Startup.cs`yapılır.</span><span class="sxs-lookup"><span data-stu-id="da2fc-168">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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

<span data-ttu-id="da2fc-169">Başka bir alternatif, Json.NET serileştirme sırasında `[JsonIgnore]` bu gezinti özelliği ni geçmeyecek şekilde talimat veren öznitelik ile gezinti özelliklerinden birini süslemektir.</span><span class="sxs-lookup"><span data-stu-id="da2fc-169">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
