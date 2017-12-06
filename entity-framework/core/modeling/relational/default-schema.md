---
title: "Varsayılan şema - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="default-schema"></a>Varsayılan şema

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa nesneleri içinde oluşturulan veritabanı şemasının ' dir.

## <a name="conventions"></a>Kurallar

Kurala göre en uygun varsayılan şema veritabanı sağlayıcısı seçeceksiniz. Örneğin, Microsoft SQL Server kullanacak `dbo` (şemaları SQLite içinde desteklenmediği) şema ve SQLite bir şema kullanmaz.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamaları kullanılarak varsayılan şema ayarlayamazsınız.

## <a name="fluent-api"></a>Fluent API'si

Varsayılan şema belirtmek için Fluent API kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
