---
title: "Devralma (ilişkisel veritabanı) - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="8bc24-102">Devralma (ilişkisel veritabanı)</span><span class="sxs-lookup"><span data-stu-id="8bc24-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="8bc24-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8bc24-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8bc24-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="8bc24-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8bc24-105">Devralma EF modeldeki varlık sınıflarını devralma veritabanında nasıl temsil denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8bc24-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="8bc24-106">Şu anda, yalnızca tablo başına hiyerarşisi (TPH) deseni EF çekirdek uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8bc24-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="8bc24-107">Diğer ortak desenler tablo başına-türü (birleştirilmiş TPT) gibi ve tablo başına-somut-türü (TPC) henüz kullanılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="8bc24-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="8bc24-108">Kurallar</span><span class="sxs-lookup"><span data-stu-id="8bc24-108">Conventions</span></span>

<span data-ttu-id="8bc24-109">Kurala göre tablo-başına-hiyerarşisi (TPH) desenini kullanarak devralma eşleşecektir.</span><span class="sxs-lookup"><span data-stu-id="8bc24-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="8bc24-110">TPH tek bir tabloyu hiyerarşideki tüm türleri için verileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="8bc24-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="8bc24-111">Ayrıştırıcı sütunun her satırın gösterdiği hangi türünü tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8bc24-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="8bc24-112">EF çekirdek yalnızca Kurulum devralma iki veya daha fazla devralınan türleri açıkça modelde varsa (bkz [devralma](../inheritance.md) daha fazla ayrıntı için).</span><span class="sxs-lookup"><span data-stu-id="8bc24-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="8bc24-113">Aşağıda bir basit devralma senaryo ve TPH desen kullanan bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="8bc24-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="8bc24-114">*Ayrıştırıcıyı* sütun türünü tanımlayan *Blog* her satır depolanır.</span><span class="sxs-lookup"><span data-stu-id="8bc24-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="8bc24-116">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="8bc24-116">Data Annotations</span></span>

<span data-ttu-id="8bc24-117">Devralma yapılandırmak için veri ek açıklamaları kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="8bc24-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8bc24-118">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="8bc24-118">Fluent API</span></span>

<span data-ttu-id="8bc24-119">Fluent API adını ve türünü ayrıştırıcı sütunun ve hiyerarşideki her türünü tanımlamak için kullanılan değerleri yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8bc24-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
