---
title: Sütun eşlemesi - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054270"
---
# <a name="column-mapping"></a><span data-ttu-id="b36fc-102">Sütun eşlemesi</span><span class="sxs-lookup"><span data-stu-id="b36fc-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="b36fc-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b36fc-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b36fc-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="b36fc-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b36fc-105">Sütun eşlemesi sütun veri öğesinden sorgulanan ve gerekir veritabanında kaydedilmiş tanımlar.</span><span class="sxs-lookup"><span data-stu-id="b36fc-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="b36fc-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="b36fc-106">Conventions</span></span>

<span data-ttu-id="b36fc-107">Kurala göre her bir özellik özelliği aynı ada sahip bir sütun eşlemek için Kur olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b36fc-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b36fc-108">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="b36fc-108">Data Annotations</span></span>

<span data-ttu-id="b36fc-109">Bir özellik eşlenmiş sütun yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36fc-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="b36fc-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="b36fc-110">Fluent API</span></span>

<span data-ttu-id="b36fc-111">Bir özellik eşlenmiş sütunu yapılandırma Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b36fc-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
