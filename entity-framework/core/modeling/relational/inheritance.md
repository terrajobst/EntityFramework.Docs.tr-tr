---
title: "Devralma (ilişkisel veritabanı) - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
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
