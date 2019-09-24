---
title: Türler hariç & içerme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197382"
---
# <a name="including--excluding-types"></a><span data-ttu-id="a80b5-102">Türleri Dahil Etme ve Dışlama</span><span class="sxs-lookup"><span data-stu-id="a80b5-102">Including & Excluding Types</span></span>

<span data-ttu-id="a80b5-103">Modele bir tür eklemek, EF 'in bu tür hakkında meta verilere sahip olduğu ve veritabanından örnekleri okumaya ve veritabanına yazmayı deneyeceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a80b5-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="a80b5-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="a80b5-104">Conventions</span></span>

<span data-ttu-id="a80b5-105">Kurala göre, bağlamınızın `DbSet` özelliklerinde kullanıma sunulan türler modelinize dahildir.</span><span class="sxs-lookup"><span data-stu-id="a80b5-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="a80b5-106">Ayrıca, `OnModelCreating` yönteminde bahsedilen türler de dahil edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a80b5-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="a80b5-107">Son olarak, bulunan türlerin gezinti özelliklerini yinelemeli olarak inceleyerek bulunan türler da modele dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a80b5-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="a80b5-108">**Örneğin, aşağıdaki kod listesinde, tüm üç tür bulunur:**</span><span class="sxs-lookup"><span data-stu-id="a80b5-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="a80b5-109">`Blog`bağlam üzerindeki bir `DbSet` özellikte kullanıma sunulduğundan</span><span class="sxs-lookup"><span data-stu-id="a80b5-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="a80b5-110">`Post``Blog.Posts` gezinti özelliği aracılığıyla bulunduğundan</span><span class="sxs-lookup"><span data-stu-id="a80b5-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="a80b5-111">`AuditEntry`içinde belirtildiği için`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="a80b5-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="a80b5-112">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a80b5-112">Data Annotations</span></span>

<span data-ttu-id="a80b5-113">Bir türü modelden dışlamak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a80b5-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="a80b5-114">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="a80b5-114">Fluent API</span></span>

<span data-ttu-id="a80b5-115">Bir tür modelden dışlamak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a80b5-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
