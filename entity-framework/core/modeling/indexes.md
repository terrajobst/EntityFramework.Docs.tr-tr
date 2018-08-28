---
title: Dizinleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995486"
---
# <a name="indexes"></a>Dizinleri

Dizinleri birçok veri deposu arasında genel bir kavram olarak oldukça basittir. Veri deposundaki kendi uygulamalar farklılık gösterebilir ancak bunlar bir sütuna (veya sütun kümesini) dayalı aramalar daha fazlasını yapmak için kullanılır etkin.

## <a name="conventions"></a>Kurallar

Kural gereği, her özellik (veya özellikler kümesi), yabancı anahtar olarak kullanılan bir dizin oluşturulur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamalarını kullanma dizin oluşturulamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, tek bir özellikte bir dizin belirtmek için kullanabilirsiniz. Varsayılan olarak, benzersiz olmayan dizinleri.

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

Bir dizini benzersiz olması iki varlık aynı değerleri belirtilen özellik için olabilir yani belirtebilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Birden fazla sütun üzerinde bir dizin de belirtebilirsiniz.

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
> Farklı özellikler kümesi başına yalnızca bir dizin yok. Önceden tanımlanmış bir dizin olan özellikler kümesi kuralı ya da önceki yapılandırma ya da dizin yapılandırmak için Fluent API'si kullanıyorsanız bu dizinin tanımını değiştirerek. Bu kural tarafından oluşturulan bir dizin daha fazla yapılandırmak istiyorsanız kullanışlıdır.
