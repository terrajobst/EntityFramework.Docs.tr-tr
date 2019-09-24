---
title: Yabancı anahtar kısıtlamaları-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197060"
---
# <a name="foreign-key-constraints"></a>Yabancı Anahtar Kısıtlamaları

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Modeldeki her ilişki için bir yabancı anahtar kısıtlaması tanıtılmıştır.

## <a name="conventions"></a>Kurallar

Kurala göre, yabancı anahtar kısıtlamaları adlandırılır `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Bileşik yabancı anahtarlar `<foreign key property name>` için, yabancı anahtar özellik adlarının alt çizgiyle ayrılmış bir listesi olur.

## <a name="data-annotations"></a>Veri Açıklamaları

Yabancı anahtar kısıtlama adları, veri açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Bir ilişkinin yabancı anahtar kısıtlama adını yapılandırmak için akıcı API 'YI kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
