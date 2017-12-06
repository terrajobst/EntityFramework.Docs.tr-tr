---
title: "Yabancı anahtar kısıtlamaları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
ms.technology: entity-framework-core
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 726f03e2ee4cd3ec851c9a861b75dd12f9203e9c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="foreign-key-constraints"></a>Yabancı anahtar kısıtlamaları

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Bir yabancı anahtar kısıtlaması modeldeki her ilişki için sunulmuştur.

## <a name="conventions"></a>Kurallar

Kurala göre yabancı anahtar kısıtlamaları adlı `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Birleşik yabancı anahtarlar için `<foreign key property name>` yabancı anahtar özellik adlarının bir alt çizgi ayrılmış listesini olur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Yabancı anahtar kısıtlaması adları veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Bir ilişki için yabancı anahtar kısıtlaması adını yapılandırmak için Fluent API'sini kullanın.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
