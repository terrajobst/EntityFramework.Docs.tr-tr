---
title: "ASP.NET Core - yeni veritabanı - EF Çekirdeğinde Başlarken"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 7e7ecaff29e9830bf3bcf742e6a5d54e1ced24de
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="3e62a-102">ASP.NET Core EF Çekirdeğinde ile yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3e62a-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="3e62a-103">Bu kılavuzda, Entity Framework Çekirdek kullanarak temel veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="3e62a-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3e62a-104">EF çekirdek modelden veritabanı oluşturmak için geçişleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e62a-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="3e62a-105">Bkz: [ek kaynaklar](#additional-resources) daha fazla Entity Framework Çekirdek öğreticileri.</span><span class="sxs-lookup"><span data-stu-id="3e62a-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="3e62a-106">Bu öğretici gerektirir:</span><span class="sxs-lookup"><span data-stu-id="3e62a-106">This tutorial requires:</span></span>
* <span data-ttu-id="3e62a-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="3e62a-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3e62a-108">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="3e62a-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3e62a-109">**.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="3e62a-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3e62a-110">[.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3e62a-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="3e62a-111">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) github'da.</span><span class="sxs-lookup"><span data-stu-id="3e62a-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="3e62a-112">Visual Studio 2017 içinde yeni bir proje oluşturun</span><span class="sxs-lookup"><span data-stu-id="3e62a-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="3e62a-113">**Dosya > Yeni > Proje**</span><span class="sxs-lookup"><span data-stu-id="3e62a-113">**File > New > Project**</span></span>
* <span data-ttu-id="3e62a-114">Sol menüden seçin **yüklü > şablonları > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="3e62a-115">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3e62a-116">Girin **EFGetStarted.AspNetCore.NewDb** tıklayın ve ad için **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="3e62a-117">İçinde **çekirdek yeni bir ASP.NET Web uygulaması** iletişim:</span><span class="sxs-lookup"><span data-stu-id="3e62a-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3e62a-118">Seçenekleri olun **.NET Core** ve **ASP.NET Core 2.0** açılan listeleri seçilir</span><span class="sxs-lookup"><span data-stu-id="3e62a-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="3e62a-119">Seçin **Web uygulaması (Model-View-Controller)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="3e62a-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="3e62a-120">Emin **kimlik doğrulaması** ayarlanır **doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="3e62a-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="3e62a-121">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3e62a-121">Click **OK**</span></span>

<span data-ttu-id="3e62a-122">Uyarı: kullanırsanız **tek tek kullanıcı hesaplarını** yerine **hiçbiri** için **kimlik doğrulaması** bir Entity Framework Çekirdek modeli projenizdeeklenirsonra`Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="3e62a-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="3e62a-123">Bu kılavuzda öğreneceksiniz teknikleri kullanarak, ikinci bir model ekleyin ya da varlık sınıflarınızı içerecek şekilde bu varolan modelini seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3e62a-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="3e62a-124">Entity Framework Çekirdek yükleyin</span><span class="sxs-lookup"><span data-stu-id="3e62a-124">Install Entity Framework Core</span></span>

<span data-ttu-id="3e62a-125">Hedeflemek istediğiniz EF çekirdek veritabanı sağlayıcı(lar) için paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3e62a-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3e62a-126">Bu kılavuz, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e62a-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="3e62a-127">Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3e62a-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="3e62a-128">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="3e62a-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="3e62a-129">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="3e62a-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="3e62a-130">EF çekirdek modelinizin dışında bir veritabanı oluşturmak için size bazı Entity Framework Çekirdek araçları kullanarak.</span><span class="sxs-lookup"><span data-stu-id="3e62a-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="3e62a-131">Böylece biz de araçları paketini yükleyecek:</span><span class="sxs-lookup"><span data-stu-id="3e62a-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="3e62a-132">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="3e62a-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="3e62a-133">Daha sonra denetleyicileri ve görünümleri oluşturmak için size bazı ASP.NET Core İskele araçlarını kullanma.</span><span class="sxs-lookup"><span data-stu-id="3e62a-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="3e62a-134">Böylece biz de bu tasarım paket yükleyecek:</span><span class="sxs-lookup"><span data-stu-id="3e62a-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="3e62a-135">Çalıştırma`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="3e62a-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="3e62a-136">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="3e62a-136">Create the model</span></span>

<span data-ttu-id="3e62a-137">Modeli olun bağlamını ve varlık sınıflarını tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="3e62a-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="3e62a-138">Sağ **modelleri** klasörü ve select **Ekle > sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="3e62a-139">Girin **Model.cs** tıklatın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="3e62a-140">Dosyasının içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="3e62a-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="3e62a-141">Not: Gerçek bir uygulamada, genellikle her sınıf ayrı bir dosyada, modelden koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3e62a-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="3e62a-142">Basitleştirmek amacıyla, biz tüm sınıflar bir dosyada Bu öğretici için koyduğunuz.</span><span class="sxs-lookup"><span data-stu-id="3e62a-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3e62a-143">İçeriğiniz bağımlılık ekleme ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="3e62a-143">Register your context with dependency injection</span></span>

<span data-ttu-id="3e62a-144">Hizmetler (gibi `BloggingContext`) ile kayıtlı [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) uygulama başlatma sırasında.</span><span class="sxs-lookup"><span data-stu-id="3e62a-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="3e62a-145">Bu Hizmetleri (örneğin, MVC denetleyicileri) gerektiren bileşenler daha sonra bu hizmetlere Oluşturucu parametreleri veya özellikleri yoluyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e62a-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="3e62a-146">Bizim MVC denetleyicileri yapmak için sırayla kullanımı `BloggingContext` sizi bir hizmet olarak kaydeder.</span><span class="sxs-lookup"><span data-stu-id="3e62a-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="3e62a-147">Açık **haline**</span><span class="sxs-lookup"><span data-stu-id="3e62a-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="3e62a-148">Aşağıdakileri ekleyin `using` deyimleri:</span><span class="sxs-lookup"><span data-stu-id="3e62a-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3e62a-149">Ekleme `AddDbContext` yöntemi bir hizmet olarak kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="3e62a-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="3e62a-150">Aşağıdaki kodu ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3e62a-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="3e62a-151">Not: Gerçek bir uygulama yapılandırma dosyasında bağlantı dizesini gennerally koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3e62a-151">Note: A real app would gennerally put the connection string in a configuration file.</span></span> <span data-ttu-id="3e62a-152">Basitleştirmek amacıyla, biz bu kodda tanımlıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="3e62a-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="3e62a-153">Bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3e62a-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="3e62a-154">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="3e62a-154">Create your database</span></span>

<span data-ttu-id="3e62a-155">Bir modeli eğittikten sonra kullanabileceğiniz [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="3e62a-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="3e62a-156">PMC açın:</span><span class="sxs-lookup"><span data-stu-id="3e62a-156">Open the PMC:</span></span>

  <span data-ttu-id="3e62a-157">**Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="3e62a-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="3e62a-158">Çalıştırma `Add-Migration InitialCreate` tabloları modeliniz için ilk kümesi oluşturmak için bir geçiş için iskele kurmak.</span><span class="sxs-lookup"><span data-stu-id="3e62a-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="3e62a-159">Belirten bir hata alırsanız `The term 'add-migration' is not recognized as the name of a cmdlet`kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="3e62a-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="3e62a-160">Çalıştırma `Update-Database` veritabanına yeni geçiş uygulanacak.</span><span class="sxs-lookup"><span data-stu-id="3e62a-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="3e62a-161">Bu komut, geçişler uygulamadan önce veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e62a-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="3e62a-162">Bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="3e62a-162">Create a controller</span></span>

<span data-ttu-id="3e62a-163">Yapı iskelesi projesinde etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="3e62a-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="3e62a-164">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3e62a-165">Seçin **en az bağımlılıkları** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="3e62a-166">Yoksay veya silme *ScaffoldingReadMe.txt* dosya.</span><span class="sxs-lookup"><span data-stu-id="3e62a-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="3e62a-167">Askılama özelliğinin etkinleştirildiğinden, bir denetleyici için iskele `Blog` varlık.</span><span class="sxs-lookup"><span data-stu-id="3e62a-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="3e62a-168">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3e62a-169">Seçin **görünümleri ile MVC Entity Framework kullanarak denetleyicisi** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="3e62a-170">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfı** için **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="3e62a-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="3e62a-171">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="3e62a-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="3e62a-172">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="3e62a-172">Run the application</span></span>

<span data-ttu-id="3e62a-173">Ve uygulamayı test çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="3e62a-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="3e62a-174">Gidin`/Blogs`</span><span class="sxs-lookup"><span data-stu-id="3e62a-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3e62a-175">Bazı Web günlüğü girişleri oluşturmak için Oluştur bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e62a-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="3e62a-176">Sınama ayrıntıları ve bağlantıları silin.</span><span class="sxs-lookup"><span data-stu-id="3e62a-176">Test the details and delete links.</span></span>

![görüntü](_static/create.png)

![görüntü](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3e62a-179">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3e62a-179">Additional Resources</span></span>

* <span data-ttu-id="3e62a-180">[EF - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.</span><span class="sxs-lookup"><span data-stu-id="3e62a-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="3e62a-181">ASP.NET Core Mac veya Linux MVC giriş</span><span class="sxs-lookup"><span data-stu-id="3e62a-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="3e62a-182">ASP.NET Core Visual Studio ile MVC giriş</span><span class="sxs-lookup"><span data-stu-id="3e62a-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="3e62a-183">ASP.NET Core ve Entity Framework Visual Studio kullanarak çekirdek ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3e62a-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
