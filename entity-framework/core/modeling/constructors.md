---
title: "Varlık türleriyle oluşturucular - EF çekirdek"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 38ab0c1c3cd8c490875abf30b8478c99bc58630f
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="b50de-102">Varlık türleriyle oluşturucular</span><span class="sxs-lookup"><span data-stu-id="b50de-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="b50de-103">Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="b50de-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="b50de-104">EF çekirdek 2.1 ile başlayarak, artık bir oluşturucu parametrelerle tanımlamasına ve EF varlık örneği oluştururken bu oluşturucuyu çağırmak çekirdek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b50de-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="b50de-105">Oluşturucu parametreleri eşlenen özelliklerine bağlı olabilir veya çeşitli kolaylaştırmak için Hizmetleri davranışları yükleme yavaş ister.</span><span class="sxs-lookup"><span data-stu-id="b50de-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="b50de-106">EF çekirdek 2.1 sürümünden itibaren tüm Oluşturucusu bağlama tarafından kuraldır.</span><span class="sxs-lookup"><span data-stu-id="b50de-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="b50de-107">Kullanmak için belirli oluşturucular yapılandırmasını gelecekteki bir sürümde planlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b50de-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="b50de-108">Bağlama için eşlenen özellikleri</span><span class="sxs-lookup"><span data-stu-id="b50de-108">Binding to mapped properties</span></span>

<span data-ttu-id="b50de-109">Tipik bir Blog/Post modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b50de-109">Consider a typical Blog/Post model:</span></span>

```Csharp
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

<span data-ttu-id="b50de-110">EF çekirdek bu tür örnekleri oluşturduğunda, gibi bir sorgu sonuçları için bunu ilk varsayılan parametresiz bir kurucusu çağırın ve sonra her bir özellik veritabanından değerine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b50de-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="b50de-111">Ancak, parametreli bir oluşturucu EF çekirdek bulursa, parametre adları ve eşleşen türleri özellikleri eşlenen, ardından değerlerle parametreli Oluşturucusu bu özellikler için bunun yerine çağırır ve her bir özellik açıkça ayarlı değil.</span><span class="sxs-lookup"><span data-stu-id="b50de-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="b50de-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b50de-112">For example:</span></span>

```Csharp
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
<span data-ttu-id="b50de-113">Dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="b50de-113">Some things to note:</span></span>
* <span data-ttu-id="b50de-114">Tüm özellikler Oluşturucu parametreleri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b50de-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="b50de-115">Örneğin, EF çekirdek Oluşturucusu normal şekilde çağrıldıktan sonra ayarlayın şekilde Post.Content özelliği herhangi bir oluşturucu parametre tarafından ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="b50de-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="b50de-116">Parametreleri başlamalıdır durumdayken özellikler Pascal ortası olabilir ancak bu parametre türleri ve adları özellik türleri ve adları eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="b50de-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="b50de-117">EF çekirdek ayarlayamıyor, gezinti özellikleri (veya Blog gönderileri yukarıdaki gibi) bir Oluşturucu kullanarak.</span><span class="sxs-lookup"><span data-stu-id="b50de-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="b50de-118">Oluşturucusu genel, özel veya diğer erişilebilirlik sahip.</span><span class="sxs-lookup"><span data-stu-id="b50de-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="b50de-119">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="b50de-119">Read-only properties</span></span>

