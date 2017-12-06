---
title: "Tablo eşlemesi - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="table-mapping"></a><span data-ttu-id="3d1ce-102">Tablo eşlemesi</span><span class="sxs-lookup"><span data-stu-id="3d1ce-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="3d1ce-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3d1ce-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="3d1ce-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3d1ce-105">Tablo eşlemesi hangi tablo verisi gelen sorgulanan ve gerekir veritabanında kaydedilmiş tanımlar.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="3d1ce-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="3d1ce-106">Conventions</span></span>

<span data-ttu-id="3d1ce-107">Kurala göre her bir varlık aynı ada sahip bir tablo eşlemek için Kur olacaktır `DbSet<TEntity>` türetilmiş bağlam varlıkta gösteren özelliği.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="3d1ce-108">Öyle değilse `DbSet<TEntity>` bulunan sınıf adı verilen varlık için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3d1ce-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="3d1ce-109">Data Annotations</span></span>

<span data-ttu-id="3d1ce-110">Bir tür eşlendiği tablo yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="3d1ce-111">Ait olduğu tabloyu bir şema de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="3d1ce-112">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="3d1ce-112">Fluent API</span></span>

<span data-ttu-id="3d1ce-113">Fluent API türü eşlendiği tablo yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="3d1ce-114">Ait olduğu tabloyu bir şema de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d1ce-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
