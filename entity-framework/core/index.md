---
title: "Hızlı bir genel bakış - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="72f73-102">Entity Framework Çekirdek hızlı bir genel bakış</span><span class="sxs-lookup"><span data-stu-id="72f73-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="72f73-103">Hafif, Genişletilebilir, Entity Framework (EF) çekirdeği olur ve platformlar arası sürümü popüler Entity Framework verilerine erişim teknolojisini.</span><span class="sxs-lookup"><span data-stu-id="72f73-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="72f73-104">EF çekirdeği .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanı ile çalışmak bir nesne ilişkisel Eşleyici (O/RM) olur.</span><span class="sxs-lookup"><span data-stu-id="72f73-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="72f73-105">Bu, geliştiriciler genellikle yazmanıza gerek veri erişim kod gereksinimini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="72f73-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="72f73-106">EF çekirdek destekleyen birçok veritabanı motoru, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="72f73-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="72f73-107">Kod yazarak öğrenmek istiyorsanız, aşağıdakilerden birini öneririz bizim [Başlarken](get-started/index.md) elde kılavuzlara EF çekirdek ile başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="72f73-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="latest-version-ef-core-20"></a><span data-ttu-id="72f73-108">En son sürümünü: EF çekirdek 2.0</span><span class="sxs-lookup"><span data-stu-id="72f73-108">Latest version: EF Core 2.0</span></span>

<span data-ttu-id="72f73-109">EF çekirdek bilgi sahibiyseniz ve düz yeni sürümün ayrıntılarını atlamak istiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="72f73-109">If you are familiar with EF Core and want to jump straight into the details of the new version:</span></span>

- <span data-ttu-id="72f73-110">**[EF çekirdek 2.0 yeni özellikler](what-is-new/index.md)**</span><span class="sxs-lookup"><span data-stu-id="72f73-110">**[New features in EF Core 2.0](what-is-new/index.md)**</span></span>
- <span data-ttu-id="72f73-111">**[EF çekirdek 2.0 için var olan uygulamaları yükseltme](miscellaneous/1x-2x-upgrade.md)**</span><span class="sxs-lookup"><span data-stu-id="72f73-111">**[Upgrading existing applications to EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span></span>

## <a name="get-entity-framework-core"></a><span data-ttu-id="72f73-112">Entity Framework Çekirdek Al</span><span class="sxs-lookup"><span data-stu-id="72f73-112">Get Entity Framework Core</span></span>

<span data-ttu-id="72f73-113">[NuGet paket yüklemesi](https://docs.nuget.org/ndocs/quickstart/use-a-package) için kullanmak istediğiniz veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="72f73-113">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="72f73-114">Örneğin</span><span class="sxs-lookup"><span data-stu-id="72f73-114">E.g.</span></span> <span data-ttu-id="72f73-115">platformlar arası geliştirme kullanarak SQL Server Sağlayıcısı'nı yüklemek için `dotnet` komut satırı aracı:</span><span class="sxs-lookup"><span data-stu-id="72f73-115">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="72f73-116">Veya Visual Studio'da Paket Yöneticisi konsolu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="72f73-116">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="72f73-117">Bkz: [veritabanı sağlayıcıları](providers/index.md) kullanılabilir sağlayıcılar hakkında bilgi için ve [yükleme EF çekirdek](get-started/install/index.md) yükleme adımlarını ayrıntılı için.</span><span class="sxs-lookup"><span data-stu-id="72f73-117">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="72f73-118">Modeli</span><span class="sxs-lookup"><span data-stu-id="72f73-118">The Model</span></span>

<span data-ttu-id="72f73-119">EF çekirdek ile veri erişim modeli kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="72f73-119">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="72f73-120">Bir model sınıflar ve sorgu ve veri kaydetmenize olanak veren veritabanı, oturumla temsil eden bir türetilmiş bağlamı oluşur.</span><span class="sxs-lookup"><span data-stu-id="72f73-120">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="72f73-121">Bkz: [bir Model oluşturma](modeling/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="72f73-121">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="72f73-122">Varolan bir veritabanından bir model oluşturmak, veritabanınızı eşleştirmek için bir model kod el veya modelinizin dışında bir veritabanı oluşturmak için (ve modelinizi zaman içinde değiştikçe gelişmesi için) EF geçişler kullanın.</span><span class="sxs-lookup"><span data-stu-id="72f73-122">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="72f73-123">Sorgulama</span><span class="sxs-lookup"><span data-stu-id="72f73-123">Querying</span></span>

<span data-ttu-id="72f73-124">Varlık sınıfların örneklerini dil tümleşik sorgu (LINQ) kullanarak veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="72f73-124">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="72f73-125">Bkz: [veri sorgulama](querying/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="72f73-125">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="72f73-126">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="72f73-126">Saving Data</span></span>

<span data-ttu-id="72f73-127">Veri oluşturulur, silinmiş ve varlık sınıfların örneklerini kullanarak veritabanında değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="72f73-127">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="72f73-128">Bkz: [verileri kaydetme](saving/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="72f73-128">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
