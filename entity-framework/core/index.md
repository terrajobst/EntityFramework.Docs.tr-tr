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
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core hafif, Genişletilebilir, ve platformlar arası sürümüne popüler Entity Framework Veri erişim teknolojisi.

EF Core, .NET geliştiricilerinin .NET nesneleri kullanarak bir veritabanıyla çalışmasına etkinleştirme bir nesne ilişkisel eşleyicidir (O/RM) olarak hizmet verebilir ve çoğu veri erişim kodu gereksinimini ortadan bunlar genellikle yazmanız gerekir.

EF Core birçok veritabanı altyapılarını destekleyen, bkz: [veritabanı sağlayıcıları](providers/index.md) Ayrıntılar için.

Kod yazarak öğrenmek istiyorsanız, aşağıdakilerden birini öneririz bizim [Başlarken](get-started/index.md) başlamanızı sağlayacak kılavuzları EF Core ile çalışmaya.

## <a name="what-is-new-in-ef-core"></a>EF Core yenilikleri

EF Core ile ilgili bilgi sahibi olduğunuz ve doğrudan en son sürümlerine ayrıntılarına gitmek istediğiniz varsa:

- **[EF Core 2.1 yenilikler nelerdir?](xref:core/what-is-new/ef-core-2.1)**
- **[EF Core için var olan uygulamaları yükseltme 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Entity Framework Core Al

[NuGet paketini yüklemek](https://docs.nuget.org/ndocs/quickstart/use-a-package) için kullanmak istediğiniz veritabanı sağlayıcısı. Örneğin, SQL Server sağlayıcısı platformlar arası geliştirme kullanarak yüklemek için `dotnet` aracında komut satırı:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Veya Visual Studio'da Paket Yöneticisi konsolu kullanarak:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Bkz: [veritabanı sağlayıcıları](providers/index.md) yok sağlayıcıları hakkında daha fazla bilgi ve [yükleme EF Core](get-started/install/index.md) daha ayrıntılı yükleme adımları için.

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
