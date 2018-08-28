---
title: UWP - yeni veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996916"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="e7493-102">Yeni bir veritabanı ile Evrensel Windows Platformu (UWP) üzerinde EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e7493-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="e7493-103">Bu öğreticide, Entity Framework Core kullanan yerel bir SQLite veritabanından karşı temel veri erişimi gerçekleştirdiği bir evrensel Windows Platformu (UWP) uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e7493-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="e7493-104">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="e7493-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7493-105">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e7493-105">Prerequisites</span></span>

* <span data-ttu-id="e7493-106">[Windows 10 Fall Creators Update (10.0; 16299 yapı) veya üzeri](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="e7493-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="e7493-107">[Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/) ile **Evrensel Windows platformu geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="e7493-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="e7493-108">[.NET core 2.1 SDK veya üzeri](https://www.microsoft.com/net/core) veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="e7493-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="e7493-109">Bir model projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e7493-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e7493-110">Şekilde sınırlamaları nedeniyle modeli geçişleri komutları çalıştırmak için bir UWP olmayan proje yerleştirilmesi gerekir UWP projeleri .NET Core araçları etkileşim **Paket Yöneticisi Konsolu** (PMC)</span><span class="sxs-lookup"><span data-stu-id="e7493-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="e7493-111">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="e7493-111">Open Visual Studio</span></span>

* <span data-ttu-id="e7493-112">**Dosya > Yeni > Proje**</span><span class="sxs-lookup"><span data-stu-id="e7493-112">**File > New > Project**</span></span>

* <span data-ttu-id="e7493-113">Sol menüden **yüklü > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="e7493-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="e7493-114">Seçin **sınıf kitaplığı (.NET Standard)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="e7493-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="e7493-115">Projeyi adlandırın *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="e7493-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="e7493-116">Çözüm adı *blog*.</span><span class="sxs-lookup"><span data-stu-id="e7493-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="e7493-117">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e7493-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="e7493-118">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="e7493-118">Install Entity Framework Core</span></span>

<span data-ttu-id="e7493-119">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e7493-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="e7493-120">Bu öğreticide SQLite kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e7493-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="e7493-121">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e7493-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="e7493-122">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="e7493-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="e7493-123">`Install-Package Microsoft.EntityFrameworkCore.Sqlite`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e7493-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="e7493-124">Veritabanını korumak için bu öğreticinin ilerleyen bölümlerinde bazı Entity Framework Core araçları kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="e7493-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="e7493-125">Bu nedenle de araçları paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e7493-125">So install the tools package as well.</span></span>

* <span data-ttu-id="e7493-126">`Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e7493-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="e7493-127">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="e7493-127">Create the model</span></span>

<span data-ttu-id="e7493-128">Artık modeli oluşturan bir bağlam ve varlık sınıflarını tanımlamak için zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="e7493-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="e7493-129">Silme *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="e7493-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="e7493-130">Oluşturma *Model.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e7493-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="e7493-131">Yeni bir UWP projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e7493-131">Create a new UWP project</span></span>

* <span data-ttu-id="e7493-132">İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="e7493-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="e7493-133">Sol menüden **yüklü > Visual C# > Windows Evrensel**.</span><span class="sxs-lookup"><span data-stu-id="e7493-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="e7493-134">Seçin **boş uygulama (Evrensel Windows)** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="e7493-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="e7493-135">Projeyi adlandırın *Blogging.UWP*, tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="e7493-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="e7493-136">En az bir hedef ve en düşük sürüm kümesine **Windows 10 Fall Creators Update (10.0; derleme 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="e7493-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="e7493-137">İlk geçiş oluştur</span><span class="sxs-lookup"><span data-stu-id="e7493-137">Create the initial migration</span></span>

<span data-ttu-id="e7493-138">Bir modeliniz olduğuna göre ilk çalıştığında bir veritabanı oluşturmak için uygulamasını ayarlama.</span><span class="sxs-lookup"><span data-stu-id="e7493-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="e7493-139">Bu bölümde, ilk geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e7493-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="e7493-140">Aşağıdaki bölümde, uygulama başlatıldığında, bu geçiş uygulayan kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e7493-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="e7493-141">Geçiş Araçları, UWP başlangıç projesi gerektirir, bu nedenle öncelikle oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e7493-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="e7493-142">İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="e7493-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="e7493-143">Sol menüden **yüklü > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e7493-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="e7493-144">Seçin **konsol uygulaması (.NET Core)** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="e7493-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="e7493-145">Projeyi adlandırın *Blogging.Migrations.Startup*, tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e7493-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="e7493-146">Proje Başvuru Ekle *Blogging.Migrations.Startup* için proje *Blogging.Model* proje.</span><span class="sxs-lookup"><span data-stu-id="e7493-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="e7493-147">Şimdi ilk geçiş oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e7493-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="e7493-148">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="e7493-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="e7493-149">Seçin *Blogging.Model* proje olarak **varsayılan proje**.</span><span class="sxs-lookup"><span data-stu-id="e7493-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="e7493-150">İçinde **Çözüm Gezgini**ayarlayın *Blogging.Migrations.Startup* projeyi başlangıç projesi olarak.</span><span class="sxs-lookup"><span data-stu-id="e7493-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="e7493-151">Çalıştırma `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="e7493-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="e7493-152">Bu komut, tablolar, modelinize için başlangıç kümesi oluşturan bir geçiş iskele oluşturulduğunu.</span><span class="sxs-lookup"><span data-stu-id="e7493-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="e7493-153">Uygulama başlangıcında veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="e7493-153">Create the database on app startup</span></span>

<span data-ttu-id="e7493-154">Uygulamanın üzerinde çalıştığı cihazda oluşturulacak veritabanının istediğinden, uygulama başlangıcından yerel veritabanında bekleyen tüm geçişler uygulamak için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e7493-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="e7493-155">Uygulama çalışır, ilk kez bu yerel veritabanı oluşturmayı ilgileniriz.</span><span class="sxs-lookup"><span data-stu-id="e7493-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="e7493-156">Proje Başvuru Ekle *Blogging.UWP* için proje *Blogging.Model* proje.</span><span class="sxs-lookup"><span data-stu-id="e7493-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="e7493-157">Açık *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="e7493-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="e7493-158">Bekleyen tüm geçişler uygulamak için vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e7493-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="e7493-159">Modelinizi değiştirmek kullanırsanız `Add-Migration` karşılık gelen uygulamak için yeni bir geçiş iskele komut, veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e7493-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="e7493-160">Uygulama başladığında bekleyen tüm geçişlerde, her cihazda yerel veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e7493-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="e7493-161">EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="e7493-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="e7493-162">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="e7493-162">Use the model</span></span>

<span data-ttu-id="e7493-163">Şimdi, veri erişimi gerçekleştirdiği modeli kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e7493-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="e7493-164">Açık *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="e7493-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="e7493-165">UI içerik aşağıda belirtildiği ve sayfa yükleme işleyicisi ekleyin</span><span class="sxs-lookup"><span data-stu-id="e7493-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="e7493-166">Şimdi veritabanı ile kullanıcı arabirimini'kurmak wire için kod ekleyin</span><span class="sxs-lookup"><span data-stu-id="e7493-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="e7493-167">Açık *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="e7493-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="e7493-168">Aşağıdaki listeden vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e7493-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="e7493-169">Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e7493-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="e7493-170">İçinde **Çözüm Gezgini**, sağ *Blogging.UWP* proje ve ardından **Dağıt**.</span><span class="sxs-lookup"><span data-stu-id="e7493-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="e7493-171">Ayarlama *Blogging.UWP* başlangıç projesi olarak.</span><span class="sxs-lookup"><span data-stu-id="e7493-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="e7493-172">**Hata ayıklama > hata ayıklama olmadan Başlat**</span><span class="sxs-lookup"><span data-stu-id="e7493-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="e7493-173">Uygulamayı derler ve çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="e7493-173">The app builds and runs.</span></span>

* <span data-ttu-id="e7493-174">Bir URL girin ve tıklayın **Ekle** düğmesi</span><span class="sxs-lookup"><span data-stu-id="e7493-174">Enter a URL and click the **Add** button</span></span>

  ![görüntü](_static/create.png)

  ![görüntü](_static/list.png)

  <span data-ttu-id="e7493-177">Tada!</span><span class="sxs-lookup"><span data-stu-id="e7493-177">Tada!</span></span> <span data-ttu-id="e7493-178">Entity Framework Core çalıştıran basit bir UWP uygulamasında artık var.</span><span class="sxs-lookup"><span data-stu-id="e7493-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7493-179">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="e7493-179">Next steps</span></span>

<span data-ttu-id="e7493-180">EF Core ile UWP kullanırken bilmeniz gereken uyumluluk ve performans için bilgi [EF Core tarafından desteklenen .NET uygulamalarıyla](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="e7493-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="e7493-181">Entity Framework Core özellikler hakkında daha fazla bilgi edinmek için bu belgeleri içindeki diğer makalelere göz atın.</span><span class="sxs-lookup"><span data-stu-id="e7493-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
