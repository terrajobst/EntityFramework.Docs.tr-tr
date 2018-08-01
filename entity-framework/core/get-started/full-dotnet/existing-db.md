---
title: .NET Framework - mevcut veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388474"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>.NET Framework ile varolan bir veritabanını EF Core ile çalışmaya başlama

Bu kılavuzda, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturacaksınız. Tersine mühendislik, varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için kullanır.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) GitHub üzerinde.

## <a name="prerequisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gereklidir:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) - en az sürüm 15.3

* [NuGet Paket Yöneticisi'nin en son sürümü](https://dist.nuget.org/index.html)

* [Windows PowerShell'in en son sürümünü](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Günlük veritabanı](#blogging-database)

### <a name="blogging-database"></a>Günlük veritabanı

Bu öğreticide bir **blog** LocalDb örneğiniz mevcut veritabanı olarak veritabanı.

> [!TIP]  
> Zaten oluşturduysanız **blog** veritabanı başka bir öğreticinin bir parçası olarak, bu adımı atlayabilirsiniz.

* Visual Studio'yu Aç

* Araçlar > veritabanına bağlan...

* Seçin **Microsoft SQL Server** tıklatıp **devam et**

* Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**

* Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**

* Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**

* Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**

* Sorgu Düzenleyicisi'ne, aşağıda listelenen betiği kopyalayın

* Sorgu düzenleyicisini sağ tıklayıp **Yürüt**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Visual Studio'yu Aç

* Dosya > Yeni > Proje...

* Sol menüden şablonları seçin > Visual C# > Windows

* Seçin **konsol uygulaması** proje şablonu

* Hedeflediğiniz olun **.NET Framework 4.6.1** veya üzeri

* Projeye bir ad verin ve tıklayın **Tamam**

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu izlenecek yol, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.

Varolan bir veritabanından tersine mühendislik etkinleştirmek için biz çok birkaç diğer paketleri yüklemeniz gerekir.

* `Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.

## <a name="reverse-engineer-your-model"></a>Modelinizi tersine mühendislik

Artık, mevcut bir veritabanını temel alan EF modeli oluşturma zamanı geldi.

* Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu

* Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Ters mühendislik işlemi, varlık sınıfları ve var olan veritabanı şemasını temel alan türetilmiş bir bağlam oluşturuldu. Varlık sınıfları, sorgulama ve kaydetme verilerini temsil eden basit C# nesneleridir.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

İçerik veritabanı ile bir oturumu temsil eder ve sorgu ve varlık sınıflarının örneklerini kaydetmenize olanak tanır.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Modelinizi kullanın

Veri erişimi gerçekleştirdiği modeliniz artık kullanabilirsiniz.

* Açık *Program.cs*

* Dosyanın içeriğini aşağıdaki kodla değiştirin.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Hata ayıklama > hata ayıklama olmadan Başlat

Bir blog veritabanına kaydedilir ve daha sonra tüm blogları ayrıntılarını konsola yazdırılır görürsünüz.

![görüntü](_static/output-existing-db.png)
