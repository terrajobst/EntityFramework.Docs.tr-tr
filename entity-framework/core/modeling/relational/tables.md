---
title: Tablo eşleme - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949015"
---
# <a name="table-mapping"></a><span data-ttu-id="0501a-102">Tablo eşleme</span><span class="sxs-lookup"><span data-stu-id="0501a-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="0501a-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0501a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0501a-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="0501a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0501a-105">Tablo eşleme hangi tablo verilerini gelen sorgulanabilen ve veritabanında kaydedilmiş tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0501a-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="0501a-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="0501a-106">Conventions</span></span>

<span data-ttu-id="0501a-107">Kural gereği, her bir varlık aynı ada sahip bir tablo eşlemek için ayarlanır `DbSet<TEntity>` sunan varlık üzerinde türetilmiş bağlam özelliği.</span><span class="sxs-lookup"><span data-stu-id="0501a-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="0501a-108">Hayır ise `DbSet<TEntity>` içerdiği için belirtilen varlık, sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0501a-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0501a-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="0501a-109">Data Annotations</span></span>

<span data-ttu-id="0501a-110">Bir türü eşleyen tablo yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0501a-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="0501a-111">Tabloya ait bir şema de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0501a-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="0501a-112">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="0501a-112">Fluent API</span></span>

<span data-ttu-id="0501a-113">Fluent API'si, bir türü eşleyen tablo yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0501a-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="0501a-114">Tabloya ait bir şema de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0501a-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
