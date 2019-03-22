---
title: Üzerinde ASP.NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 25e5a683acf4bbed0b978cc6a80f1b50a0b64ca1
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319185"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="1c177-102">EF çekirdekli ASP.NET Core üzerinde yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1c177-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="1c177-103">Bu öğreticide, Entity Framework Core kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1c177-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="1c177-104">Öğreticide geçişleri veri modeli veritabanını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1c177-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="1c177-105">Windows üzerinde Visual Studio 2017 kullanılarak veya Windows, macOS veya Linux üzerinde .NET Core CLI kullanarak öğreticiyi izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c177-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="1c177-106">Bu makaledeki örnek Github'da görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="1c177-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="1c177-107">SQL Server ile Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1c177-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="1c177-108">[.NET core CLI SQLite ile](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="1c177-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c177-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="1c177-109">Prerequisites</span></span>

<span data-ttu-id="1c177-110">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="1c177-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c177-112">[Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="1c177-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="1c177-113">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="1c177-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="1c177-114">**.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)</span><span class="sxs-lookup"><span data-stu-id="1c177-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="1c177-115">[.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="1c177-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-116">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="1c177-117">[.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="1c177-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="1c177-118">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c177-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c177-120">Açık Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1c177-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="1c177-121">**Dosya > Yeni > Proje**</span><span class="sxs-lookup"><span data-stu-id="1c177-121">**File > New > Project**</span></span>
* <span data-ttu-id="1c177-122">Sol menüden **yüklü > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1c177-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="1c177-123">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1c177-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1c177-124">Girin **EFGetStarted.AspNetCore.NewDb** tıklayın ve ad için **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1c177-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="1c177-125">İçinde **yeni ASP.NET Core Web uygulaması** iletişim:</span><span class="sxs-lookup"><span data-stu-id="1c177-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="1c177-126">Emin olun **.NET Core** ve **ASP.NET Core 2.1** seçildiğinden açılan listeler</span><span class="sxs-lookup"><span data-stu-id="1c177-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="1c177-127">Seçin **Web uygulaması (Model-View-Controller)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="1c177-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="1c177-128">Emin olun **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="1c177-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="1c177-129">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1c177-129">Click **OK**</span></span>

<span data-ttu-id="1c177-130">Uyarı: Kullanırsanız **bireysel kullanıcı hesapları** yerine **hiçbiri** için **kimlik doğrulaması** projede Entity Framework Core modeli eklenecek sonra `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="1c177-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="1c177-131">Bu öğreticide şunların tekniklerini kullanarak ikinci bir model eklemek veya var olan bu modeli, varlık sınıfları içeren genişletmek seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c177-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-132">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="1c177-133">Bir MVC projesi oluşturmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1c177-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="1c177-134">Proje dizinine değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c177-134">Change to the project directory.</span></span> <span data-ttu-id="1c177-135">Yeni projeyi karşı çalıştırmak girdiğiniz komutları gerekir.</span><span class="sxs-lookup"><span data-stu-id="1c177-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="1c177-136">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="1c177-136">Install Entity Framework Core</span></span>

<span data-ttu-id="1c177-137">EF Core yüklemek için hedeflemek istediğiniz EF Core veritabanı sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1c177-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="1c177-138">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1c177-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1c177-140">Bu öğreticide, öğretici, SQL Server kullandığından sağlayıcı paketi yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1c177-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="1c177-141">SQL Server sağlayıcı paketi eklenmiştir [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="1c177-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1c177-143">Bu öğreticide, .NET Core destekleyen tüm platformlarda çalıştığı için SQLite kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1c177-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="1c177-144">SQLite sağlayıcısını yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1c177-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="1c177-145">Modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c177-145">Create the model</span></span>

<span data-ttu-id="1c177-146">Bağlam sınıfı ve modelini olun varlık sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1c177-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c177-148">Sağ **modelleri** klasörü ve select **Ekle > sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="1c177-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="1c177-149">Girin **Model.cs** tıklayın ve adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1c177-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="1c177-150">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1c177-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="1c177-152">İçinde **modelleri** klasör oluşturma **Model.cs** aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="1c177-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="1c177-153">Bir üretim uygulaması, genellikle ayrı bir dosyada her sınıf koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c177-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="1c177-154">Basitleştirmek amacıyla, bu Öğreticide bu sınıfların tek bir dosyaya koyar.</span><span class="sxs-lookup"><span data-stu-id="1c177-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="1c177-155">Bağımlılık ekleme bağlam kaydı</span><span class="sxs-lookup"><span data-stu-id="1c177-155">Register the context with dependency injection</span></span>

<span data-ttu-id="1c177-156">Yapmak `BloggingContext` MVC denetleyicileri için kullanılabilir bir hizmet olarak kaydetme `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="1c177-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="1c177-157">Hizmetler (gibi `BloggingContext`) ile kaydedilen [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) uygulama başlatma sırasında böylece Oluşturucusu aracılığıyla (örneğin, MVC denetleyicileri) hizmetlerini bileşenleri otomatik olarak sağlanabilir parametreleri ve özellikleri.</span><span class="sxs-lookup"><span data-stu-id="1c177-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c177-159">İçinde **Startup.cs** aşağıdaki `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="1c177-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="1c177-160">Aşağıdaki vurgulanmış kodu ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1c177-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-161">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="1c177-162">İçinde **Startup.cs** aşağıdaki `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="1c177-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="1c177-163">Aşağıdaki vurgulanmış kodu ekleyin `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1c177-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="1c177-164">Bir üretim uygulaması, genellikle bir yapılandırma dosyası veya ortam değişkenine bağlantı dizesini koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c177-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="1c177-165">Basitleştirmek amacıyla, bu öğreticide kod tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1c177-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="1c177-166">Bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1c177-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="1c177-167">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c177-167">Create the database</span></span>

<span data-ttu-id="1c177-168">Aşağıdaki adımları kullanın [geçişler](xref:core/managing-schemas/migrations/index) bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="1c177-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c177-170">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="1c177-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="1c177-171">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1c177-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="1c177-172">Belirten bir hata alırsanız `The term 'add-migration' is not recognized as the name of a cmdlet`kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="1c177-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="1c177-173">`Add-Migration` Komut iskele oluşturulduğunu tablo modeli için başlangıç kümesi oluşturmak için bir geçiş.</span><span class="sxs-lookup"><span data-stu-id="1c177-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="1c177-174">`Update-Database` Komutu veritabanı oluşturur ve yeni bir geçiş uygular.</span><span class="sxs-lookup"><span data-stu-id="1c177-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-175">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="1c177-176">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1c177-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="1c177-177">`migrations` Komut iskele oluşturulduğunu tablo modeli için başlangıç kümesi oluşturmak için bir geçiş.</span><span class="sxs-lookup"><span data-stu-id="1c177-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="1c177-178">`database update` Komutu veritabanı oluşturur ve yeni bir geçiş uygular.</span><span class="sxs-lookup"><span data-stu-id="1c177-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="1c177-179">Bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c177-179">Create a controller</span></span>

<span data-ttu-id="1c177-180">Bir denetleyici ve görünüm için iskele `Blog` varlık.</span><span class="sxs-lookup"><span data-stu-id="1c177-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c177-182">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="1c177-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="1c177-183">Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="1c177-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="1c177-184">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="1c177-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="1c177-185">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1c177-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-186">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="1c177-187">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1c177-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="1c177-188">`tool install` Ve `add package` komutları denetleyicileri ve görünümleri iskelesini oluşturabilirsiniz araçları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1c177-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="1c177-189">`restore` Komut, tüm projenin paketlerin yüklenmesi, sağlar ve `aspnet-codegenerator` komutu iskele yapar.</span><span class="sxs-lookup"><span data-stu-id="1c177-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="1c177-190">Yapı iskelesi altyapısı aşağıdaki dosyaları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1c177-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="1c177-191">Bir denetleyici (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="1c177-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="1c177-192">Oluşturma, silme, Ayrıntılar, düzenleme ve dizin sayfaları için Razor görünümleri (_Views/Blogs/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="1c177-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="1c177-193">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1c177-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c177-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c177-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c177-195">**Hata ayıklama** > **hata ayıklama olmadan Başlat**</span><span class="sxs-lookup"><span data-stu-id="1c177-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1c177-196">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c177-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="1c177-197">Gidin `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="1c177-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="1c177-198">Kullanım **Yeni Oluştur** bağlantı bazı blog girişleri oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="1c177-198">Use the **Create New** link to create some blog entries.</span></span>

  ![sayfası oluşturma](_static/create.png)

* <span data-ttu-id="1c177-200">Test **ayrıntıları**, **Düzenle**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="1c177-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Dizin Sayfası](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="1c177-202">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1c177-202">Additional Resources</span></span>

* [<span data-ttu-id="1c177-203">Öğretici: EF Core üzerinde .NET Core ile SQLite kullanarak yeni bir veritabanı ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1c177-203">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* [<span data-ttu-id="1c177-204">Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1c177-204">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="1c177-205">Öğretici: ASP.NET Core, Entity Framework Core ile Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="1c177-205">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
