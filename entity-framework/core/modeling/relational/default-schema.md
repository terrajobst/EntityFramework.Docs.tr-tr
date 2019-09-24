---
title: Varsayılan şema-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197136"
---
# <a name="default-schema"></a>Varsayılan Şema

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa nesnelerin oluşturulacağı veritabanı şemadır.

## <a name="conventions"></a>Kurallar

Kural gereği, veritabanı sağlayıcısı en uygun varsayılan şemayı seçer. Örneğin, Microsoft SQL Server `dbo` şemayı kullanacaktır ve SQLite bir şema kullanmaz (çünkü şemalar SQLite ' de desteklenmez).

## <a name="data-annotations"></a>Veri Açıklamaları

Veri ek açıklamalarını kullanarak varsayılan şemayı ayarlayamazsınız.

## <a name="fluent-api"></a>Akıcı API

Varsayılan bir şema belirtmek için Floent API 'sini kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
