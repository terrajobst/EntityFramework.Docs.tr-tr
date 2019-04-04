---
title: Genel Bakış - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: fa0695be29668789a179f9a0d6330f3361dbac29
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131425"
---
# <a name="entity-framework-core"></a><span data-ttu-id="8c6fc-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8c6fc-102">Entity Framework Core</span></span>

<span data-ttu-id="8c6fc-103">Entity Framework (EF) Core, hafif, Genişletilebilir, [açık kaynak](https://github.com/aspnet/EntityFrameworkCore) ve çoklu platform sürümünü teknoloji erişim popüler Entity Framework Veri.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="8c6fc-104">EF Core, .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanıyla çalışmasına etkinleştirme bir nesne ilişkisel eşleyicidir (O/RM) olarak hizmet verebilir ve çoğu veri erişim kodu gereksinimini ortadan bunlar genellikle yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="8c6fc-105">EF Core birçok veritabanı altyapılarını destekleyen, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="8c6fc-106">Model</span><span class="sxs-lookup"><span data-stu-id="8c6fc-106">The Model</span></span>

<span data-ttu-id="8c6fc-107">EF Core ile veri erişimi, bir modeli kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="8c6fc-108">Bir modeli varlık sınıfları ve böylece sorgu ve veri kaydetmek, veritabanı ile bir oturumu temsil eden bir bağlam nesnesi oluşur.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="8c6fc-109">Bkz: [Model oluşturma](modeling/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="8c6fc-110">Model veritabanınızın eşleşen veya EF geçişleri modelinizden bir veritabanı oluşturmak için kullanın ve modelinizi evrim Geçiren zaman içindeki değişimini elle kod mevcut bir veritabanından, bir model oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-110">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model, and then evolve it as your model changes over time.</span></span>

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
            optionsBuilder.UseSqlServer(
                @"Server=(localdb)\mssqllocaldb;Database=Blogging;Integrated Security=True");
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

## <a name="querying"></a><span data-ttu-id="8c6fc-111">Sorgulama</span><span class="sxs-lookup"><span data-stu-id="8c6fc-111">Querying</span></span>

<span data-ttu-id="8c6fc-112">Varlık sınıflarının örneklerini dil tümleşik sorgu (LINQ) kullanarak veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="8c6fc-113">Bkz: [veri sorgulama](querying/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="8c6fc-114">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="8c6fc-114">Saving Data</span></span>

<span data-ttu-id="8c6fc-115">Veri oluşturulan, silinen ve örnekleri, varlık sınıfları kullanarak veritabanında değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="8c6fc-116">Bkz: [verileri kaydetme](saving/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8c6fc-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="8c6fc-117">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="8c6fc-117">Next steps</span></span>

<span data-ttu-id="8c6fc-118">Tanıtım amaçlı öğreticiler için bkz. [Entity Framework Core ile çalışmaya başlama](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="8c6fc-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

