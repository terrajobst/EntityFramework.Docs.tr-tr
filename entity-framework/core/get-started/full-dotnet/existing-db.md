---
title: .NET Framework - mevcut veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: edcdc0b76394c4d604cf43fc170424e474532b17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993424"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="09292-102">.NET Framework ile varolan bir veritabanını EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="09292-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="09292-103">Bu öğreticide, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09292-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="09292-104">Bir Entity Framework modelini tarafından ters mühendislik mevcut bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09292-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="09292-105">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="09292-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09292-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="09292-106">Prerequisites</span></span>

* [<span data-ttu-id="09292-107">Visual Studio 2017 sürüm 15.7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="09292-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="09292-108">Günlük veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="09292-108">Create Blogging database</span></span>

<span data-ttu-id="09292-109">Bu öğreticide bir **blog** LocalDb örneğinde var olan veritabanının veritabanı.</span><span class="sxs-lookup"><span data-stu-id="09292-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="09292-110">Zaten oluşturduysanız **blog** veritabanı başka bir öğreticinin bir parçası olarak, bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="09292-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="09292-111">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="09292-111">Open Visual Studio</span></span>

* <span data-ttu-id="09292-112">**Araçlar > veritabanına bağlan...**</span><span class="sxs-lookup"><span data-stu-id="09292-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="09292-113">Seçin **Microsoft SQL Server** tıklatıp **devam et**</span><span class="sxs-lookup"><span data-stu-id="09292-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="09292-114">Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**</span><span class="sxs-lookup"><span data-stu-id="09292-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="09292-115">Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="09292-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="09292-116">Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="09292-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="09292-117">Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="09292-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="09292-118">Sorgu Düzenleyicisi'ne aşağıdaki betiği kopyalayın</span><span class="sxs-lookup"><span data-stu-id="09292-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="09292-119">Sorgu düzenleyicisini sağ tıklayıp **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="09292-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="09292-120">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="09292-120">Create a new project</span></span>

* <span data-ttu-id="09292-121">Açık Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="09292-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="09292-122">**Dosya > Yeni > Proje...**</span><span class="sxs-lookup"><span data-stu-id="09292-122">**File > New > Project...**</span></span>

* <span data-ttu-id="09292-123">Sol menüden **yüklü > Visual C# > Windows Masaüstü**</span><span class="sxs-lookup"><span data-stu-id="09292-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="09292-124">Seçin **konsol uygulaması (.NET Framework)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="09292-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="09292-125">Emin olun projenizin hedeflediği **.NET Framework 4.6.1** veya üzeri</span><span class="sxs-lookup"><span data-stu-id="09292-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="09292-126">Projeyi adlandırın *ConsoleApp.ExistingDb* tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="09292-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="09292-127">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="09292-127">Install Entity Framework</span></span>

<span data-ttu-id="09292-128">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="09292-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="09292-129">Bu öğreticide, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="09292-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="09292-130">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="09292-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="09292-131">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="09292-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="09292-132">`Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="09292-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="09292-133">Sonraki adımda, veritabanı tersine için bazı Entity Framework araçları kullanın.</span><span class="sxs-lookup"><span data-stu-id="09292-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="09292-134">Bu nedenle de araçları paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="09292-134">So install the tools package as well.</span></span>

* <span data-ttu-id="09292-135">`Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="09292-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="09292-136">Ters mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="09292-136">Reverse engineer the model</span></span>

<span data-ttu-id="09292-137">Artık mevcut bir veritabanını temel alan EF modeli oluşturma zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="09292-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="09292-138">**Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="09292-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="09292-139">Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="09292-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="09292-140">Varlıklar için ekleyerek oluşturmak üzere tablolara belirtebilirsiniz `-Tables` bağımsız değişkeni için yukarıdaki komutu.</span><span class="sxs-lookup"><span data-stu-id="09292-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="09292-141">Örneğin, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="09292-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="09292-142">Oluşturulan varlık sınıfları ters mühendislik süreci (`Blog` ve `Post`) ve türetilmiş bir içerik (`BloggingContext`) var olan veritabanı şemasını temel alan.</span><span class="sxs-lookup"><span data-stu-id="09292-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="09292-143">Varlık sınıfları, sorgulama ve kaydetme verilerini temsil eden basit C# nesneleridir.</span><span class="sxs-lookup"><span data-stu-id="09292-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="09292-144">İşte `Blog` ve `Post` varlık sınıfları:</span><span class="sxs-lookup"><span data-stu-id="09292-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="09292-145">Yavaş yükleniyor etkinleştirmek için Gezinti özellikleri yapabilirsiniz `virtual` (Blog.Post ve Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="09292-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="09292-146">İçerik veritabanı ile bir oturumu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="09292-146">The context represents a session with the database.</span></span> <span data-ttu-id="09292-147">Bu, sorgulama ve varlık sınıflarının örneklerini kaydetmek için kullanabileceğiniz yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="09292-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="09292-148">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="09292-148">Use the model</span></span>

<span data-ttu-id="09292-149">Şimdi, veri erişimi gerçekleştirdiği modeli kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="09292-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="09292-150">Açık *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="09292-150">Open *Program.cs*</span></span>

* <span data-ttu-id="09292-151">Dosyanın içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="09292-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="09292-152">Hata ayıklama > hata ayıklama olmadan Başlat</span><span class="sxs-lookup"><span data-stu-id="09292-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="09292-153">Bir blog veritabanına kaydedilir ve daha sonra tüm blogları ayrıntılarını konsola yazdırılır görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="09292-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![görüntü](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="09292-155">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="09292-155">Additional Resources</span></span>

* [<span data-ttu-id="09292-156">.NET Framework ile yeni bir veritabanı üzerinde EF Core</span><span class="sxs-lookup"><span data-stu-id="09292-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="09292-157">[EF Core ile yeni bir veritabanı - SQLite .NET core'da](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.</span><span class="sxs-lookup"><span data-stu-id="09292-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
