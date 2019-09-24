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
# <a name="indexes"></a><span data-ttu-id="481b5-102">Dizinlerde</span><span class="sxs-lookup"><span data-stu-id="481b5-102">Indexes</span></span>

<span data-ttu-id="481b5-103">Dizinler birçok veri deposu genelinde ortak bir kavramdır.</span><span class="sxs-lookup"><span data-stu-id="481b5-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="481b5-104">Veri deposundaki uygulamaları farklılık gösterebilir, ancak bir sütuna (veya sütun kümesine) göre aramalar yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="481b5-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="481b5-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="481b5-105">Conventions</span></span>

<span data-ttu-id="481b5-106">Kurala göre, yabancı anahtar olarak kullanılan her bir özellikte (veya özellik kümesinde) bir dizin oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="481b5-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="481b5-107">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="481b5-107">Data Annotations</span></span>

<span data-ttu-id="481b5-108">Dizinler, veri açıklamaları kullanılarak oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="481b5-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="481b5-109">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="481b5-109">Fluent API</span></span>

<span data-ttu-id="481b5-110">Tek bir özellikte dizin belirtmek için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="481b5-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="481b5-111">Dizinler, varsayılan olarak benzersiz değildir.</span><span class="sxs-lookup"><span data-stu-id="481b5-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="481b5-112">Ayrıca, bir dizinin benzersiz olması gerektiğini de belirtebilirsiniz. Bu, iki varlığın verilen Özellik (ler) için aynı değere sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="481b5-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="481b5-113">Ayrıca, birden fazla sütundan oluşan bir dizin belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="481b5-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="481b5-114">Ayrı özellik kümesi başına yalnızca bir dizin vardır.</span><span class="sxs-lookup"><span data-stu-id="481b5-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="481b5-115">Zaten bir dizini tanımlanmış, kural veya önceki yapılandırma ile tanımlanmış bir dizi özellik kümesi üzerinde bir dizin yapılandırmak için akıcı API kullanıyorsanız, bu dizinin tanımını değiştirmiş olursunuz.</span><span class="sxs-lookup"><span data-stu-id="481b5-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="481b5-116">Kural tarafından oluşturulan bir dizini daha fazla yapılandırmak istiyorsanız bu yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="481b5-116">This is useful if you want to further configure an index that was created by convention.</span></span>
