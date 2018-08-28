---
title: .NET Framework - yeni veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997593"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="906e9-102">.NET Framework ile yeni bir veritabanı üzerinde EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="906e9-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="906e9-103">Bu öğreticide, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="906e9-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="906e9-104">Bir modelden veritabanı oluşturmaya geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="906e9-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="906e9-105">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="906e9-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="906e9-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="906e9-106">Prerequisites</span></span>

* [<span data-ttu-id="906e9-107">Visual Studio 2017 sürüm 15.7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="906e9-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="906e9-108">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="906e9-108">Create a new project</span></span>

* <span data-ttu-id="906e9-109">Açık Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="906e9-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="906e9-110">**Dosya > Yeni > Proje...**</span><span class="sxs-lookup"><span data-stu-id="906e9-110">**File > New > Project...**</span></span>

* <span data-ttu-id="906e9-111">Sol menüden **yüklü > Visual C# > Windows Masaüstü**</span><span class="sxs-lookup"><span data-stu-id="906e9-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="906e9-112">Seçin **konsol uygulaması (.NET Framework)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="906e9-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="906e9-113">Emin olun projenizin hedeflediği **.NET Framework 4.6.1** veya üzeri</span><span class="sxs-lookup"><span data-stu-id="906e9-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="906e9-114">Projeyi adlandırın *ConsoleApp.NewDb* tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="906e9-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="906e9-115">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="906e9-115">Install Entity Framework</span></span>

<span data-ttu-id="906e9-116">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="906e9-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="906e9-117">Bu öğreticide, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="906e9-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="906e9-118">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="906e9-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="906e9-119">Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="906e9-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="906e9-120">`Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="906e9-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="906e9-121">Bu öğreticide daha sonra veritabanını korumak için bazı Entity Framework araçları kullanın.</span><span class="sxs-lookup"><span data-stu-id="906e9-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="906e9-122">Bu nedenle de araçları paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="906e9-122">So install the tools package as well.</span></span>

* <span data-ttu-id="906e9-123">`Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="906e9-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="906e9-124">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="906e9-124">Create the model</span></span>

<span data-ttu-id="906e9-125">Artık modeli oluşturan bir bağlam ve varlık sınıflarını tanımlamak için zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="906e9-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="906e9-126">**Proje > sınıfı Ekle...**</span><span class="sxs-lookup"><span data-stu-id="906e9-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="906e9-127">Girin *Model.cs* tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="906e9-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="906e9-128">Dosyanın içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="906e9-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="906e9-129">Gerçek bir uygulamada ayrı bir dosyada her sınıf koyun ve bağlantı dizesini bir yapılandırma dosyası veya ortam değişkeninde yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="906e9-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="906e9-130">Basitleştirmek amacıyla, her şey Bu öğretici için bir tek bir kod dosyasında tutmaktır.</span><span class="sxs-lookup"><span data-stu-id="906e9-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="906e9-131">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="906e9-131">Create the database</span></span>

<span data-ttu-id="906e9-132">Bir modeliniz olduğuna göre bir veritabanı oluşturmaya geçişleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="906e9-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="906e9-133">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="906e9-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="906e9-134">Çalıştırma `Add-Migration InitialCreate` tablo modeli için başlangıç kümesi oluşturmak için bir geçiş iskele.</span><span class="sxs-lookup"><span data-stu-id="906e9-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="906e9-135">Çalıştırma `Update-Database` veritabanına yeni geçiş uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="906e9-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="906e9-136">Veritabanı henüz mevcut olmadığından geçiş uygulanmadan önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="906e9-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="906e9-137">Modele değişiklik yaparsanız, kullanabileceğiniz `Add-Migration` karşılık gelen şema yapmak için yeni bir geçiş iskele komut, veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="906e9-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="906e9-138">Gerekli iskele kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `Update-Database` veritabanına değişiklikleri uygulamak için komutu.</span><span class="sxs-lookup"><span data-stu-id="906e9-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="906e9-139">EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="906e9-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="906e9-140">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="906e9-140">Use the model</span></span>

<span data-ttu-id="906e9-141">Şimdi, veri erişimi gerçekleştirdiği modeli kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="906e9-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="906e9-142">Açık *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="906e9-142">Open *Program.cs*</span></span>

* <span data-ttu-id="906e9-143">Dosyanın içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="906e9-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="906e9-144">**Hata ayıklama > hata ayıklama olmadan Başlat**</span><span class="sxs-lookup"><span data-stu-id="906e9-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="906e9-145">Bir blog veritabanına kaydedilir ve daha sonra tüm blogları ayrıntılarını konsola yazdırılır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="906e9-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![görüntü](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="906e9-147">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="906e9-147">Additional Resources</span></span>

* [<span data-ttu-id="906e9-148">Mevcut bir veritabanı ile .NET Framework üzerinde EF Core</span><span class="sxs-lookup"><span data-stu-id="906e9-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="906e9-149">[EF Core ile yeni bir veritabanı - SQLite .NET core'da](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.</span><span class="sxs-lookup"><span data-stu-id="906e9-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
