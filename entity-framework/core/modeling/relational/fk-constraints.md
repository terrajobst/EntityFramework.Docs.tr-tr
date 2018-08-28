---
title: Yabancı anahtar kısıtlamaları - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993994"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="e04fd-102">Yabancı anahtar kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="e04fd-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="e04fd-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e04fd-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e04fd-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="e04fd-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e04fd-105">Bir yabancı anahtar kısıtlaması, modeldeki her ilişki için sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e04fd-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="e04fd-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="e04fd-106">Conventions</span></span>

<span data-ttu-id="e04fd-107">Kural gereği, yabancı anahtar kısıtlamalarını adlandırılır `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="e04fd-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="e04fd-108">Bileşik yabancı anahtarlar için `<foreign key property name>` yabancı anahtar özellik adlarının bir alt çizgi ayrılmış listesi olur.</span><span class="sxs-lookup"><span data-stu-id="e04fd-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e04fd-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="e04fd-109">Data Annotations</span></span>

<span data-ttu-id="e04fd-110">Veri ek açıklamalarını kullanma yabancı anahtar kısıtlaması adları yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="e04fd-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e04fd-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="e04fd-111">Fluent API</span></span>

<span data-ttu-id="e04fd-112">Fluent API'si, bir ilişki için yabancı anahtar kısıtlaması adını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e04fd-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

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
