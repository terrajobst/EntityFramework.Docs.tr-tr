---
title: Oluşturucularla varlık türleri-EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811888"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="cafb1-102">Oluşturucularla varlık türleri</span><span class="sxs-lookup"><span data-stu-id="cafb1-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="cafb1-103">Bu özellik EF Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="cafb1-104">EF Core 2,1 ' den başlayarak, bir oluşturucunun bir Oluşturucu tanımlanması ve varlığın bir örneğini oluştururken EF Core bu oluşturucuyu çağırması mümkündür.</span><span class="sxs-lookup"><span data-stu-id="cafb1-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="cafb1-105">Oluşturucu parametreleri, eşleşen özelliklere veya yavaş yükleme gibi davranışları kolaylaştırmak için çeşitli türlerde hizmetlere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="cafb1-106">EF Core 2,1 itibariyle, tüm Oluşturucu bağlama kurala göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="cafb1-107">Kullanılacak belirli oluşturucuların yapılandırması gelecek bir sürüm için planlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="cafb1-108">Eşlenmiş özelliklere bağlama</span><span class="sxs-lookup"><span data-stu-id="cafb1-108">Binding to mapped properties</span></span>

<span data-ttu-id="cafb1-109">Tipik bir blog/post modelini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="cafb1-109">Consider a typical Blog/Post model:</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="cafb1-110">EF Core, bir sorgunun sonuçları gibi bu türlerin örneklerini oluşturduğunda, önce varsayılan parametresiz oluşturucuyu çağırır ve sonra her bir özelliği veritabanından değer olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="cafb1-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="cafb1-111">Ancak, EF Core eşlenmiş özelliklerden eşleşen parametre adları ve türleri olan parametreli bir Oluşturucu bulursa, bunun yerine bu özellikler için değerler içeren parametreli oluşturucuyu çağırır ve her bir özelliği açıkça ayarlayameyecektir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="cafb1-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cafb1-112">For example:</span></span>

``` csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="cafb1-113">Dikkat edilmesi gerekenler:</span><span class="sxs-lookup"><span data-stu-id="cafb1-113">Some things to note:</span></span>

* <span data-ttu-id="cafb1-114">Tüm özelliklerin Oluşturucu parametrelerine sahip olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="cafb1-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="cafb1-115">Örneğin, post. Content özelliği herhangi bir oluşturucu parametresi tarafından ayarlanmadı, bu nedenle EF Core Oluşturucu normal şekilde çağrıldıktan sonra ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="cafb1-116">Parametre türleri ve adları Özellik türleri ve adlarıyla eşleşmelidir, ancak parametreler Camel-cased iken özellikler, özelliklerle uyumlu olabilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="cafb1-117">EF Core, bir Oluşturucu kullanılarak gezinti özelliklerini (blog veya yukarıdaki postalar gibi) ayarlayamamaktır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="cafb1-118">Oluşturucu ortak, özel veya başka bir erişilebilirliğe sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="cafb1-119">Ancak, yavaş yükleme proxy 'leri, oluşturucunun devralan proxy sınıfından erişilebilir olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="cafb1-120">Genellikle bu, genel ya da korumalı hale gelir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="cafb1-121">Salt okunurdur özellikleri</span><span class="sxs-lookup"><span data-stu-id="cafb1-121">Read-only properties</span></span>

<span data-ttu-id="cafb1-122">Özellikler Oluşturucu aracılığıyla ayarlandıktan sonra, bunlardan bazılarını Salt okunabilir hale getirmek mantıklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="cafb1-123">EF Core bunu destekler, ancak şunları göz atmak için bazı şeyler vardır:</span><span class="sxs-lookup"><span data-stu-id="cafb1-123">EF Core supports this, but there are some things to look out for:</span></span>

* <span data-ttu-id="cafb1-124">Ayarlayıcıları olmayan özellikler kurala göre eşlenmedi.</span><span class="sxs-lookup"><span data-stu-id="cafb1-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="cafb1-125">(Bu durumda, hesaplanan özellikler gibi eşlenmemelidir.)</span><span class="sxs-lookup"><span data-stu-id="cafb1-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="cafb1-126">Otomatik olarak oluşturulan anahtar değerlerini kullanmak, anahtar değerinin yeni varlıklar eklenirken anahtar Oluşturucu tarafından ayarlanması gerektiğinden, okuma-yazma olan bir anahtar özelliği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="cafb1-127">Bu şeyleri önlemenin kolay bir yolu özel ayarlayıcıları kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="cafb1-128">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cafb1-128">For example:</span></span>

``` csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="cafb1-129">EF Core, tüm özelliklerin daha önce olduğu gibi eşlendiği ve anahtarın yine de depolanacak şekilde eşlendiği anlamına gelen, bir özel ayarlayıcıya sahip bir özelliği okuma-yazma olarak görür.</span><span class="sxs-lookup"><span data-stu-id="cafb1-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="cafb1-130">Özel ayarlayıcıları kullanmanın bir alternatifi, özellikleri gerçekten salt okunurdur ve Onmodelyaratırken daha açık eşleme eklemektir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="cafb1-131">Benzer şekilde, bazı özellikler tamamen kaldırılabilir ve yalnızca alanlarla değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="cafb1-132">Örneğin, şu varlık türlerini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="cafb1-132">For example, consider these entity types:</span></span>

``` csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="cafb1-133">Ve Onmodelyaratırken bu yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="cafb1-133">And this configuration in OnModelCreating:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```

<span data-ttu-id="cafb1-134">Dikkat edilmesi gerekenler:</span><span class="sxs-lookup"><span data-stu-id="cafb1-134">Things to note:</span></span>

* <span data-ttu-id="cafb1-135">"Property" anahtarı artık bir alandır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-135">The key "property" is now a field.</span></span> <span data-ttu-id="cafb1-136">Mağaza tarafından oluşturulan anahtarların kullanılabilmesi için `readonly` bir alan değildir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="cafb1-137">Diğer özellikler yalnızca oluşturucuda ayarlanmış salt yazılır özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="cafb1-138">Birincil anahtar değeri yalnızca EF tarafından ayarlandıysa veya veritabanından okunmadan, oluşturucuya dahil etmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="cafb1-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="cafb1-139">Bu, "Property" anahtarını basit bir alan olarak bırakır ve yeni blog veya gönderi oluştururken açıkça ayarlanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="cafb1-140">Bu kod, alanın hiç kullanılmadığını belirten ' 169 ' derleyici uyarısına neden olur.</span><span class="sxs-lookup"><span data-stu-id="cafb1-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="cafb1-141">Bu, gerçeklik EF Core alanı bir extralinguistic biçimde kullandığından yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="cafb1-142">Ekleme Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="cafb1-142">Injecting services</span></span>

<span data-ttu-id="cafb1-143">EF Core ayrıca varlık türünün oluşturucusuna "Hizmetler" ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="cafb1-144">Örneğin, aşağıdakiler eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="cafb1-144">For example, the following can be injected:</span></span>

* <span data-ttu-id="cafb1-145">`DbContext`-türetilmiş DbContext türü olarak da yazılabilir olan geçerli bağlam örneği</span><span class="sxs-lookup"><span data-stu-id="cafb1-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="cafb1-146">`ILazyLoader`-yavaş yükleme hizmeti-daha fazla bilgi için [yavaş yükleme belgelerine](../querying/related-data.md) bakın</span><span class="sxs-lookup"><span data-stu-id="cafb1-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="cafb1-147">`Action<object, string>`-yavaş yükleme temsilcisi--daha fazla bilgi için [yavaş yükleme belgelerine](../querying/related-data.md) bakın</span><span class="sxs-lookup"><span data-stu-id="cafb1-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="cafb1-148">`IEntityType`-bu varlık türüyle ilişkili EF Core meta verileri</span><span class="sxs-lookup"><span data-stu-id="cafb1-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="cafb1-149">EF Core 2,1 itibariyle, yalnızca EF Core tarafından bilinen hizmetler eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="cafb1-150">Daha sonraki bir sürüm için ekleme uygulama hizmetleri desteği göz önünde bulundurulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cafb1-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="cafb1-151">Örneğin, eklenen bir DbContext, bir bütün olarak yüklenmeden ilgili varlıklar hakkında bilgi almak üzere veritabanına seçmeli olarak erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="cafb1-152">Aşağıdaki örnekte, gönderimler yüklenmeden blogdaki gönderi sayısını almak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cafb1-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="cafb1-153">Bu konuda dikkat etmeniz gereken birkaç nokta vardır:</span><span class="sxs-lookup"><span data-stu-id="cafb1-153">A few things to notice about this:</span></span>

* <span data-ttu-id="cafb1-154">Yalnızca EF Core tarafından çağrıldığından ve genel kullanım için başka bir genel Oluşturucu olduğundan, Oluşturucu özeldir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="cafb1-155">Eklenen hizmeti (yani bağlam) kullanan kod, EF Core örneği oluşturmayan durumları işlemek için `null` karşı savunma sürecinde.</span><span class="sxs-lookup"><span data-stu-id="cafb1-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="cafb1-156">Hizmet bir okuma/yazma özelliğinde depolandığından, varlık yeni bir bağlam örneğine eklendiğinde sıfırlanacak.</span><span class="sxs-lookup"><span data-stu-id="cafb1-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="cafb1-157">Bunun gibi DbContext, genellikle EF Core, varlık türlerinizi doğrudan için bir kenar yumuşatma olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cafb1-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="cafb1-158">Hizmet ekleme işlemini kullanmadan önce bu seçenekleri dikkatle göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="cafb1-158">Carefully consider all options before using service injection like this.</span></span>
