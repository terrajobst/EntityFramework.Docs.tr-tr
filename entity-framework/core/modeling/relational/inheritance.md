---
title: Devralma (Ilişkisel veritabanı)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: c660107619470a726fe13ad8eee2850749e6dcd9
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812086"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="2afae-102">Devralma (İlişkisel Veritabanı)</span><span class="sxs-lookup"><span data-stu-id="2afae-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="2afae-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2afae-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2afae-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="2afae-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2afae-105">EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2afae-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="2afae-106">Şu anda EF Core ' de yalnızca hiyerarşi başına tablo (TPH) düzeni uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2afae-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="2afae-107">Tablo/tür (TPT) ve tablo başına somut-tür (TPC) gibi diğer yaygın desenler henüz kullanılamamaktadır.</span><span class="sxs-lookup"><span data-stu-id="2afae-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="2afae-108">Kurallar</span><span class="sxs-lookup"><span data-stu-id="2afae-108">Conventions</span></span>

<span data-ttu-id="2afae-109">Kural gereği, devralma, hiyerarşi başına tablo (TPH) düzeni kullanılarak eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2afae-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="2afae-110">TPH, hiyerarşideki tüm türlerin verilerini depolamak için tek bir tablo kullanır.</span><span class="sxs-lookup"><span data-stu-id="2afae-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="2afae-111">Bir Ayrıştırıcı sütunu, her bir satırın hangi tür temsil ettiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2afae-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="2afae-112">EF Core, yalnızca modele açıkça iki veya daha fazla devralınmış tür varsa devralma ayarı yapılır (daha fazla ayrıntı için bkz. [Devralma](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="2afae-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="2afae-113">Aşağıda, bir basit devralma senaryosunu ve TPH deseninin kullanıldığı bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2afae-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="2afae-114">*Ayrıştırıcı* sütunu, her satırda hangi *Blog* türünün depolandığını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2afae-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> <span data-ttu-id="2afae-116">Veritabanı sütunları, TPH eşlemesi kullanılırken gerektiğinde otomatik olarak null yapılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="2afae-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2afae-117">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="2afae-117">Data Annotations</span></span>

<span data-ttu-id="2afae-118">Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="2afae-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2afae-119">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="2afae-119">Fluent API</span></span>

<span data-ttu-id="2afae-120">Ayrıştırıcı sütununun adını ve türünü ve hiyerarşideki her türü tanımlamak için kullanılan değerleri yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2afae-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="2afae-121">Ayrıştırıcı özelliğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2afae-121">Configuring the discriminator property</span></span>

<span data-ttu-id="2afae-122">Yukarıdaki örneklerde, ayrıştırıcı hiyerarşinin temel varlığında bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2afae-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="2afae-123">Modeldeki bir özellik olduğu için, diğer özellikler gibi yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="2afae-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="2afae-124">Örneğin, varsayılan, kural olarak ayrıştırıcı kullanılıyorsa en fazla uzunluğu ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="2afae-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="2afae-125">Ayrıştırıcı, varlığınızda gerçek bir CLR özelliğine de eşleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2afae-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="2afae-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2afae-126">For example:</span></span>

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

<span data-ttu-id="2afae-127">Bu iki şeyi birlikte birleştirmek, Ayrıştırıcıyı gerçek bir özellik ile eşleştirmek ve yapılandırmak mümkündür:</span><span class="sxs-lookup"><span data-stu-id="2afae-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
