---
title: Üzerinde ASP.NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996070"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="3da44-102">EF çekirdekli ASP.NET Core üzerinde yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3da44-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="3da44-103">Bu öğreticide, Entity Framework Core kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3da44-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3da44-104">EF Core modelinizden veritabanı oluşturmaya geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="3da44-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="3da44-105">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="3da44-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3da44-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="3da44-106">Prerequisites</span></span>

<span data-ttu-id="3da44-107">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="3da44-107">Install the following software:</span></span>

* <span data-ttu-id="3da44-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="3da44-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3da44-109">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="3da44-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3da44-110">**.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)</span><span class="sxs-lookup"><span data-stu-id="3da44-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3da44-111">[.NET core SDK'sını 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3da44-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="3da44-112">Visual Studio 2017'de yeni bir proje oluşturun</span><span class="sxs-lookup"><span data-stu-id="3da44-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="3da44-113">Açık Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3da44-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="3da44-114">**Dosya > Yeni > Proje**</span><span class="sxs-lookup"><span data-stu-id="3da44-114">**File > New > Project**</span></span>
* <span data-ttu-id="3da44-115">Sol menüden **yüklü > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3da44-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="3da44-116">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="3da44-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3da44-117">Girin **EFGetStarted.AspNetCore.NewDb** tıklayın ve ad için **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3da44-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="3da44-118">İçinde **yeni ASP.NET Core Web uygulaması** iletişim:</span><span class="sxs-lookup"><span data-stu-id="3da44-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3da44-119">Seçenekleri sağlamak **.NET Core** ve **ASP.NET Core 2.1** açılan listeleri seçili</span><span class="sxs-lookup"><span data-stu-id="3da44-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="3da44-120">Seçin **Web uygulaması (Model-View-Controller)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="3da44-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="3da44-121">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="3da44-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="3da44-122">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3da44-122">Click **OK**</span></span>

<span data-ttu-id="3da44-123">Uyarı: kullanırsanız **bireysel kullanıcı hesapları** yerine **hiçbiri** için **kimlik doğrulaması** projenizdebirEntityFrameworkCoremodeliekleneceksonra`Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="3da44-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="3da44-124">Bu öğreticide şunların tekniklerini kullanarak ikinci bir model eklemek veya var olan bu modeli, varlık sınıfları içeren genişletmek seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3da44-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="3da44-125">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="3da44-125">Install Entity Framework Core</span></span>

<span data-ttu-id="3da44-126">EF Core yüklemek için hedeflemek istediğiniz EF Core veritabanı sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3da44-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3da44-127">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3da44-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="3da44-128">Bu öğreticide, öğretici, SQL Server kullandığından sağlayıcı paketi yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3da44-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="3da44-129">SQL Server sağlayıcı paketi eklenmiştir [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="3da44-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="3da44-130">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="3da44-130">Create the model</span></span>

<span data-ttu-id="3da44-131">Bağlam sınıfı ve modelini yapmak bir varlık sınıfları tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="3da44-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="3da44-132">Sağ **modelleri** klasörü ve select **Ekle > sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="3da44-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="3da44-133">Girin **Model.cs** tıklayın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3da44-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="3da44-134">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="3da44-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="3da44-135">Gerçek bir uygulamada her sınıf genellikle ayrı bir dosyada modelinizdeki koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3da44-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="3da44-136">Basitleştirmek amacıyla, bu öğreticide tüm sınıflar tek dosyada koyar.</span><span class="sxs-lookup"><span data-stu-id="3da44-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3da44-137">Bağlamınızı bağımlılık ekleme ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="3da44-137">Register your context with dependency injection</span></span>

<span data-ttu-id="3da44-138">Hizmetler (gibi `BloggingContext`) ile kaydedilen [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) uygulama başlatma sırasında.</span><span class="sxs-lookup"><span data-stu-id="3da44-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="3da44-139">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler sonra hizmetlerin Oluşturucu parametresi veya özellikleri aracılığıyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="3da44-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="3da44-140">Yapmak `BloggingContext` MVC denetleyicileri için kullanılabilir bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="3da44-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="3da44-141">Açık **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="3da44-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="3da44-142">Aşağıdaki `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="3da44-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3da44-143">Çağrı `AddDbContext` bağlamı bir hizmet olarak kaydetmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3da44-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="3da44-144">Aşağıdaki vurgulanmış kodu ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3da44-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="3da44-145">Not: Gerçek bir uygulamada genellikle bir yapılandırma dosyası veya ortam değişkenine bağlantı dizesini koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3da44-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="3da44-146">Basitleştirmek amacıyla, bu öğreticide kod tanımlar.</span><span class="sxs-lookup"><span data-stu-id="3da44-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="3da44-147">Bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3da44-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="3da44-148">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="3da44-148">Create the database</span></span>

<span data-ttu-id="3da44-149">Bir model açtıktan sonra kullanabileceğiniz [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="3da44-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="3da44-150">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="3da44-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="3da44-151">Çalıştırma `Add-Migration InitialCreate` tablolar, modelinize için başlangıç kümesi oluşturmak için bir geçiş iskele.</span><span class="sxs-lookup"><span data-stu-id="3da44-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="3da44-152">Belirten bir hata alırsanız `The term 'add-migration' is not recognized as the name of a cmdlet`kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="3da44-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="3da44-153">Çalıştırma `Update-Database` veritabanına yeni geçiş uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="3da44-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="3da44-154">Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3da44-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="3da44-155">Bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="3da44-155">Create a controller</span></span>

<span data-ttu-id="3da44-156">Bir denetleyici ve görünüm için iskele `Blog` varlık.</span><span class="sxs-lookup"><span data-stu-id="3da44-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="3da44-157">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="3da44-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3da44-158">Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="3da44-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="3da44-159">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="3da44-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="3da44-160">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="3da44-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="3da44-161">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="3da44-161">Run the application</span></span>

<span data-ttu-id="3da44-162">Uygulamayı test etmek ve çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="3da44-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="3da44-163">Gidin `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="3da44-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3da44-164">Bazı blog girişleri oluşturmak için Oluştur bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="3da44-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="3da44-165">Test ayrıntıları ve bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="3da44-165">Test the details and delete links.</span></span>

![görüntü](_static/create.png)

![görüntü](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3da44-168">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3da44-168">Additional Resources</span></span>

* <span data-ttu-id="3da44-169">[EF - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.</span><span class="sxs-lookup"><span data-stu-id="3da44-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="3da44-170">MVC Mac veya Linux'ta ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="3da44-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="3da44-171">Visual Studio ile MVC ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="3da44-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="3da44-172">Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3da44-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
