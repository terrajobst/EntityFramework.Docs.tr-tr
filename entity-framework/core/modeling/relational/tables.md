---
title: Tablo eşlemesi - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054183"
---
# <a name="table-mapping"></a>Tablo eşlemesi

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Tablo eşlemesi hangi tablo verisi gelen sorgulanan ve gerekir veritabanında kaydedilmiş tanımlar.

## <a name="conventions"></a>Kurallar

Kurala göre her bir varlık aynı ada sahip bir tablo eşlemek için Kur olacaktır `DbSet<TEntity>` türetilmiş bağlam varlıkta gösteren özelliği. Öyle değilse `DbSet<TEntity>` bulunan sınıf adı verilen varlık için kullanılır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir tür eşlendiği tablo yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

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

Ait olduğu tabloyu bir şema de belirtebilirsiniz.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API'si

Fluent API türü eşlendiği tablo yapılandırmak için kullanabilirsiniz.

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

Ait olduğu tabloyu bir şema de belirtebilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
