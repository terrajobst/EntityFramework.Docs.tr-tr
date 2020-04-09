---
title: Başlarken - EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416885"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="e154d-102">EF Core ile Başlarken</span><span class="sxs-lookup"><span data-stu-id="e154d-102">Getting Started with EF Core</span></span>

<span data-ttu-id="e154d-103">Bu eğitimde, Entity Framework Core'u kullanarak bir SQLite veritabanına karşı veri erişimi gerçekleştiren bir .NET Core konsol uygulaması oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="e154d-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="e154d-104">Öğreticiyi Windows'da Visual Studio'yu kullanarak veya Windows, macOS veya Linux'ta .NET Core CLI'yi kullanarak takip edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e154d-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="e154d-105">[Bu makalenin örneğini GitHub'da görüntüleyin.](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted)</span><span class="sxs-lookup"><span data-stu-id="e154d-105">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e154d-106">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="e154d-106">Prerequisites</span></span>

<span data-ttu-id="e154d-107">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="e154d-107">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e154d-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e154d-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e154d-109">[.NET Çekirdek SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="e154d-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e154d-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e154d-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e154d-111">[Visual Studio 2019 sürüm 16.3 veya daha sonra](https://www.visualstudio.com/downloads/) bu iş yükü ile:</span><span class="sxs-lookup"><span data-stu-id="e154d-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="e154d-112">**.NET Çekirdek çapraz platform geliştirme** **(Diğer Araç Setleri**altında)</span><span class="sxs-lookup"><span data-stu-id="e154d-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="e154d-113">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="e154d-113">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e154d-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e154d-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="e154d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e154d-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e154d-116">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="e154d-116">Open Visual Studio</span></span>
* <span data-ttu-id="e154d-117">**Yeni proje Oluştur'u** tıklatın</span><span class="sxs-lookup"><span data-stu-id="e154d-117">Click **Create a new project**</span></span>
* <span data-ttu-id="e154d-118">**C#** etiketiyle **Konsol Uygulaması'nı (.NET Core)** seçin ve **İleri'yi** tıklatın</span><span class="sxs-lookup"><span data-stu-id="e154d-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="e154d-119">Ad için **EFGetStarted'u** girin ve **Oluştur'u** tıklatın</span><span class="sxs-lookup"><span data-stu-id="e154d-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="e154d-120">Varlık Çerçeve Çekirdeğini Yükle</span><span class="sxs-lookup"><span data-stu-id="e154d-120">Install Entity Framework Core</span></span>

<span data-ttu-id="e154d-121">EF Core'u yüklemek için, hedeflemek istediğiniz EF Core veritabanı sağlayıcısı(lar) için paketi yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="e154d-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="e154d-122">Bu öğretici, .NET Core'un desteklediği tüm platformlarda çalıştığından SQLite kullanır.</span><span class="sxs-lookup"><span data-stu-id="e154d-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="e154d-123">Kullanılabilir sağlayıcıların listesi için [Veritabanı Sağlayıcıları'na](../providers/index.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="e154d-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e154d-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e154d-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="e154d-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e154d-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e154d-126">**NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu > Araçlar**</span><span class="sxs-lookup"><span data-stu-id="e154d-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="e154d-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e154d-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="e154d-128">İpucu: Projeye sağ tıklayıp **NuGet Paketlerini Yönet'i** seçerek paketleri de yükleyebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="e154d-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="e154d-129">Modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="e154d-129">Create the model</span></span>

<span data-ttu-id="e154d-130">Modeli oluşturan bağlam sınıfı ve varlık sınıfları tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="e154d-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e154d-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e154d-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e154d-132">Proje dizininde, aşağıdaki kodla **Model.cs**</span><span class="sxs-lookup"><span data-stu-id="e154d-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e154d-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e154d-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e154d-134">Projeye sağ tıklayın ve **> Sınıfı Ekle'yi** seçin</span><span class="sxs-lookup"><span data-stu-id="e154d-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="e154d-135">Ad olarak **Model.cs** girin ve **Ekle'yi** tıklatın</span><span class="sxs-lookup"><span data-stu-id="e154d-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="e154d-136">Dosyanın içeriğini aşağıdaki kodla değiştirme</span><span class="sxs-lookup"><span data-stu-id="e154d-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="e154d-137">EF Core, varolan bir veritabanından bir modeli [tersine çevirebilir.](../managing-schemas/scaffolding.md)</span><span class="sxs-lookup"><span data-stu-id="e154d-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="e154d-138">İpucu: Gerçek bir uygulamada, her sınıfı ayrı bir dosyaya koyar ve [bağlantı dizesini](../miscellaneous/connection-strings.md) bir yapılandırma dosyasına veya ortam değişkenine koyarsınız.</span><span class="sxs-lookup"><span data-stu-id="e154d-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="e154d-139">Öğreticiyi basit tutmak için her şey tek bir dosyada bulunur.</span><span class="sxs-lookup"><span data-stu-id="e154d-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e154d-140">Veritabanını oluşturma</span><span class="sxs-lookup"><span data-stu-id="e154d-140">Create the database</span></span>

