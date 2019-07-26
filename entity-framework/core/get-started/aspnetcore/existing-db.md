---
title: ASP.NET Core var olan veritabanı ile çalışmaya başlama-EF Core
author: rowanmiller
description: Mevcut bir veritabanıyla ASP.NET Core EF Core kullanmaya başlama
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497520"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="4ecbb-103">Mevcut bir veritabanıyla ASP.NET Core EF Core kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="4ecbb-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="4ecbb-104">Bu öğreticide, Entity Framework Core kullanarak temel veri erişimi gerçekleştiren bir ASP.NET Core MVC uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="4ecbb-105">Bir Entity Framework modeli oluşturmak için var olan bir veritabanına ters mühendislik uygulamalısınız.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="4ecbb-106">[Bu makalenin örneğini GitHub 'Da görüntüleyin](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="4ecbb-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ecbb-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4ecbb-107">Prerequisites</span></span>

<span data-ttu-id="4ecbb-108">Aşağıdaki yazılımı yükler:</span><span class="sxs-lookup"><span data-stu-id="4ecbb-108">Install the following software:</span></span>

* <span data-ttu-id="4ecbb-109">Bu iş yükleriyle [Visual Studio 2017 15,7](https://www.visualstudio.com/downloads/) :</span><span class="sxs-lookup"><span data-stu-id="4ecbb-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="4ecbb-110">**ASP.net ve Web geliştirme** ( **Web & bulutu**altında)</span><span class="sxs-lookup"><span data-stu-id="4ecbb-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="4ecbb-111">**.NET Core platformlar arası geliştirme** ( **diğer araç kümeleri**altında)</span><span class="sxs-lookup"><span data-stu-id="4ecbb-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="4ecbb-112">[.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="4ecbb-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="4ecbb-113">Blog veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ecbb-113">Create Blogging database</span></span>

<span data-ttu-id="4ecbb-114">Bu öğretici, LocalDb örneğinizdeki bir **Blog** veritabanını var olan veritabanı olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="4ecbb-115">**Blog** veritabanını başka bir öğreticinin parçası olarak zaten oluşturduysanız, bu adımları atlayın.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="4ecbb-116">Visual Studio 'Yu aç</span><span class="sxs-lookup"><span data-stu-id="4ecbb-116">Open Visual Studio</span></span>
* <span data-ttu-id="4ecbb-117">**Araçlar-> veritabanına bağlan...**</span><span class="sxs-lookup"><span data-stu-id="4ecbb-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="4ecbb-118">**Microsoft SQL Server** seçin ve **devam** ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="4ecbb-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="4ecbb-119">**Sunucu adı** olarak **(LocalDB) \mssqllocaldb** yazın</span><span class="sxs-lookup"><span data-stu-id="4ecbb-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="4ecbb-120">**Asıl** **veritabanı adı** olarak girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="4ecbb-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="4ecbb-121">Ana veritabanı artık **Sunucu Gezgini** Içindeki **veri bağlantıları** altında görüntülenir</span><span class="sxs-lookup"><span data-stu-id="4ecbb-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="4ecbb-122">**Sunucu Gezgini** veritabanında veritabanına sağ tıklayın ve **Yeni sorgu** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="4ecbb-123">Aşağıda listelenen betiği sorgu düzenleyicisine Kopyala</span><span class="sxs-lookup"><span data-stu-id="4ecbb-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="4ecbb-124">Sorgu düzenleyicisine sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="4ecbb-125">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ecbb-125">Create a new project</span></span>

* <span data-ttu-id="4ecbb-126">Visual Studio 2017 'yi açın</span><span class="sxs-lookup"><span data-stu-id="4ecbb-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="4ecbb-127">**Dosya yeni > projesi >...**</span><span class="sxs-lookup"><span data-stu-id="4ecbb-127">**File > New > Project...**</span></span>
* <span data-ttu-id="4ecbb-128">Soldaki menüden, **Visual C# > Web > yüklü** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="4ecbb-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="4ecbb-129">**ASP.NET Core Web uygulaması** proje şablonunu seçin</span><span class="sxs-lookup"><span data-stu-id="4ecbb-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="4ecbb-130">Ad olarak **Efgetstarted. AspNetCore. ExistingDb** yazın (kodda daha sonra kullanılan ad alanıyla tam olarak eşleşmelidir) ve **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="4ecbb-131">**Yeni ASP.NET Core Web uygulaması** iletişim kutusunun görüntülenmesini bekleyin</span><span class="sxs-lookup"><span data-stu-id="4ecbb-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="4ecbb-132">Hedef çerçeve açılan menüsünün **.NET Core**olarak ayarlandığından ve sürüm açılan menüsünün **ASP.NET Core 2,1** olarak ayarlandığından emin olun</span><span class="sxs-lookup"><span data-stu-id="4ecbb-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="4ecbb-133">**Web uygulaması (Model-View-Controller)** şablonunu seçin</span><span class="sxs-lookup"><span data-stu-id="4ecbb-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="4ecbb-134">**Kimlik doğrulamasının** **kimlik doğrulaması yok** olarak ayarlandığından emin olun</span><span class="sxs-lookup"><span data-stu-id="4ecbb-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="4ecbb-135">          **Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="4ecbb-136">Entity Framework Core yüklensin</span><span class="sxs-lookup"><span data-stu-id="4ecbb-136">Install Entity Framework Core</span></span>

<span data-ttu-id="4ecbb-137">EF Core yüklemek için, hedeflemek istediğiniz EF Core veritabanı sağlayıcılarının paketini yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="4ecbb-138">Kullanılabilir sağlayıcıların listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="4ecbb-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="4ecbb-139">Öğretici SQL Server kullandığından, bu öğretici için bir sağlayıcı paketi yüklemenize gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="4ecbb-140">SQL Server sağlayıcısı paketi [Microsoft. AspnetCore. app metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)'e dahildir.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="4ecbb-141">Modelinize ters mühendislik</span><span class="sxs-lookup"><span data-stu-id="4ecbb-141">Reverse engineer your model</span></span>

<span data-ttu-id="4ecbb-142">Şimdi de mevcut veritabanınıza göre EF modeli oluşturma zamanı.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="4ecbb-143">**Araçlar – > NuGet Paket Yöneticisi – > Paket Yöneticisi konsolu**</span><span class="sxs-lookup"><span data-stu-id="4ecbb-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="4ecbb-144">Varolan veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4ecbb-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="4ecbb-145">Belirten `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`bir hata alırsanız Visual Studio 'yu kapatıp yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="4ecbb-146">Yukarıdaki komuta `-Tables` bağımsız değişkenini ekleyerek hangi tabloları için varlık oluşturmak istediğinizi belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="4ecbb-147">Örneğin: `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="4ecbb-148">Tersine mühendislik işlemi, mevcut veritabanının şemasına bağlı`Blog.cs`olarak varlık sınıfları ( & `Post.cs`) ve`BloggingContext.cs`türetilmiş bir bağlam () oluşturdu.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="4ecbb-149">Varlık sınıfları, sorguladığınız C# ve kaydedilecektir verileri temsil eden basit nesnelerdir.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="4ecbb-150">`Blog` Ve`Post` varlık sınıfları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4ecbb-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="4ecbb-151">Yavaş yüklemeyi etkinleştirmek için, gezinti özelliklerini `virtual` (blog.Post ve post. blog) yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="4ecbb-152">Bağlam, veritabanı ile bir oturumu temsil eder ve varlık sınıflarının örneklerini sorgulamanızı ve kaydetmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="4ecbb-153">Bağlama ekleme ile bağlamını kaydetme</span><span class="sxs-lookup"><span data-stu-id="4ecbb-153">Register your context with dependency injection</span></span>

<span data-ttu-id="4ecbb-154">Bağımlılık ekleme kavramı ASP.NET Core merkezidir.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="4ecbb-155">Hizmetler (gibi `BloggingContext`), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="4ecbb-156">Bu hizmetleri gerektiren bileşenler (MVC denetleyicileriniz gibi), daha sonra bu hizmetleri Oluşturucu parametreleri veya özellikleri aracılığıyla sağlıyordu.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="4ecbb-157">Bağımlılık ekleme hakkında daha fazla bilgi için ASP.NET sitesindeki [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="4ecbb-158">Startup.cs 'de bağlamını kaydetme ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4ecbb-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="4ecbb-159">MVC denetleyicileri `BloggingContext` için kullanılabilir hale getirmek için bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="4ecbb-160">Açık **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="4ecbb-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="4ecbb-161">Aşağıdaki `using` deyimlerini dosyanın başlangıcına ekleyin</span><span class="sxs-lookup"><span data-stu-id="4ecbb-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="4ecbb-162">Artık `AddDbContext(...)` yöntemi bir hizmet olarak kaydetmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="4ecbb-163">`ConfigureServices(...)` Yöntemi bulun</span><span class="sxs-lookup"><span data-stu-id="4ecbb-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="4ecbb-164">Bağlamı hizmet olarak kaydetmek için aşağıdaki vurgulanmış kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="4ecbb-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="4ecbb-165">Gerçek bir uygulamada, genellikle bağlantı dizesini bir yapılandırma dosyasına veya ortam değişkenine yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="4ecbb-166">Kolaylık sağlaması için, bu öğretici kodu kod içinde tanımlamanızı ister.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="4ecbb-167">Daha fazla bilgi için bkz. [bağlantı dizeleri](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="4ecbb-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="4ecbb-168">Denetleyici ve görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ecbb-168">Create a controller and views</span></span>

* <span data-ttu-id="4ecbb-169">**Çözüm Gezgini** ' de **denetleyiciler** klasörüne sağ tıklayın ve **Ekle-> denetleyicisi ' ni seçin...**</span><span class="sxs-lookup"><span data-stu-id="4ecbb-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="4ecbb-170">**Görünümler Ile MVC denetleyicisi ' ni seçin, Entity Framework kullanarak** **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="4ecbb-171">**Model sınıfını** **Blog** ve **veri bağlamı sınıfına** **BloggingContext** olarak ayarla</span><span class="sxs-lookup"><span data-stu-id="4ecbb-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="4ecbb-172">**Ekle** 'ye tıklayın</span><span class="sxs-lookup"><span data-stu-id="4ecbb-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="4ecbb-173">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="4ecbb-173">Run the application</span></span>

<span data-ttu-id="4ecbb-174">Artık uygulamayı çalışır durumda görmek için çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="4ecbb-175">**Hata ayıklama-> hata ayıklama olmadan Başlat**</span><span class="sxs-lookup"><span data-stu-id="4ecbb-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="4ecbb-176">Uygulama bir Web tarayıcısında oluşturulup açılıyor</span><span class="sxs-lookup"><span data-stu-id="4ecbb-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="4ecbb-177">Gidin `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="4ecbb-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="4ecbb-178">**Yeni oluştur** ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="4ecbb-178">Click **Create New**</span></span>
* <span data-ttu-id="4ecbb-179">Yeni blog için bir **URL** girin ve **Oluştur** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![sayfası oluşturma](_static/create.png)

  ![Dizin sayfası](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="4ecbb-182">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="4ecbb-182">Next steps</span></span>

<span data-ttu-id="4ecbb-183">Bağlam ve varlık sınıflarının nasıl kullanılacağı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="4ecbb-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="4ecbb-184">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="4ecbb-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="4ecbb-185">Entity Framework Core araçları başvurusu-.NET CLı</span><span class="sxs-lookup"><span data-stu-id="4ecbb-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="4ecbb-186">Entity Framework Core araçları başvurusu-Paket Yöneticisi konsolu</span><span class="sxs-lookup"><span data-stu-id="4ecbb-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
