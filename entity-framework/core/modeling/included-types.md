---
title: Dahil olan ve dışlanan türler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996156"
---
# <a name="including--excluding-types"></a><span data-ttu-id="7b511-102">Dahil olan ve dışlanan türler</span><span class="sxs-lookup"><span data-stu-id="7b511-102">Including & Excluding Types</span></span>

<span data-ttu-id="7b511-103">Bir tür yazın ve okuma ve yazma / için veritabanı örnekleri dener EF hakkında meta veriler içeren model anlamına gelir dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="7b511-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7b511-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="7b511-104">Conventions</span></span>

<span data-ttu-id="7b511-105">Kural, sunulan türleri tarafından `DbSet` Bağlamınızı özellikleri modelinizde dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="7b511-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="7b511-106">Ayrıca, belirtilmiştir türleri `OnModelCreating` yöntemi da dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="7b511-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="7b511-107">Son olarak, yinelemeli olarak bulunan türleri Gezinti özelliklerini keşfetme tarafından bulunan tüm türleri de modele dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="7b511-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="7b511-108">**Örneğin, aşağıdaki kod listesinde üç türü bulunur:**</span><span class="sxs-lookup"><span data-stu-id="7b511-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="7b511-109">`Blog` olarak açığa çıkarıldığı bir `DbSet` bağlam özelliği</span><span class="sxs-lookup"><span data-stu-id="7b511-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="7b511-110">`Post` aracılığıyla bulunan çünkü `Blog.Posts` gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="7b511-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="7b511-111">`AuditEntry` içinde bahsettiğiniz için `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="7b511-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="7b511-112">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="7b511-112">Data Annotations</span></span>

<span data-ttu-id="7b511-113">Modelden bir türü çıkarmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b511-113">You can use Data Annotations to exclude a type from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="7b511-114">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="7b511-114">Fluent API</span></span>

<span data-ttu-id="7b511-115">Modelden bir türü dışlanacak Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b511-115">You can use the Fluent API to exclude a type from the model.</span></span>

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