<span data-ttu-id="e154d-141">Aşağıdaki adımlar, bir veritabanı oluşturmak için [geçişleri](xref:core/managing-schemas/migrations/index) kullanır.</span><span class="sxs-lookup"><span data-stu-id="e154d-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e154d-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e154d-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e154d-143">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e154d-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="e154d-144">Bu, [dotnet ef'i](../miscellaneous/cli/dotnet.md) ve projedeki komutu çalıştırmak için gereken tasarım paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="e154d-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="e154d-145">Komut, `migrations` model için ilk tablo kümesini oluşturmak için bir geçiş iskelesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e154d-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e154d-146">Komut `database update` veritabanını oluşturur ve yeni geçişi uygular.</span><span class="sxs-lookup"><span data-stu-id="e154d-146">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e154d-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e154d-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e154d-148">**Paket Yöneticisi Konsolunda** aşağıdaki komutları çalıştırın</span><span class="sxs-lookup"><span data-stu-id="e154d-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="e154d-149">Bu, [EF Core için PMC araçlarını](../miscellaneous/cli/powershell.md)yükler.</span><span class="sxs-lookup"><span data-stu-id="e154d-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="e154d-150">Komut, `Add-Migration` model için ilk tablo kümesini oluşturmak için bir geçiş iskelesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e154d-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e154d-151">Komut `Update-Database` veritabanını oluşturur ve yeni geçişi uygular.</span><span class="sxs-lookup"><span data-stu-id="e154d-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="e154d-152">Oluşturma, okuma, güncelleme & silme</span><span class="sxs-lookup"><span data-stu-id="e154d-152">Create, read, update & delete</span></span>

* <span data-ttu-id="e154d-153">*Program.cs* açın ve içindekileri aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e154d-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="e154d-154">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e154d-154">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e154d-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e154d-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="e154d-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e154d-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e154d-157">Visual Studio,.NET Core konsol uygulamalarını çalıştırırken tutarsız bir çalışma dizini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e154d-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="e154d-158">(bkz: [dotnet/proje-sistem#3619](https://github.com/dotnet/project-system/issues/3619)) Bu bir özel durum atılıyor sonuçlanır: *böyle bir tablo: Bloglar*.</span><span class="sxs-lookup"><span data-stu-id="e154d-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="e154d-159">Çalışma dizini güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="e154d-159">To update the working directory:</span></span>

* <span data-ttu-id="e154d-160">Projeye sağ tıklayın ve **Proje Dosyasını Edit'i** seçin</span><span class="sxs-lookup"><span data-stu-id="e154d-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="e154d-161">*TargetFramework* özelliğinin hemen altında aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e154d-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="e154d-162">Dosyayı kaydetme</span><span class="sxs-lookup"><span data-stu-id="e154d-162">Save the file</span></span>

<span data-ttu-id="e154d-163">Artık uygulamayı çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e154d-163">Now you can run the app:</span></span>

* <span data-ttu-id="e154d-164">**Hata Ayıklama > Hata Ayıklama olmadan Başlat**</span><span class="sxs-lookup"><span data-stu-id="e154d-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="e154d-165">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="e154d-165">Next steps</span></span>

* <span data-ttu-id="e154d-166">Bir web uygulamasında EF Core'u kullanmak için [ASP.NET Core Tutorial'ı](/aspnet/core/data/ef-rp/intro) izleyin</span><span class="sxs-lookup"><span data-stu-id="e154d-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="e154d-167">[LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="e154d-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="e154d-168">Gerekli [ve](xref:core/modeling/entity-properties#required-and-optional-properties) [maksimum uzunluk](xref:core/modeling/entity-properties#maximum-length) gibi şeyleri belirtecek [şekilde modelinizi yapılandırın](xref:core/modeling/index)</span><span class="sxs-lookup"><span data-stu-id="e154d-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="e154d-169">Modelinizi değiştirdikten sonra veritabanı şemasını güncelleştirmek için [Geçişler'i](xref:core/managing-schemas/migrations/index) kullanma</span><span class="sxs-lookup"><span data-stu-id="e154d-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
