---
title: Devralma (ilişkisel veritabanı) - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152374"
---
# <a name="inheritance-relational-database"></a>Devralma (ilişkisel veritabanı)

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Devralma EF modeldeki varlık sınıflarını devralma veritabanında nasıl temsil denetlemek için kullanılır.

> [!NOTE]  
> Şu anda, yalnızca tablo başına hiyerarşisi (TPH) deseni EF çekirdek uygulanır. Diğer ortak desenler tablo başına-türü (birleştirilmiş TPT) gibi ve tablo başına-somut-türü (TPC) henüz kullanılabilir değildir.

## <a name="conventions"></a>Kurallar

Kurala göre tablo-başına-hiyerarşisi (TPH) desenini kullanarak devralma eşleşecektir. TPH tek bir tabloyu hiyerarşideki tüm türleri için verileri depolamak için kullanır. Ayrıştırıcı sütunun her satırın gösterdiği hangi türünü tanımlamak için kullanılır.

EF çekirdek yalnızca Kurulum devralma iki veya daha fazla devralınan türleri açıkça modelde varsa (bkz [devralma](../inheritance.md) daha fazla ayrıntı için).

Aşağıda bir basit devralma senaryo ve TPH desen kullanan bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnektir. *Ayrıştırıcıyı* sütun türünü tanımlayan *Blog* her satır depolanır.

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

## <a name="data-annotations"></a>Veri ek açıklamaları

Devralma yapılandırmak için veri ek açıklamaları kullanamazsınız.

## <a name="fluent-api"></a>Fluent API'si

Fluent API adını ve türünü ayrıştırıcı sütunun ve hiyerarşideki her türünü tanımlamak için kullanılan değerleri yapılandırmak için kullanabilirsiniz.

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

## <a name="configuring-the-discriminator-property"></a>Ayrıştırıcı özelliğini yapılandırma

Yukarıdaki örneklerde ayrıştırıcı olarak oluşturulan bir [gölge özelliği](xref:core/modeling/shadow-properties) hiyerarşinin temel varlık üzerinde. Modeldeki bir özellik olduğundan yalnızca gibi diğer özellikleri yapılandırılabilir. Örneğin, varsayılan olarak, kural tarafından Ayrıştırıcıyı kullanıldığında, maksimum uzunluk ayarlamak için şunu yazın:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Ayrıştırıcı varlığınızdaki gerçek bir CLR özellik de eşlenebilir. Örneğin:
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

Bu iki şey birlikte birleştirme hem gerçek özelliğine Ayrıştırıcıyı harita ve yapılandırma mümkündür:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
