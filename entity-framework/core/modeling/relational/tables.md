---
title: Tablo eşleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656100"
---
# <a name="table-mapping"></a><span data-ttu-id="9aa54-102">Tablo Eşleme</span><span class="sxs-lookup"><span data-stu-id="9aa54-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="9aa54-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9aa54-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9aa54-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="9aa54-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9aa54-105">Tablo eşleme, hangi tablo verilerinin sorgulandığını ve veritabanında veritabanına kaydedileceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="9aa54-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="9aa54-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="9aa54-106">Conventions</span></span>

<span data-ttu-id="9aa54-107">Kurala göre, her varlık türetilmiş bağlamda varlığı sunan `DbSet<TEntity>` özelliği ile aynı ada sahip bir tabloya eşlenecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9aa54-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="9aa54-108">Verilen varlığa `DbSet<TEntity>` dahil yoksa, sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9aa54-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9aa54-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="9aa54-109">Data Annotations</span></span>

<span data-ttu-id="9aa54-110">Bir türün eşlendiği tabloyu yapılandırmak için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aa54-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="9aa54-111">Ayrıca, tablonun ait olduğu bir şemayı de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aa54-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="9aa54-112">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="9aa54-112">Fluent API</span></span>

<span data-ttu-id="9aa54-113">Bir türün eşlendiği tabloyu yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aa54-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;

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

<span data-ttu-id="9aa54-114">Ayrıca, tablonun ait olduğu bir şemayı de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aa54-114">You can also specify a schema that the table belongs to.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
