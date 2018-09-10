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
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core hafif, Genişletilebilir, ve platformlar arası sürümüne popüler Entity Framework Veri erişim teknolojisi.

EF Core, .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanıyla çalışmasına etkinleştirme bir nesne ilişkisel eşleyicidir (O/RM) olarak hizmet verebilir ve çoğu veri erişim kodu gereksinimini ortadan bunlar genellikle yazmanız gerekir.

EF Core birçok veritabanı altyapılarını destekleyen, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.

## <a name="the-model"></a>Model

EF Core ile veri erişimi, bir modeli kullanılarak gerçekleştirilir. Bir modeli varlık sınıfları ve böylece sorgu ve veri kaydetmek, veritabanı ile bir oturumu temsil eden türetilmiş bir bağlam oluşur. Bkz: [Model oluşturma](modeling/index.md) daha fazla bilgi için.

Varolan bir veritabanından bir model oluşturmak, kod, veritabanıyla eşleşmesi için bir model el veya modelinizden bir veritabanı oluşturmak (ve modelinizi zamanla değiştikçe evrim geçiren için) EF geçişleri kullanın.

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

## <a name="querying"></a>Sorgulama

Varlık sınıflarının örneklerini dil tümleşik sorgu (LINQ) kullanarak veritabanından alınır. Bkz: [veri sorgulama](querying/index.md) daha fazla bilgi için.

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Verileri Kaydetme

Veri oluşturulan, silinen ve örnekleri, varlık sınıfları kullanarak veritabanında değiştirildi. Bkz: [verileri kaydetme](saving/index.md) daha fazla bilgi için.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a>Sonraki adımlar

Tanıtım amaçlı öğreticiler için bkz. [Entity Framework Core ile çalışmaya başlama](get-started/index.md).

