---
title: "Yükleme ilgili verileri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 0d7705e0e5368435536e98d319c853ea8c732643
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="773f6-102">İlgili verileri yükleniyor</span><span class="sxs-lookup"><span data-stu-id="773f6-102">Loading Related Data</span></span>

<span data-ttu-id="773f6-103">Entity Framework Çekirdek, gezinti özellikleri ilgili varlıklar yüklemek için modelinizde kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="773f6-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="773f6-104">İlgili verileri yüklemek için kullanılan üç ortak O/RM desenler vardır.</span><span class="sxs-lookup"><span data-stu-id="773f6-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="773f6-105">**İstekli yükleme** ilgili verileri ilk sorgu bir parçası olarak veritabanından yüklenen anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="773f6-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="773f6-106">**Açık yükleme** ilgili verileri veritabanından açıkça daha sonraki bir zamanda yüklenip yüklenmediğine anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="773f6-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="773f6-107">**Yavaş Yükleniyor** gezinti özelliği erişildiğinde ilgili verileri veritabanından saydam yüklenen anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="773f6-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="773f6-108">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.</span><span class="sxs-lookup"><span data-stu-id="773f6-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="773f6-109">istekli yükleniyor</span><span class="sxs-lookup"><span data-stu-id="773f6-109">Eager loading</span></span>

<span data-ttu-id="773f6-110">Kullanabileceğiniz `Include` yöntemi sorgu sonuçlarında dahil edilecek ilgili verileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="773f6-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="773f6-111">Aşağıdaki örnekte, döndürülen sonuçlarda bloglar olacaktır kendi `Posts` özelliği ile ilgili gönderileri doldurulur.</span><span class="sxs-lookup"><span data-stu-id="773f6-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="773f6-112">Entity Framework Çekirdek otomatik olarak düzeltme yukarı Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için.</span><span class="sxs-lookup"><span data-stu-id="773f6-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="773f6-113">Bu nedenle bir gezinme özelliği için veri açıkça içerme olsa bile, özellik hala bazıları doldurulmuş veya tüm ilgili varlıklar daha önce yüklenen.</span><span class="sxs-lookup"><span data-stu-id="773f6-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="773f6-114">Tek bir sorguda birden çok ilişki ilgili verileri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="773f6-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="773f6-115">Birden çok düzeyi de dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="773f6-115">Including multiple levels</span></span>

<span data-ttu-id="773f6-116">Birden çok düzeyinden birini kullanarak ilgili verileri içerecek şekilde ilişkileri ayrıntıya girebilirsiniz `ThenInclude` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="773f6-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="773f6-117">Aşağıdaki örnek, tüm blogları, kendi ilgili gönderileri ve her posta yazarı yükler.</span><span class="sxs-lookup"><span data-stu-id="773f6-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="773f6-118">Visual Studio'nun geçerli sürümleri yanlış kod tamamlama seçenekleri sunar ve sözdizimi hataları ile kullanırken işaretlenmesini doğru ifadeleri neden olabilir `ThenInclude` yöntemi sonra bir koleksiyon gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="773f6-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="773f6-119">Https://github.com/dotnet/roslyn/issues/8237 izlenen bir IntelliSense hatanın belirtisidir.</span><span class="sxs-lookup"><span data-stu-id="773f6-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="773f6-120">Kod doğru olduğundan ve başarıyla derlenen sürece bu alacaklardır sözdizimi hataları yoksaymak güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="773f6-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="773f6-121">Birden fazla çağrı zincir `ThenInclude` daha ilgili verileri düzeylerini dahil olmak üzere devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="773f6-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="773f6-122">Bu aynı sorguda birden çok düzeyi ve birden çok kök ilgili verileri içerecek şekilde tüm birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="773f6-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="773f6-123">Dahil varlıklar biri için birden çok ilgili varlık eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="773f6-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="773f6-124">Örneğin, sorgulanırken `Blog`s, eklediğiniz `Posts` ve ardından her ikisi de dahil etmek istediğiniz `Author` ve `Tags` , `Posts`.</span><span class="sxs-lookup"><span data-stu-id="773f6-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="773f6-125">Bunu yapmak için her dahil kökte başlayan yolunu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="773f6-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="773f6-126">Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="773f6-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="773f6-127">Bu, çoğu durumda EF birleştirmek yedekli birleştirmeler SQL oluştururken birleştirmeler alırsınız gelmez.</span><span class="sxs-lookup"><span data-stu-id="773f6-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="773f6-128">Türetilen türler</span><span class="sxs-lookup"><span data-stu-id="773f6-128">Include on derived types</span></span>

