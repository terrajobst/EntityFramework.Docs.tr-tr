---
title: ".NET Core - yeni veritabanı - EF Çekirdeğinde Başlarken"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: "Entity Framework Çekirdek kullanarak .NET Core'u kullanmaya başlama"
keywords: ".NET core, Entity Framework Çekirdek, VS kodu, Visual Studio kodu, Mac, Linux"
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="35e2c-104">.NET Core konsol uygulaması EF Çekirdeğinde ile yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="35e2c-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="35e2c-105">Bu kılavuzda, Entity Framework Çekirdek kullanarak bir SQLite veritabanı karşı temel veri erişimi gerçekleştirdiği bir .NET Core konsol uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="35e2c-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="35e2c-106">Geçişler, modelden veritabanı oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="35e2c-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="35e2c-107">Bkz: [ASP.NET Core - yeni veritabanı](xref:core/get-started/aspnetcore/new-db) ASP.NET Core MVC kullanarak bir Visual Studio sürümü.</span><span class="sxs-lookup"><span data-stu-id="35e2c-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!NOTE]  
> <span data-ttu-id="35e2c-108">[.NET Core SDK](https://www.microsoft.com/net/download/core) artık `project.json` veya Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="35e2c-108">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="35e2c-109">Öneririz [project.json için csproj geçirmek](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="35e2c-109">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="35e2c-110">Visual Studio kullanıyorsanız, geçiş için öneririz [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="35e2c-110">If you are using Visual Studio, we recommend you migrate to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

> [!TIP]  
> <span data-ttu-id="35e2c-111">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) github'da.</span><span class="sxs-lookup"><span data-stu-id="35e2c-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35e2c-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="35e2c-112">Prerequisites</span></span>

