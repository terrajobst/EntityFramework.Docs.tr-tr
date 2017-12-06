---
title: "UWP - yeni veritabanı - EF Çekirdeğinde Başlarken"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="213bc-102">Evrensel Windows Platformu (UWP) yeni bir veritabanı EF Çekirdeğinde ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="213bc-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="213bc-103">Bu öğretici EF çekirdek 2.0.1 (ASP.NET Core ve .NET Core SDK 2.0.3 yayımlanan) kullanır.</span><span class="sxs-lookup"><span data-stu-id="213bc-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="213bc-104">EF çekirdek 2.0.0 iyi bir UWP deneyimi için gereken bazı önemli hata düzeltmeleri eksik.</span><span class="sxs-lookup"><span data-stu-id="213bc-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="213bc-105">Bu kılavuzda, Entity Framework kullanarak yerel bir SQLite veritabanı karşı temel veri erişimi gerçekleştirdiği bir evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="213bc-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="213bc-106">**Anonim türler UWP LINQ sorgularında önleme göz önünde bulundurun**.</span><span class="sxs-lookup"><span data-stu-id="213bc-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="213bc-107">Bir UWP uygulamasını Uygulama mağazası dağıtılması .NET yerel ile derlenecek uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="213bc-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="213bc-108">Anonim türler sorgularıyla daha zayıf bir performans yerel .NET vardır.</span><span class="sxs-lookup"><span data-stu-id="213bc-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="213bc-109">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) github'da.</span><span class="sxs-lookup"><span data-stu-id="213bc-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="213bc-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="213bc-110">Prerequisites</span></span>

<span data-ttu-id="213bc-111">Aşağıdaki öğeler bu yönlendirmeyi tamamlamak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="213bc-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="213bc-112">[Windows 10 sonbaharda oluşturucuları güncelleştirmesi](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="213bc-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="213bc-113">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="213bc-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="213bc-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.4 veya ile sonraki bir sürümü **Evrensel Windows platformu geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="213bc-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="213bc-115">Yeni bir modeli projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="213bc-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="213bc-116">Model Paket Yöneticisi konsolunda geçişler komutları çalıştırılabilmesi için UWP olmayan projesinde yerleştirilmesi gerekir UWP projeleri etkileşimde şekilde .NET Core araçlarında sınırlamaları nedeniyle</span><span class="sxs-lookup"><span data-stu-id="213bc-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="213bc-117">Açık Visual Studio</span><span class="sxs-lookup"><span data-stu-id="213bc-117">Open Visual Studio</span></span>

* <span data-ttu-id="213bc-118">Dosya > Yeni > Proje...</span><span class="sxs-lookup"><span data-stu-id="213bc-118">File > New > Project...</span></span>

* <span data-ttu-id="213bc-119">Sol menüden şablonları seçin > Visual C#</span><span class="sxs-lookup"><span data-stu-id="213bc-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="213bc-120">Seçin **sınıf kitaplığı (.NET standart)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="213bc-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="213bc-121">Proje bir ad verin ve tıklatın **Tamam**</span><span class="sxs-lookup"><span data-stu-id="213bc-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="213bc-122">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="213bc-122">Install Entity Framework</span></span>

