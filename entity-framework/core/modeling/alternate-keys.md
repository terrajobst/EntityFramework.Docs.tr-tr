---
title: Alternatif anahtarlar - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996977"
---
# <a name="alternate-keys"></a>Alternatif anahtarlar

Alternatif anahtar, birincil anahtar ek olarak her varlık örneği için bir alternatif benzersiz tanımlayıcısı olarak görev yapar. Alternatif anahtarlar bir ilişki hedefi olarak kullanılabilir. İlişkisel bir veritabanı kullanılırken bu kavramı benzersiz dizin/kısıtlama alternatif anahtar sütunların ve sütunların başvuran bir veya daha fazla yabancı anahtar kısıtlamaları, eşler.

> [!TIP]  
> Alternatif anahtar yerine benzersiz bir dizin istediğiniz sonra bir sütunun benzersizlik istiyorsanız bkz [dizinleri](indexes.md). Yabancı anahtar hedefi olarak kullanılabildiğinden, EF alternatif anahtarlar benzersiz dizinler daha büyük işlevsellik sağlar.

Alternatif anahtarlar genellikle sizin için gerektiğinde sunulan ve bunları el ile yapılandırmanız gerekmez. Bkz: [kuralları](#conventions) daha fazla ayrıntı için.

## <a name="conventions"></a>Kurallar

Birincil anahtar, bir ilişki hedefi olarak değil bir özelliği, tanımladığınızda, kural olarak, alternatif anahtar sizin için kullanıma sunulmuştur.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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

## <a name="data-annotations"></a>Veri ek açıklamaları

Alternatif anahtarlar veri ek açıklamalarını kullanma yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si alternatif anahtar tek bir özelliğini yapılandırmak için kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
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

(Alternatif bir bileşik anahtarı bilinir) alternatif anahtar olarak birden çok özelliklerini yapılandırmak için Fluent API'si de kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
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
