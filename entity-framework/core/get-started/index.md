---
title: Başlarken-EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: d46c4bb9ac6c8f718b4da5ecd82d54710d41935f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824493"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="8b4a5-102">EF Core kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="8b4a5-102">Getting Started with EF Core</span></span>

<span data-ttu-id="8b4a5-103">Bu öğreticide, Entity Framework Core kullanarak bir SQLite veritabanına yönelik veri erişimi gerçekleştiren bir .NET Core konsol uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="8b4a5-104">Windows üzerinde Visual Studio 'yu kullanarak veya Windows, macOS veya Linux üzerinde .NET Core CLI kullanarak öğreticiyi izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="8b4a5-105">[Bu makalenin örneğini GitHub 'Da görüntüleyin](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="8b4a5-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b4a5-106">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="8b4a5-106">Prerequisites</span></span>

<span data-ttu-id="8b4a5-107">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-107">Install the following software:</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8b4a5-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8b4a5-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8b4a5-109">[.NET Core 3,0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="8b4a5-109">[.NET Core 3.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b4a5-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b4a5-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b4a5-111">[Visual Studio 2019 sürüm 16,3 veya üzeri](https://www.visualstudio.com/downloads/) bu iş yükü:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="8b4a5-112">**.NET Core platformlar arası geliştirme** ( **diğer araç kümeleri**altında)</span><span class="sxs-lookup"><span data-stu-id="8b4a5-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="8b4a5-113">Yeni bir proje oluşturun</span><span class="sxs-lookup"><span data-stu-id="8b4a5-113">Create a new project</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8b4a5-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8b4a5-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b4a5-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b4a5-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b4a5-116">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="8b4a5-116">Open Visual Studio</span></span>
* <span data-ttu-id="8b4a5-117">**Yeni proje oluştur ' a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="8b4a5-117">Click **Create a new project**</span></span>
* <span data-ttu-id="8b4a5-118">Etiketi ile **konsol uygulaması (.NET Core)** seçeneğini belirleyin ve ileri ' ye tıklayın. **C#**</span><span class="sxs-lookup"><span data-stu-id="8b4a5-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="8b4a5-119">Ad için **Efgetstarted** girin ve **Oluştur** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="8b4a5-120">Entity Framework Core yüklensin</span><span class="sxs-lookup"><span data-stu-id="8b4a5-120">Install Entity Framework Core</span></span>

<span data-ttu-id="8b4a5-121">EF Core yüklemek için, hedeflemek istediğiniz EF Core veritabanı sağlayıcılarının paketini yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="8b4a5-122">Bu öğretici, .NET Core 'un desteklediği tüm platformlarda çalıştığı için SQLite kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="8b4a5-123">Kullanılabilir sağlayıcıların bir listesi için bkz. [veritabanı sağlayıcıları](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8b4a5-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8b4a5-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8b4a5-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b4a5-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b4a5-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b4a5-126">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi konsolu**</span><span class="sxs-lookup"><span data-stu-id="8b4a5-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="8b4a5-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="8b4a5-128">İpucu: Ayrıca, projeye sağ tıklayıp **NuGet Paketlerini Yönet** ' i seçerek paketleri yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="8b4a5-129">Modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b4a5-129">Create the model</span></span>

<span data-ttu-id="8b4a5-130">Modeli oluşturan bir bağlam sınıfı ve varlık sınıfları tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-130">Define a context class and entity classes that make up the model.</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8b4a5-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8b4a5-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8b4a5-132">Proje dizininde aşağıdaki kodla **model.cs** oluşturun</span><span class="sxs-lookup"><span data-stu-id="8b4a5-132">In the project directory, create **Model.cs** with the following code</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b4a5-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b4a5-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b4a5-134">Projeye sağ tıklayın ve **> sınıfı Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="8b4a5-135">Ad olarak **model.cs** girin ve **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="8b4a5-136">Dosyanın içeriğini aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="8b4a5-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="8b4a5-137">EF Core Ayrıca, varolan bir veritabanından bir modele [ters mühendislik](../managing-schemas/scaffolding.md) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="8b4a5-138">İpucu: gerçek bir uygulamada, her bir sınıfı ayrı bir dosyaya yerleştirip [bağlantı dizesini](../miscellaneous/connection-strings.md) bir yapılandırma dosyasına veya ortam değişkenine yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="8b4a5-139">Öğreticiyi bir şekilde korumak için her şey tek bir dosyada yer alır.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="8b4a5-140">Veritabanını oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b4a5-140">Create the database</span></span>

<span data-ttu-id="8b4a5-141">Aşağıdaki adımlar bir veritabanı oluşturmak için [geçişleri](xref:core/managing-schemas/migrations/index) kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8b4a5-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8b4a5-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8b4a5-143">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="8b4a5-144">Bu, bir projede komutu çalıştırmak için gerekli olan [DotNet EF](../miscellaneous/cli/dotnet.md) ve tasarım paketini de yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="8b4a5-145">`migrations` komutu, modelin ilk tablo kümesini oluşturmak için bir geçişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="8b4a5-146">`database update` komutu veritabanını oluşturur ve yeni geçişi uygular.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-146">The `database update` command creates the database and applies the new migration to it.</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b4a5-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b4a5-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b4a5-148">**Paket Yöneticisi konsolunda** aşağıdaki komutları çalıştırın</span><span class="sxs-lookup"><span data-stu-id="8b4a5-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="8b4a5-149">Bu, [EF Core Için PMC araçlarını](../miscellaneous/cli/powershell.md)kurar.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="8b4a5-150">`Add-Migration` komutu, modelin ilk tablo kümesini oluşturmak için bir geçişi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="8b4a5-151">`Update-Database` komutu veritabanını oluşturur ve yeni geçişi uygular.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="8b4a5-152">Oluşturma, okuma, güncelleştirme & silme</span><span class="sxs-lookup"><span data-stu-id="8b4a5-152">Create, read, update & delete</span></span>

* <span data-ttu-id="8b4a5-153">*Program.cs* açın ve içeriğini şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="8b4a5-154">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="8b4a5-154">Run the app</span></span>

## <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8b4a5-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8b4a5-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b4a5-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b4a5-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8b4a5-157">Visual Studio, .NET Core konsol uygulamaları çalıştırırken tutarsız bir çalışma dizini kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="8b4a5-158">(bkz. [DotNet/Project-System # 3619](https://github.com/dotnet/project-system/issues/3619)) Bu durum, oluşan bir özel durumla sonuçlanır: *böyle bir tablo: blogları*.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="8b4a5-159">Çalışma dizinini güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-159">To update the working directory:</span></span>

* <span data-ttu-id="8b4a5-160">Projeye sağ tıklayın ve **Proje dosyasını Düzenle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="8b4a5-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="8b4a5-161">*TargetFramework* özelliğinin hemen altında aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="8b4a5-162">Dosyayı kaydetme</span><span class="sxs-lookup"><span data-stu-id="8b4a5-162">Save the file</span></span>

<span data-ttu-id="8b4a5-163">Artık uygulamayı çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8b4a5-163">Now you can run the app:</span></span>

* <span data-ttu-id="8b4a5-164">**Hata ayıklama > hata ayıklama olmadan Başlat**</span><span class="sxs-lookup"><span data-stu-id="8b4a5-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="8b4a5-165">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="8b4a5-165">Next steps</span></span>

* <span data-ttu-id="8b4a5-166">Bir Web uygulamasında EF Core kullanmak için [ASP.NET Core öğreticisini](/aspnet/core/data/ef-rp/intro) izleyin</span><span class="sxs-lookup"><span data-stu-id="8b4a5-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="8b4a5-167">[LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="8b4a5-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="8b4a5-168">[Gerekli](xref:core/modeling/required-optional) ve [en fazla uzunluk](xref:core/modeling/max-length) gibi Işlemleri belirtmek için [modelinizi yapılandırın](xref:core/modeling/index)</span><span class="sxs-lookup"><span data-stu-id="8b4a5-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/required-optional) and [maximum length](xref:core/modeling/max-length)</span></span>
* <span data-ttu-id="8b4a5-169">Modelinizi değiştirdikten sonra veritabanı şemasını güncelleştirmek için [geçişleri](xref:core/managing-schemas/migrations/index) kullanma</span><span class="sxs-lookup"><span data-stu-id="8b4a5-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
