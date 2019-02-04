---
title: İlgili verileri - EF Core yükleme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4e042acb805c743ee794f4e61105b8d2136973b1
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668732"
---
# <a name="loading-related-data"></a><span data-ttu-id="cd3b2-102">İlgili verileri yükleme</span><span class="sxs-lookup"><span data-stu-id="cd3b2-102">Loading Related Data</span></span>

<span data-ttu-id="cd3b2-103">Entity Framework Core ilgili varlıkları yükleme için modelinizde gezinme özelliklerini kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="cd3b2-104">İlgili verileri yüklemek için kullanılan üç genel O/RM Düzen vardır.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="cd3b2-105">**İstekli yükleme** ilgili verileri veritabanından ilk sorgunun bir parçası yüklendiğini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="cd3b2-106">**Açık yükleme** daha sonraki bir zamanda veritabanından, ilgili veriler açıkça yüklendikten anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="cd3b2-107">**Yavaş Yükleniyor** gezinme özelliğini erişildiğinde ilgili verileri veritabanından şeffaf bir şekilde yüklendiğini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="cd3b2-108">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="cd3b2-109">İstekli yükleme</span><span class="sxs-lookup"><span data-stu-id="cd3b2-109">Eager loading</span></span>

<span data-ttu-id="cd3b2-110">Kullanabileceğiniz `Include` sorgu sonuçlarında eklenecek ilgili verileri belirtmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="cd3b2-111">Aşağıdaki örnekte, döndürülen sonuçlarda blogları olacaktır kendi `Posts` özelliği ile ilgili gönderileri doldurulur.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="cd3b2-112">Entity Framework Core olacak otomatik olarak düzeltme yukarı Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="cd3b2-113">Bu nedenle açıkça bir gezinme özelliği için bir veri içermez olsa bile, özellik hala bazıları doldurulabilir veya tüm ilişkili varlıkları önceden yüklenen.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="cd3b2-114">Tek bir sorguda birden çok ilişki ilgili verileri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="cd3b2-115">Birden çok düzeyleri dahil</span><span class="sxs-lookup"><span data-stu-id="cd3b2-115">Including multiple levels</span></span>

<span data-ttu-id="cd3b2-116">İlgili verileri kullanarak, birden çok düzeyi içerecek şekilde ilişkileri detaya gidebilirsiniz `ThenInclude` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="cd3b2-117">Aşağıdaki örnek, tüm blogları, kendi ilgili gönderileri ve her bir gönderi yazarı yükler.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="cd3b2-118">Visual Studio'nun geçerli sürümleri yanlış kod tamamlama seçenekleri sunar ve söz dizimi hataları ile kullanırken işaretlenmesini doğru ifadeleri neden olabilir `ThenInclude` sonrasına bir koleksiyon gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="cd3b2-119">Bu, izlenen bir IntelliSense hatanın belirtisidir https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="cd3b2-120">Kod doğru olduğundan ve başarıyla derlenen sürece bu alacaklardır söz dizimi hataları yoksaymak güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="cd3b2-121">Birden çok çağrı zincirleyebilirsiniz `ThenInclude` başka ilgili verileri düzeyini de dahil olmak üzere devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="cd3b2-122">Tüm bunlar aynı sorguda birden çok düzeyi ve birden çok kök ilgili verileri içerecek şekilde birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="cd3b2-123">Dahil varlıklardan biri için birden çok ilişkili varlıkları içermesi isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="cd3b2-124">Örneğin, sorgulanırken `Blog`s, eklediğiniz `Posts` ve ardından her ikisini dahil etmek istediğiniz `Author` ve `Tags` , `Posts`.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="cd3b2-125">Bunu yapmak için her ekleme kök dizininde başlangıç yolu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="cd3b2-126">Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="cd3b2-127">Bu, yedekli birleştirmeler erişmenizi sağlayacak gelmez; Çoğu durumda, SQL oluştururken, birleştirmeler EF birleştirecek.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="cd3b2-128">Türetilmiş türler</span><span class="sxs-lookup"><span data-stu-id="cd3b2-128">Include on derived types</span></span>

<span data-ttu-id="cd3b2-129">Yalnızca bir türetilmiş bir tür kullanarak tanımlanan gezintiler ilgili verileri içerebilir `Include` ve `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="cd3b2-130">Şu model verilen:</span><span class="sxs-lookup"><span data-stu-id="cd3b2-130">Given the following model:</span></span>

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

<span data-ttu-id="cd3b2-131">İçeriğini `School` Öğrenciler tüm kişilerin Gezinti eagerly yüklenebilir bir desenlerinin kullanarak:</span><span class="sxs-lookup"><span data-stu-id="cd3b2-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="cd3b2-132">Cast kullanma</span><span class="sxs-lookup"><span data-stu-id="cd3b2-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="cd3b2-133">kullanarak `as` işleci</span><span class="sxs-lookup"><span data-stu-id="cd3b2-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="cd3b2-134">aşırı yüklemesini kullanarak `Include` türünde parametre almayan `string`</span><span class="sxs-lookup"><span data-stu-id="cd3b2-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="cd3b2-135">Yoksayılan içerir</span><span class="sxs-lookup"><span data-stu-id="cd3b2-135">Ignored includes</span></span>

<span data-ttu-id="cd3b2-136">Sorgu değiştirirseniz, böylece artık sorgu başlamış varlık türü örneklerini döndürür içerme işleçleri göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="cd3b2-137">Ekleme işleçleri dayalı aşağıdaki örnekte, `Blog`, ancak ardından `Select` işleci, anonim bir tür sorguyu değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="cd3b2-138">Bu durumda, INCLUDE işleçleri hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="cd3b2-139">Varsayılan olarak, bir uyarı EF Core başlar.SSH zaman dahil işleçleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="cd3b2-140">Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="cd3b2-141">Ekle işleci throw veya hiçbir şey yapma göz ardı edilir olduğunda davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="cd3b2-142">Bağlamınızı - seçeneklerini normalde ayarlarken yapıldığını `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="cd3b2-143">açık yükleme</span><span class="sxs-lookup"><span data-stu-id="cd3b2-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="cd3b2-144">Bu özellik, EF Core 1.1 içinde kullanılmaya başlandı.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="cd3b2-145">Bir gezinme özelliği aracılığıyla açıkça yüklemek `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="cd3b2-146">Bir gezinme özelliği, ilgili varlıkları döndürür, ayrı bir sorgu yürüterek de açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="cd3b2-147">Değişiklik izleme etkin sonra bir varlık yüklenirken, EF Core otomatik olarak önceden yüklenmiş tüm varlıklara başvurmak için yeni yüklenen varlık Gezinti özelliklerini ayarlayın ve başvurmak için önceden yüklenmiş varlıkları Gezinti özelliklerini ayarlama Yeni yüklenen varlık.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="cd3b2-148">İlgili varlıkları sorgulama</span><span class="sxs-lookup"><span data-stu-id="cd3b2-148">Querying related entities</span></span>

