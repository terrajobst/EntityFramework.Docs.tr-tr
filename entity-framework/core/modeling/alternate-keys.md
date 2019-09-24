---
title: Alternatif anahtarlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197465"
---
# <a name="alternate-keys"></a>Alternatif Anahtarlar

Alternatif bir anahtar, birincil anahtara ek olarak her bir varlık örneği için alternatif benzersiz bir tanımlayıcı işlevi görür. Alternatif anahtarlar bir ilişkinin hedefi olarak kullanılabilir. İlişkisel bir veritabanı kullanırken, bu, diğer anahtar sütunları üzerinde benzersiz bir dizin/kısıtlama kavramına eşlenir ve sütunlara (ler) başvuran bir veya daha fazla yabancı anahtar kısıtlamasıdır.

> [!TIP]  
> Yalnızca bir sütunun benzersizlik düzeyini zorlamak isterseniz, alternatif bir anahtar yerine benzersiz bir dizin isterseniz bkz. [dizinler](indexes.md). EF 'de, alternatif anahtarlar yabancı bir anahtarın hedefi olarak kullanılabildiğinden benzersiz dizinlerden daha fazla işlevsellik sağlar.

Diğer anahtarlar genellikle gerektiğinde sizin için sunulmuştur ve bunları el ile yapılandırmanız gerekmez. Daha fazla ayrıntı için bkz. [kurallar](#conventions) .

## <a name="conventions"></a>Kurallar

Kural gereği, bir ilişkinin hedefi olarak birincil anahtar olmayan bir özelliği tanımlarken sizin için alternatif bir anahtar sunulmuştur.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Veri Açıklamaları

Alternatif anahtarlar, veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Tek bir özelliği alternatif bir anahtar olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

Ayrıca, birden çok özelliği farklı bir anahtar olacak şekilde (bileşik alternatif anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
