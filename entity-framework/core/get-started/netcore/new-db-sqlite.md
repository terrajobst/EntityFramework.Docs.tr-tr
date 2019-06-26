---
title: Üzerinde .NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
description: .NET Core kullanarak Entity Framework Core ile çalışmaya başlama
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e6996630e399659807d23304993c8e19c11ca6f5
ms.sourcegitcommit: 83c1e2fc034e5eb1fec1ebabc8d629ffcc7c0632
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67351333"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="9c265-103">EF Core üzerinde .NET Core konsol uygulaması ile yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9c265-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="9c265-104">Bu öğreticide, Entity Framework Core kullanan bir SQLite veritabanında veri erişimi gerçekleştirdiği bir .NET Core konsol uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9c265-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="9c265-105">Kullandığınız [geçişler](xref:core/managing-schemas/migrations/index) modelden veritabanını oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9c265-105">You use [migrations](xref:core/managing-schemas/migrations/index) to create the database from the model.</span></span> <span data-ttu-id="9c265-106">Bkz: [ASP.NET Core - yeni veritabanı](xref:core/get-started/aspnetcore/new-db) ASP.NET Core MVC kullanarak bir Visual Studio sürümü için.</span><span class="sxs-lookup"><span data-stu-id="9c265-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="9c265-107">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="9c265-107">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c265-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9c265-108">Prerequisites</span></span>

* [<span data-ttu-id="9c265-109">.NET Core 2.1 SDK'sı</span><span class="sxs-lookup"><span data-stu-id="9c265-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="9c265-110">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="9c265-110">Create a new project</span></span>

* <span data-ttu-id="9c265-111">Yeni bir konsol projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9c265-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="9c265-112">Geçerli dizin değiştirme</span><span class="sxs-lookup"><span data-stu-id="9c265-112">Change the current directory</span></span>

<span data-ttu-id="9c265-113">Verilecek ihtiyacımız sonraki adımlarda `dotnet` uygulama komutları.</span><span class="sxs-lookup"><span data-stu-id="9c265-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="9c265-114">Biz bu gibi uygulamanın dizinine geçerli dizine değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9c265-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="9c265-115">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="9c265-115">Install Entity Framework Core</span></span>

<span data-ttu-id="9c265-116">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9c265-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="9c265-117">Bu izlenecek yolda SQLite kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c265-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="9c265-118">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9c265-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="9c265-119">Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.Design yükleyin</span><span class="sxs-lookup"><span data-stu-id="9c265-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="9c265-120">Çalıştırma `dotnet restore` yeni paketlerini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9c265-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9c265-121">Modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="9c265-121">Create the model</span></span>

<span data-ttu-id="9c265-122">Modelinizi olun bağlamını ve varlık sınıfları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9c265-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="9c265-123">Yeni bir *Model.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="9c265-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="9c265-124">İpucu: Gerçek bir uygulamada ayrı bir dosyada her sınıf koyun ve bağlantı dizesini bir yapılandırma dosyası veya ortam değişkeninde yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="9c265-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="9c265-125">Bu öğreticide basit tutmak için her şeyi bir dosyada yer alır.</span><span class="sxs-lookup"><span data-stu-id="9c265-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="9c265-126">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9c265-126">Create the database</span></span>

<span data-ttu-id="9c265-127">Bir modeli oluşturduktan sonra kullandığınız [geçişler](xref:core/managing-schemas/migrations/index) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9c265-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="9c265-128">Çalıştırma `dotnet ef migrations add InitialCreate` geçiş iskelesini ve tablo modeli için başlangıç kümesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9c265-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="9c265-129">Çalıştırma `dotnet ef database update` veritabanına yeni geçiş uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="9c265-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="9c265-130">Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9c265-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="9c265-131">*Blogging.db* SQLite DB proje dizininde olduğu.</span><span class="sxs-lookup"><span data-stu-id="9c265-131">The *blogging.db* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="9c265-132">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="9c265-132">Use the model</span></span>

* <span data-ttu-id="9c265-133">Açık *Program.cs* ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9c265-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="9c265-134">Konsolundan uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="9c265-134">Test the app from the console.</span></span> <span data-ttu-id="9c265-135">Bkz: [Visual Studio Not](#vs) uygulamayı Visual Studio'dan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="9c265-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="9c265-136">Bir blog veritabanına kaydedilir ve tüm blogları ayrıntılarını konsolda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9c265-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="9c265-137">Model değiştiriliyor:</span><span class="sxs-lookup"><span data-stu-id="9c265-137">Changing the model:</span></span>

- <span data-ttu-id="9c265-138">Modele değişiklik yaparsanız, kullanabileceğiniz `dotnet ef migrations add` yeni iskele komut [geçiş](xref:core/managing-schemas/migrations/index).</span><span class="sxs-lookup"><span data-stu-id="9c265-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="9c265-139">Gerekli iskele kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `dotnet ef database update` şema uygulamak için komut, veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="9c265-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="9c265-140">EF Core kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="9c265-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="9c265-141">SQLite veritabanı altyapısı, çoğu ilişkisel veritabanı tarafından desteklenen belirli şema değişiklikleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="9c265-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="9c265-142">Örneğin, `DropColumn` işlemi desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="9c265-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="9c265-143">EF Core geçişleri bu işlemler için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9c265-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="9c265-144">Ancak, bunları bir veritabanı için geçerli veya bir komut dosyası oluşturmayı denerseniz, EF Core özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9c265-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="9c265-145">Bkz: [SQLite sınırlamaları](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="9c265-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="9c265-146">Yeni geliştirme projeleri için veritabanı bırakmadan yeni bir tane oluşturmak yerine ve model değiştiğinde migrations'ı kullanma göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="9c265-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="9c265-147">Visual Studio'dan çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9c265-147">Run from Visual Studio</span></span>

<span data-ttu-id="9c265-148">Visual Studio'dan Bu örneği çalıştırmak için çalışma dizini, proje kökündeki el ile olarak ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9c265-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="9c265-149">Çalışma dizini, aşağıdaki ayarlamazsanız `Microsoft.Data.Sqlite.SqliteException` oluşturulur: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="9c265-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="9c265-150">Çalışma dizini ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="9c265-150">To set the working directory:</span></span>

* <span data-ttu-id="9c265-151">İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="9c265-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="9c265-152">Seçin **hata ayıklama** sol bölmede sekme.</span><span class="sxs-lookup"><span data-stu-id="9c265-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="9c265-153">Ayarlama **çalışma dizini** proje dizininin.</span><span class="sxs-lookup"><span data-stu-id="9c265-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="9c265-154">Değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9c265-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c265-155">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9c265-155">Additional Resources</span></span>

* [<span data-ttu-id="9c265-156">Öğretici: EF çekirdekli ASP.NET Core üzerinde SQLite kullanarak yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9c265-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="9c265-157">Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9c265-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="9c265-158">Öğretici: ASP.NET Core, Entity Framework Core ile Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="9c265-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
