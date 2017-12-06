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
# <a name="entity-framework-core-quick-overview"></a>Entity Framework Çekirdek hızlı bir genel bakış

Hafif, Genişletilebilir, Entity Framework (EF) çekirdeği olur ve platformlar arası sürümü popüler Entity Framework verilerine erişim teknolojisini.

EF çekirdeği .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanı ile çalışmak bir nesne ilişkisel Eşleyici (O/RM) olur. Bu, geliştiriciler genellikle yazmanıza gerek veri erişim kod gereksinimini ortadan kaldırır. EF çekirdek destekleyen birçok veritabanı motoru, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.

Kod yazarak öğrenmek istiyorsanız, aşağıdakilerden birini öneririz bizim [Başlarken](get-started/index.md) elde kılavuzlara EF çekirdek ile başlatıldı.

## <a name="latest-version-ef-core-20"></a>En son sürümünü: EF çekirdek 2.0

EF çekirdek bilgi sahibiyseniz ve düz yeni sürümün ayrıntılarını atlamak istiyorsanız:

- **[EF çekirdek 2.0 yeni özellikler](what-is-new/index.md)**
- **[EF çekirdek 2.0 için var olan uygulamaları yükseltme](miscellaneous/1x-2x-upgrade.md)**

## <a name="get-entity-framework-core"></a>Entity Framework Çekirdek Al

[NuGet paket yüklemesi](https://docs.nuget.org/ndocs/quickstart/use-a-package) için kullanmak istediğiniz veritabanı sağlayıcısı. Örneğin platformlar arası geliştirme kullanarak SQL Server Sağlayıcısı'nı yüklemek için `dotnet` komut satırı aracı:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Veya Visual Studio'da Paket Yöneticisi konsolu kullanarak:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Bkz: [veritabanı sağlayıcıları](providers/index.md) kullanılabilir sağlayıcılar hakkında bilgi için ve [yükleme EF çekirdek](get-started/install/index.md) yükleme adımlarını ayrıntılı için.

## <a name="the-model"></a>Modeli

EF çekirdek ile veri erişim modeli kullanılarak gerçekleştirilir. Bir model sınıflar ve sorgu ve veri kaydetmenize olanak veren veritabanı, oturumla temsil eden bir türetilmiş bağlamı oluşur. Bkz: [bir Model oluşturma](modeling/index.md) daha fazla bilgi için.

Varolan bir veritabanından bir model oluşturmak, veritabanınızı eşleştirmek için bir model kod el veya modelinizin dışında bir veritabanı oluşturmak için (ve modelinizi zaman içinde değiştikçe gelişmesi için) EF geçişler kullanın.

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

Varlık sınıfların örneklerini dil tümleşik sorgu (LINQ) kullanarak veritabanından alınır. Bkz: [veri sorgulama](querying/index.md) daha fazla bilgi için.

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

Veri oluşturulur, silinmiş ve varlık sınıfların örneklerini kullanarak veritabanında değiştirildi. Bkz: [verileri kaydetme](saving/index.md) daha fazla bilgi için.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