<span data-ttu-id="773f6-129">Yalnızca bir türetilmiş bir tür kullanarak tanımlanan gezintilerini ilgili verileri içerebilir `Include` ve `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="773f6-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="773f6-130">Aşağıdaki model verilen:</span><span class="sxs-lookup"><span data-stu-id="773f6-130">Given the following model:</span></span>

```Csharp
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

<span data-ttu-id="773f6-131">İçeriği `School` Gezinti Öğrenciler tüm kişilerin isteğini önleyebiliriz yüklenebilir bir desenlerinin kullanarak:</span><span class="sxs-lookup"><span data-stu-id="773f6-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="773f6-132">Cast kullanma</span><span class="sxs-lookup"><span data-stu-id="773f6-132">using cast</span></span>
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- <span data-ttu-id="773f6-133">kullanarak `as` işleci</span><span class="sxs-lookup"><span data-stu-id="773f6-133">using `as` operator</span></span>
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- <span data-ttu-id="773f6-134">aşırı yüklemesini kullanarak `Include` türünde bir parametre alır `string`</span><span class="sxs-lookup"><span data-stu-id="773f6-134">using overload of `Include` that takes parameter of type `string`</span></span>
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a><span data-ttu-id="773f6-135">Göz ardı içerir</span><span class="sxs-lookup"><span data-stu-id="773f6-135">Ignored includes</span></span>

<span data-ttu-id="773f6-136">Böylece artık sorgu ile başlamıştır varlık türünün örneklerini döndürür sorgu değiştirirseniz, INCLUDE işleçleri göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="773f6-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="773f6-137">INCLUDE işleçleri aşağıdaki örnekte, temel alan `Blog`, ancak ardından `Select` işleci anonim bir tür döndürmek için sorguyu değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="773f6-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="773f6-138">Bu durumda, INCLUDE işleçleri hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="773f6-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="773f6-139">Varsayılan olarak, bir uyarı EF çekirdek oturum zaman dahil işleçleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="773f6-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="773f6-140">Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="773f6-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="773f6-141">Ekle işleci throw veya hiçbir şey yapma göz ardı davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="773f6-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="773f6-142">Bu genellikle, içeriğiniz - seçeneklerini ayarlama zaman yapılır `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="773f6-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="773f6-143">Açık yükleniyor</span><span class="sxs-lookup"><span data-stu-id="773f6-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="773f6-144">Bu özellik EF çekirdek 1.1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="773f6-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="773f6-145">Bir gezinti özelliği aracılığıyla açıkça yükleyebilir `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="773f6-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="773f6-146">İlgili varlıklar döndürüyor ayrı bir sorgu yürüterek bir gezinti özelliği de açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="773f6-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="773f6-147">Değişiklik izleme etkinleştirildi, varsa bir varlık yüklenirken EF çekirdek otomatik olarak zaten yüklenmiş herhangi bir varlık başvurmak için yeni yüklenen entitiy Gezinti özelliklerini ayarlamak ve başvurmak için daha önce yüklenmiş varlıklar Gezinti özelliklerini ayarlama Yeni yüklenen varlık.</span><span class="sxs-lookup"><span data-stu-id="773f6-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="773f6-148">İlgili varlıklar sorgulama</span><span class="sxs-lookup"><span data-stu-id="773f6-148">Querying related entities</span></span>

