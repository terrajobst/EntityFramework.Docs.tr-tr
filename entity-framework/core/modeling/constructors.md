---
title: Oluşturucular - EF Core ile varlık türleri
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 1b36197465fb9a6571a306d36eb1e9d885a5399e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152471"
---
# <a name="entity-types-with-constructors"></a>Varlık oluşturuculara sahip türleri

> [!NOTE]  
> Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.

EF Core 2.1 ile başlayarak, artık parametrelere sahip bir oluşturucu tanımlayamaz ve EF Core varlık örneğini oluştururken bu oluşturucuyu çağırma mümkündür. Oluşturucu parametresi eşleşen özelliklere bağlanabilir veya çeşitli Hizmetleri kolaylaştırmak için davranışları yavaş yükleniyor ister.

> [!NOTE]  
> EF Core 2.1'ten itibaren tüm Oluşturucusu bağlama tarafından kuralıdır. Yapılandırma kullanmak için belirli oluşturucular gelecekteki sürümlerde sunulması planlanmaktadır.

## <a name="binding-to-mapped-properties"></a>Eşlenen özellikler bağlama

Tipik bir blogda/model göz önünde bulundurun:

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

EF Core bu türlerin örneklerini oluşturduğunda, gibi bir sorgunun sonuçlarını için bunu varsayılan parametresiz bir oluşturucu çağırmanız ve ardından her bir özellik veritabanından değere ayarlayın. Ancak, EF Core ile parametreli bir kurucu bulursa parametre adları ve eşleşen türleri özellikleriyle eşleştirilmiş ve parametreli bir kurucu değerlerle bu özellikler için bunun yerine çağırır ve her bir özellik açıkça ayarlı değil. Örneğin:

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
Dikkat edilecek bazı noktalar:
* Tüm özellikler Oluşturucu parametresi olmalıdır. Örneğin, EF Core, normal bir şekilde Oluşturucu çağrıldıktan sonra ayarlayacak şekilde Post.Content özelliği için herhangi bir oluşturucu parametresi ayarlanmadı.
* Başlamalıdır olsa özellikler Pascal büyük küçük harfleri olabilir dışında parametre türleri ve adları özellik türleri ve adları eşleşmelidir.
* EF Core Gezinti özelliklerini (örneğin, Blog veya yukarıdaki gönderileri) ayarlayamazsınız oluşturucu kullanılarak.
* Kurucu ortak, özel veya diğer bir erişilebilirliğine sahip olmalıdır.

### <a name="read-only-properties"></a>Salt okunur özellikler

Oluşturucusu özellikler ayarlandıktan sonra bunlardan bazıları salt okunur hale getirmek için anlamlı olabilir. EF Core bu destekler, ancak bazı işlemler için dikkat edin:
* Kural gereği özelliklerini ayarlayıcılar olmadan eşlenmedi. (Bunun yapılması harita, hesaplanan özellikler gibi eşlenmelidir değil özellikleri eğilimindedir.)
* Otomatik olarak oluşturulan anahtar değerleri kullanılarak anahtar değerini temel oluşturucu tarafından yeni varlıklar eklerken ayarlanması gerekir bu yana, okunur-yazılır, bir anahtar özellik gerektirir.

Bu işlemleri önlemek için kolay bir yol özel ayarlayıcılar kullanmaktır. Örneğin:
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
EF Core ile özel bir ayarlayıcı bir özellik depoda üretilmiş anlamına gelir, tüm özellikler önceki gibi eşlenir ve anahtar yine de salt okunur, görür.

Özel ayarlayıcılar kullanarak özellikleri salt okunur gerçekten yapıp daha açık OnModelCreating eklemesi alternatiftir. Benzer şekilde, bazı özellikler tamamen kaldırılır ve yalnızca alanları ile değiştirilmiştir. Örneğin, bu varlık türleri göz önünde bulundurun:

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
Ve bu yapılandırmada OnModelCreating:
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
Dikkat edilecek noktalar:
* ' % S'anahtarı "özelliği" şimdi bir alandır. Bu bir `readonly` depoda üretilmiş anahtarlar kullanılabilir alanın.
* Diğer özellikler yalnızca oluşturucuda ayarlayın salt okunur özelliklerdir.
* Birincil anahtar değeri her zaman sadece tarafından EF ayarlayın veya veritabanından okumak, oluşturucuda içerecek şekilde gerek yoktur. Bu anahtar "özelliği" basit bir alan olarak bırakır ve onu açıkça yeni Web günlükleri veya gönderiler oluştururken ayarlanmamalıdır temizleyip kolaylaştırır.

> [!NOTE]  
> Bu kod, alanın hiçbir zaman kullanılmaz '169' gösteren uyarı derleyicide neden olur. Gerçekte extralinguistic bir şekilde alan EF Core kullanmakta olduğundan bu yoksayılabilir.

## <a name="injecting-services"></a>Hizmetleri ekleme

EF Core, ayrıca bir varlık türü oluşturucusuna "Hizmetler" ekleyebilir. Örneğin, aşağıdaki yerleştirilebilir:
* `DbContext` -türetilmiş DbContext tür olarak girilebilen geçerli bağlam örneği
* `ILazyLoader` -yükleme yavaş hizmeti--bkz [yavaş yükleniyor belgeleri](../querying/related-data.md) daha fazla ayrıntı için
* `Action<object, string>` -yükleme yavaş temsilci--bkz [yavaş yükleniyor belgeleri](../querying/related-data.md) daha fazla ayrıntı için
* `IEntityType` -Bu varlık türüyle ilişkili EF Core meta verileri

> [!NOTE]  
> EF Core 2.1 itibarıyla EF Core tarafından bilinen hizmetleri yerleştirilebilir. Uygulama Hizmetleri ekleme desteği gelecekteki sürümlerde sunulması kabul edilir.

Örneğin, eklenen bir DbContext seçerek tüm bunları yüklemeden ilgili varlıkları hakkında bilgi edinmek için veritabanına erişmek için kullanılabilir. Aşağıdaki örnekte bu gönderiler yüklemeden blog gönderilerinde sayısını almak için kullanılır:

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
Bu konuda fark gereken bazı noktalar:
* Yalnızca şimdiye kadar EF Core tarafından çağrılır ve genel kullanım için başka bir Genel oluşturucu olduğundan Oluşturucu özeldir.
* Eklenen hizmet (bağlam) kullanarak kodu karşı savunma olan `null` durumlarında olduğu EF Core değil oluşturduğundan örneği.
* Hizmet bir okuma/yazma özelliği olarak depolandığından varlığı yeni bir bağlam örneğine bağlandığında sıfırlanacak.

> [!WARNING]  
> Doğrudan EF Core için varlık türlerinizi couples beri şöyle DbContext ekleme önleyici bir deseni genellikle kabul edilir. Bu gibi hizmet ekleme kullanmadan önce tüm seçenekleri dikkatlice düşünün.
