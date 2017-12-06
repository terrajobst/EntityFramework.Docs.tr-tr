---
title: "Dahil olmak üzere & türlerini - EF çekirdek dışarıda"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-types"></a>Dahil olmak üzere & türlerini dışarıda

Bir tür yazın ve okuma ve yazma örneklerine / veritabanı dener EF hakkında meta veri olan model anlamına gelir dahil olmak üzere.

## <a name="conventions"></a>Kurallar

Kural, sunulan türleri tarafından `DbSet` içeriğiniz özellikleri modelinizde dahil edilir. Ayrıca, bölümünde belirtildiği türleri `OnModelCreating` yöntemi dahil de. Son olarak, gezinti özellikleri bulunan türlerinin keşfetme yinelemeli tarafından bulunan herhangi bir türünün modelde de dahil edilir.

**Örneğin, aşağıdaki kod listesindeki tüm üç tür bulunur:**

* `Blog`içinde sunulan çünkü bir `DbSet` bağlam özelliği

* `Post`aracılığıyla bulunan çünkü `Blog.Posts` gezinti özelliği

* `AuditEntry`bölümünde belirtildiği için`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a>Veri ek açıklamaları

Modelden bir türü çıkarmak için veri ek açıklamaları kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API'si

Modelden bir türü çıkarmak için Fluent API'sini kullanın.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
