---
title: Birincil anahtarlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196951"
---
# <a name="primary-keys"></a>Birincil Anahtarlar

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Her varlık türünün anahtarı için bir birincil anahtar kısıtlaması tanıtılmıştır.

## <a name="conventions"></a>Kurallar

Kurala göre, veritabanındaki birincil anahtar olarak adlandırılır `PK_<type name>`.

## <a name="data-annotations"></a>Veri Açıklamaları

Birincil anahtarın ilişkisel veritabanı özel yönleri, veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Akıcı API 'YI, veritabanında birincil anahtar kısıtlamasının adını yapılandırmak için kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
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
