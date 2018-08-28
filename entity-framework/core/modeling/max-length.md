---
title: En fazla uzunluk - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996197"
---
# <a name="maximum-length"></a>En fazla uzunluk

En fazla yapılandırma, belirli bir özellik için kullanılmak üzere uygun veri türü hakkında veri deposunda bir ipucu sağlar. En fazla uzunluk yalnızca geçerli dizi veri türleri için gibi `string` ve `byte[]`.

> [!NOTE]  
> Varlık çerçevesi sağlayıcısına verileri geçirmeden önce tüm doğrulama en fazla uzunluğu yapmaz. Buna uygun doğrulamak için sağlayıcısı veya veri deposuna kadar var. Örneğin, en büyük uzunluğu aşan SQL Server'ı hedefleyen bir özel durum temel alınan sütunun veri türü olarak neden olması durumunda depolanacak fazlalık veri izin vermez.

## <a name="conventions"></a>Kurallar

Kural gereği, bunu bir uygun veri türü özellikleri seçmek için veritabanı sağlayıcısı kadar bırakılır. Bir uzunluğa sahip özellikler için veritabanı sağlayıcısı uzun veri uzunluğu için izin veren bir veri türü genellikle seçersiniz. Örneğin, Microsoft SQL Server kullanacak `nvarchar(max)` için `string` özelliklerini (veya `nvarchar(450)` sütunu bir anahtar olarak kullanılıyorsa).

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özellik için en fazla uzunluk yapılandırmak için veri ek açıklamaları kullanabilirsiniz. Bu örnekte, SQL Server'ın bu içinde sonuçlanır hedefleyen `nvarchar(500)` veri türü.

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

Fluent API'si, bir özellik için en fazla uzunluk yapılandırmak için kullanabilirsiniz. Bu örnekte, SQL Server'ın bu içinde sonuçlanır hedefleyen `nvarchar(500)` veri türü.

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
