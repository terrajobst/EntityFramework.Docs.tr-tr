---
title: İlişkiler - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 0e60ade605ffb73b02934ea32f1c757affce970d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949224"
---
# <a name="relationships"></a>İlişkiler

Bir ilişki nasıl iki varlıklar tanımlayan birbirleriyle. İlişkisel bir veritabanında bu bir yabancı anahtar kısıtlaması tarafından temsil edilir.

> [!NOTE]  
> Bu makaledeki örnekleri çoğu bire çok ilişkisi kavramları göstermek için kullanın. Bire bir veya çoktan çoğa ilişkilerini örneklerini görmek için [diğer ilişki desenleri](#other-relationship-patterns) makalenin sonunda bölüm.

## <a name="definition-of-terms"></a>Koşulları tanımı

Bir dizi ilişkileri tanımlamak için kullanılan terimler vardır.

* **Bağımlı varlık:** yabancı anahtar özellik içeren varlık budur. Bazen 'alt' ilişkisinin olarak da adlandırılır.

* **Asıl varlık:** birincil/diğer temel özelliklerini içeren varlık budur. Bazen 'parent' ilişkisinin da adlandırılır.

* **Yabancı anahtar:** bağımlı varlığındaki varlık ilgili asıl anahtar özellik değerlerini depolamak için kullanılan özellik.

* **Birincil anahtar:** asıl varlığı benzersiz olarak tanımlayan özellik. Bu, birincil anahtar veya alternatif anahtar olabilir.

* **Gezinti özelliği:** ilgili entity(s) bir başvuruları içeren asıl ve/veya bağımlı varlık üzerinde tanımlanmış bir özellik.

  * **Koleksiyon gezinme özelliği:** birçok ilgili varlıklara başvurular içeren bir gezinme özelliği.

  * **Gezinti özelliğinin başvuru:** tek bir ilgili varlığa bir başvuru tutan bir gezinme özelliği.

  * **Ters gezinti özelliği:** belirli gezinti özelliği ele alırken, diğer ucundaki ilişkinin gezinme özelliğini bu terim başvuruyor.

Aşağıdaki kod listesi arasında bir-çok ilişkisi gösterilir `Blog` ve `Post`

* `Post` bağımlı bir varlık

* `Blog` Asıl varlığı

* `Post.BlogId` yabancı anahtar

* `Blog.BlogId` (Bu durumda alternatif anahtar yerine birincil anahtar olduğu) birincil anahtar

* `Post.Blog` bir başvuru gezinti özelliği

* `Blog.Posts` bir koleksiyon gezinme özelliği

* `Post.Blog` Ters gezinti özelliği `Blog.Posts` (ve tersi)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Kurallar

Bir tür üzerinde bulunan bir gezinti özelliği olduğunda, kural olarak, bir ilişki oluşturulur. İşaret türü geçerli bir veritabanı sağlayıcısı tarafından skaler bir tür olarak eşleştirilemez bir özelliği bir gezinti özelliği kabul edilir.

> [!NOTE]  
> Kural gereği bulunan ilişkiler her zaman asıl varlığın birincil anahtarı hedefleyecektir. Alternatif anahtar hedeflemek için ek yapılandırma Fluent API'sini kullanarak gerçekleştirilmesi gerekir.

### <a name="fully-defined-relationships"></a>Tam olarak tanımlanan ilişkileri

İlişkiler için en yaygın desen, her iki ucunda da ilişki ve bağımlı varlık sınıfı içinde tanımlanan bir yabancı anahtar özellik tanımlanan Gezinti özelliklerini sağlamaktır.

* Ardından, gezinti özellikleri bir çift iki tür arasında bulunursa, aynı ilişki ters Gezinti özellikleri olarak yapılandırılır.

* Bağımlı varlık adlı bir özellik varsa `<primary key property name>`, `<navigation property name><primary key property name>`, veya `<principal entity name><primary key property name>` havuzla yabancı anahtar olarak yapılandırılır.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> İki tür arasında tanımlanan birden çok gezinti özellikleri olup olmadığını (diğer bir deyişle, birden fazla ayrı çiftinin birbirine noktası gezintiler), hiçbir ilişki kural tarafından oluşturulur ve bunları tanımlamak için el ile yapılandırmanız gerekecektir nasıl Gezinti özellikleri çifti ayarlama.

### <a name="no-foreign-key-property"></a>Yabancı anahtar özelliği yok

Bağımlı varlık sınıfı içinde tanımlanan bir yabancı anahtarı özelliği olması önerilir, ancak gerekli değildir. Yabancı anahtar özellik bulunursa, bir gölge yabancı anahtar özellik adıyla tanıtılmak `<navigation property name><principal key property name>` (bkz [gölge Özellikler](shadow-properties.md) daha fazla bilgi için).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Gezinme özelliği

Tek bir gezinti özelliği (ters hiçbir gezinti ve herhangi bir yabancı anahtar özelliği) dahil olmak üzere kural olarak tanımlanmış bir ilişkisi olması yeterlidir. Bir gezinme özelliği ve yabancı anahtar özelliğine de sahip olabilir.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Art arda silme

Kural gereği, art arda silme ayarlanacak *Cascade* gerekli ilişkileri ve *ClientSetNull* isteğe bağlı ilişkiler için. *Art arda* bağımlı varlıklar da silinir anlamına gelir. *ClientSetNull* belleğe yüklenmeyen bağımlı varlıkları kalacağı anlamına gelir değişmeden ve gereken el ile silinmesi veya geçerli bir asıl varlığa işaret edecek şekilde güncelleştirildi. Belleğe yüklenen varlıklar için yabancı anahtar özellikleri null olarak ayarlamak EF Core dener.

Bkz: [gerekli ve isteğe bağlı ilişkileri](#required-and-optional-relationships) gerekli ve isteğe bağlı ilişkileri arasındaki fark için bölüm.

Bkz: [art arda silme](../saving/cascade-delete.md) davranışları ve kuralı tarafından kullanılan varsayılan değerleri farklı hakkında daha fazla ayrıntı silin.

## <a name="data-annotations"></a>Veri ek açıklamaları

İlişkiler yapılandırmak için kullanılan iki veri ek açıklamaları vardır `[ForeignKey]` ve `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Veri ek açıklamaları belirli bir ilişki için yabancı anahtar özellik olarak hangi özelliğinin kullanılması gerektiğini yapılandırmak için kullanabilirsiniz. Kural gereği yabancı anahtar özelliği bulunmayan olduğunda bu genellikle gerçekleştirilir.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> `[ForeignKey]` Ek açıklama ya da gezinti özelliğindeki ilişkisinde yerleştirilebilir. Gezinti özelliğindeki bağımlı varlık sınıfı içinde Git gerekmez.

### <a name="inverseproperty"></a>[InverseProperty]

Veri ek açıklamaları, bağımlı ve asıl varlık Gezinti özellikleri nasıl pair yapılandırmak için kullanabilirsiniz. Bu, genellikle birden fazla Çifti iki varlık türleri arasında gezinti özellikleri olduğunda gerçekleştirilir.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Fluent API'si

Bir ilişki Fluent API'si yapılandırmak için ilişkiyi gezinme özelliklerini belirleyerek başlayın. `HasOne` veya `HasMany` , başlangıç yapılandırması üzerinde varlık türünün gezinme özelliğini tanımlar. Ardından bir çağrı zinciri `WithOne` veya `WithMany` ters gezinti tanımlamak için. `HasOne`/`WithOne` başvuru Gezinti özellikleri için kullanılır ve `HasMany` / `WithMany` koleksiyon Gezinti özellikleri için kullanılır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Gezinme özelliği

Yalnızca sahip olduğunuz bir gezinti özelliği sonra parametresiz aşırı yükleme `WithOne` ve `WithMany`. Bu, kavramsal olarak bir başvuru ya da koleksiyon ilişkinin diğer ucundaki olmakla birlikte, varlık sınıfı dahil bir gezinti özelliği yok, gösterir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Yabancı anahtar

Hangi özelliği için belirtilen bir ilişki yabancı anahtar özellik olarak kullanılmalıdır yapılandırmak için Fluent API'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Aşağıdaki kod listesi, bileşik bir yabancı anahtar yapılandırma işlemi gösterilmektedir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Dize aşırı yüklemesini kullanabilirsiniz `HasForeignKey(...)` yabancı anahtar olarak bir gölge özelliğini yapılandırmak için (bkz [gölge Özellikler](shadow-properties.md) daha fazla bilgi için). Yabancı anahtar olarak (aşağıda gösterildiği gibi) kullanmadan önce gölge özellik modele açıkça eklemenizi öneririz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Sorumlusu anahtarı

Birincil anahtarı dışında bir özellik başvurmak için yabancı anahtarı istiyorsanız, birincil anahtar özelliği ilişki için yapılandırmak için Fluent API'sini kullanabilirsiniz. Asıl Anahtar otomatik olarak yapılandırma özelliği alternatif anahtar olarak Kurulum olabilir (bakın [alternatif anahtarlar](alternate-keys.md) daha fazla bilgi için).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

Aşağıdaki kod listesi, bileşik bir birincil anahtar yapılandırma işlemi gösterilmektedir.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> Birincil anahtar özellikleri belirttiğiniz sırada yabancı anahtar için belirtilen sırayla eşleşmelidir.

### <a name="required-and-optional-relationships"></a>Gerekli ve isteğe bağlı bir ilişki

İlişkiyi gerekli veya isteğe bağlı olup olmadığını yapılandırmak için Fluent API'sini kullanabilirsiniz. Sonuç olarak bu yabancı anahtar özellik gerekli veya isteğe bağlı olup olmadığını denetler. Bu, bir gölge durum yabancı anahtarı kullanılırken en kullanışlıdır. Varlık Sınıfınız içinde bir yabancı anahtar özelliğine sahip sonra requiredness ilişkinin yabancı anahtar özellik gerekli veya isteğe bağlı olarak belirlenir (bkz [gerekli ve isteğe bağlı özellikler](required-optional.md) daha fazla bilgi için bilgiler).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a>Art arda silme

Fluent API'si, açıkça belirtilen bir ilişki art arda silme davranışını yapılandırmak için kullanabilirsiniz.

Bkz: [art arda silme](../saving/cascade-delete.md) verileri kaydetme bölümünde ayrıntılı bir irdelemesi ve her seçeneği.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Diğer ilişki desenleri

### <a name="one-to-one"></a>Bire bir

Bire bir ilişkiler, her iki kenarı da bir başvuru Gezinti özelliğine sahiptir. Bire çok ilişkileri aynı kuralları takip etmeleri ancak benzersiz bir dizin üzerinde yalnızca bir bağımlı her asıl ilgili emin olmak için yabancı anahtar özellik sunulmuştur.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF bir yabancı anahtar özellik algılamak için yeteneklerine bağlı bağımlı olmasını varlıkları birini seçin. Yanlış varlık bağlı belirtildiyse, bu sorunu gidermek için Fluent API'sini kullanabilirsiniz.

İlişki Fluent API'si ile yapılandırırken kullandığınız `HasOne` ve `WithOne` yöntemleri.

Sağlanan genel parametre - bağımlı varlık türünü belirtmek için yabancı anahtarı yapılandırırken fark `HasForeignKey` listesinde. Bir-çok ilişkisine başvuru Gezinti varlığa bağlıdır ve koleksiyon ile bir sorumlu olduğunu işaretlenmemiştir. Ancak bu bire bir ilişki - bu nedenle, bu nedenle açıkça tanımlamak için gerek.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>Çok-çok

Çoktan çoğa ilişkiler birleştirme tablosunu temsil edecek bir varlık sınıfı olmadan henüz desteklenmemektedir. Ancak, birleşim tablosu ve iki ayrı bir-çok ilişkileri eşleme için bir varlık sınıfı dahil ederek bir çoktan çoğa ilişki temsil edebilir.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
