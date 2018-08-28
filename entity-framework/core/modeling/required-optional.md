---
title: Gerekli/isteğe bağlı özellikleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995503"
---
# <a name="required-and-optional-properties"></a>Gerekli ve isteğe bağlı özellikler

Bir özellik içeren için geçerli ise isteğe bağlı olarak sayılır `null`. Varsa `null` gerekli özelliği olduğu düşünülür sonra bir özelliğe atanacak geçerli bir değer değil.

## <a name="conventions"></a>Kurallar

Kural gereği, CLR türü, null içerebilir bir özellik isteğe bağlı olarak yapılandırılır (`string`, `int?`, `byte[]`vb..). CLR türü, null içeremez özellikleri yapılandırılması gerektiği gibi (`int`, `decimal`, `bool`vb..).

> [!NOTE]  
> Bir özelliğin CLR türü null içeremez, isteğe bağlı olarak yapılandırılamaz. Entity Framework tarafından gerekli özellik her zaman değerlendirilir.

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

Fluent API'si, bir özelliğin gerekli olduğunu belirtmek için kullanabilirsiniz.

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
