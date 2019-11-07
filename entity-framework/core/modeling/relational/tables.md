---
title: Tablo eşleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656100"
---
# <a name="table-mapping"></a>Tablo Eşleme

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Tablo eşleme, hangi tablo verilerinin sorgulandığını ve veritabanında veritabanına kaydedileceğini belirler.

## <a name="conventions"></a>Kurallar

Kurala göre, her varlık türetilmiş bağlamda varlığı sunan `DbSet<TEntity>` özelliği ile aynı ada sahip bir tabloya eşlenecek şekilde ayarlanır. Verilen varlığa `DbSet<TEntity>` dahil yoksa, sınıf adı kullanılır.

## <a name="data-annotations"></a>Veri Açıklamaları

Bir türün eşlendiği tabloyu yapılandırmak için, veri açıklamalarını kullanabilirsiniz.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Ayrıca, tablonun ait olduğu bir şemayı de belirtebilirsiniz.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Akıcı API

Bir türün eşlendiği tabloyu yapılandırmak için Floent API 'sini kullanabilirsiniz.

``` csharp
using Microsoft.EntityFrameworkCore;

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

Ayrıca, tablonun ait olduğu bir şemayı de belirtebilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
