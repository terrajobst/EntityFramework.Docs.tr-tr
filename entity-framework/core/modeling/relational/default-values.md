---
title: "Varsayılan değerleri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="default-values"></a><span data-ttu-id="702fb-102">Varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="702fb-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="702fb-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="702fb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="702fb-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="702fb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="702fb-105">Yeni bir satır eklenir, ancak hiçbir değer belirtilen sütun için eklenecek değer bir sütunun varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="702fb-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="702fb-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="702fb-106">Conventions</span></span>

<span data-ttu-id="702fb-107">Kurala göre varsayılan bir değer yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="702fb-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="702fb-108">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="702fb-108">Data Annotations</span></span>

<span data-ttu-id="702fb-109">Veri ek açıklamaları kullanılarak varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="702fb-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="702fb-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="702fb-110">Fluent API</span></span>

<span data-ttu-id="702fb-111">Bir özellik için varsayılan değer belirtmek için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="702fb-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="702fb-112">Varsayılan değer hesaplamak için kullanılan bir SQL parça de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="702fb-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