<span data-ttu-id="213bc-123">EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="213bc-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="213bc-124">Bu kılavuzda SQLite kullanılır.</span><span class="sxs-lookup"><span data-stu-id="213bc-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="213bc-125">Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="213bc-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="213bc-126">Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="213bc-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="213bc-127">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="213bc-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="213bc-128">Bu kılavuzda daha sonra biz de bazı Entity Framework Araçları veritabanını korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="213bc-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="213bc-129">Böylece biz de araçları paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="213bc-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="213bc-130">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="213bc-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="213bc-131">.Csproj dosyasını düzenleyin ve değiştirme `<TargetFramework>netstandard2.0</TargetFramework>` ile`<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span><span class="sxs-lookup"><span data-stu-id="213bc-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="213bc-132">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="213bc-132">Create your model</span></span>

<span data-ttu-id="213bc-133">Şimdi modelinizi yapmak bağlamını ve varlık sınıflarını tanımlamak için zaman yapılır.</span><span class="sxs-lookup"><span data-stu-id="213bc-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="213bc-134">Proje > sınıfı Ekle...</span><span class="sxs-lookup"><span data-stu-id="213bc-134">Project > Add Class...</span></span>

* <span data-ttu-id="213bc-135">Girin *Model.cs* tıklatın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="213bc-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="213bc-136">Dosyasının içeriğini aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="213bc-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="213bc-137">Yeni bir UWP projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="213bc-137">Create a new UWP project</span></span>

* <span data-ttu-id="213bc-138">Açık Visual Studio</span><span class="sxs-lookup"><span data-stu-id="213bc-138">Open Visual Studio</span></span>

* <span data-ttu-id="213bc-139">Dosya > Yeni > Proje...</span><span class="sxs-lookup"><span data-stu-id="213bc-139">File > New > Project...</span></span>

* <span data-ttu-id="213bc-140">Sol menüden şablonları seçin > Visual C# > Windows Evrensel</span><span class="sxs-lookup"><span data-stu-id="213bc-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="213bc-141">Seçin **boş uygulama (Evrensel Windows)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="213bc-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="213bc-142">Proje bir ad verin ve tıklatın **Tamam**</span><span class="sxs-lookup"><span data-stu-id="213bc-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="213bc-143">Hedef ve en düşük sürümler için en az ayarlayın`Windows 10 Fall Creators Update (10.0; build 16299.0)`</span><span class="sxs-lookup"><span data-stu-id="213bc-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="213bc-144">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="213bc-144">Create your database</span></span>

<span data-ttu-id="213bc-145">Bir model sahip olduğunuza göre sizin için bir veritabanı oluşturmak için geçiş kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="213bc-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="213bc-146">Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="213bc-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="213bc-147">Modeli projesi varsayılan proje olarak seçin ve başlangıç projesi olarak ayarla</span><span class="sxs-lookup"><span data-stu-id="213bc-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="213bc-148">Çalıştırma `Add-Migration MyFirstMigration` tabloları modeliniz için ilk kümesi oluşturmak için bir geçiş için iskele kurmak.</span><span class="sxs-lookup"><span data-stu-id="213bc-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="213bc-149">Uygulamanın çalıştığı cihaz üzerinde oluşturulacak veritabanına istiyoruz olduğundan, uygulama başlatma yerel veritabanında bekleyen tüm geçişleri uygulamak için bazı kod ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="213bc-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="213bc-150">Uygulama çalıştığında, ilk kez bu bize için yerel veritabanı oluşturmayı ilgilenebilmek.</span><span class="sxs-lookup"><span data-stu-id="213bc-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="213bc-151">Sağ **App.xaml** içinde **Çözüm Gezgini** seçip **görünümü kodu**</span><span class="sxs-lookup"><span data-stu-id="213bc-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="213bc-152">Vurgulanan kullanma dosyasının başlangıcına ekleyin</span><span class="sxs-lookup"><span data-stu-id="213bc-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="213bc-153">Bekleyen tüm geçişleri uygulamak için vurgulanmış kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="213bc-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="213bc-154">Modelinize gelecekteki değişiklikler yapmak isterseniz, kullanabileceğiniz `Add-Migration` karşılık gelen uygulamak için yeni bir geçiş için iskele kurmak komut veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="213bc-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="213bc-155">Uygulama başladığında bekleyen tüm geçişleri her cihazda yerel veritabanı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="213bc-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="213bc-156">EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulandı izlemek için veritabanı tablosunda.</span><span class="sxs-lookup"><span data-stu-id="213bc-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="213bc-157">Modelinizi kullanın</span><span class="sxs-lookup"><span data-stu-id="213bc-157">Use your model</span></span>

<span data-ttu-id="213bc-158">Veri erişimi gerçekleştirdiği modelinizi artık kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="213bc-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="213bc-159">Açık *MainPage.xaml*</span><span class="sxs-lookup"><span data-stu-id="213bc-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="213bc-160">Aşağıda vurgulanan UI içerik ve sayfa yükleme işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="213bc-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="213bc-161">UI veritabanıyla kablo kod artık ekleyeceğiz</span><span class="sxs-lookup"><span data-stu-id="213bc-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="213bc-162">Sağ **MainPage.xaml** içinde **Çözüm Gezgini** seçip **görünümü kodu**</span><span class="sxs-lookup"><span data-stu-id="213bc-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="213bc-163">Aşağıdaki listeden vurgulanmış kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="213bc-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="213bc-164">Şimdi eylemde görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="213bc-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="213bc-165">Hata ayıklama > hata ayıklama olmadan Başlat</span><span class="sxs-lookup"><span data-stu-id="213bc-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="213bc-166">Uygulama oluşturma ve başlatma</span><span class="sxs-lookup"><span data-stu-id="213bc-166">The application will build and launch</span></span>

* <span data-ttu-id="213bc-167">Bir URL girin ve tıklayın **Ekle** düğmesi</span><span class="sxs-lookup"><span data-stu-id="213bc-167">Enter a URL and click the **Add** button</span></span>

![görüntü](_static/create.png)

![görüntü](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="213bc-170">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="213bc-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="213bc-171">`SaveChanges()`Performans geliştirilmiş uygulayarak [ `INotifyPropertyChanged` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [ `INotifyPropertyChanging` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [ `INotifyCollectionChanged` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) , varlık türleri ve kullanarak `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="213bc-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="213bc-172">Tada!</span><span class="sxs-lookup"><span data-stu-id="213bc-172">Tada!</span></span> <span data-ttu-id="213bc-173">Entity Framework çalışan basit bir UWP uygulamasında artık sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="213bc-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="213bc-174">Entity Framework'ün özellikler hakkında daha fazla bilgi edinmek için bu belgeleri içindeki diğer makalelere göz atın.</span><span class="sxs-lookup"><span data-stu-id="213bc-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