<span data-ttu-id="773f6-149">Ayrıca bir gezinti özelliği içeriğini temsil eden bir LINQ Sorgu alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="773f6-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="773f6-150">Bu belleğe yüklemeden ilgili varlıklar üzerinde bir toplama işleci çalıştırma gibi şeyler olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="773f6-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="773f6-151">Ayrıca, hangi ilgili varlıklar belleğe yüklenen filtre uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="773f6-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="773f6-152">yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="773f6-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="773f6-153">Bu özellik EF çekirdek 2.1 içinde sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="773f6-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="773f6-154">Yükleme yavaş kullanmak için en basit yolu yüklemektir [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket ve çağrısıyla etkinleştirme `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="773f6-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="773f6-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="773f6-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="773f6-156">Ya da AddDbContext kullanırken:</span><span class="sxs-lookup"><span data-stu-id="773f6-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="773f6-157">EF çekirdek sonra yükleme yavaş olan kılınabilen--herhangi gezinme özelliği için etkinleştir, olmalıdır `virtual` ve öğesinden devralınan bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="773f6-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="773f6-158">Örneğin, aşağıdaki varlıklar olarak `Post.Blog` ve `Blog.Posts` Gezinti özellikleri yüklenen yavaş olacaktır.</span><span class="sxs-lookup"><span data-stu-id="773f6-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="773f6-159">Lazy yükleme proxy'leri olmadan</span><span class="sxs-lookup"><span data-stu-id="773f6-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="773f6-160">Lazy yükleme proxy'leri iş ekleyerek `ILazyLoader` açıklandığı gibi bir varlık kümesine hizmet [varlık türü oluşturucuları](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="773f6-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="773f6-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="773f6-161">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="773f6-162">Bu varlık türleri kaynağından devralındı ya da gezinti özellikleri sanal olmasını gerektirmez ve ile oluşturulan varlık örnekleri sağlar `new` yavaş bir kez yük bağlı bir bağlam.</span><span class="sxs-lookup"><span data-stu-id="773f6-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="773f6-163">Ancak, bir başvuru gerektirir `ILazyLoader` , varlık türleri EF çekirdek derlemeye tüm çiftler hizmetini.</span><span class="sxs-lookup"><span data-stu-id="773f6-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="773f6-164">Bu EF çekirdek önlemek için verir `ILazyLoader.Load` temsilci olarak eklenemeyebilir yöntemi.</span><span class="sxs-lookup"><span data-stu-id="773f6-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="773f6-165">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="773f6-165">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="773f6-166">Yukarıdaki kullanan kodu bir `Load` genişletme yöntemi temsilci biraz temizleyici kullanarak yapmak için:</span><span class="sxs-lookup"><span data-stu-id="773f6-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
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
> <span data-ttu-id="773f6-167">Yükleme yavaş temsilci Oluşturucu parametresi "lazyLoader" çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="773f6-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="773f6-168">Gelecekteki bir sürümde bu planlanan farklı bir ad kullanmak üzere yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="773f6-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="773f6-169">İlgili tüm verileri ve seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="773f6-169">Related data and serialization</span></span>

<span data-ttu-id="773f6-170">EF çekirdek işlem otomatik olarak düzeltme yukarı Gezinti özellikleri, döngü ile nesne grafiğinde düşebilir olduğundan.</span><span class="sxs-lookup"><span data-stu-id="773f6-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="773f6-171">Örneğin, bir blog ve yükleme gönderileri gönderileri koleksiyonu başvuruda bulunan bir blog nesnesinde sonuçlanır ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="773f6-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="773f6-172">Bu gönderileri her bir başvuru geri blog sahip olur.</span><span class="sxs-lookup"><span data-stu-id="773f6-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="773f6-173">Bazı serileştirme çerçeveler gibi döngüleri izin vermez.</span><span class="sxs-lookup"><span data-stu-id="773f6-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="773f6-174">Örneğin, bir döngü karşılaştıysanız Json.NET şu özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="773f6-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="773f6-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="773f6-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="773f6-176">ASP.NET Core kullanıyorsanız, nesne grafiğinde bulduğu döngüleri yoksaymak için Json.NET yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="773f6-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="773f6-177">Bu yapılır `ConfigureServices(...)` yönteminde `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="773f6-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
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
