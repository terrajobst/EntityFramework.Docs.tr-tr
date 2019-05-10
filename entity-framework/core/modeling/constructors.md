---
title: Oluşturucular - EF Core ile varlık türleri
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 5bf49718f02c1860871b1f4c255ec4d98fce2fc7
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405255"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="3e77b-102">Varlık oluşturuculara sahip türleri</span><span class="sxs-lookup"><span data-stu-id="3e77b-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="3e77b-103">Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="3e77b-104">EF Core 2.1 ile başlayarak, artık parametrelere sahip bir oluşturucu tanımlayamaz ve EF Core varlık örneğini oluştururken bu oluşturucuyu çağırma mümkündür.</span><span class="sxs-lookup"><span data-stu-id="3e77b-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="3e77b-105">Oluşturucu parametresi eşleşen özelliklere bağlanabilir veya çeşitli Hizmetleri kolaylaştırmak için davranışları yavaş yükleniyor ister.</span><span class="sxs-lookup"><span data-stu-id="3e77b-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="3e77b-106">EF Core 2.1'ten itibaren tüm Oluşturucusu bağlama tarafından kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="3e77b-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="3e77b-107">Yapılandırma kullanmak için belirli oluşturucular gelecekteki sürümlerde sunulması planlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3e77b-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="3e77b-108">Eşlenen özellikler bağlama</span><span class="sxs-lookup"><span data-stu-id="3e77b-108">Binding to mapped properties</span></span>

<span data-ttu-id="3e77b-109">Tipik bir blogda/model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="3e77b-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="3e77b-110">EF Core bu türlerin örneklerini oluşturduğunda, gibi bir sorgunun sonuçlarını için bunu varsayılan parametresiz bir oluşturucu çağırmanız ve ardından her bir özellik veritabanından değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e77b-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="3e77b-111">Ancak, EF Core ile parametreli bir kurucu bulursa parametre adları ve eşleşen türleri özellikleriyle eşleştirilmiş ve parametreli bir kurucu değerlerle bu özellikler için bunun yerine çağırır ve her bir özellik açıkça ayarlı değil.</span><span class="sxs-lookup"><span data-stu-id="3e77b-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="3e77b-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3e77b-112">For example:</span></span>

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
<span data-ttu-id="3e77b-113">Dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="3e77b-113">Some things to note:</span></span>
* <span data-ttu-id="3e77b-114">Tüm özellikler Oluşturucu parametresi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e77b-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="3e77b-115">Örneğin, EF Core, normal bir şekilde Oluşturucu çağrıldıktan sonra ayarlayacak şekilde Post.Content özelliği için herhangi bir oluşturucu parametresi ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="3e77b-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="3e77b-116">Başlamalıdır olsa özellikler Pascal büyük küçük harfleri olabilir dışında parametre türleri ve adları özellik türleri ve adları eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="3e77b-117">EF Core Gezinti özelliklerini (örneğin, Blog veya yukarıdaki gönderileri) ayarlayamazsınız oluşturucu kullanılarak.</span><span class="sxs-lookup"><span data-stu-id="3e77b-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="3e77b-118">Kurucu ortak, özel veya diğer bir erişilebilirliğine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e77b-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="3e77b-119">Oluşturucu türetilen proxy sınıfı erişilebilir olduğunu ancak yavaş yükleniyor proxy'leri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="3e77b-120">Bu genellikle ortak veya korumalı hale getirme anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="3e77b-121">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="3e77b-121">Read-only properties</span></span>

