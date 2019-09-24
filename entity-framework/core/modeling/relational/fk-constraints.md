---
title: Yabancı anahtar kısıtlamaları-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197060"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="334e7-102">Yabancı Anahtar Kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="334e7-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="334e7-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="334e7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="334e7-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="334e7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="334e7-105">Modeldeki her ilişki için bir yabancı anahtar kısıtlaması tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="334e7-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="334e7-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="334e7-106">Conventions</span></span>

<span data-ttu-id="334e7-107">Kurala göre, yabancı anahtar kısıtlamaları adlandırılır `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="334e7-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="334e7-108">Bileşik yabancı anahtarlar `<foreign key property name>` için, yabancı anahtar özellik adlarının alt çizgiyle ayrılmış bir listesi olur.</span><span class="sxs-lookup"><span data-stu-id="334e7-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="334e7-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="334e7-109">Data Annotations</span></span>

<span data-ttu-id="334e7-110">Yabancı anahtar kısıtlama adları, veri açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="334e7-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="334e7-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="334e7-111">Fluent API</span></span>

<span data-ttu-id="334e7-112">Bir ilişkinin yabancı anahtar kısıtlama adını yapılandırmak için akıcı API 'YI kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="334e7-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
