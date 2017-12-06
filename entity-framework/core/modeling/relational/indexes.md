---
title: "Dizinler - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>Dizinler

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

İlişkisel bir veritabanındaki bir dizin aynı kavram çekirdek Entity Framework'ün bir dizin olarak eşleştirir.

## <a name="conventions"></a>Kurallar

Kurala göre dizinleri adlı `IX_<type name>_<property name>`. Bileşik dizinler için `<property name>` özellik adları alt çizgi ayrılmış listesini olur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Dizinleri veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Bir dizinin adını yapılandırmak için Fluent API kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
