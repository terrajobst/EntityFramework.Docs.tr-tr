---
title: En fazla uzunluk - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054165"
---
# <a name="maximum-length"></a>En fazla uzunluğu

En fazla yapılandırma veri deposu belirli bir özellik için kullanılacak uygun veri türü hakkında bir ipucu sağlar. En fazla uzunluk yalnızca geçerlidir dizi veri türleri için gibi `string` ve `byte[]`.

> [!NOTE]  
> Entity Framework veri sağlayıcısına geçirmeden önce tüm doğrulama en fazla uzunluğu yapmaz. Uygunsa doğrulamak için sağlayıcısı veya veri deposunu kadar olan. Örneğin, SQL Server, en fazla uzunluğu aşan hedefleyen bir özel durum temel sütununun veri türü olarak neden olması durumunda depolanan veri fazlasını izin vermez.

## <a name="conventions"></a>Kurallar

Kurala göre uygun bir veri türü özellikleri seçmek için veritabanı sağlayıcısı kadar bırakılır. Bir uzunluğa sahip özellikler için veritabanı sağlayıcısı genellikle uzun veri uzunluğu için izin veren bir veri türü seçeceksiniz. Örneğin, Microsoft SQL Server kullanacak `nvarchar(max)` için `string` özellikleri (veya `nvarchar(450)` sütunun bir anahtar olarak kullanılıyorsa).

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özellik için en fazla uzunluk yapılandırmak için veri ek açıklamaları kullanabilirsiniz. Bu örnekte, SQL Server'ın bu sonuçlanacak hedefleme `nvarchar(500)` veri türü.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API'si

Bir özellik için en fazla uzunluk yapılandırmak için Fluent API kullanabilirsiniz. Bu örnekte, SQL Server'ın bu sonuçlanacak hedefleme `nvarchar(500)` veri türü.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
