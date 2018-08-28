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
# <a name="indexes"></a><span data-ttu-id="7dbcf-102">Dizinleri</span><span class="sxs-lookup"><span data-stu-id="7dbcf-102">Indexes</span></span>

<span data-ttu-id="7dbcf-103">Dizinleri birçok veri deposu arasında genel bir kavram olarak oldukça basittir.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="7dbcf-104">Veri deposundaki kendi uygulamalar farklılık gösterebilir ancak bunlar bir sütuna (veya sütun kümesini) dayalı aramalar daha fazlasını yapmak için kullanılır etkin.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="7dbcf-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="7dbcf-105">Conventions</span></span>

<span data-ttu-id="7dbcf-106">Kural gereği, her özellik (veya özellikler kümesi), yabancı anahtar olarak kullanılan bir dizin oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7dbcf-107">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="7dbcf-107">Data Annotations</span></span>

<span data-ttu-id="7dbcf-108">Veri ek açıklamalarını kullanma dizin oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7dbcf-109">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="7dbcf-109">Fluent API</span></span>

<span data-ttu-id="7dbcf-110">Fluent API'si, tek bir özellikte bir dizin belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="7dbcf-111">Varsayılan olarak, benzersiz olmayan dizinleri.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="7dbcf-112">Bir dizini benzersiz olması iki varlık aynı değerleri belirtilen özellik için olabilir yani belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="7dbcf-113">Birden fazla sütun üzerinde bir dizin de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="7dbcf-114">Farklı özellikler kümesi başına yalnızca bir dizin yok.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="7dbcf-115">Önceden tanımlanmış bir dizin olan özellikler kümesi kuralı ya da önceki yapılandırma ya da dizin yapılandırmak için Fluent API'si kullanıyorsanız bu dizinin tanımını değiştirerek.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="7dbcf-116">Bu kural tarafından oluşturulan bir dizin daha fazla yapılandırmak istiyorsanız kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="7dbcf-116">This is useful if you want to further configure an index that was created by convention.</span></span>
