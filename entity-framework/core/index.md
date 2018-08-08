---
title: Genel Bakış - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: d2c40356fc3b37251f95b08ee8bf07ed4eab7b80
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614314"
---
# <a name="entity-framework-core"></a><span data-ttu-id="c66ed-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c66ed-102">Entity Framework Core</span></span>

<span data-ttu-id="c66ed-103">Entity Framework (EF) Core hafif, Genişletilebilir, ve platformlar arası sürümüne popüler Entity Framework Veri erişim teknolojisi.</span><span class="sxs-lookup"><span data-stu-id="c66ed-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="c66ed-104">EF Core, .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanıyla çalışmasına etkinleştirme bir nesne ilişkisel eşleyicidir (O/RM) olarak hizmet verebilir ve çoğu veri erişim kodu gereksinimini ortadan bunlar genellikle yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c66ed-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="c66ed-105">EF Core birçok veritabanı altyapılarını destekleyen, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="c66ed-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="c66ed-106">Kod yazarak öğrenmek istiyorsanız, aşağıdakilerden birini öneririz bizim [Başlarken](get-started/index.md) başlamanızı sağlayacak kılavuzları EF Core ile çalışmaya.</span><span class="sxs-lookup"><span data-stu-id="c66ed-106">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="c66ed-107">EF Core yenilikleri</span><span class="sxs-lookup"><span data-stu-id="c66ed-107">What is new in EF Core</span></span>

<span data-ttu-id="c66ed-108">EF Core ile ilgili bilgi sahibi olduğunuz ve doğrudan en son sürümlerine ayrıntılarına gitmek istediğiniz varsa:</span><span class="sxs-lookup"><span data-stu-id="c66ed-108">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="c66ed-109">**[EF Core 2.1 yenilikler nelerdir?](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="c66ed-109">**[What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="c66ed-110">**[EF Core için var olan uygulamaları yükseltme 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="c66ed-110">**[Upgrading existing applications to EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="c66ed-111">Entity Framework Core Al</span><span class="sxs-lookup"><span data-stu-id="c66ed-111">Get Entity Framework Core</span></span>

<span data-ttu-id="c66ed-112">[NuGet paketini yüklemek](https://docs.nuget.org/ndocs/quickstart/use-a-package) için kullanmak istediğiniz veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="c66ed-112">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="c66ed-113">Örneğin, SQL Server sağlayıcısı platformlar arası geliştirme kullanarak yüklemek için `dotnet` aracında komut satırı:</span><span class="sxs-lookup"><span data-stu-id="c66ed-113">For example, to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="c66ed-114">Veya Visual Studio'da Paket Yöneticisi konsolu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="c66ed-114">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="c66ed-115">Bkz: [veritabanı sağlayıcıları](providers/index.md) yok sağlayıcıları hakkında daha fazla bilgi ve [yükleme EF Core](get-started/install/index.md) daha ayrıntılı yükleme adımları için.</span><span class="sxs-lookup"><span data-stu-id="c66ed-115">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="c66ed-116">Model</span><span class="sxs-lookup"><span data-stu-id="c66ed-116">The Model</span></span>

<span data-ttu-id="c66ed-117">EF Core ile veri erişimi, bir modeli kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c66ed-117">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="c66ed-118">Bir modeli varlık sınıfları ve böylece sorgu ve veri kaydetmek, veritabanı ile bir oturumu temsil eden türetilmiş bir bağlam oluşur.</span><span class="sxs-lookup"><span data-stu-id="c66ed-118">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="c66ed-119">Bkz: [Model oluşturma](modeling/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c66ed-119">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="c66ed-120">Varolan bir veritabanından bir model oluşturmak, kod, veritabanıyla eşleşmesi için bir model el veya modelinizden bir veritabanı oluşturmak (ve modelinizi zamanla değiştikçe evrim geçiren için) EF geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="c66ed-120">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="c66ed-121">Sorgulama</span><span class="sxs-lookup"><span data-stu-id="c66ed-121">Querying</span></span>

<span data-ttu-id="c66ed-122">Varlık sınıflarının örneklerini dil tümleşik sorgu (LINQ) kullanarak veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="c66ed-122">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="c66ed-123">Bkz: [veri sorgulama](querying/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c66ed-123">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="c66ed-124">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="c66ed-124">Saving Data</span></span>

<span data-ttu-id="c66ed-125">Veri oluşturulan, silinen ve örnekleri, varlık sınıfları kullanarak veritabanında değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="c66ed-125">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="c66ed-126">Bkz: [verileri kaydetme](saving/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c66ed-126">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
