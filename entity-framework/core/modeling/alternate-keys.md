---
title: Alternatif anahtarlar - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996977"
---
# <a name="alternate-keys"></a><span data-ttu-id="df9f6-102">Alternatif anahtarlar</span><span class="sxs-lookup"><span data-stu-id="df9f6-102">Alternate Keys</span></span>

<span data-ttu-id="df9f6-103">Alternatif anahtar, birincil anahtar ek olarak her varlık örneği için bir alternatif benzersiz tanımlayıcısı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="df9f6-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="df9f6-104">Alternatif anahtarlar bir ilişki hedefi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df9f6-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="df9f6-105">İlişkisel bir veritabanı kullanılırken bu kavramı benzersiz dizin/kısıtlama alternatif anahtar sütunların ve sütunların başvuran bir veya daha fazla yabancı anahtar kısıtlamaları, eşler.</span><span class="sxs-lookup"><span data-stu-id="df9f6-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="df9f6-106">Alternatif anahtar yerine benzersiz bir dizin istediğiniz sonra bir sütunun benzersizlik istiyorsanız bkz [dizinleri](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="df9f6-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="df9f6-107">Yabancı anahtar hedefi olarak kullanılabildiğinden, EF alternatif anahtarlar benzersiz dizinler daha büyük işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="df9f6-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="df9f6-108">Alternatif anahtarlar genellikle sizin için gerektiğinde sunulan ve bunları el ile yapılandırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="df9f6-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="df9f6-109">Bkz: [kuralları](#conventions) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="df9f6-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="df9f6-110">Kurallar</span><span class="sxs-lookup"><span data-stu-id="df9f6-110">Conventions</span></span>

<span data-ttu-id="df9f6-111">Birincil anahtar, bir ilişki hedefi olarak değil bir özelliği, tanımladığınızda, kural olarak, alternatif anahtar sizin için kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="df9f6-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="df9f6-112">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="df9f6-112">Data Annotations</span></span>

<span data-ttu-id="df9f6-113">Alternatif anahtarlar veri ek açıklamalarını kullanma yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="df9f6-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="df9f6-114">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="df9f6-114">Fluent API</span></span>

<span data-ttu-id="df9f6-115">Fluent API'si alternatif anahtar tek bir özelliğini yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df9f6-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="df9f6-116">(Alternatif bir bileşik anahtarı bilinir) alternatif anahtar olarak birden çok özelliklerini yapılandırmak için Fluent API'si de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df9f6-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
