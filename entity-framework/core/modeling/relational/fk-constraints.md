---
title: Yabancı anahtar kısıtlamaları - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
ms.technology: entity-framework-core
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 726f03e2ee4cd3ec851c9a861b75dd12f9203e9c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054186"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="558ae-102">Yabancı anahtar kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="558ae-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="558ae-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="558ae-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="558ae-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="558ae-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="558ae-105">Bir yabancı anahtar kısıtlaması modeldeki her ilişki için sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="558ae-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="558ae-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="558ae-106">Conventions</span></span>

<span data-ttu-id="558ae-107">Kurala göre yabancı anahtar kısıtlamaları adlı `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="558ae-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="558ae-108">Birleşik yabancı anahtarlar için `<foreign key property name>` yabancı anahtar özellik adlarının bir alt çizgi ayrılmış listesini olur.</span><span class="sxs-lookup"><span data-stu-id="558ae-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="558ae-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="558ae-109">Data Annotations</span></span>

<span data-ttu-id="558ae-110">Yabancı anahtar kısıtlaması adları veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="558ae-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="558ae-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="558ae-111">Fluent API</span></span>

<span data-ttu-id="558ae-112">Bir ilişki için yabancı anahtar kısıtlaması adını yapılandırmak için Fluent API'sini kullanın.</span><span class="sxs-lookup"><span data-stu-id="558ae-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
