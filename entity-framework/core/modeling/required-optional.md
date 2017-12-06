---
title: "Gerekli/isteğe bağlı özellikleri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a>Gerekli ve isteğe bağlı özellikler

Bir özellik içeren için geçerli olup olmadığını isteğe bağlı olarak kabul edilir `null`. Varsa `null` gerekli bir özellik olarak kabul edilir sonra bir özelliğe atanmak için geçerli bir değer değil.

## <a name="conventions"></a>Kurallar

Kurala göre CLR türü, null içerebilir bir özellik isteğe bağlı olarak yapılandırılacak (`string`, `int?`, `byte[]`vb..). Özellikler, CLR türü, null içeremez yapılandırılması gerektiği gibi (`int`, `decimal`, `bool`vb..).

> [!NOTE]  
> Bir özellik, CLR türü null içeremez, isteğe bağlı olarak yapılandırılamaz. Özellik her zaman gerekli Entity Framework tarafından kabul edilir.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özelliğin gerekli olduğunu belirtmek için veri ek açıklamaları kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API'si

Bir özelliğin gerekli olduğunu belirtmek için Fluent API'sini kullanın.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
