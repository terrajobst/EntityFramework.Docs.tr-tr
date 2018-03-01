---
title: "Hızlı bir genel bakış - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: c76b4cd318151b502c549fa0a82800f9987ed94c
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="54135-102">Entity Framework Çekirdek hızlı bir genel bakış</span><span class="sxs-lookup"><span data-stu-id="54135-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="54135-103">Hafif, Genişletilebilir, Entity Framework (EF) çekirdeği olur ve platformlar arası sürümü popüler Entity Framework verilerine erişim teknolojisini.</span><span class="sxs-lookup"><span data-stu-id="54135-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="54135-104">EF çekirdeği .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanı ile çalışmak bir nesne ilişkisel Eşleyici (O/RM) olur.</span><span class="sxs-lookup"><span data-stu-id="54135-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="54135-105">Genellikle geliştiricilerin yazmak zorunda olduğu çoğu veri erişim koduna yönelik gereksinimi ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="54135-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="54135-106">EF çekirdek destekleyen birçok veritabanı motoru, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="54135-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="54135-107">Kod yazarak öğrenmek istiyorsanız, aşağıdakilerden birini öneririz bizim [Başlarken](get-started/index.md) elde kılavuzlara EF çekirdek ile başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="54135-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="54135-108">EF çekirdek yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="54135-108">What is new in EF Core</span></span>

<span data-ttu-id="54135-109">EF çekirdek bilgi sahibiyseniz ve en son sürümlerini ayrıntılara düz atlamak istiyorsanız:</span><span class="sxs-lookup"><span data-stu-id="54135-109">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="54135-110">**[(Şu anda önizlemede) EF çekirdek 2.1 yenilikler nelerdir?](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="54135-110">**[What is new in EF Core 2.1 (currently in preview)](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="54135-111">**[EF çekirdek 2.0 (son yayımlanmış bir sürüm) yenilikler](xref:core/what-is-new/ef-core-2.0)**</span><span class="sxs-lookup"><span data-stu-id="54135-111">**[What is new in EF Core 2.0 (the latest released version)](xref:core/what-is-new/ef-core-2.0)**</span></span>
- <span data-ttu-id="54135-112">**[EF çekirdek 2.0 için var olan uygulamaları yükseltme](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="54135-112">**[Upgrading existing applications to EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="54135-113">Entity Framework Çekirdek Al</span><span class="sxs-lookup"><span data-stu-id="54135-113">Get Entity Framework Core</span></span>

<span data-ttu-id="54135-114">[NuGet paket yüklemesi](https://docs.nuget.org/ndocs/quickstart/use-a-package) için kullanmak istediğiniz veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="54135-114">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="54135-115">Örneğin</span><span class="sxs-lookup"><span data-stu-id="54135-115">E.g.</span></span> <span data-ttu-id="54135-116">platformlar arası geliştirme kullanarak SQL Server Sağlayıcısı'nı yüklemek için `dotnet` komut satırı aracı:</span><span class="sxs-lookup"><span data-stu-id="54135-116">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="54135-117">Veya Visual Studio'da Paket Yöneticisi konsolu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="54135-117">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="54135-118">Bkz: [veritabanı sağlayıcıları](providers/index.md) kullanılabilir sağlayıcılar hakkında bilgi için ve [yükleme EF çekirdek](get-started/install/index.md) yükleme adımlarını ayrıntılı için.</span><span class="sxs-lookup"><span data-stu-id="54135-118">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="54135-119">Modeli</span><span class="sxs-lookup"><span data-stu-id="54135-119">The Model</span></span>

<span data-ttu-id="54135-120">EF çekirdek ile veri erişim modeli kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="54135-120">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="54135-121">Bir model sınıflar ve sorgu ve veri kaydetmenize olanak veren veritabanı, oturumla temsil eden bir türetilmiş bağlamı oluşur.</span><span class="sxs-lookup"><span data-stu-id="54135-121">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="54135-122">Bkz: [bir Model oluşturma](modeling/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="54135-122">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="54135-123">Varolan bir veritabanından bir model oluşturmak, veritabanınızı eşleştirmek için bir model kod el veya modelinizin dışında bir veritabanı oluşturmak için (ve modelinizi zaman içinde değiştikçe gelişmesi için) EF geçişler kullanın.</span><span class="sxs-lookup"><span data-stu-id="54135-123">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="54135-124">Sorgulama</span><span class="sxs-lookup"><span data-stu-id="54135-124">Querying</span></span>

<span data-ttu-id="54135-125">Varlık sınıfların örneklerini dil tümleşik sorgu (LINQ) kullanarak veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="54135-125">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="54135-126">Bkz: [veri sorgulama](querying/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="54135-126">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="54135-127">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="54135-127">Saving Data</span></span>

<span data-ttu-id="54135-128">Veri oluşturulur, silinmiş ve varlık sınıfların örneklerini kullanarak veritabanında değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="54135-128">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="54135-129">Bkz: [verileri kaydetme](saving/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="54135-129">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