<span data-ttu-id="35e2c-113">Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:</span><span class="sxs-lookup"><span data-stu-id="35e2c-113">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="35e2c-114">.NET Core destekleyen bir işletim sistemi.</span><span class="sxs-lookup"><span data-stu-id="35e2c-114">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="35e2c-115">[.NET Core SDK](https://www.microsoft.com/net/core) 2.0 (yönergeleri çok az değişiklik yapılması açısından önceki bir sürümüyle bir uygulama oluşturmak için kullanılır ancak kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="35e2c-115">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="35e2c-116">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="35e2c-116">Create a new project</span></span>

* <span data-ttu-id="35e2c-117">Yeni bir `ConsoleApp.SQLite` projenizi ve kullanmak için klasör `dotnet` .NET Core uygulamayla doldurmak için komutu.</span><span class="sxs-lookup"><span data-stu-id="35e2c-117">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="35e2c-118">Entity Framework Çekirdek yükleyin</span><span class="sxs-lookup"><span data-stu-id="35e2c-118">Install Entity Framework Core</span></span>

<span data-ttu-id="35e2c-119">EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="35e2c-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="35e2c-120">Bu kılavuzda SQLite kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35e2c-120">This walkthrough uses SQLite.</span></span> <span data-ttu-id="35e2c-121">Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="35e2c-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="35e2c-122">Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.Design yükleyin</span><span class="sxs-lookup"><span data-stu-id="35e2c-122">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="35e2c-123">El ile düzenleme `ConsoleApp.SQLite.csproj` Microsoft.EntityFrameworkCore.Tools.DotNet için bir DotNetCliToolReference eklemek için:</span><span class="sxs-lookup"><span data-stu-id="35e2c-123">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 <span data-ttu-id="35e2c-124">Not: Sonraki bir sürümünde `dotnet` aracılığıyla DotNetCliToolReferences destekler`dotnet add tool`</span><span class="sxs-lookup"><span data-stu-id="35e2c-124">Note: A future version of `dotnet` will support DotNetCliToolReferences via `dotnet add tool`</span></span>

<span data-ttu-id="35e2c-125">`ConsoleApp.SQLite.csproj`Şimdi aşağıdakileri içermelidir:</span><span class="sxs-lookup"><span data-stu-id="35e2c-125">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="35e2c-126">Not: Yukarıdaki kullanılan sürüm numaralarını yayımlama aynı anda doğru.</span><span class="sxs-lookup"><span data-stu-id="35e2c-126">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="35e2c-127">Çalıştırma `dotnet restore` yeni paketleri yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="35e2c-127">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="35e2c-128">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="35e2c-128">Create the model</span></span>

<span data-ttu-id="35e2c-129">Modelinizi olun bağlamını ve varlık sınıfları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="35e2c-129">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="35e2c-130">Yeni bir *Model.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="35e2c-130">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="35e2c-131">İpucu: Gerçek bir uygulamada, her sınıf ayrı bir dosyaya koymak ve bağlantı dizesini yapılandırma dosyasındaki yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="35e2c-131">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="35e2c-132">Öğretici basit tutmak için biz her şeyi bir dosyaya koymak.</span><span class="sxs-lookup"><span data-stu-id="35e2c-132">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="35e2c-133">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="35e2c-133">Create the database</span></span>

<span data-ttu-id="35e2c-134">Bir modeli eğittikten sonra kullanabileceğiniz [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="35e2c-134">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="35e2c-135">Çalıştırma `dotnet ef migrations add InitialCreate` bir geçişin iskelesini kurun ve tablo modeli için ilk kümesini oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="35e2c-135">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="35e2c-136">Çalıştırma `dotnet ef database update` veritabanına yeni geçiş uygulanacak.</span><span class="sxs-lookup"><span data-stu-id="35e2c-136">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="35e2c-137">Bu komut, geçişler uygulamadan önce veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35e2c-137">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="35e2c-138">Göreli yollar SQLite ile kullanırken, uygulamanın ana derleme göreli yol olacaktır.</span><span class="sxs-lookup"><span data-stu-id="35e2c-138">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="35e2c-139">Bu örnekte, ana ikili dosyadır `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, SQLite veritabanı olacak şekilde `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="35e2c-139">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="35e2c-140">Modelinizi kullanın</span><span class="sxs-lookup"><span data-stu-id="35e2c-140">Use your model</span></span>

* <span data-ttu-id="35e2c-141">Açık *Program.cs* ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="35e2c-141">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="35e2c-142">Uygulamayı test etme:</span><span class="sxs-lookup"><span data-stu-id="35e2c-142">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="35e2c-143">Bir blog veritabanına kaydedilir ve tüm Web günlüklerini ayrıntılarını konsolunda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="35e2c-143">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="35e2c-144">Model değiştirme:</span><span class="sxs-lookup"><span data-stu-id="35e2c-144">Changing the model:</span></span>

- <span data-ttu-id="35e2c-145">Modelinize değişiklik yaparsanız, kullanabileceğiniz `dotnet ef migrations add` yeni iskele komutu [geçiş](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) karşılık gelen şema değişikliklerini veritabanına.</span><span class="sxs-lookup"><span data-stu-id="35e2c-145">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="35e2c-146">Gerekli kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `dotnet ef database update` veritabanına değişiklikleri uygulamak için komutu.</span><span class="sxs-lookup"><span data-stu-id="35e2c-146">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="35e2c-147">EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulandı izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="35e2c-147">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="35e2c-148">SQLite içinde SQLite sınırlamaları nedeniyle tüm geçişler (şema değişiklikleri) desteklemez.</span><span class="sxs-lookup"><span data-stu-id="35e2c-148">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="35e2c-149">Bkz: [SQLite sınırlamalar](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="35e2c-149">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="35e2c-150">Yeni geliştirme projeleri için veritabanını silmek ve yeni bir tane oluşturmak yerine modelinizi değiştiğinde geçişleri kullanmaya göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="35e2c-150">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35e2c-151">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="35e2c-151">Additional Resources</span></span>

* <span data-ttu-id="35e2c-152">[.NET core - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.</span><span class="sxs-lookup"><span data-stu-id="35e2c-152">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="35e2c-153">ASP.NET Core Mac veya Linux MVC giriş</span><span class="sxs-lookup"><span data-stu-id="35e2c-153">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="35e2c-154">ASP.NET Core Visual Studio ile MVC giriş</span><span class="sxs-lookup"><span data-stu-id="35e2c-154">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="35e2c-155">ASP.NET Core ve Entity Framework Visual Studio kullanarak çekirdek ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="35e2c-155">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
