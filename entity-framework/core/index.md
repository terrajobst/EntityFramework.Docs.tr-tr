---
title: Hızlı bir genel bakış - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: f9aac91545b97e56686e3a8d2eb9e83c849587d9
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="entity-framework-core-quick-overview"></a>Entity Framework Çekirdek hızlı bir genel bakış

Hafif, Genişletilebilir, Entity Framework (EF) çekirdeği olur ve platformlar arası sürümü popüler Entity Framework verilerine erişim teknolojisini.

EF çekirdek .NET nesneleri kullanarak bir veritabanı ile çalışmak .NET geliştiricilerinin etkinleştirme bir nesne ilişkisel Eşleyici (O/RM) olarak hizmet verebilir ve veri erişim kodu çoğunu gereksinimini genellikle yazma gerekir. 

EF çekirdek destekleyen birçok veritabanı motoru, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.

Kod yazarak öğrenmek istiyorsanız, aşağıdakilerden birini öneririz bizim [Başlarken](get-started/index.md) elde kılavuzlara EF çekirdek ile başlatıldı.

## <a name="what-is-new-in-ef-core"></a>EF çekirdek yenilikler nelerdir?

EF çekirdek bilgi sahibiyseniz ve en son sürümlerini ayrıntılara düz atlamak istiyorsanız:

- **[(Şu anda önizlemede) EF çekirdek 2.1 yenilikler nelerdir?](xref:core/what-is-new/ef-core-2.1)**
- **[EF çekirdek 2.0 (son yayımlanmış bir sürüm) yenilikler](xref:core/what-is-new/ef-core-2.0)**
- **[EF çekirdek 2.0 için var olan uygulamaları yükseltme](xref:core/miscellaneous/1x-2x-upgrade)**


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
