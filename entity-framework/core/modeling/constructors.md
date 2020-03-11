---
title: Oluşturucularla varlık türleri-EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417331"
---
# <a name="entity-types-with-constructors"></a>Oluşturucularla varlık türleri

> [!NOTE]  
> Bu özellik EF Core 2,1 ' de yenidir.

EF Core 2,1 ' den başlayarak, bir oluşturucunun bir Oluşturucu tanımlanması ve varlığın bir örneğini oluştururken EF Core bu oluşturucuyu çağırması mümkündür. Oluşturucu parametreleri, eşleşen özelliklere veya yavaş yükleme gibi davranışları kolaylaştırmak için çeşitli türlerde hizmetlere bağlanabilir.

> [!NOTE]  
> EF Core 2,1 itibariyle, tüm Oluşturucu bağlama kurala göre yapılır. Kullanılacak belirli oluşturucuların yapılandırması gelecek bir sürüm için planlanmaktadır.

## <a name="binding-to-mapped-properties"></a>Eşlenmiş özelliklere bağlama

Tipik bir blog/post modelini göz önünde bulundurun:

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

EF Core, bir sorgunun sonuçları gibi bu türlerin örneklerini oluşturduğunda, önce varsayılan parametresiz oluşturucuyu çağırır ve sonra her bir özelliği veritabanından değer olarak ayarlar. Ancak, EF Core eşlenmiş özelliklerden eşleşen parametre adları ve türleri olan parametreli bir Oluşturucu bulursa, bunun yerine bu özellikler için değerler içeren parametreli oluşturucuyu çağırır ve her bir özelliği açıkça ayarlayameyecektir. Örnek:

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

Dikkat edilmesi gerekenler:

* Tüm özelliklerin Oluşturucu parametrelerine sahip olması gerekmez. Örneğin, post. Content özelliği herhangi bir oluşturucu parametresi tarafından ayarlanmadı, bu nedenle EF Core Oluşturucu normal şekilde çağrıldıktan sonra ayarlanır.
* Parametre türleri ve adları Özellik türleri ve adlarıyla eşleşmelidir, ancak parametreler Camel-cased iken özellikler, özelliklerle uyumlu olabilir.
* EF Core, bir Oluşturucu kullanılarak gezinti özelliklerini (blog veya yukarıdaki postalar gibi) ayarlayamamaktır.
* Oluşturucu ortak, özel veya başka bir erişilebilirliğe sahip olabilir. Ancak, yavaş yükleme proxy 'leri, oluşturucunun devralan proxy sınıfından erişilebilir olmasını gerektirir. Genellikle bu, genel ya da korumalı hale gelir.

### <a name="read-only-properties"></a>Salt okunurdur özellikleri

Özellikler Oluşturucu aracılığıyla ayarlandıktan sonra, bunlardan bazılarını Salt okunabilir hale getirmek mantıklı olabilir. EF Core bunu destekler, ancak şunları göz atmak için bazı şeyler vardır:

* Ayarlayıcıları olmayan özellikler kurala göre eşlenmedi. (Bu durumda, hesaplanan özellikler gibi eşlenmemelidir.)
* Otomatik olarak oluşturulan anahtar değerlerini kullanmak, anahtar değerinin yeni varlıklar eklenirken anahtar Oluşturucu tarafından ayarlanması gerektiğinden, okuma-yazma olan bir anahtar özelliği gerektirir.

Bu şeyleri önlemenin kolay bir yolu özel ayarlayıcıları kullanmaktır. Örnek:

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

EF Core, tüm özelliklerin daha önce olduğu gibi eşlendiği ve anahtarın yine de depolanacak şekilde eşlendiği anlamına gelen, bir özel ayarlayıcıya sahip bir özelliği okuma-yazma olarak görür.

Özel ayarlayıcıları kullanmanın bir alternatifi, özellikleri gerçekten salt okunurdur ve Onmodelyaratırken daha açık eşleme eklemektir. Benzer şekilde, bazı özellikler tamamen kaldırılabilir ve yalnızca alanlarla değiştirilebilir. Örneğin, şu varlık türlerini göz önünde bulundurun:

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

Ve Onmodelyaratırken bu yapılandırma:

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

Dikkat edilmesi gerekenler:

* "Property" anahtarı artık bir alandır. Mağaza tarafından oluşturulan anahtarların kullanılabilmesi için `readonly` bir alan değildir.
* Diğer özellikler yalnızca oluşturucuda ayarlanmış salt yazılır özelliklerdir.
* Birincil anahtar değeri yalnızca EF tarafından ayarlandıysa veya veritabanından okunmadan, oluşturucuya dahil etmeniz gerekmez. Bu, "Property" anahtarını basit bir alan olarak bırakır ve yeni blog veya gönderi oluştururken açıkça ayarlanmamalıdır.

> [!NOTE]  
> Bu kod, alanın hiç kullanılmadığını belirten ' 169 ' derleyici uyarısına neden olur. Bu, gerçeklik EF Core alanı bir extralinguistic biçimde kullandığından yoksayılabilir.

## <a name="injecting-services"></a>Ekleme Hizmetleri

EF Core ayrıca varlık türünün oluşturucusuna "Hizmetler" ekleyebilir. Örneğin, aşağıdakiler eklenebilir:

* `DbContext`-türetilmiş DbContext türü olarak da yazılabilir olan geçerli bağlam örneği
* `ILazyLoader`-yavaş yükleme hizmeti-daha fazla bilgi için [yavaş yükleme belgelerine](../querying/related-data.md) bakın
* `Action<object, string>`-yavaş yükleme temsilcisi--daha fazla bilgi için [yavaş yükleme belgelerine](../querying/related-data.md) bakın
* `IEntityType`-bu varlık türüyle ilişkili EF Core meta verileri

> [!NOTE]  
> EF Core 2,1 itibariyle, yalnızca EF Core tarafından bilinen hizmetler eklenebilir. Daha sonraki bir sürüm için ekleme uygulama hizmetleri desteği göz önünde bulundurulmaktadır.

Örneğin, eklenen bir DbContext, bir bütün olarak yüklenmeden ilgili varlıklar hakkında bilgi almak üzere veritabanına seçmeli olarak erişmek için kullanılabilir. Aşağıdaki örnekte, gönderimler yüklenmeden blogdaki gönderi sayısını almak için kullanılır:

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

Bu konuda dikkat etmeniz gereken birkaç nokta vardır:

* Yalnızca EF Core tarafından çağrıldığından ve genel kullanım için başka bir genel Oluşturucu olduğundan, Oluşturucu özeldir.
* Eklenen hizmeti (yani bağlam) kullanan kod, EF Core örneği oluşturmayan durumları işlemek için `null` karşı savunma sürecinde.
* Hizmet bir okuma/yazma özelliğinde depolandığından, varlık yeni bir bağlam örneğine eklendiğinde sıfırlanacak.

> [!WARNING]  
> Bunun gibi DbContext, genellikle EF Core, varlık türlerinizi doğrudan için bir kenar yumuşatma olarak değerlendirilir. Hizmet ekleme işlemini kullanmadan önce bu seçenekleri dikkatle göz önünde bulundurun.
