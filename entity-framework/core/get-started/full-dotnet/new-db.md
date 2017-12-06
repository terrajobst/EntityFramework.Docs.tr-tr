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
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="952a4-102">.NET Framework ile yeni bir veritabanı EF Çekirdeğinde ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="952a4-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="952a4-103">Bu kılavuzda, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="952a4-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="952a4-104">Geçişler, modelden veritabanı oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="952a4-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="952a4-105">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) github'da.</span><span class="sxs-lookup"><span data-stu-id="952a4-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="952a4-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="952a4-106">Prerequisites</span></span>

<span data-ttu-id="952a4-107">Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:</span><span class="sxs-lookup"><span data-stu-id="952a4-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="952a4-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="952a4-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="952a4-109">NuGet Paket Yöneticisi'nin en son sürümü</span><span class="sxs-lookup"><span data-stu-id="952a4-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="952a4-110">Windows PowerShell'in en son sürümünü</span><span class="sxs-lookup"><span data-stu-id="952a4-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="952a4-111">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="952a4-111">Create a new project</span></span>

* <span data-ttu-id="952a4-112">Açık Visual Studio</span><span class="sxs-lookup"><span data-stu-id="952a4-112">Open Visual Studio</span></span>

* <span data-ttu-id="952a4-113">Dosya > Yeni > Proje...</span><span class="sxs-lookup"><span data-stu-id="952a4-113">File > New > Project...</span></span>

* <span data-ttu-id="952a4-114">Sol menüden şablonları seçin > Visual C# > Klasik Windows Masaüstü</span><span class="sxs-lookup"><span data-stu-id="952a4-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="952a4-115">Seçin **konsol uygulaması (.NET Framework)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="952a4-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="952a4-116">Hedefleme olun **.NET Framework 4.5.1** veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="952a4-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="952a4-117">Proje bir ad verin ve tıklatın **Tamam**</span><span class="sxs-lookup"><span data-stu-id="952a4-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="952a4-118">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="952a4-118">Install Entity Framework</span></span>

<span data-ttu-id="952a4-119">EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="952a4-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="952a4-120">Bu kılavuz, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="952a4-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="952a4-121">Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="952a4-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="952a4-122">Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="952a4-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="952a4-123">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="952a4-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="952a4-124">Bu kılavuzda daha sonra biz de bazı Entity Framework Araçları veritabanını korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="952a4-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="952a4-125">Böylece biz de araçları paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="952a4-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="952a4-126">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="952a4-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="952a4-127">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="952a4-127">Create your model</span></span>

<span data-ttu-id="952a4-128">Şimdi modelinizi yapmak bağlamını ve varlık sınıflarını tanımlamak için zaman yapılır.</span><span class="sxs-lookup"><span data-stu-id="952a4-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="952a4-129">Proje > sınıfı Ekle...</span><span class="sxs-lookup"><span data-stu-id="952a4-129">Project > Add Class...</span></span>

* <span data-ttu-id="952a4-130">Girin *Model.cs* tıklatın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="952a4-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="952a4-131">Dosyasının içeriğini aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="952a4-131">Replace the contents of the file with the following code</span></span>

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
> <span data-ttu-id="952a4-132">Gerçek bir uygulamada siz her sınıf ayrı bir dosyaya koymak ve bağlantı dizesini koyun `App.Config` dosya ve kullanılarak okunan `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="952a4-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="952a4-133">Basitleştirmek amacıyla, biz her şey tek kod dosyasında Bu öğretici için koyduğunuz.</span><span class="sxs-lookup"><span data-stu-id="952a4-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="952a4-134">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="952a4-134">Create your database</span></span>

<span data-ttu-id="952a4-135">Bir model sahip olduğunuza göre sizin için bir veritabanı oluşturmak için geçiş kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="952a4-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="952a4-136">Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="952a4-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="952a4-137">Çalıştırma `Add-Migration MyFirstMigration` tabloları modeliniz için ilk kümesi oluşturmak için bir geçiş için iskele kurmak.</span><span class="sxs-lookup"><span data-stu-id="952a4-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="952a4-138">Çalıştırma `Update-Database` veritabanına yeni geçiş uygulanacak.</span><span class="sxs-lookup"><span data-stu-id="952a4-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="952a4-139">Veritabanınız henüz var olmadığı için geçiş uygulanmadan önce onu sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="952a4-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="952a4-140">Modelinize gelecekteki değişiklikler yapmak isterseniz, kullanabileceğiniz `Add-Migration` karşılık gelen şema yapmak için yeni bir geçiş için iskele kurmak komut veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="952a4-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="952a4-141">Gerekli kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `Update-Database` veritabanına değişiklikleri uygulamak için komutu.</span><span class="sxs-lookup"><span data-stu-id="952a4-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="952a4-142">EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulandı izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="952a4-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="952a4-143">Modelinizi kullanın</span><span class="sxs-lookup"><span data-stu-id="952a4-143">Use your model</span></span>

<span data-ttu-id="952a4-144">Veri erişimi gerçekleştirdiği modelinizi artık kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="952a4-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="952a4-145">Açık *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="952a4-145">Open *Program.cs*</span></span>

* <span data-ttu-id="952a4-146">Dosyasının içeriğini aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="952a4-146">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="952a4-147">Hata ayıklama > hata ayıklama olmadan Başlat</span><span class="sxs-lookup"><span data-stu-id="952a4-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="952a4-148">Bir blog veritabanına kaydedilir ve ardından tüm bloglar ayrıntılarını konsola yazdırılır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="952a4-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![görüntü](_static/output-new-db.png)
