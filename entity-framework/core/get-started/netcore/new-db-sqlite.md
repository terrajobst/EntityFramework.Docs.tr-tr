---
title: Üzerinde .NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
description: .NET Core kullanarak Entity Framework Core ile çalışmaya başlama
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: b30800afb63a51ab14aecb559dcc83fd89f71a71
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415776"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="9dfc2-103">EF Core üzerinde .NET Core konsol uygulaması ile yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9dfc2-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="9dfc2-104">Bu öğreticide, Entity Framework Core kullanan bir SQLite veritabanında veri erişimi gerçekleştirdiği bir .NET Core konsol uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="9dfc2-105">Modelden veritabanı oluşturmaya geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="9dfc2-106">Bkz: [ASP.NET Core - yeni veritabanı](xref:core/get-started/aspnetcore/new-db) ASP.NET Core MVC kullanarak bir Visual Studio sürümü için.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="9dfc2-107">Bu makaledeki örnek Github'da görüntüleyin] (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="9dfc2-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9dfc2-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9dfc2-108">Prerequisites</span></span>

* [<span data-ttu-id="9dfc2-109">.NET Core 2.1 SDK'sı</span><span class="sxs-lookup"><span data-stu-id="9dfc2-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="9dfc2-110">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="9dfc2-110">Create a new project</span></span>

* <span data-ttu-id="9dfc2-111">Yeni bir konsol projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9dfc2-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="9dfc2-112">Geçerli dizin değiştirme</span><span class="sxs-lookup"><span data-stu-id="9dfc2-112">Change the current directory</span></span> 

<span data-ttu-id="9dfc2-113">Verilecek ihtiyacımız sonraki adımlarda `dotnet` uygulama komutları.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span> 

* <span data-ttu-id="9dfc2-114">Biz bu gibi uygulamanın dizinine geçerli dizine değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9dfc2-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="9dfc2-115">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="9dfc2-115">Install Entity Framework Core</span></span>

<span data-ttu-id="9dfc2-116">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="9dfc2-117">Bu izlenecek yolda SQLite kullanır.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="9dfc2-118">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9dfc2-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="9dfc2-119">Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.Design yükleyin</span><span class="sxs-lookup"><span data-stu-id="9dfc2-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="9dfc2-120">Çalıştırma `dotnet restore` yeni paketlerini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9dfc2-121">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="9dfc2-121">Create the model</span></span>

<span data-ttu-id="9dfc2-122">Modelinizi olun bağlamını ve varlık sınıfları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="9dfc2-123">Yeni bir *Model.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="9dfc2-124">İpucu: Gerçek bir uygulamada, ayrı bir dosyada her sınıf koyun ve bağlantı dizesini bir yapılandırma dosyası veya ortam değişkeninde yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="9dfc2-125">Bu öğreticide basit tutmak için her şeyi bir dosyada yer alır.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="9dfc2-126">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9dfc2-126">Create the database</span></span>

<span data-ttu-id="9dfc2-127">Bir modeli oluşturduktan sonra kullandığınız [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-127">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="9dfc2-128">Çalıştırma `dotnet ef migrations add InitialCreate` geçiş iskelesini ve tablo modeli için başlangıç kümesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="9dfc2-129">Çalıştırma `dotnet ef database update` veritabanına yeni geçiş uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="9dfc2-130">Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="9dfc2-131">*Blogging.db*\* SQLite DB proje dizininde olduğu.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-131">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="9dfc2-132">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="9dfc2-132">Use the model</span></span>

* <span data-ttu-id="9dfc2-133">Açık *Program.cs* ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9dfc2-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="9dfc2-134">Konsolundan uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-134">Test the app from the console.</span></span> <span data-ttu-id="9dfc2-135">Bkz: [Visual Studio Not](#vs) uygulamayı Visual Studio'dan çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="9dfc2-136">Bir blog veritabanına kaydedilir ve tüm blogları ayrıntılarını konsolda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="9dfc2-137">Model değiştiriliyor:</span><span class="sxs-lookup"><span data-stu-id="9dfc2-137">Changing the model:</span></span>

- <span data-ttu-id="9dfc2-138">Modele değişiklik yaparsanız, kullanabileceğiniz `dotnet ef migrations add` yeni iskele komut [geçiş](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="9dfc2-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="9dfc2-139">Gerekli iskele kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `dotnet ef database update` şema uygulamak için komut, veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="9dfc2-140">EF Core kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="9dfc2-141">SQLite veritabanı altyapısı, çoğu ilişkisel veritabanı tarafından desteklenen belirli şema değişiklikleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="9dfc2-142">Örneğin, `DropColumn` işlemi desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="9dfc2-143">EF Core geçişleri bu işlemler için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="9dfc2-144">Ancak, bunları bir veritabanı için geçerli veya bir komut dosyası oluşturmayı denerseniz, EF Core özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="9dfc2-145">Bkz: [SQLite sınırlamaları](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="9dfc2-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="9dfc2-146">Yeni geliştirme projeleri için veritabanı bırakmadan yeni bir tane oluşturmak yerine ve model değiştiğinde migrations'ı kullanma göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>
- 

<a name="vs"></a>

### <a name="run-from-visual-studio"></a><span data-ttu-id="9dfc2-147">Visual Studio'dan çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9dfc2-147">Run from Visual Studio</span></span>

<span data-ttu-id="9dfc2-148">Visual Studio'dan Bu örneği çalıştırmak için çalışma dizini, proje kökündeki el ile olarak ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="9dfc2-149">Çalışma dizini, aşağıdaki ayarlamazsanız `Microsoft.Data.Sqlite.SqliteException` oluşturulur: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="9dfc2-150">Çalışma dizini ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="9dfc2-150">To set the working directory:</span></span>

* <span data-ttu-id="9dfc2-151">İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="9dfc2-152">Seçin **hata ayıklama** sol bölmede sekme.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="9dfc2-153">Ayarlama **çalışma dizini** proje dizininin.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="9dfc2-154">Değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9dfc2-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9dfc2-155">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9dfc2-155">Additional Resources</span></span>

* [<span data-ttu-id="9dfc2-156">MVC Mac veya Linux'ta ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="9dfc2-156">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="9dfc2-157">Visual Studio ile MVC ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="9dfc2-157">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="9dfc2-158">Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9dfc2-158">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
