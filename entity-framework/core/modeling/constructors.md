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
# <a name="entity-types-with-constructors"></a>Varlık türleriyle oluşturucular

> [!NOTE]  
> Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.

EF çekirdek 2.1 ile başlayarak, artık bir oluşturucu parametrelerle tanımlamasına ve EF varlık örneği oluştururken bu oluşturucuyu çağırmak çekirdek mümkündür. Oluşturucu parametreleri eşlenen özelliklerine bağlı olabilir veya çeşitli kolaylaştırmak için Hizmetleri davranışları yükleme yavaş ister.

> [!NOTE]  
> EF çekirdek 2.1 sürümünden itibaren tüm Oluşturucusu bağlama tarafından kuraldır. Kullanmak için belirli oluşturucular yapılandırmasını gelecekteki bir sürümde planlanmaktadır.

## <a name="binding-to-mapped-properties"></a>Bağlama için eşlenen özellikleri

Tipik bir Blog/Post modeli göz önünde bulundurun:

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

EF çekirdek bu tür örnekleri oluşturduğunda, gibi bir sorgu sonuçları için bunu ilk varsayılan parametresiz bir kurucusu çağırın ve sonra her bir özellik veritabanından değerine ayarlayın. Ancak, parametreli bir oluşturucu EF çekirdek bulursa, parametre adları ve eşleşen türleri özellikleri eşlenen, ardından değerlerle parametreli Oluşturucusu bu özellikler için bunun yerine çağırır ve her bir özellik açıkça ayarlı değil. Örneğin:

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
Dikkat edilecek bazı noktalar:
* Tüm özellikler Oluşturucu parametreleri olması gerekir. Örneğin, EF çekirdek Oluşturucusu normal şekilde çağrıldıktan sonra ayarlayın şekilde Post.Content özelliği herhangi bir oluşturucu parametre tarafından ayarlanmadı.
* Parametreleri başlamalıdır durumdayken özellikler Pascal ortası olabilir ancak bu parametre türleri ve adları özellik türleri ve adları eşleşmelidir.
* EF çekirdek ayarlayamıyor, gezinti özellikleri (veya Blog gönderileri yukarıdaki gibi) bir Oluşturucu kullanarak.
* Oluşturucusu genel, özel veya diğer erişilebilirlik sahip.

### <a name="read-only-properties"></a>Salt okunur özellikler

Oluşturucu aracılığıyla özellikleri ayarladıktan sonra bazıları salt okunur anlamlı. EF çekirdek bu destekler, ancak için dikkat edilecek bazı noktalar vardır:
* Kurala göre özellikleri ayarlayıcılar olmadan eşlenmedi. (Bunun yapılması, hesaplanan özellikleri gibi eşlenmelidir değil özellikleri eşlemek eğilimindedir.)
* Otomatik olarak oluşturulan anahtar değerlerini kullanarak yeni varlıklar eklerken anahtar Oluşturucu tarafından ayarlanacak anahtar değeri gerektiğinden okuma-yazma, bir anahtar özelliği gerektirir.

Bunlardan önlemek için kolay bir yol özel ayarlayıcılar kullanmaktır. Örneğin:
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
EF çekirdek özel ayarlayıcı sahip bir özellik depoda oluşturulan tüm özellikleri eskisi eşleştirilir ve anahtar hala anlamına salt okunur, görür.

Özel ayarlayıcılar kullanmaya alternatif özellikleri salt okunur gerçekten yapma ve daha açık eşleme içinde OnModelCreating eklemektir. Benzer şekilde, bazı özellikler tamamen kaldırılabilir veya yalnızca alanları ile değiştirilir. Örneğin, bu varlık türleri göz önünde bulundurun:

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
Ve bu yapılandırmada OnModelCreating:
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
Dikkat edilecek noktalar:
* "Özellik" anahtar şimdi bir alandır. Olmayan bir `readonly` böylece depoda üretilmiş anahtarlar kullanılabilir alan. 
* Diğer özellikler yalnızca oluşturucuda ayarlamak salt okunur özelliklerdir.
* Birincil anahtar değeri her zaman sadece EF tarafından ayarlayın veya veritabanından okunur oluşturucuda içerecek şekilde gerek yoktur. Bu basit bir alan olarak "özellik" anahtar bırakır ve onu açıkça oluştururken yeni Web günlükleri veya gönderileri ayarlanmamalıdır temizleyip kolaylaştırır.

> [!NOTE]  
> Bu kod '169' gösteren alan hiçbir zaman kullanılır uyarı derleyicide neden olur. Gerçekte EF temel alan bir extralinguistic şekilde kullandığından bu yoksayılabilir.

## <a name="injecting-services"></a>İnjecting Hizmetleri

EF çekirdek, ayrıca bir varlık türünün oluşturucuya "Hizmetler" ekleyemezsiniz. Örneğin, aşağıdaki yerleştirilebilir:
* `DbContext` -türetilen DbContext tür olarak girilebilen geçerli bağlam örneği
* `ILazyLoader` -yükleme yavaş hizmeti--bkz [yükleme yavaş belgelerine](../querying/related-data.md) daha fazla ayrıntı için
* `Action<object, string>` -yükleme yavaş temsilci--bkz [yükleme yavaş belgelerine](../querying/related-data.md) daha fazla ayrıntı için
* `IEntityType` -Bu varlık türüyle ilişkili EF çekirdek meta verileri

> [!NOTE]  
> EF çekirdek 2.1 itibariyle yalnızca EF çekirdek tarafından bilinen hizmetleri yerleştirilebilir. Uygulama Hizmetleri injecting desteği için gelecekteki bir sürüm kabul edilir.

Örneğin, eklenen bir DbContext seçmeli olarak tümünü yüklemeden ilgili varlıklar hakkında bilgi edinmek için veritabanına erişmek için kullanılabilir. Aşağıdaki örnekte bu gönderileri yüklemeden bir blog gönderilerinde sayısı elde etmek için kullanılır:

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
Bu konuda fark edilecek bazı noktalar:
* Şimdiye kadar EF çekirdek tarafından çağrı ise yalnızca ve genel kullanım için başka bir ortak oluşturucu olduğundan Oluşturucusu özeldir.
* Eklenen hizmetini kullanarak (yani içerik) karşı savunma kodudur olan `null` nerede EF çekirdek değildir oluştururken örneği durumlarında.
* Hizmet bir okuma/yazma özelliği depolandığı için varlığı yeni bir bağlam örneğine iliştirildiğinde sıfırlanır.

> [!WARNING]  
> Doğrudan EF çekirdek varlık türlerinizi couples beri şöyle DbContext injecting genellikle bir koruma desen olarak kabul edilir. Bu gibi hizmeti ekleme kullanmadan önce tüm seçenekleri dikkatlice düşünün.
