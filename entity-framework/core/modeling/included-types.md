---
title: Türler hariç & içerme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197382"
---
# <a name="including--excluding-types"></a>Türleri Dahil Etme ve Dışlama

Modele bir tür eklemek, EF 'in bu tür hakkında meta verilere sahip olduğu ve veritabanından örnekleri okumaya ve veritabanına yazmayı deneyeceği anlamına gelir.

## <a name="conventions"></a>Kurallar

Kurala göre, bağlamınızın `DbSet` özelliklerinde kullanıma sunulan türler modelinize dahildir. Ayrıca, `OnModelCreating` yönteminde bahsedilen türler de dahil edilmiştir. Son olarak, bulunan türlerin gezinti özelliklerini yinelemeli olarak inceleyerek bulunan türler da modele dahil edilir.

**Örneğin, aşağıdaki kod listesinde, tüm üç tür bulunur:**

* `Blog`bağlam üzerindeki bir `DbSet` özellikte kullanıma sunulduğundan

* `Post``Blog.Posts` gezinti özelliği aracılığıyla bulunduğundan

* `AuditEntry`içinde belirtildiği için`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a>Veri Açıklamaları

Bir türü modelden dışlamak için veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Akıcı API

Bir tür modelden dışlamak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
