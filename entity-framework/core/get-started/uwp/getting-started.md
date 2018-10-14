---
title: UWP - yeni veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315626"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="45bf3-102">Yeni bir veritabanı ile Evrensel Windows Platformu (UWP) üzerinde EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="45bf3-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="45bf3-103">Bu öğreticide, Entity Framework Core kullanan yerel bir SQLite veritabanından karşı temel veri erişimi gerçekleştirdiği bir evrensel Windows Platformu (UWP) uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="45bf3-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="45bf3-104">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="45bf3-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45bf3-105">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="45bf3-105">Prerequisites</span></span>

* <span data-ttu-id="45bf3-106">[Windows 10 Fall Creators Update (10.0; 16299 yapı) veya üzeri](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="45bf3-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="45bf3-107">[Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/) ile **Evrensel Windows platformu geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="45bf3-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="45bf3-108">[.NET core 2.1 SDK veya üzeri](https://www.microsoft.com/net/core) veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="45bf3-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45bf3-109">Bu öğreticide, Entity Framework Core kullanan [geçişler](xref:core/managing-schemas/migrations/index) oluşturmak ve veritabanı şeması güncelleştirmek için komutları.</span><span class="sxs-lookup"><span data-stu-id="45bf3-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="45bf3-110">Bu komutlar, doğrudan UWP projeleri ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="45bf3-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="45bf3-111">Bu nedenle, uygulamanın veri modelini bir paylaşılan kitaplığı projesinde yerleştirilir ve ayrı bir .NET Core konsol uygulaması komutları çalıştırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="45bf3-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="45bf3-112">Veri modelinde tutmak için bir kitaplık projesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="45bf3-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="45bf3-113">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="45bf3-113">Open Visual Studio</span></span>

* <span data-ttu-id="45bf3-114">**Dosya > Yeni > Proje**</span><span class="sxs-lookup"><span data-stu-id="45bf3-114">**File > New > Project**</span></span>

* <span data-ttu-id="45bf3-115">Sol menüden **yüklü > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="45bf3-116">Seçin **sınıf kitaplığı (.NET Standard)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="45bf3-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="45bf3-117">Projeyi adlandırın *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="45bf3-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="45bf3-118">Çözüm adı *blog*.</span><span class="sxs-lookup"><span data-stu-id="45bf3-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="45bf3-119">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="45bf3-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="45bf3-120">Veri modeli projede Entity Framework Core runtime'ı yükleyin</span><span class="sxs-lookup"><span data-stu-id="45bf3-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="45bf3-121">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="45bf3-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="45bf3-122">Bu öğreticide SQLite kullanılır.</span><span class="sxs-lookup"><span data-stu-id="45bf3-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="45bf3-123">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="45bf3-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="45bf3-124">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="45bf3-125">Emin olun kitaplık projesi *Blogging.Model* olarak seçilen **varsayılan proje** Paket Yöneticisi konsolunda.</span><span class="sxs-lookup"><span data-stu-id="45bf3-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="45bf3-126">`Install-Package Microsoft.EntityFrameworkCore.Sqlite`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="45bf3-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="45bf3-127">Veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="45bf3-127">Create the data model</span></span>

<span data-ttu-id="45bf3-128">Tanımlama artık *DbContext* ve modelini yapmak bir varlık sınıfları.</span><span class="sxs-lookup"><span data-stu-id="45bf3-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="45bf3-129">Silme *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="45bf3-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="45bf3-130">Oluşturma *Model.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="45bf3-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="45bf3-131">Geçişleri komutlarını çalıştırmak için yeni bir konsol projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="45bf3-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="45bf3-132">İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="45bf3-133">Sol menüden **yüklü > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="45bf3-134">Seçin **konsol uygulaması (.NET Core)** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="45bf3-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="45bf3-135">Projeyi adlandırın *Blogging.Migrations.Startup*, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="45bf3-136">Proje Başvuru Ekle *Blogging.Migrations.Startup* için proje *Blogging.Model* proje.</span><span class="sxs-lookup"><span data-stu-id="45bf3-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="45bf3-137">Geçişleri başlangıç projede Entity Framework Core Araçları'nı yükleyin</span><span class="sxs-lookup"><span data-stu-id="45bf3-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="45bf3-138">Paket Yöneticisi konsolu EF Core geçiş komutları etkinleştirmek için konsol uygulamasında EF Core araçları paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="45bf3-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="45bf3-139">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="45bf3-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="45bf3-140">`Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="45bf3-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="45bf3-141">İlk geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="45bf3-141">Create the initial migration</span></span>

 <span data-ttu-id="45bf3-142">Konsol uygulaması başlangıç projesi olarak belirterek ilk geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="45bf3-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="45bf3-143">`Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="45bf3-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="45bf3-144">Bu komut, ilk veritabanı tabloları veri modeliniz için kümesini oluşturan bir geçiş iskele oluşturulduğunu.</span><span class="sxs-lookup"><span data-stu-id="45bf3-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="45bf3-145">UWP projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="45bf3-145">Create the UWP project</span></span>

* <span data-ttu-id="45bf3-146">İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="45bf3-147">Sol menüden **yüklü > Visual C# > Windows Evrensel**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="45bf3-148">Seçin **boş uygulama (Evrensel Windows)** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="45bf3-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="45bf3-149">Projeyi adlandırın *Blogging.UWP*, tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="45bf3-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45bf3-150">En az bir hedef ve en düşük sürüm kümesine **Windows 10 Fall Creators Update (10.0; derleme 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="45bf3-151">Entity Framework Core tarafından gerekli olan .NET Standard 2.0, Windows 10 'un önceki sürümlerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="45bf3-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="45bf3-152">Uygulama başlangıcında veritabanını oluşturmak için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="45bf3-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="45bf3-153">Uygulamanın üzerinde çalıştığı cihazda oluşturulacak veritabanının istediğinden, uygulama başlangıcından yerel veritabanında bekleyen tüm geçişler uygulamak için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="45bf3-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="45bf3-154">Uygulama çalışır, ilk kez bu yerel veritabanı oluşturmayı ilgileniriz.</span><span class="sxs-lookup"><span data-stu-id="45bf3-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="45bf3-155">Proje Başvuru Ekle *Blogging.UWP* için proje *Blogging.Model* proje.</span><span class="sxs-lookup"><span data-stu-id="45bf3-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="45bf3-156">Açık *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="45bf3-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="45bf3-157">Bekleyen tüm geçişler uygulamak için vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="45bf3-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="45bf3-158">Modelinizi değiştirmek kullanırsanız `Add-Migration` karşılık gelen uygulamak için yeni bir geçiş iskele komut, veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="45bf3-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="45bf3-159">Uygulama başladığında bekleyen tüm geçişlerde, her cihazda yerel veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="45bf3-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="45bf3-160">EF Core kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="45bf3-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="45bf3-161">Veri modelini kullanın</span><span class="sxs-lookup"><span data-stu-id="45bf3-161">Use the data model</span></span>

<span data-ttu-id="45bf3-162">EF Core, veri erişimi gerçekleştirdiği artık kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45bf3-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="45bf3-163">Açık *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="45bf3-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="45bf3-164">UI içerik aşağıda belirtildiği ve sayfa yükleme işleyicisi ekleyin</span><span class="sxs-lookup"><span data-stu-id="45bf3-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="45bf3-165">Şimdi veritabanı ile kullanıcı arabirimini'kurmak wire için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="45bf3-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="45bf3-166">Açık *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="45bf3-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="45bf3-167">Aşağıdaki listeden vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="45bf3-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="45bf3-168">Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45bf3-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="45bf3-169">İçinde **Çözüm Gezgini**, sağ *Blogging.UWP* proje ve ardından **Dağıt**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="45bf3-170">Ayarlama *Blogging.UWP* başlangıç projesi olarak.</span><span class="sxs-lookup"><span data-stu-id="45bf3-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="45bf3-171">**Hata ayıklama > hata ayıklama olmadan Başlat**</span><span class="sxs-lookup"><span data-stu-id="45bf3-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="45bf3-172">Uygulamayı derler ve çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="45bf3-172">The app builds and runs.</span></span>

* <span data-ttu-id="45bf3-173">Bir URL girin ve tıklayın **Ekle** düğmesi</span><span class="sxs-lookup"><span data-stu-id="45bf3-173">Enter a URL and click the **Add** button</span></span>

  ![görüntü](_static/create.png)

  ![görüntü](_static/list.png)

  <span data-ttu-id="45bf3-176">Tada!</span><span class="sxs-lookup"><span data-stu-id="45bf3-176">Tada!</span></span> <span data-ttu-id="45bf3-177">Entity Framework Core çalıştıran basit bir UWP uygulamasında artık var.</span><span class="sxs-lookup"><span data-stu-id="45bf3-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45bf3-178">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="45bf3-178">Next steps</span></span>

<span data-ttu-id="45bf3-179">EF Core ile UWP kullanırken bilmeniz gereken uyumluluk ve performans için bilgi [EF Core tarafından desteklenen .NET uygulamalarıyla](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="45bf3-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="45bf3-180">Entity Framework Core özellikler hakkında daha fazla bilgi edinmek için bu belgeleri içindeki diğer makalelere göz atın.</span><span class="sxs-lookup"><span data-stu-id="45bf3-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
