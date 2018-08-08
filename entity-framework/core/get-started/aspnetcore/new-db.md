---
title: Üzerinde ASP.NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 08/03/2018
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 9e86bc9cff028ad9791f23cbb45f0a93110c0064
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614356"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="db71b-102">EF çekirdekli ASP.NET Core üzerinde yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="db71b-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="db71b-103">Bu öğreticide, Entity Framework Core kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db71b-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="db71b-104">EF Core modelinizden veritabanı oluşturmaya geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="db71b-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="db71b-105">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="db71b-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db71b-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="db71b-106">Prerequisites</span></span>

<span data-ttu-id="db71b-107">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="db71b-107">Install the following software:</span></span>

* <span data-ttu-id="db71b-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="db71b-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="db71b-109">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="db71b-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="db71b-110">**.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)</span><span class="sxs-lookup"><span data-stu-id="db71b-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="db71b-111">[.NET core SDK'sını 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="db71b-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="db71b-112">Visual Studio 2017'de yeni bir proje oluşturun</span><span class="sxs-lookup"><span data-stu-id="db71b-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="db71b-113">Açık Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="db71b-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="db71b-114">**Dosya > Yeni > Proje**</span><span class="sxs-lookup"><span data-stu-id="db71b-114">**File > New > Project**</span></span>
* <span data-ttu-id="db71b-115">Sol menüden **yüklü > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="db71b-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="db71b-116">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="db71b-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="db71b-117">Girin **EFGetStarted.AspNetCore.NewDb** tıklayın ve ad için **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="db71b-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="db71b-118">İçinde **yeni ASP.NET Core Web uygulaması** iletişim:</span><span class="sxs-lookup"><span data-stu-id="db71b-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="db71b-119">Seçenekleri sağlamak **.NET Core** ve **ASP.NET Core 2.1** açılan listeleri seçili</span><span class="sxs-lookup"><span data-stu-id="db71b-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="db71b-120">Seçin **Web uygulaması (Model-View-Controller)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="db71b-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="db71b-121">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="db71b-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="db71b-122">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="db71b-122">Click **OK**</span></span>

<span data-ttu-id="db71b-123">Uyarı: kullanırsanız **bireysel kullanıcı hesapları** yerine **hiçbiri** için **kimlik doğrulaması** projenizdebirEntityFrameworkCoremodeliekleneceksonra`Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="db71b-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="db71b-124">Bu öğreticide şunların tekniklerini kullanarak ikinci bir model eklemek veya var olan bu modeli, varlık sınıfları içeren genişletmek seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db71b-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="db71b-125">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="db71b-125">Install Entity Framework Core</span></span>

<span data-ttu-id="db71b-126">EF Core yüklemek için hedeflemek istediğiniz EF Core veritabanı sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="db71b-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="db71b-127">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="db71b-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="db71b-128">Bu öğreticide, öğretici, SQL Server kullandığından sağlayıcı paketi yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="db71b-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="db71b-129">SQL Server sağlayıcı paketi eklenmiştir [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="db71b-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="db71b-130">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="db71b-130">Create the model</span></span>

<span data-ttu-id="db71b-131">Bağlam sınıfı ve modelini yapmak bir varlık sınıfları tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="db71b-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="db71b-132">Sağ **modelleri** klasörü ve select **Ekle > sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="db71b-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="db71b-133">Girin **Model.cs** tıklayın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="db71b-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="db71b-134">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="db71b-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="db71b-135">Gerçek bir uygulamada her sınıf genellikle ayrı bir dosyada modelinizdeki koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db71b-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="db71b-136">Basitleştirmek amacıyla, bu öğreticide tüm sınıflar tek dosyada koyar.</span><span class="sxs-lookup"><span data-stu-id="db71b-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="db71b-137">Bağlamınızı bağımlılık ekleme ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="db71b-137">Register your context with dependency injection</span></span>

<span data-ttu-id="db71b-138">Hizmetler (gibi `BloggingContext`) ile kaydedilen [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) uygulama başlatma sırasında.</span><span class="sxs-lookup"><span data-stu-id="db71b-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="db71b-139">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler sonra hizmetlerin Oluşturucu parametresi veya özellikleri aracılığıyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="db71b-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="db71b-140">Yapmak `BloggingContext` MVC denetleyicileri için kullanılabilir bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="db71b-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="db71b-141">Açık **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="db71b-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="db71b-142">Aşağıdaki `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="db71b-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="db71b-143">Çağrı `AddDbContext` bağlamı bir hizmet olarak kaydetmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db71b-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="db71b-144">Aşağıdaki vurgulanmış kodu ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db71b-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="db71b-145">Not: Gerçek bir uygulamada genellikle bir yapılandırma dosyası veya ortam değişkenine bağlantı dizesini koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db71b-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="db71b-146">Basitleştirmek amacıyla, bu öğreticide kod tanımlar.</span><span class="sxs-lookup"><span data-stu-id="db71b-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="db71b-147">Bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="db71b-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="db71b-148">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="db71b-148">Create the database</span></span>

<span data-ttu-id="db71b-149">Bir model açtıktan sonra kullanabileceğiniz [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="db71b-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="db71b-150">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="db71b-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="db71b-151">Çalıştırma `Add-Migration InitialCreate` tablolar, modelinize için başlangıç kümesi oluşturmak için bir geçiş iskele.</span><span class="sxs-lookup"><span data-stu-id="db71b-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="db71b-152">Belirten bir hata alırsanız `The term 'add-migration' is not recognized as the name of a cmdlet`kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="db71b-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="db71b-153">Çalıştırma `Update-Database` veritabanına yeni geçiş uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="db71b-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="db71b-154">Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db71b-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="db71b-155">Bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="db71b-155">Create a controller</span></span>

<span data-ttu-id="db71b-156">Bir denetleyici ve görünüm için iskele `Blog` varlık.</span><span class="sxs-lookup"><span data-stu-id="db71b-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="db71b-157">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="db71b-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="db71b-158">Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db71b-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="db71b-159">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="db71b-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="db71b-160">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="db71b-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="db71b-161">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="db71b-161">Run the application</span></span>

<span data-ttu-id="db71b-162">Uygulamayı test etmek ve çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="db71b-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="db71b-163">Gidin `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="db71b-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="db71b-164">Bazı blog girişleri oluşturmak için Oluştur bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="db71b-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="db71b-165">Test ayrıntıları ve bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="db71b-165">Test the details and delete links.</span></span>

![görüntü](_static/create.png)

![görüntü](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="db71b-168">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="db71b-168">Additional Resources</span></span>

* <span data-ttu-id="db71b-169">[EF - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.</span><span class="sxs-lookup"><span data-stu-id="db71b-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="db71b-170">MVC Mac veya Linux'ta ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="db71b-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="db71b-171">Visual Studio ile MVC ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="db71b-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="db71b-172">Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="db71b-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
