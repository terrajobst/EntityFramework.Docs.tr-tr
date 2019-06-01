---
title: Devralma (ilişkisel veritabanı) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2d0a2abc554f5f115479f886ca3f9f4f01b80b5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452286"
---
# <a name="inheritance-relational-database"></a>Devralma (ilişkisel veritabanı)

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Devralma EF modeli, varlık sınıflarının Devralmada veritabanında nasıl temsil edildiğini denetlemek için kullanılır.

> [!NOTE]  
> Şu anda yalnızca tablo başına hiyerarşi (TPH) deseni EF Core uygulanır. Tablo başına tür (TPT) diğer ortak desenler ister ve tablo başına somut-tür (TPC) henüz kullanılamıyor.

## <a name="conventions"></a>Kurallar

Kural gereği, devralma tablo-başına-hiyerarşi (TPH) deseni kullanılarak eşleştirilir. TPH tek bir tabloyu hiyerarşideki tüm türleri için verileri depolamak için kullanır. Bir Ayrıştırıcı sütunu, her satırı temsil eden hangi tür tanımlamak için kullanılır.

EF Core yalnızca Kurulum devralma iki veya daha fazla devralınan türlerin açıkça modele dahil edilirse (bkz [devralma](../inheritance.md) daha fazla ayrıntı için).

Basit devralma senaryo ve TPH desenini kullanarak, bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnek aşağıda verilmiştir. *Ayrıştırıcı* sütun türünü tanımlayan *Blog* her satırda depolanır.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![görüntü](_static/inheritance-tph-data.png)

>[!NOTE]
> Veritabanı sütunları otomatik olarak TPH eşleme kullanırken gerektiği gibi boş değer atanabilir yapılır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamaları, devralmayı yapılandırmak için kullanamazsınız.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir ayrıştırıcı sütunu ve hiyerarşideki her bir tür tanımlamak için kullanılan değerleri türünü ve adını yapılandırmak için kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a>Ayrıştırıcı özelliği yapılandırma

Yukarıdaki örneklerde, ayrıştırıcı olarak oluşturulan bir [gölge özelliği](xref:core/modeling/shadow-properties) üzerinde temel bir varlık hiyerarşisi. Modeldeki bir özellik olduğundan, diğer özellikleri gibi yapılandırılabilir. Örneğin, varsayılan kuralı tarafından ayrıştırıcı kullanılırken, en fazla uzunluk ayarlamak için şunu yazın:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Ayrıştırıcı da gerçek bir CLR özellik varlığınızdaki eşlenebilir. Örneğin:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Peki bu ikisi birlikte birleştirerek hem ayrıştırıcı gerçek özellik eşleme ve yapılandırma mümkündür:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
