---
title: .NET Framework - var olan veritabanı - EF Çekirdeğinde Başlarken
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812631"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>.NET Framework ile varolan bir veritabanını temel EF ile çalışmaya başlama

Bu kılavuzda, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturacaksınız. Varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için ters mühendislik kullanır.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) github'da.

## <a name="prerequisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [NuGet Paket Yöneticisi'nin en son sürümü](https://dist.nuget.org/index.html)

* [Windows PowerShell'in en son sürümünü](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Blog veritabanı](#blogging-database)

### <a name="blogging-database"></a>Blog veritabanı

Bu öğretici kullanan bir **blog** veritabanı, yerel veritabanı örneğinde var olan veritabanı olarak.

> [!TIP]  
> Zaten oluşturduysanız **blog** veritabanı başka bir öğretici bir parçası olarak, bu adımı atlayabilirsiniz.

* Açık Visual Studio

* Araçlar > veritabanına bağlan...

* Seçin **Microsoft SQL Server** tıklatıp **devam et**

* Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**

* Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**

* Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**

* Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**

* Aşağıda, sorgu Düzenleyicisi'ne komut dosyasını kopyalayın

* Üzerinde sorgu Düzenleyicisi'ni sağ tıklatın ve seçin **çalıştırma**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Açık Visual Studio

* Dosya > Yeni > Proje...

* Sol menüden şablonları seçin > Visual C# > Windows

* Seçin **konsol uygulaması** proje şablonu

* Hedefleme olun **.NET Framework 4.5.1** veya daha yenisi

* Proje bir ad verin ve tıklatın **Tamam**

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin. Bu kılavuz, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).

* Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.

Varolan bir veritabanından ters mühendislik etkinleştirmek için sizi birkaç diğer paketleri çok yüklemeniz gerekir.

* `Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.

## <a name="reverse-engineer-your-model"></a>Tersine mühendislik modelinizi

Şimdi, varolan bir veritabanını temel alan EF modeli oluşturmak için zaman yapılır.

* Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu

* Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Ters mühendislik işlemi oluşturulan sınıflar ve var olan veritabanı şemasını temel alan bir türetilmiş bağlamı. Sınıflar, sorgulama kaydetme ve verilerini temsil eden basit C# nesneleridir.

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

Bağlam veritabanı oturumla temsil eder ve sorgu ve varlık sınıfları kaydetmenize olanak sağlar.

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

Veri erişimi gerçekleştirdiği modelinizi artık kullanabilirsiniz.

* Açık *Program.cs*

* Dosyasının içeriğini aşağıdaki kodla değiştirin

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

Bir blog veritabanına kaydedilir ve ardından tüm bloglar ayrıntılarını konsola yazdırılır görürsünüz.

![görüntü](_static/output-existing-db.png)
