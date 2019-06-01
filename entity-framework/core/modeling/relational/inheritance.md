---
title: Devralma (ilişkisel veritabanı) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2d0a2abc554f5f115479f886ca3f9f4f01b80b5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452286"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="84530-102">Devralma (ilişkisel veritabanı)</span><span class="sxs-lookup"><span data-stu-id="84530-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="84530-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="84530-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="84530-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="84530-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="84530-105">Devralma EF modeli, varlık sınıflarının Devralmada veritabanında nasıl temsil edildiğini denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84530-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="84530-106">Şu anda yalnızca tablo başına hiyerarşi (TPH) deseni EF Core uygulanır.</span><span class="sxs-lookup"><span data-stu-id="84530-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="84530-107">Tablo başına tür (TPT) diğer ortak desenler ister ve tablo başına somut-tür (TPC) henüz kullanılamıyor.</span><span class="sxs-lookup"><span data-stu-id="84530-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="84530-108">Kurallar</span><span class="sxs-lookup"><span data-stu-id="84530-108">Conventions</span></span>

<span data-ttu-id="84530-109">Kural gereği, devralma tablo-başına-hiyerarşi (TPH) deseni kullanılarak eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="84530-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="84530-110">TPH tek bir tabloyu hiyerarşideki tüm türleri için verileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="84530-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="84530-111">Bir Ayrıştırıcı sütunu, her satırı temsil eden hangi tür tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84530-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="84530-112">EF Core yalnızca Kurulum devralma iki veya daha fazla devralınan türlerin açıkça modele dahil edilirse (bkz [devralma](../inheritance.md) daha fazla ayrıntı için).</span><span class="sxs-lookup"><span data-stu-id="84530-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="84530-113">Basit devralma senaryo ve TPH desenini kullanarak, bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="84530-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="84530-114">*Ayrıştırıcı* sütun türünü tanımlayan *Blog* her satırda depolanır.</span><span class="sxs-lookup"><span data-stu-id="84530-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

>[!NOTE]
> <span data-ttu-id="84530-116">Veritabanı sütunları otomatik olarak TPH eşleme kullanırken gerektiği gibi boş değer atanabilir yapılır.</span><span class="sxs-lookup"><span data-stu-id="84530-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="84530-117">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="84530-117">Data Annotations</span></span>

<span data-ttu-id="84530-118">Veri ek açıklamaları, devralmayı yapılandırmak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="84530-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="84530-119">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="84530-119">Fluent API</span></span>

<span data-ttu-id="84530-120">Fluent API'si, bir ayrıştırıcı sütunu ve hiyerarşideki her bir tür tanımlamak için kullanılan değerleri türünü ve adını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84530-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="84530-121">Ayrıştırıcı özelliği yapılandırma</span><span class="sxs-lookup"><span data-stu-id="84530-121">Configuring the discriminator property</span></span>

<span data-ttu-id="84530-122">Yukarıdaki örneklerde, ayrıştırıcı olarak oluşturulan bir [gölge özelliği](xref:core/modeling/shadow-properties) üzerinde temel bir varlık hiyerarşisi.</span><span class="sxs-lookup"><span data-stu-id="84530-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="84530-123">Modeldeki bir özellik olduğundan, diğer özellikleri gibi yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="84530-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="84530-124">Örneğin, varsayılan kuralı tarafından ayrıştırıcı kullanılırken, en fazla uzunluk ayarlamak için şunu yazın:</span><span class="sxs-lookup"><span data-stu-id="84530-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="84530-125">Ayrıştırıcı da gerçek bir CLR özellik varlığınızdaki eşlenebilir.</span><span class="sxs-lookup"><span data-stu-id="84530-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="84530-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="84530-126">For example:</span></span>
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

<span data-ttu-id="84530-127">Peki bu ikisi birlikte birleştirerek hem ayrıştırıcı gerçek özellik eşleme ve yapılandırma mümkündür:</span><span class="sxs-lookup"><span data-stu-id="84530-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
