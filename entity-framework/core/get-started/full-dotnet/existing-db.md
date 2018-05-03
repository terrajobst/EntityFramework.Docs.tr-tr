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
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="86d66-102">.NET Framework ile varolan bir veritabanını temel EF ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="86d66-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="86d66-103">Bu kılavuzda, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="86d66-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="86d66-104">Varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için ters mühendislik kullanır.</span><span class="sxs-lookup"><span data-stu-id="86d66-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="86d66-105">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) github'da.</span><span class="sxs-lookup"><span data-stu-id="86d66-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86d66-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="86d66-106">Prerequisites</span></span>

<span data-ttu-id="86d66-107">Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:</span><span class="sxs-lookup"><span data-stu-id="86d66-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="86d66-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="86d66-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="86d66-109">NuGet Paket Yöneticisi'nin en son sürümü</span><span class="sxs-lookup"><span data-stu-id="86d66-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="86d66-110">Windows PowerShell'in en son sürümünü</span><span class="sxs-lookup"><span data-stu-id="86d66-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="86d66-111">Blog veritabanı</span><span class="sxs-lookup"><span data-stu-id="86d66-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="86d66-112">Blog veritabanı</span><span class="sxs-lookup"><span data-stu-id="86d66-112">Blogging database</span></span>

<span data-ttu-id="86d66-113">Bu öğretici kullanan bir **blog** veritabanı, yerel veritabanı örneğinde var olan veritabanı olarak.</span><span class="sxs-lookup"><span data-stu-id="86d66-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="86d66-114">Zaten oluşturduysanız **blog** veritabanı başka bir öğretici bir parçası olarak, bu adımı atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="86d66-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="86d66-115">Açık Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86d66-115">Open Visual Studio</span></span>

* <span data-ttu-id="86d66-116">Araçlar > veritabanına bağlan...</span><span class="sxs-lookup"><span data-stu-id="86d66-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="86d66-117">Seçin **Microsoft SQL Server** tıklatıp **devam et**</span><span class="sxs-lookup"><span data-stu-id="86d66-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="86d66-118">Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**</span><span class="sxs-lookup"><span data-stu-id="86d66-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="86d66-119">Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="86d66-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="86d66-120">Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="86d66-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="86d66-121">Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="86d66-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="86d66-122">Aşağıda, sorgu Düzenleyicisi'ne komut dosyasını kopyalayın</span><span class="sxs-lookup"><span data-stu-id="86d66-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="86d66-123">Üzerinde sorgu Düzenleyicisi'ni sağ tıklatın ve seçin **çalıştırma**</span><span class="sxs-lookup"><span data-stu-id="86d66-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="86d66-124">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="86d66-124">Create a new project</span></span>

* <span data-ttu-id="86d66-125">Açık Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86d66-125">Open Visual Studio</span></span>

* <span data-ttu-id="86d66-126">Dosya > Yeni > Proje...</span><span class="sxs-lookup"><span data-stu-id="86d66-126">File > New > Project...</span></span>

* <span data-ttu-id="86d66-127">Sol menüden şablonları seçin > Visual C# > Windows</span><span class="sxs-lookup"><span data-stu-id="86d66-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="86d66-128">Seçin **konsol uygulaması** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="86d66-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="86d66-129">Hedefleme olun **.NET Framework 4.5.1** veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="86d66-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="86d66-130">Proje bir ad verin ve tıklatın **Tamam**</span><span class="sxs-lookup"><span data-stu-id="86d66-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="86d66-131">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="86d66-131">Install Entity Framework</span></span>

<span data-ttu-id="86d66-132">EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="86d66-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="86d66-133">Bu kılavuz, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="86d66-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="86d66-134">Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="86d66-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="86d66-135">Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="86d66-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="86d66-136">`Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86d66-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="86d66-137">Varolan bir veritabanından ters mühendislik etkinleştirmek için sizi birkaç diğer paketleri çok yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="86d66-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="86d66-138">`Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="86d66-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="86d66-139">Tersine mühendislik modelinizi</span><span class="sxs-lookup"><span data-stu-id="86d66-139">Reverse engineer your model</span></span>

<span data-ttu-id="86d66-140">Şimdi, varolan bir veritabanını temel alan EF modeli oluşturmak için zaman yapılır.</span><span class="sxs-lookup"><span data-stu-id="86d66-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="86d66-141">Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="86d66-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="86d66-142">Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="86d66-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="86d66-143">Ters mühendislik işlemi oluşturulan sınıflar ve var olan veritabanı şemasını temel alan bir türetilmiş bağlamı.</span><span class="sxs-lookup"><span data-stu-id="86d66-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="86d66-144">Sınıflar, sorgulama kaydetme ve verilerini temsil eden basit C# nesneleridir.</span><span class="sxs-lookup"><span data-stu-id="86d66-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

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

<span data-ttu-id="86d66-145">Bağlam veritabanı oturumla temsil eder ve sorgu ve varlık sınıfları kaydetmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="86d66-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="use-your-model"></a><span data-ttu-id="86d66-146">Modelinizi kullanın</span><span class="sxs-lookup"><span data-stu-id="86d66-146">Use your model</span></span>

<span data-ttu-id="86d66-147">Veri erişimi gerçekleştirdiği modelinizi artık kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="86d66-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="86d66-148">Açık *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="86d66-148">Open *Program.cs*</span></span>

* <span data-ttu-id="86d66-149">Dosyasının içeriğini aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="86d66-149">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="86d66-150">Hata ayıklama > hata ayıklama olmadan Başlat</span><span class="sxs-lookup"><span data-stu-id="86d66-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="86d66-151">Bir blog veritabanına kaydedilir ve ardından tüm bloglar ayrıntılarını konsola yazdırılır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="86d66-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![görüntü](_static/output-existing-db.png)