<span data-ttu-id="3e77b-122">Oluşturucusu özellikler ayarlandıktan sonra bunlardan bazıları salt okunur hale getirmek için anlamlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="3e77b-123">EF Core bu destekler, ancak bazı işlemler için dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="3e77b-123">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="3e77b-124">Kural gereği özelliklerini ayarlayıcılar olmadan eşlenmedi.</span><span class="sxs-lookup"><span data-stu-id="3e77b-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="3e77b-125">(Bunun yapılması harita, hesaplanan özellikler gibi eşlenmelidir değil özellikleri eğilimindedir.)</span><span class="sxs-lookup"><span data-stu-id="3e77b-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="3e77b-126">Otomatik olarak oluşturulan anahtar değerleri kullanılarak anahtar değerini temel oluşturucu tarafından yeni varlıklar eklerken ayarlanması gerekir bu yana, okunur-yazılır, bir anahtar özellik gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="3e77b-127">Bu işlemleri önlemek için kolay bir yol özel ayarlayıcılar kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="3e77b-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="3e77b-128">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3e77b-128">For example:</span></span>
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
<span data-ttu-id="3e77b-129">EF Core ile özel bir ayarlayıcı bir özellik depoda üretilmiş anlamına gelir, tüm özellikler önceki gibi eşlenir ve anahtar yine de salt okunur, görür.</span><span class="sxs-lookup"><span data-stu-id="3e77b-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="3e77b-130">Özel ayarlayıcılar kullanarak özellikleri salt okunur gerçekten yapıp daha açık OnModelCreating eklemesi alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="3e77b-131">Benzer şekilde, bazı özellikler tamamen kaldırılır ve yalnızca alanları ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="3e77b-132">Örneğin, bu varlık türleri göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="3e77b-132">For example, consider these entity types:</span></span>

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
<span data-ttu-id="3e77b-133">Ve bu yapılandırmada OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="3e77b-133">And this configuration in OnModelCreating:</span></span>
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
<span data-ttu-id="3e77b-134">Dikkat edilecek noktalar:</span><span class="sxs-lookup"><span data-stu-id="3e77b-134">Things to note:</span></span>
* <span data-ttu-id="3e77b-135">' % S'anahtarı "özelliği" şimdi bir alandır.</span><span class="sxs-lookup"><span data-stu-id="3e77b-135">The key "property" is now a field.</span></span> <span data-ttu-id="3e77b-136">Bu bir `readonly` depoda üretilmiş anahtarlar kullanılabilir alanın.</span><span class="sxs-lookup"><span data-stu-id="3e77b-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="3e77b-137">Diğer özellikler yalnızca oluşturucuda ayarlayın salt okunur özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="3e77b-138">Birincil anahtar değeri her zaman sadece tarafından EF ayarlayın veya veritabanından okumak, oluşturucuda içerecek şekilde gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="3e77b-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="3e77b-139">Bu anahtar "özelliği" basit bir alan olarak bırakır ve onu açıkça yeni Web günlükleri veya gönderiler oluştururken ayarlanmamalıdır temizleyip kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="3e77b-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="3e77b-140">Bu kod, alanın hiçbir zaman kullanılmaz '169' gösteren uyarı derleyicide neden olur.</span><span class="sxs-lookup"><span data-stu-id="3e77b-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="3e77b-141">Gerçekte extralinguistic bir şekilde alan EF Core kullanmakta olduğundan bu yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="3e77b-142">Hizmetleri ekleme</span><span class="sxs-lookup"><span data-stu-id="3e77b-142">Injecting services</span></span>

<span data-ttu-id="3e77b-143">EF Core, ayrıca bir varlık türü oluşturucusuna "Hizmetler" ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="3e77b-144">Örneğin, aşağıdaki yerleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="3e77b-144">For example, the following can be injected:</span></span>
* <span data-ttu-id="3e77b-145">`DbContext` -türetilmiş DbContext tür olarak girilebilen geçerli bağlam örneği</span><span class="sxs-lookup"><span data-stu-id="3e77b-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="3e77b-146">`ILazyLoader` -yükleme yavaş hizmeti--bkz [yavaş yükleniyor belgeleri](../querying/related-data.md) daha fazla ayrıntı için</span><span class="sxs-lookup"><span data-stu-id="3e77b-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="3e77b-147">`Action<object, string>` -yükleme yavaş temsilci--bkz [yavaş yükleniyor belgeleri](../querying/related-data.md) daha fazla ayrıntı için</span><span class="sxs-lookup"><span data-stu-id="3e77b-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="3e77b-148">`IEntityType` -Bu varlık türüyle ilişkili EF Core meta verileri</span><span class="sxs-lookup"><span data-stu-id="3e77b-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="3e77b-149">EF Core 2.1 itibarıyla EF Core tarafından bilinen hizmetleri yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="3e77b-150">Uygulama Hizmetleri ekleme desteği gelecekteki sürümlerde sunulması kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="3e77b-151">Örneğin, eklenen bir DbContext seçerek tüm bunları yüklemeden ilgili varlıkları hakkında bilgi edinmek için veritabanına erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="3e77b-152">Aşağıdaki örnekte bu gönderiler yüklemeden blog gönderilerinde sayısını almak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="3e77b-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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
<span data-ttu-id="3e77b-153">Bu konuda fark gereken bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="3e77b-153">A few things to notice about this:</span></span>
* <span data-ttu-id="3e77b-154">Yalnızca şimdiye kadar EF Core tarafından çağrılır ve genel kullanım için başka bir Genel oluşturucu olduğundan Oluşturucu özeldir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="3e77b-155">Eklenen hizmet (bağlam) kullanarak kodu karşı savunma olan `null` durumlarında olduğu EF Core değil oluşturduğundan örneği.</span><span class="sxs-lookup"><span data-stu-id="3e77b-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="3e77b-156">Hizmet bir okuma/yazma özelliği olarak depolandığından varlığı yeni bir bağlam örneğine bağlandığında sıfırlanacak.</span><span class="sxs-lookup"><span data-stu-id="3e77b-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="3e77b-157">Doğrudan EF Core için varlık türlerinizi couples beri şöyle DbContext ekleme önleyici bir deseni genellikle kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="3e77b-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="3e77b-158">Bu gibi hizmet ekleme kullanmadan önce tüm seçenekleri dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="3e77b-158">Carefully consider all options before using service injection like this.</span></span>
