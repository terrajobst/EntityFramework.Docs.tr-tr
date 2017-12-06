---
title: "Alternatif anahtarları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys"></a>Alternatif anahtarları

Diğer anahtar birincil anahtarı yanı sıra her varlık örneği için bir alternatif benzersiz tanımlayıcı olarak görev yapar. Alternatif anahtarları bir ilişki hedef olarak kullanılabilir. İlişkisel veritabanı kullanılırken bu kavramı benzersiz bir dizin/kısıtlama alternatif anahtar sütunları ve sütunlara başvuran bir veya daha fazla yabancı anahtar kısıtlamaları, eşler.

> [!TIP]  
> Alternatif bir anahtarı yerine benzersiz bir dizin istediğiniz sonra bir sütunun benzersizlik istiyorsanız bkz [dizinleri](indexes.md). Çünkü bir yabancı anahtar hedef olarak kullanılabilir EF diğer anahtarları benzersiz dizinler daha büyük işlevsellik sağlar.

Alternatif anahtarlar genellikle sizin için gerekli olduğunda sunulur ve bunları el ile yapılandırmanız gerekmez. Bkz: [kuralları](#conventions) daha fazla ayrıntı için.

## <a name="conventions"></a>Kurallar

Birincil anahtar, bir ilişki hedefi değil bir özellik, tanımladığınızda, kurala göre sizin için bir alternatif anahtarı tanıtılmaktadır.

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

Alternatif anahtarları veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API, alternatif bir anahtarı olması için tek bir özellikte yapılandırmak için kullanabilirsiniz.

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

(Alternatif bir bileşik anahtarı da bilinir) bir alternatif anahtarı için birden çok özelliklerini yapılandırmak için Fluent API de kullanabilirsiniz.

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
