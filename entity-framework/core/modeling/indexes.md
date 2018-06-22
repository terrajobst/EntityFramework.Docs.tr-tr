---
title: Dizinler - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054078"
---
# <a name="indexes"></a>Dizinler

Dizinleri bir ortak birçok veri depoları arasında kavramıdır. Veri deposunda uygulamalarının farklılık gösterebilir, ancak daha fazla bir sütun (veya sütun kümesini) göre arama yapmak için kullanıldıkları verimli.

## <a name="conventions"></a>Kurallar

Kurala göre her özellik (veya özellikler kümesi) yabancı anahtar olarak kullanılan bir dizin oluşturulur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Dizinleri veri ek açıklamaları kullanılarak oluşturulamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API, tek bir özellikte dizin belirtmek için kullanabilirsiniz. Varsayılan olarak, benzersiz olmayan dizinler.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Ayrıca bir dizini benzersiz olmalıdır iki varlık aynı değerleri belirtilen özellik için sağlayabilirsiniz anlamı belirtebilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Dizin birden fazla sütun üzerinde de belirtebilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> Farklı özellikler kümesi başına yalnızca bir dizin yok. Bir dizin zaten tanımlanmış bir dizin olan özellikler kümesi ya da kuralı veya önceki yapılandırma yapılandırmak için Fluent API kullanırsanız, bu dizin tanımını değiştirme. Bu kural tarafından oluşturulan bir dizin daha fazla yapılandırmak istiyorsanız kullanışlıdır.
