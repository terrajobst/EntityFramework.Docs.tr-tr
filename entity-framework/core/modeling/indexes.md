---
title: Dizinler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197250"
---
# <a name="indexes"></a>Dizinlerde

Dizinler birçok veri deposu genelinde ortak bir kavramdır. Veri deposundaki uygulamaları farklılık gösterebilir, ancak bir sütuna (veya sütun kümesine) göre aramalar yapmak için kullanılır.

## <a name="conventions"></a>Kurallar

Kurala göre, yabancı anahtar olarak kullanılan her bir özellikte (veya özellik kümesinde) bir dizin oluşturulur.

## <a name="data-annotations"></a>Veri Açıklamaları

Dizinler, veri açıklamaları kullanılarak oluşturulamaz.

## <a name="fluent-api"></a>Akıcı API

Tek bir özellikte dizin belirtmek için Floent API 'sini kullanabilirsiniz. Dizinler, varsayılan olarak benzersiz değildir.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

Ayrıca, bir dizinin benzersiz olması gerektiğini de belirtebilirsiniz. Bu, iki varlığın verilen Özellik (ler) için aynı değere sahip olamaz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Ayrıca, birden fazla sütundan oluşan bir dizin belirtebilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> Ayrı özellik kümesi başına yalnızca bir dizin vardır. Zaten bir dizini tanımlanmış, kural veya önceki yapılandırma ile tanımlanmış bir dizi özellik kümesi üzerinde bir dizin yapılandırmak için akıcı API kullanıyorsanız, bu dizinin tanımını değiştirmiş olursunuz. Kural tarafından oluşturulan bir dizini daha fazla yapılandırmak istiyorsanız bu yararlı olur.
