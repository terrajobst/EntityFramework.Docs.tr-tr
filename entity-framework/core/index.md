---
title: Genel Bakış - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: ee3fac9e9103749195886a632fbeac3163a46689
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250549"
---
# <a name="entity-framework-core"></a><span data-ttu-id="0f258-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0f258-102">Entity Framework Core</span></span>

<span data-ttu-id="0f258-103">Entity Framework (EF) Core hafif, Genişletilebilir, ve platformlar arası sürümüne popüler Entity Framework Veri erişim teknolojisi.</span><span class="sxs-lookup"><span data-stu-id="0f258-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="0f258-104">EF Core, .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanıyla çalışmasına etkinleştirme bir nesne ilişkisel eşleyicidir (O/RM) olarak hizmet verebilir ve çoğu veri erişim kodu gereksinimini ortadan bunlar genellikle yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0f258-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="0f258-105">EF Core birçok veritabanı altyapılarını destekleyen, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="0f258-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="0f258-106">Model</span><span class="sxs-lookup"><span data-stu-id="0f258-106">The Model</span></span>

<span data-ttu-id="0f258-107">EF Core ile veri erişimi, bir modeli kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="0f258-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="0f258-108">Bir modeli varlık sınıfları ve böylece sorgu ve veri kaydetmek, veritabanı ile bir oturumu temsil eden türetilmiş bir bağlam oluşur.</span><span class="sxs-lookup"><span data-stu-id="0f258-108">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="0f258-109">Bkz: [Model oluşturma](modeling/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0f258-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="0f258-110">Varolan bir veritabanından bir model oluşturmak, kod, veritabanıyla eşleşmesi için bir model el veya modelinizden bir veritabanı oluşturmak (ve modelinizi zamanla değiştikçe evrim geçiren için) EF geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="0f258-110">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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
}
```

## <a name="querying"></a><span data-ttu-id="0f258-111">Sorgulama</span><span class="sxs-lookup"><span data-stu-id="0f258-111">Querying</span></span>

<span data-ttu-id="0f258-112">Varlık sınıflarının örneklerini dil tümleşik sorgu (LINQ) kullanarak veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="0f258-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="0f258-113">Bkz: [veri sorgulama](querying/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0f258-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="0f258-114">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="0f258-114">Saving Data</span></span>

<span data-ttu-id="0f258-115">Veri oluşturulan, silinen ve örnekleri, varlık sınıfları kullanarak veritabanında değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="0f258-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="0f258-116">Bkz: [verileri kaydetme](saving/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0f258-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="0f258-117">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0f258-117">Next steps</span></span>

<span data-ttu-id="0f258-118">Tanıtım amaçlı öğreticiler için bkz. [Entity Framework Core ile çalışmaya başlama](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="0f258-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

