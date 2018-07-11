---
title: Tablo eşleme - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949015"
---
# <a name="table-mapping"></a>Tablo eşleme

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Tablo eşleme hangi tablo verilerini gelen sorgulanabilen ve veritabanında kaydedilmiş tanımlar.

## <a name="conventions"></a>Kurallar

Kural gereği, her bir varlık aynı ada sahip bir tablo eşlemek için ayarlanır `DbSet<TEntity>` sunan varlık üzerinde türetilmiş bağlam özelliği. Hayır ise `DbSet<TEntity>` içerdiği için belirtilen varlık, sınıf adı kullanılır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir türü eşleyen tablo yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Tabloya ait bir şema de belirtebilirsiniz.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir türü eşleyen tablo yapılandırmak için kullanabilirsiniz.

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Tabloya ait bir şema de belirtebilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
