---
title: Entity Framework Core genel bakış
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: 0107a520e5a698eaf76426b63c6f784392559167
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196975"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core, popüler Entity Framework veri erişim teknolojisinin basit, Genişletilebilir, [Açık kaynaklı](https://github.com/aspnet/EntityFrameworkCore) ve platformlar arası bir sürümüdür.

EF Core, nesne ilişkisel Eşleyici (O/RM) olarak görev yapabilir, .NET geliştiricilerin .NET nesnelerini kullanarak bir veritabanıyla çalışmasını sağlar ve genellikle yazması gereken veri erişimi kodunun çoğunu ortadan kaldırır.

EF Core birçok veritabanı altyapısını destekler, Ayrıntılar için bkz. [veritabanı sağlayıcıları](providers/index.md) .

## <a name="the-model"></a>Model

EF Core, veri erişimi bir model kullanılarak gerçekleştirilir. Bir model, varlık sınıflarından ve veritabanı ile bir oturumu temsil eden bir bağlam nesnesinden oluşur ve verileri sorgulayıp kaydetmenizi sağlar. Daha fazla bilgi için bkz. [model oluşturma](modeling/index.md) .

Var olan bir veritabanından bir model oluşturabilir, bir modeli veritabanınızdan eşlemek üzere kodlayabilir veya modelinizden bir veritabanı oluşturmak için [EF geçişleri](managing-schemas/migrations/index.md) kullanabilirsiniz.

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

## <a name="querying"></a>Sorgulama

Varlık sınıflarınızın örnekleri, dil ile tümleşik sorgu (LINQ) kullanılarak veritabanından alınır. Daha fazla bilgi için bkz. [veri sorgulama](querying/index.md) .

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

Veri, varlık sınıflarınızın örnekleri kullanılarak oluşturulur, silinir ve değiştirilir. Daha fazla bilgi edinmek için [verileri kaydetme](saving/index.md) konusuna bakın.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a>Sonraki adımlar

Tanıtım öğreticileri için bkz. [Entity Framework Core kullanmaya](get-started/index.md)başlama.

