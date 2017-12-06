---
title: ".NET Framework - yeni veritabanı - EF Çekirdeğinde Başlarken"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>.NET Framework ile yeni bir veritabanı EF Çekirdeğinde ile çalışmaya başlama

Bu kılavuzda, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturacaksınız. Geçişler, modelden veritabanı oluşturmak için kullanır.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) github'da.

## <a name="prerequisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [NuGet Paket Yöneticisi'nin en son sürümü](https://dist.nuget.org/index.html)

* [Windows PowerShell'in en son sürümünü](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Açık Visual Studio

* Dosya > Yeni > Proje...

* Sol menüden şablonları seçin > Visual C# > Klasik Windows Masaüstü

* Seçin **konsol uygulaması (.NET Framework)** proje şablonu

* Hedefleme olun **.NET Framework 4.5.1** veya daha yenisi

* Proje bir ad verin ve tıklatın **Tamam**

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin. Bu kılavuz, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).

* Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu

* Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Bu kılavuzda daha sonra biz de bazı Entity Framework Araçları veritabanını korumak için kullanır. Böylece biz de araçları paketini yükler.

* Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-your-model"></a>Model oluşturma

Şimdi modelinizi yapmak bağlamını ve varlık sınıflarını tanımlamak için zaman yapılır.

* Proje > sınıfı Ekle...

* Girin *Model.cs* tıklatın ve adı olarak **Tamam**

* Dosyasının içeriğini aşağıdaki kodla değiştirin

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

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

> [!TIP]  
> Gerçek bir uygulamada siz her sınıf ayrı bir dosyaya koymak ve bağlantı dizesini koyun `App.Config` dosya ve kullanılarak okunan `ConfigurationManager`. Basitleştirmek amacıyla, biz her şey tek kod dosyasında Bu öğretici için koyduğunuz.

## <a name="create-your-database"></a>Veritabanı oluşturma

Bir model sahip olduğunuza göre sizin için bir veritabanı oluşturmak için geçiş kullanabilirsiniz.

* Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu

* Çalıştırma `Add-Migration MyFirstMigration` tabloları modeliniz için ilk kümesi oluşturmak için bir geçiş için iskele kurmak.

* Çalıştırma `Update-Database` veritabanına yeni geçiş uygulanacak. Veritabanınız henüz var olmadığı için geçiş uygulanmadan önce onu sizin için oluşturulur.

> [!TIP]  
> Modelinize gelecekteki değişiklikler yapmak isterseniz, kullanabileceğiniz `Add-Migration` karşılık gelen şema yapmak için yeni bir geçiş için iskele kurmak komut veritabanına değiştirir. Gerekli kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `Update-Database` veritabanına değişiklikleri uygulamak için komutu.
>
>EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulandı izlemek için veritabanı tablosunda.

## <a name="use-your-model"></a>Modelinizi kullanın

Veri erişimi gerçekleştirdiği modelinizi artık kullanabilirsiniz.

* Açık *Program.cs*

* Dosyasının içeriğini aşağıdaki kodla değiştirin

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
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

![görüntü](_static/output-new-db.png)
