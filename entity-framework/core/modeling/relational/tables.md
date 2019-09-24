---
title: Tablo eşleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196962"
---
# <a name="table-mapping"></a><span data-ttu-id="2d98c-102">Tablo Eşleme</span><span class="sxs-lookup"><span data-stu-id="2d98c-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="2d98c-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2d98c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2d98c-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="2d98c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2d98c-105">Tablo eşleme, hangi tablo verilerinin sorgulandığını ve veritabanında veritabanına kaydedileceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="2d98c-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="2d98c-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="2d98c-106">Conventions</span></span>

<span data-ttu-id="2d98c-107">Kural gereği, her varlık türetilmiş bağlamda varlığı sunan `DbSet<TEntity>` özelliği ile aynı ada sahip bir tabloya eşlenecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2d98c-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="2d98c-108">Belirtilen varlığa `DbSet<TEntity>` hiçbir değer dahil değilse, sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2d98c-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2d98c-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="2d98c-109">Data Annotations</span></span>

<span data-ttu-id="2d98c-110">Bir türün eşlendiği tabloyu yapılandırmak için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d98c-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="2d98c-111">Ayrıca, tablonun ait olduğu bir şemayı de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d98c-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="2d98c-112">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="2d98c-112">Fluent API</span></span>

<span data-ttu-id="2d98c-113">Bir türün eşlendiği tabloyu yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d98c-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="2d98c-114">Ayrıca, tablonun ait olduğu bir şemayı de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d98c-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
