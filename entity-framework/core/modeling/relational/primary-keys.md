---
title: "Birincil anahtarlar - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="primary-keys"></a>Birincil anahtarlar

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Bir birincil anahtar kısıtlaması her bir varlık türü anahtarı için sunulmuştur.

## <a name="conventions"></a>Kurallar

Kurala göre veritabanı birincil anahtarda adlandırılacağını `PK_<type name>`.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir birincil anahtar yok ilişkisel veritabanı belirli yönlerini veri ek açıklamaları kullanılarak yapılandırılabilir.

## <a name="fluent-api"></a>Fluent API'si

Fluent API veritabanında birincil anahtar kısıtlaması adını yapılandırmak için kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