<span data-ttu-id="b50de-120">Oluşturucu aracılığıyla özellikleri ayarladıktan sonra bazıları salt okunur anlamlı.</span><span class="sxs-lookup"><span data-stu-id="b50de-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="b50de-121">EF çekirdek bu destekler, ancak için dikkat edilecek bazı noktalar vardır:</span><span class="sxs-lookup"><span data-stu-id="b50de-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="b50de-122">Kurala göre özellikleri ayarlayıcılar olmadan eşlenmedi.</span><span class="sxs-lookup"><span data-stu-id="b50de-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="b50de-123">(Bunun yapılması, hesaplanan özellikleri gibi eşlenmelidir değil özellikleri eşlemek eğilimindedir.)</span><span class="sxs-lookup"><span data-stu-id="b50de-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="b50de-124">Otomatik olarak oluşturulan anahtar değerlerini kullanarak yeni varlıklar eklerken anahtar Oluşturucu tarafından ayarlanacak anahtar değeri gerektiğinden okuma-yazma, bir anahtar özelliği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="b50de-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="b50de-125">Bunlardan önlemek için kolay bir yol özel ayarlayıcılar kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="b50de-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="b50de-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b50de-126">For example:</span></span>
```Csharp
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
<span data-ttu-id="b50de-127">EF çekirdek özel ayarlayıcı sahip bir özellik depoda oluşturulan tüm özellikleri eskisi eşleştirilir ve anahtar hala anlamına salt okunur, görür.</span><span class="sxs-lookup"><span data-stu-id="b50de-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="b50de-128">Özel ayarlayıcılar kullanmaya alternatif özellikleri salt okunur gerçekten yapma ve daha açık eşleme içinde OnModelCreating eklemektir.</span><span class="sxs-lookup"><span data-stu-id="b50de-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="b50de-129">Benzer şekilde, bazı özellikler tamamen kaldırılabilir veya yalnızca alanları ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="b50de-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="b50de-130">Örneğin, bu varlık türleri göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b50de-130">For example, consider these entity types:</span></span>

```Csharp
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
<span data-ttu-id="b50de-131">Ve bu yapılandırmada OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="b50de-131">And this configuration in OnModelCreating:</span></span>
```Csharp
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
<span data-ttu-id="b50de-132">Dikkat edilecek noktalar:</span><span class="sxs-lookup"><span data-stu-id="b50de-132">Things to note:</span></span>
* <span data-ttu-id="b50de-133">"Özellik" anahtar şimdi bir alandır.</span><span class="sxs-lookup"><span data-stu-id="b50de-133">The key "property" is now a field.</span></span> <span data-ttu-id="b50de-134">Olmayan bir `readonly` böylece depoda üretilmiş anahtarlar kullanılabilir alan.</span><span class="sxs-lookup"><span data-stu-id="b50de-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="b50de-135">Diğer özellikler yalnızca oluşturucuda ayarlamak salt okunur özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="b50de-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="b50de-136">Birincil anahtar değeri her zaman sadece EF tarafından ayarlayın veya veritabanından okunur oluşturucuda içerecek şekilde gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="b50de-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="b50de-137">Bu basit bir alan olarak "özellik" anahtar bırakır ve onu açıkça oluştururken yeni Web günlükleri veya gönderileri ayarlanmamalıdır temizleyip kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b50de-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="b50de-138">Bu kod '169' gösteren alan hiçbir zaman kullanılır uyarı derleyicide neden olur.</span><span class="sxs-lookup"><span data-stu-id="b50de-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="b50de-139">Gerçekte EF temel alan bir extralinguistic şekilde kullandığından bu yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="b50de-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="b50de-140">İnjecting Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="b50de-140">Injecting services</span></span>

<span data-ttu-id="b50de-141">EF çekirdek, ayrıca bir varlık türünün oluşturucuya "Hizmetler" ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="b50de-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="b50de-142">Örneğin, aşağıdaki yerleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="b50de-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="b50de-143">`DbContext` -türetilen DbContext tür olarak girilebilen geçerli bağlam örneği</span><span class="sxs-lookup"><span data-stu-id="b50de-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="b50de-144">`ILazyLoader` -yükleme yavaş hizmeti--bkz [yükleme yavaş belgelerine](../querying/related-data.md) daha fazla ayrıntı için</span><span class="sxs-lookup"><span data-stu-id="b50de-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="b50de-145">`Action<object, string>` -yükleme yavaş temsilci--bkz [yükleme yavaş belgelerine](../querying/related-data.md) daha fazla ayrıntı için</span><span class="sxs-lookup"><span data-stu-id="b50de-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="b50de-146">`IEntityType` -Bu varlık türüyle ilişkili EF çekirdek meta verileri</span><span class="sxs-lookup"><span data-stu-id="b50de-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="b50de-147">EF çekirdek 2.1 itibariyle yalnızca EF çekirdek tarafından bilinen hizmetleri yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b50de-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="b50de-148">Uygulama Hizmetleri injecting desteği için gelecekteki bir sürüm kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="b50de-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="b50de-149">Örneğin, eklenen bir DbContext seçmeli olarak tümünü yüklemeden ilgili varlıklar hakkında bilgi edinmek için veritabanına erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b50de-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="b50de-150">Aşağıdaki örnekte bu gönderileri yüklemeden bir blog gönderilerinde sayısı elde etmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b50de-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```Csharp
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
<span data-ttu-id="b50de-151">Bu konuda fark edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="b50de-151">A few things to notice about this:</span></span>
* <span data-ttu-id="b50de-152">Şimdiye kadar EF çekirdek tarafından çağrı ise yalnızca ve genel kullanım için başka bir ortak oluşturucu olduğundan Oluşturucusu özeldir.</span><span class="sxs-lookup"><span data-stu-id="b50de-152">The constructor is private, since it is only ever call by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="b50de-153">Eklenen hizmetini kullanarak (yani içerik) karşı savunma kodudur olan `null` nerede EF çekirdek değildir oluştururken örneği durumlarında.</span><span class="sxs-lookup"><span data-stu-id="b50de-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="b50de-154">Hizmet bir okuma/yazma özelliği depolandığı için varlığı yeni bir bağlam örneğine iliştirildiğinde sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="b50de-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="b50de-155">Doğrudan EF çekirdek varlık türlerinizi couples beri şöyle DbContext injecting genellikle bir koruma desen olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="b50de-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="b50de-156">Bu gibi hizmeti ekleme kullanmadan önce tüm seçenekleri dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="b50de-156">Carefully consider all options before using service injection like this.</span></span>
