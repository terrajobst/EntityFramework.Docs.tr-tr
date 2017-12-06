---
title: "İlişkileri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a>İlişkileri

Bir ilişki nasıl iki varlık tanımlar birbirleri ile. İlişkisel bir veritabanında bu bir yabancı anahtar kısıtlaması ile temsil edilir.

> [!NOTE]  
> Bu makaledeki örnekler çoğunu bir-çok ilişkisi kavramları göstermek için kullanın. Bire bir ve çok-çok ilişkileri örnekleri görmek için [diğer ilişki desenleri](#other-relationship-patterns) makalenin sonunda bölüm.

## <a name="definition-of-terms"></a>Terimlerin tanımı

Birkaç ilişkileri tanımlamak için kullanılan terimler vardır

* **Bağımlı varlık:** bu yabancı anahtar özellik içeren bir varlıktır. Bazen 'alt' ilişkisinin olarak da adlandırılır.

* **Asıl varlık:** bu birincil/alternatif temel özelliklerini içeren varlıktır. Bazen 'parent' ilişkisinin da adlandırılır.

* **Yabancı anahtar:** bağımlı varlığındaki için ilgili varlık asıl anahtar özellik değerlerini depolamak için kullanılan özellik.

* **Asıl Anahtar:** asıl varlık benzersiz olarak tanımlayan özellik. Bu, birincil anahtar veya alternatif bir anahtarı olabilir.

* **Gezinti özelliği:** ilgili entity(s) alanlara içeren asıl ve/veya bağımlı varlık üzerinde tanımlanmış bir özellik.

  * **Koleksiyon gezinti özelliği:** birçok ilgili varlık başvuruları içeren bir gezinme özelliği.

  * **Başvuru gezinti özelliği:** ilgili tek bir varlığa bir başvuru içeren bir gezinme özelliği.

  * **Ters gezinti özelliği:** belirli gezinti özelliği açıklanırken, bu terimi ilişkinin diğer ucundaki gezinti özelliği anlamına gelir.

Aşağıdaki kod listesi arasında bir-çok ilişkisi gösterir `Blog` ve`Post`

* `Post`bağımlı varlık

* `Blog`Asıl varlığı

* `Post.BlogId`yabancı anahtar

* `Blog.BlogId`(Bu durumda, alternatif bir anahtarı yerine bir birincil anahtar olan) asıl anahtarı

* `Post.Blog`bir başvuru Gezinti özelliğidir

* `Blog.Posts`bir koleksiyon gezinti özelliği değil

* `Post.Blog`Ters gezinti özelliği `Blog.Posts` (ve tersi)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Kurallar

Bir tür üzerinde bulunan bir gezinti özelliği olduğunda kurala göre bir ilişki oluşturulur. İşaret türü skaler bir tür geçerli veritabanı sağlayıcısı tarafından eşleştirilemez varsa bir özelliği bir gezinti özelliği olarak kabul edilir.

> [!NOTE]  
> Kural tarafından bulunan ilişkileri her zaman asıl varlığın birincil anahtarı hedefleyecektir. Alternatif bir anahtarı hedeflemek için ek yapılandırma Fluent API kullanılarak gerçekleştirilmelidir.

### <a name="fully-defined-relationships"></a>Tam olarak tanımlanmış ilişkileri

En yaygın düzeni ilişkiler için Gezinti özellikleri ilişkinin ve yabancı anahtar özelliği bağımlı varlık sınıfında tanımlanmış iki ucunda tanımlanmış olmalıdır.

* Sonra Gezinti özellikleri çifti arasında iki tür bulunursa, aynı ilişki ters Gezinti özellikleri olarak yapılandırılır.

* Bağımlı varlık adında bir özellik varsa `<primary key property name>`, `<navigation property name><primary key property name>`, veya `<principal entity name><primary key property name>` yabancı anahtar olarak yapılandırılacak sonra.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Birden çok gezinti özellikleri arasında iki tür tanımlı olup olmadığını (yani birbirlerine noktası gezintilerini birden fazla ayrı çiftinin), ardından hiçbir ilişki kurala göre oluşturulur ve bunları tanımlamak için el ile yapılandırmanız gerekecektir nasıl Gezinti özellikleri çifti ayarlama.

### <a name="no-foreign-key-property"></a>Yabancı anahtar özellik

Yabancı anahtar özelliği bağımlı varlık sınıfında tanımlanmış önerilir, ancak gerekli değildir. Yabancı anahtar özellik bulunursa, bir gölge yabancı anahtar özellik adı ile görülecektir `<navigation property name><principal key property name>` (bkz [gölge özellikleri](shadow-properties.md) daha fazla bilgi için).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Tek gezinme özelliği

Tek bir gezinti özelliği (hiçbir ters gezinti ve yabancı anahtar özellik) dahil olmak üzere kurala göre tanımlanan bir ilişki için yeterlidir. Tek bir gezinme özelliği ve yabancı anahtar özelliğine de sahip olabilir.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Art arda silme

Kurala göre art arda silme ayarlanacak *Cascade* gerekli ilişkiler ve *ClientSetNull* isteğe bağlı ilişkiler. *CASCADE* bağımlı varlıkları da silinir anlamına gelir. *ClientSetNull* belleğe yüklenmez bağımlı varlıkları kalacak anlamına gelir değişmeden ve el ile silinmiş veya gerekir geçerli bir asıl varlık işaret edecek şekilde güncelleştirildi. Belleğe yüklenen varlıklar için EF çekirdek yabancı anahtar özellikleri null olarak ayarlamanız dener.

Bkz: [gerekli ve isteğe bağlı ilişkileri](#required-and-optional-relationships) bölümü için gerekli ve isteğe bağlı ilişkileri arasındaki fark.

Bkz: [art arda silme](../saving/cascade-delete.md) farklı hakkında daha fazla ayrıntı davranışları ve kural tarafından kullanılan varsayılan silmek için.

## <a name="data-annotations"></a>Veri ek açıklamaları

İlişkiler yapılandırmak için kullanılan iki veri ek açıklamaları vardır `[ForeignKey]` ve `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Hangi özelliği için belirtilen bir ilişki yabancı anahtar özelliği olarak kullanılmalıdır yapılandırmak için veri ek açıklamaları kullanabilirsiniz. Yabancı anahtar özelliği kurala göre bulunmayan olduğunda bu genellikle gerçekleştirilir.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> `[ForeignKey]` Ek açıklama ya da gezinti özelliği ilişkideki yerleştirilebilen. Bağımlı varlık sınıfı Gezinti özelliğinde gitmek gerekmez.

### <a name="inverseproperty"></a>[InverseProperty]

Veri ek açıklamaları nasıl bağımlı ve asıl varlık Gezinti özellikleri eşleştirin yapılandırmak için kullanabilirsiniz. İki varlık türleri arasında gezinti özellikleri birden fazla Çifti olduğunda bu genellikle gerçekleştirilir.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Fluent API'si

Bir ilişki Fluent API'si yapılandırmak için ilişkisi olun Gezinti özellikleri belirleyerek başlatın. `HasOne`veya `HasMany` , başlangıç yapılandırması üzerinde varlık türünün gezinti özelliği tanımlar. Ardından bir çağrı zincir `WithOne` veya `WithMany` ters gezinti tanımlamak için. `HasOne`/`WithOne`başvuru Gezinti özellikleri için kullanılır ve `HasMany` / `WithMany` koleksiyon Gezinti özellikleri için kullanılır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Tek gezinme özelliği

Yalnızca bir gezinti özelliğine sahip sonra parametresiz aşırı `WithOne` ve `WithMany`. Bu kavramsal olarak bir başvuru veya koleksiyon ilişkinin diğer ucundaki olmakla birlikte, varlık sınıfında dahil gezinti özelliği yok olduğunu gösterir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Yabancı anahtar

Hangi özelliği için belirtilen bir ilişki yabancı anahtar özelliği olarak kullanılmalıdır yapılandırmak için Fluent API kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Aşağıdaki kod listesi nasıl birleşik yabancı anahtar yapılandırılacağını göstermektedir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Dize aşırı yüklemesini kullanabilirsiniz `HasForeignKey(...)` gölge özelliğini yabancı anahtar olarak yapılandırmak için (bkz [gölge özellikleri](shadow-properties.md) daha fazla bilgi için). Yabancı anahtar olarak (aşağıda gösterildiği gibi) kullanmadan önce gölge özelliği modeline açıkça eklenmesi öneririz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Asıl anahtar

Birincil anahtar dışındaki bir özellik başvurmak için yabancı anahtarı istiyorsanız, asıl anahtar özelliği ilişki için yapılandırmak için Fluent API kullanabilirsiniz. Asıl Anahtar otomatik olarak yapılandırma özelliği tarafından Kurulum alternatif bir anahtar olarak (bkz [alternatif anahtarları](alternate-keys.md) daha fazla bilgi için).

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

Aşağıdaki kod listesi, bileşik bir asıl anahtar yapılandırma gösterilmektedir.

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
> Asıl anahtar özelliklerini belirtmek için yabancı anahtar belirtilen sırada eşleşmesi gerekir.

### <a name="required-and-optional-relationships"></a>Gerekli ve isteğe bağlı ilişkileri

Fluent API ilişki gerekli veya isteğe bağlı olup olmadığını yapılandırmak için kullanabilirsiniz. Sonuç olarak bu yabancı anahtar özellik gerekli veya isteğe bağlı olup olmadığını denetler. Gölge durumu yabancı anahtar kullanırken bu kullanışlıdır. Varlık sınıfınızda bir yabancı anahtar özelliğine sahip sonra requiredness ilişkinin yabancı anahtar özellik gerekli veya isteğe bağlı olarak belirlenir (bkz [gerekli ve isteğe bağlı özellikler](required-optional.md) daha fazla bilgi için bilgileri).

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

Fluent API açıkça belirtilen bir ilişki için cascade delete davranışı yapılandırmak için kullanabilirsiniz.

Bkz: [art arda silme](../saving/cascade-delete.md) her seçenek hakkında ayrıntılı bilgi için verileri kaydetme bölümünde.

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

Bire bir ilişkiler iki tarafta bir başvuru gezinti özelliği vardır. Bir-çok ilişkileri aynı kuralları izleyin, ancak benzersiz bir dizin, yalnızca bir bağımlı her asıl ilgili emin olmak için yabancı anahtar özellik sunulmuştur.

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
> EF yabancı anahtar özelliği algılamak için kendi yeteneği dayalı bağımlı olması için varlıkları birini seçeceksiniz. Yanlış varlık bağımlı seçilirse, bu sorunu gidermek için Fluent API kullanabilirsiniz.

İlişki Fluent API'si ile yapılandırırken kullandığınız `HasOne` ve `WithOne` yöntemleri.

Bağımlı varlık türü - belirtmeniz gerekir yabancı anahtar yapılandırırken genel parametresi için sağlanan fark `HasForeignKey` listesinde. Bir-çok ilişkisi içinde varlıkla başvuru Gezinti bağımlı ve koleksiyon ile bir asıl olduğunu işaretlenmemiştir. Ancak bu bire bir ilişkide - bu nedenle bunu açıkça tanımlamak için gereken.

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

### <a name="many-to-many"></a>Çok-

Çok-çok ilişkileri birleşim tablosundan temsil etmek için bir varlık sınıfı olmadan henüz desteklenmiyor. Ancak, bir varlık sınıfı birleşim tablosundan ve iki ayrı bir-çok ilişkileri eşleme için ekleyerek bir çok-çok ilişkisi temsil edebilir.

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