<span data-ttu-id="cd3b2-149">Ayrıca, bir gezinti özelliği içeriğini temsil eden bir LINQ Sorgu alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="cd3b2-150">Bu belleğe yüklemeden ilgili varlıklar üzerinde bir toplama işleci çalıştırma gibi şeyleri yapmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="cd3b2-151">Ayrıca, hangi ilgili varlıkları belleğe yüklenen filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="cd3b2-152">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="cd3b2-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="cd3b2-153">Bu özellik, EF Core 2.1 içinde kullanılmaya başlandı.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="cd3b2-154">Yavaş yükleniyor kullanmanın en basit yolu yüklemektir [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket ve çağrısıyla etkinleştirme `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="cd3b2-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cd3b2-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="cd3b2-156">Veya AddDbContext kullanırken:</span><span class="sxs-lookup"><span data-stu-id="cd3b2-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="cd3b2-157">EF Core olacak etkinleştir--diğer bir deyişle, geçersiz kılınabilir herhangi bir gezinti özelliği için yükleme yavaş olması gerektiğini sonra `virtual` ve öğesinden devralınan bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="cd3b2-158">Örneğin, aşağıdaki varlıkların içinde `Post.Blog` ve `Blog.Posts` Gezinti özellikleri, yüklenen yavaş olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="cd3b2-159">Lazy proxy'si yükleme</span><span class="sxs-lookup"><span data-stu-id="cd3b2-159">Lazy loading without proxies</span></span>

<span data-ttu-id="cd3b2-160">Lazy yükleme proxy'leri iş ekleyerek `ILazyLoader` açıklandığı bir varlığa hizmet [varlık türü oluşturucuları](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="cd3b2-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="cd3b2-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cd3b2-161">For example:</span></span>
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
<span data-ttu-id="cd3b2-162">Bu devralınan için varlık türleri veya gezinti özelliklerini sanal gerektirmez ve ile oluşturulan varlık örnekleri sağlar `new` yavaş bir kez yük için bir bağlamına bağlı.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="cd3b2-163">Ancak, bir başvuru gerektirir `ILazyLoader` tanımlanan hizmet [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) paket.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="cd3b2-164">Çok az etkisi bağlı olarak, yani bu paket türleri en az bir kümesini içerir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="cd3b2-165">Ancak, tüm varlık türlerini EF Core paketlerinde bağlı olarak tamamen önlemek için eklemesine mümkündür `ILazyLoader.Load` yöntemi temsilci olarak.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="cd3b2-166">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cd3b2-166">For example:</span></span>
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
<span data-ttu-id="cd3b2-167">Yukarıdaki kullanan kodu bir `Load` temsilci biraz Temizleyicisi hale getirmek için genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd3b2-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
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
> <span data-ttu-id="cd3b2-168">Yavaş yükleniyor temsilci Oluşturucu parametresi "lazyLoader" çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="cd3b2-169">Bu, gelecekteki sürümlerde sunulması planlanmaktadır olandan farklı bir ad kullanmak için yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="cd3b2-170">İlgili verileri ve Serileştirme</span><span class="sxs-lookup"><span data-stu-id="cd3b2-170">Related data and serialization</span></span>

<span data-ttu-id="cd3b2-171">EF Core otomatik olarak düzeltme yukarı Gezinti özelliklerini, döngüleriyle nesne graftaki kalabilirsiniz olduğundan.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="cd3b2-172">Örneğin, blog ve kendi ilgili gönderileri yükleme gönderileri koleksiyonunu başvuran bir blog nesneyle sonuçlanacak.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="cd3b2-173">Bu gönderilerin her blog geri başvuru gerekir.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="cd3b2-174">Bazı serileştirme çerçeveleri gibi döngüleri izin vermez.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="cd3b2-175">Örneğin, bir döngüye ile karşılaşılırsa Json.NET şu özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="cd3b2-176">Newtonsoft.Json.JsonSerializationException: Kendi kendine başvuran döngü 'Blog' özelliği için 'MyApplication.Models.Blog' türüyle algılandı.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="cd3b2-177">ASP.NET Core kullanıyorsanız, nesne grafiğinde bulduğu döngüleri yok saymak için Json.NET yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="cd3b2-178">Bu yapılır `ConfigureServices(...)` yönteminde `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="cd3b2-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
