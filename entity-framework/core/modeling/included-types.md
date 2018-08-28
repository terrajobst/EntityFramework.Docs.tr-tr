---
title: Dahil olan ve dışlanan türler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996156"
---
# <a name="including--excluding-types"></a>Dahil olan ve dışlanan türler

Bir tür yazın ve okuma ve yazma / için veritabanı örnekleri dener EF hakkında meta veriler içeren model anlamına gelir dahil olmak üzere.

## <a name="conventions"></a>Kurallar

Kural, sunulan türleri tarafından `DbSet` Bağlamınızı özellikleri modelinizde dahil edilir. Ayrıca, belirtilmiştir türleri `OnModelCreating` yöntemi da dahil edilir. Son olarak, yinelemeli olarak bulunan türleri Gezinti özelliklerini keşfetme tarafından bulunan tüm türleri de modele dahil edilir.

**Örneğin, aşağıdaki kod listesinde üç türü bulunur:**

* `Blog` olarak açığa çıkarıldığı bir `DbSet` bağlam özelliği

* `Post` aracılığıyla bulunan çünkü `Blog.Posts` gezinme özelliği

* `AuditEntry` içinde bahsettiğiniz için `OnModelCreating`

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

Modelden bir türü dışlanacak Fluent API'sini kullanabilirsiniz.

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
