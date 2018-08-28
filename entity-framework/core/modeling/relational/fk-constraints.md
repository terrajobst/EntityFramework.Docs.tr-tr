---
title: Yabancı anahtar kısıtlamaları - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993994"
---
# <a name="foreign-key-constraints"></a>Yabancı anahtar kısıtlamaları

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Bir yabancı anahtar kısıtlaması, modeldeki her ilişki için sunulmuştur.

## <a name="conventions"></a>Kurallar

Kural gereği, yabancı anahtar kısıtlamalarını adlandırılır `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Bileşik yabancı anahtarlar için `<foreign key property name>` yabancı anahtar özellik adlarının bir alt çizgi ayrılmış listesi olur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamalarını kullanma yabancı anahtar kısıtlaması adları yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir ilişki için yabancı anahtar kısıtlaması adını yapılandırmak için kullanabilirsiniz.

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
