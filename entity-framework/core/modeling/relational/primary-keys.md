---
title: Birincil anahtarlar - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998074"
---
# <a name="primary-keys"></a>Birincil anahtarlar

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Bir birincil anahtar kısıtlaması her varlık türü için anahtar sunulmuştur.

## <a name="conventions"></a>Kurallar

Kural gereği, veritabanının birincil anahtarı yeniden adlandırılacak `PK_<type name>`.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir birincil anahtar yok ilişkisel veritabanı belirli yönlerini veri ek açıklamalarını kullanma yapılandırılabilir.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, birincil anahtar kısıtlaması adı veritabanında yapılandırmak için kullanabilirsiniz.

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
