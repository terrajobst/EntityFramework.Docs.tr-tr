---
title: Üzerinde .NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: .NET Core kullanarak Entity Framework Core ile çalışmaya başlama
keywords: .NET core, Entity Framework Core, VS kodu, Visual Studio kodu, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911508"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="e5968-104">EF Core üzerinde .NET Core konsol uygulaması ile yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e5968-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="e5968-105">Bu kılavuzda, Entity Framework Core kullanan bir SQLite veritabanında veri erişimi gerçekleştirdiği bir .NET Core konsol uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="e5968-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="e5968-106">Modelden veritabanı oluşturmaya geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5968-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="e5968-107">Bkz: [ASP.NET Core - yeni veritabanı](xref:core/get-started/aspnetcore/new-db) ASP.NET Core MVC kullanarak bir Visual Studio sürümü için.</span><span class="sxs-lookup"><span data-stu-id="e5968-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="e5968-108">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="e5968-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5968-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e5968-109">Prerequisites</span></span>

<span data-ttu-id="e5968-110">[.NET Core SDK'sı](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="e5968-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="e5968-111">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5968-111">Create a new project</span></span>

* <span data-ttu-id="e5968-112">Yeni bir konsol projesi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="e5968-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="e5968-113">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="e5968-113">Install Entity Framework Core</span></span>

<span data-ttu-id="e5968-114">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e5968-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="e5968-115">Bu izlenecek yolda SQLite kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5968-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="e5968-116">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e5968-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="e5968-117">Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.Design yükleyin</span><span class="sxs-lookup"><span data-stu-id="e5968-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="e5968-118">Çalıştırma `dotnet restore` yeni paketlerini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e5968-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="e5968-119">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5968-119">Create the model</span></span>

<span data-ttu-id="e5968-120">Modelinizi olun bağlamını ve varlık sınıfları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e5968-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="e5968-121">Yeni bir *Model.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="e5968-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="e5968-122">İpucu: Gerçek bir uygulamada, ayrı bir dosyada her sınıf koyun ve bağlantı dizesini yapılandırma dosyası yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="e5968-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="e5968-123">Bu öğreticide basit tutmak için her şeyi bir dosyada yer alır.</span><span class="sxs-lookup"><span data-stu-id="e5968-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e5968-124">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5968-124">Create the database</span></span>

<span data-ttu-id="e5968-125">Bir modeli oluşturduktan sonra kullandığınız [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e5968-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="e5968-126">Çalıştırma `dotnet ef migrations add InitialCreate` geçiş iskelesini ve tablo modeli için başlangıç kümesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e5968-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="e5968-127">Çalıştırma `dotnet ef database update` veritabanına yeni geçiş uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="e5968-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="e5968-128">Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5968-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="e5968-129">*Blogging.db*\* SQLite DB proje dizininde olduğu.</span><span class="sxs-lookup"><span data-stu-id="e5968-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="e5968-130">Modelinizi kullanın</span><span class="sxs-lookup"><span data-stu-id="e5968-130">Use your model</span></span>

* <span data-ttu-id="e5968-131">Açık *Program.cs* ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e5968-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="e5968-132">Uygulamayı test edin:</span><span class="sxs-lookup"><span data-stu-id="e5968-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="e5968-133">Bir blog veritabanına kaydedilir ve tüm blogları ayrıntılarını konsolda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e5968-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="e5968-134">Model değiştiriliyor:</span><span class="sxs-lookup"><span data-stu-id="e5968-134">Changing the model:</span></span>

- <span data-ttu-id="e5968-135">Modelinize değişiklik yaparsanız, kullanabileceğiniz `dotnet ef migrations add` yeni iskele komut [geçiş](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) karşılık gelen şema değişikliklerini veritabanına.</span><span class="sxs-lookup"><span data-stu-id="e5968-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="e5968-136">Gerekli iskele kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `dotnet ef database update` veritabanına değişiklikleri uygulamak için komutu.</span><span class="sxs-lookup"><span data-stu-id="e5968-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="e5968-137">EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="e5968-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="e5968-138">SQLite sınırlamaları nedeniyle tüm geçişler (şema değişiklikleri) içinde SQLite desteklemez.</span><span class="sxs-lookup"><span data-stu-id="e5968-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="e5968-139">Bkz: [SQLite sınırlamaları](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="e5968-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="e5968-140">Yeni geliştirme projeleri için veritabanı bırakmadan yeni bir tane oluşturmak yerine ve modelinizi değiştiğinde migrations'ı kullanma göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="e5968-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5968-141">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e5968-141">Additional Resources</span></span>

* <span data-ttu-id="e5968-142">[.NET core - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.</span><span class="sxs-lookup"><span data-stu-id="e5968-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="e5968-143">MVC Mac veya Linux'ta ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="e5968-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="e5968-144">Visual Studio ile MVC ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="e5968-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="e5968-145">Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e5968-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
