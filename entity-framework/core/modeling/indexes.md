---
title: "Dizinler - EF çekirdek"
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
---
# <a name="indexes"></a><span data-ttu-id="b1d2e-102">Dizinler</span><span class="sxs-lookup"><span data-stu-id="b1d2e-102">Indexes</span></span>

<span data-ttu-id="b1d2e-103">Dizinleri bir ortak birçok veri depoları arasında kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="b1d2e-104">Veri deposunda uygulamalarının farklılık gösterebilir, ancak daha fazla bir sütun (veya sütun kümesini) göre arama yapmak için kullanıldıkları verimli.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="b1d2e-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="b1d2e-105">Conventions</span></span>

<span data-ttu-id="b1d2e-106">Kurala göre her özellik (veya özellikler kümesi) yabancı anahtar olarak kullanılan bir dizin oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b1d2e-107">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="b1d2e-107">Data Annotations</span></span>

<span data-ttu-id="b1d2e-108">Dizinleri veri ek açıklamaları kullanılarak oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b1d2e-109">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="b1d2e-109">Fluent API</span></span>

<span data-ttu-id="b1d2e-110">Fluent API, tek bir özellikte dizin belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="b1d2e-111">Varsayılan olarak, benzersiz olmayan dizinler.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="b1d2e-112">Ayrıca bir dizini benzersiz olmalıdır iki varlık aynı değerleri belirtilen özellik için sağlayabilirsiniz anlamı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="b1d2e-113">Dizin birden fazla sütun üzerinde de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="b1d2e-114">Farklı özellikler kümesi başına yalnızca bir dizin yok.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="b1d2e-115">Bir dizin zaten tanımlanmış bir dizin olan özellikler kümesi ya da kuralı veya önceki yapılandırma yapılandırmak için Fluent API kullanırsanız, bu dizin tanımını değiştirme.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="b1d2e-116">Bu kural tarafından oluşturulan bir dizin daha fazla yapılandırmak istiyorsanız kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="b1d2e-116">This is useful if you want to further configure an index that was created by convention.</span></span>
