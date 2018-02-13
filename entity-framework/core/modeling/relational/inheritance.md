---
title: "Devralma (ilişkisel veritabanı) - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="8dcfa-102">Devralma (ilişkisel veritabanı)</span><span class="sxs-lookup"><span data-stu-id="8dcfa-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="8dcfa-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8dcfa-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="8dcfa-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8dcfa-105">Devralma EF modeldeki varlık sınıflarını devralma veritabanında nasıl temsil denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="8dcfa-106">Şu anda, yalnızca tablo başına hiyerarşisi (TPH) deseni EF çekirdek uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="8dcfa-107">Diğer ortak desenler tablo başına-türü (birleştirilmiş TPT) gibi ve tablo başına-somut-türü (TPC) henüz kullanılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="8dcfa-108">Kurallar</span><span class="sxs-lookup"><span data-stu-id="8dcfa-108">Conventions</span></span>

<span data-ttu-id="8dcfa-109">Kurala göre tablo-başına-hiyerarşisi (TPH) desenini kullanarak devralma eşleşecektir.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="8dcfa-110">TPH tek bir tabloyu hiyerarşideki tüm türleri için verileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="8dcfa-111">Ayrıştırıcı sütunun her satırın gösterdiği hangi türünü tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="8dcfa-112">EF çekirdek yalnızca Kurulum devralma iki veya daha fazla devralınan türleri açıkça modelde varsa (bkz [devralma](../inheritance.md) daha fazla ayrıntı için).</span><span class="sxs-lookup"><span data-stu-id="8dcfa-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="8dcfa-113">Aşağıda bir basit devralma senaryo ve TPH desen kullanan bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="8dcfa-114">*Ayrıştırıcıyı* sütun türünü tanımlayan *Blog* her satır depolanır.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![görüntü](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a><span data-ttu-id="8dcfa-116">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="8dcfa-116">Data Annotations</span></span>

<span data-ttu-id="8dcfa-117">Devralma yapılandırmak için veri ek açıklamaları kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8dcfa-118">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="8dcfa-118">Fluent API</span></span>

<span data-ttu-id="8dcfa-119">Fluent API adını ve türünü ayrıştırıcı sütunun ve hiyerarşideki her türünü tanımlamak için kullanılan değerleri yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="8dcfa-120">Ayrıştırıcı özelliğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8dcfa-120">Configuring the discriminator property</span></span>

<span data-ttu-id="8dcfa-121">Yukarıdaki örneklerde ayrıştırıcı olarak oluşturulan bir [gölge özelliği](xref:core/modeling/shadow-properties) hiyerarşinin temel varlık üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="8dcfa-122">Modeldeki bir özellik olduğundan yalnızca gibi diğer özellikleri yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="8dcfa-123">Örneğin, varsayılan olarak, kural tarafından Ayrıştırıcıyı kullanıldığında, maksimum uzunluk ayarlamak için şunu yazın:</span><span class="sxs-lookup"><span data-stu-id="8dcfa-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="8dcfa-124">Ayrıştırıcı varlığınızdaki gerçek bir CLR özellik de eşlenebilir.</span><span class="sxs-lookup"><span data-stu-id="8dcfa-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="8dcfa-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8dcfa-125">For example:</span></span>
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="8dcfa-126">Bu iki şey birlikte birleştirme hem gerçek özelliğine Ayrıştırıcıyı harita ve yapılandırma mümkündür:</span><span class="sxs-lookup"><span data-stu-id="8dcfa-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
