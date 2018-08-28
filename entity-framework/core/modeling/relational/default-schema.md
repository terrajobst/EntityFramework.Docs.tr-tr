---
title: Varsayılan şema - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995372"
---
# <a name="default-schema"></a>Varsayılan şema

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa, nesneleri oluşturulacak veritabanı şeması ' dir.

## <a name="conventions"></a>Kurallar

Kural gereği, veritabanı sağlayıcısı en uygun varsayılan şema seçersiniz. Örneğin, Microsoft SQL Server kullanacak `dbo` (şemaları SQLite desteklenmediği) şema ve SQLite bir şema kullanmaz.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamalarını kullanma varsayılan şema ayarlayamazsınız.

## <a name="fluent-api"></a>Fluent API'si

Varsayılan şema belirtmek için Fluent API'sini kullanabilirsiniz.

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
