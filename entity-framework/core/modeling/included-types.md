---
title: Dahil olmak üzere & türlerini - EF çekirdek dışarıda
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054129"
---
# <a name="including--excluding-types"></a><span data-ttu-id="018ae-102">Dahil olmak üzere & türlerini dışarıda</span><span class="sxs-lookup"><span data-stu-id="018ae-102">Including & Excluding Types</span></span>

<span data-ttu-id="018ae-103">Bir tür yazın ve okuma ve yazma örneklerine / veritabanı dener EF hakkında meta veri olan model anlamına gelir dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="018ae-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="018ae-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="018ae-104">Conventions</span></span>

<span data-ttu-id="018ae-105">Kural, sunulan türleri tarafından `DbSet` içeriğiniz özellikleri modelinizde dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="018ae-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="018ae-106">Ayrıca, bölümünde belirtildiği türleri `OnModelCreating` yöntemi dahil de.</span><span class="sxs-lookup"><span data-stu-id="018ae-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="018ae-107">Son olarak, gezinti özellikleri bulunan türlerinin keşfetme yinelemeli tarafından bulunan herhangi bir türünün modelde de dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="018ae-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="018ae-108">**Örneğin, aşağıdaki kod listesindeki tüm üç tür bulunur:**</span><span class="sxs-lookup"><span data-stu-id="018ae-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="018ae-109">`Blog`içinde sunulan çünkü bir `DbSet` bağlam özelliği</span><span class="sxs-lookup"><span data-stu-id="018ae-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="018ae-110">`Post`aracılığıyla bulunan çünkü `Blog.Posts` gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="018ae-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="018ae-111">`AuditEntry`bölümünde belirtildiği için`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="018ae-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="018ae-112">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="018ae-112">Data Annotations</span></span>

<span data-ttu-id="018ae-113">Modelden bir türü çıkarmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="018ae-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="018ae-114">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="018ae-114">Fluent API</span></span>

<span data-ttu-id="018ae-115">Modelden bir türü çıkarmak için Fluent API'sini kullanın.</span><span class="sxs-lookup"><span data-stu-id="018ae-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
