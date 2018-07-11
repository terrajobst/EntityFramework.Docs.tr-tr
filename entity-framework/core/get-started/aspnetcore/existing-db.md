---
title: Üzerinde ASP.NET Core - mevcut veritabanı - EF Core kullanmaya başlama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949159"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="8998f-102">Mevcut bir veritabanı ile ASP.NET Core üzerinde EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="8998f-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="8998f-103">Bu kılavuzda, Entity Framework kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8998f-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="8998f-104">Tersine mühendislik, varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="8998f-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="8998f-105">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8998f-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8998f-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8998f-106">Prerequisites</span></span>

<span data-ttu-id="8998f-107">Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gereklidir:</span><span class="sxs-lookup"><span data-stu-id="8998f-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="8998f-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="8998f-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="8998f-109">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="8998f-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="8998f-110">**.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)</span><span class="sxs-lookup"><span data-stu-id="8998f-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="8998f-111">[.NET core 2.0 SDK'sı](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="8998f-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="8998f-112">Günlük veritabanı</span><span class="sxs-lookup"><span data-stu-id="8998f-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="8998f-113">Günlük veritabanı</span><span class="sxs-lookup"><span data-stu-id="8998f-113">Blogging database</span></span>

<span data-ttu-id="8998f-114">Bu öğreticide bir **blog** LocalDb örneğiniz mevcut veritabanı olarak veritabanı.</span><span class="sxs-lookup"><span data-stu-id="8998f-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="8998f-115">Zaten oluşturduysanız **blog** veritabanı başka bir öğreticinin bir parçası olarak, bu adımı atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8998f-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="8998f-116">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="8998f-116">Open Visual Studio</span></span>
* <span data-ttu-id="8998f-117">**Araçlar -> veritabanına bağlan...**</span><span class="sxs-lookup"><span data-stu-id="8998f-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="8998f-118">Seçin **Microsoft SQL Server** tıklatıp **devam et**</span><span class="sxs-lookup"><span data-stu-id="8998f-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="8998f-119">Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**</span><span class="sxs-lookup"><span data-stu-id="8998f-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="8998f-120">Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="8998f-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="8998f-121">Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="8998f-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="8998f-122">Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="8998f-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="8998f-123">Sorgu Düzenleyicisi'ne, aşağıda listelenen betiği kopyalayın</span><span class="sxs-lookup"><span data-stu-id="8998f-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="8998f-124">Sorgu düzenleyicisini sağ tıklayıp **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="8998f-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="8998f-125">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="8998f-125">Create a new project</span></span>

* <span data-ttu-id="8998f-126">Açık Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8998f-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="8998f-127">**Dosya -> Yeni -> Proje...**</span><span class="sxs-lookup"><span data-stu-id="8998f-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="8998f-128">Sol menüden **yüklü şablonları -> Visual C# ' -> Web ->**</span><span class="sxs-lookup"><span data-stu-id="8998f-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="8998f-129">Seçin **ASP.NET Core Web uygulaması (.NET Core)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="8998f-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="8998f-130">Girin **EFGetStarted.AspNetCore.ExistingDb** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="8998f-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="8998f-131">Bekle **yeni ASP.NET Core Web uygulaması** görüntülenecek iletişim</span><span class="sxs-lookup"><span data-stu-id="8998f-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="8998f-132">Altında **ASP.NET Core şablonları 2.0** seçin **Web uygulaması (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="8998f-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="8998f-133">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="8998f-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="8998f-134">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8998f-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="8998f-135">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="8998f-135">Install Entity Framework</span></span>

<span data-ttu-id="8998f-136">EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="8998f-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="8998f-137">Bu izlenecek yol, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="8998f-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="8998f-138">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8998f-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="8998f-139">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="8998f-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="8998f-140">`Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8998f-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="8998f-141">Veritabanından bir model oluşturmak için size bazı Entity Framework Tools kullanıyor olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8998f-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="8998f-142">Bu nedenle Araçları paketi de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="8998f-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="8998f-143">`Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8998f-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="8998f-144">Daha sonra denetleyicileri ve görünümleri oluşturmak için size bazı ASP.NET Core yapı İskelesi araçları kullanarak.</span><span class="sxs-lookup"><span data-stu-id="8998f-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="8998f-145">Bu nedenle bu tasarım paketi de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="8998f-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="8998f-146">`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8998f-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="8998f-147">Modelinizi tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="8998f-147">Reverse engineer your model</span></span>

<span data-ttu-id="8998f-148">Artık, mevcut bir veritabanını temel alan EF modeli oluşturma zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="8998f-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="8998f-149">**Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="8998f-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="8998f-150">Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8998f-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="8998f-151">Belirten bir hata alırsanız `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, ardından kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="8998f-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="8998f-152">Varlıklar için ekleyerek oluşturmak istediğiniz tabloları belirtebilirsiniz `-Tables` bağımsız değişkeni için yukarıdaki komutu.</span><span class="sxs-lookup"><span data-stu-id="8998f-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="8998f-153">Örneğin, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="8998f-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="8998f-154">Oluşturulan varlık sınıfları ters mühendislik süreci (`Blog.cs` & `Post.cs`) ve türetilmiş bir içerik (`BloggingContext.cs`) var olan veritabanı şemasını temel alan.</span><span class="sxs-lookup"><span data-stu-id="8998f-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="8998f-155">Varlık sınıfları, sorgulama ve kaydetme verilerini temsil eden basit C# nesneleridir.</span><span class="sxs-lookup"><span data-stu-id="8998f-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="8998f-156">İçerik veritabanı ile bir oturumu temsil eder ve sorgu ve varlık sınıflarının örneklerini kaydetmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8998f-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="8998f-157">Bağlamınızı bağımlılık ekleme ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="8998f-157">Register your context with dependency injection</span></span>

<span data-ttu-id="8998f-158">ASP.NET Core için bağımlılık ekleme kavramının taşımaktadır.</span><span class="sxs-lookup"><span data-stu-id="8998f-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="8998f-159">Hizmetler (gibi `BloggingContext`) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8998f-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="8998f-160">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler sonra hizmetlerin Oluşturucu parametresi veya özellikleri aracılığıyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="8998f-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="8998f-161">Bağımlılık ekleme hakkında daha fazla bilgi için bkz. [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) ASP.NET sitesinde makalesi.</span><span class="sxs-lookup"><span data-stu-id="8998f-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="8998f-162">Satır içi içeriği yapılandırmasını Kaldır</span><span class="sxs-lookup"><span data-stu-id="8998f-162">Remove inline context configuration</span></span>

<span data-ttu-id="8998f-163">ASP.NET Core, yapılandırma genellikle içinde gerçekleştirilen **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="8998f-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="8998f-164">Bu deseni için uygun, biz veritabanı sağlayıcısı için yapılandırma taşınacağı **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="8998f-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="8998f-165">Açık `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="8998f-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="8998f-166">Silme `OnConfiguring(...)` yöntemi</span><span class="sxs-lookup"><span data-stu-id="8998f-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="8998f-167">Bağımlılık ekleme tarafından bağlamına geçirilecek yapılandırma izin veren aşağıdaki oluşturucuyu ekleyin</span><span class="sxs-lookup"><span data-stu-id="8998f-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="8998f-168">Kaydetme ve Bağlamınızı Startup.cs içinde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8998f-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="8998f-169">Bizim MVC denetleyicileri yapmak için sırayla kullanım `BloggingContext` hizmet olarak kaydetmek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="8998f-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="8998f-170">Açık **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="8998f-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="8998f-171">Aşağıdaki `using` deyimlerini dosyanın başında</span><span class="sxs-lookup"><span data-stu-id="8998f-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="8998f-172">Kullanabiliriz artık `AddDbContext(...)` hizmet olarak kaydetmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8998f-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="8998f-173">Bulun `ConfigureServices(...)` yöntemi</span><span class="sxs-lookup"><span data-stu-id="8998f-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="8998f-174">Bağlam bir hizmet olarak kaydetmek için aşağıdaki kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="8998f-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="8998f-175">Gerçek bir uygulamada bir yapılandırma dosyasında bağlantı dizesi genellikle koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8998f-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="8998f-176">Basitleştirmek amacıyla, biz bunu kodda tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="8998f-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="8998f-177">Daha fazla bilgi için [bağlantı dizeleri](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="8998f-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="8998f-178">Bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="8998f-178">Create a controller</span></span>

<span data-ttu-id="8998f-179">Ardından, biz yapı iskelesi projemizdeki tıklatmalarını sağlarsınız.</span><span class="sxs-lookup"><span data-stu-id="8998f-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="8998f-180">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**</span><span class="sxs-lookup"><span data-stu-id="8998f-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="8998f-181">Seçin **tam bağımlılıkları** tıklatıp **Ekle**</span><span class="sxs-lookup"><span data-stu-id="8998f-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="8998f-182">Yönergeleri yoksayabilirsiniz `ScaffoldingReadMe.txt` açılan dosya</span><span class="sxs-lookup"><span data-stu-id="8998f-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="8998f-183">Yapı iskelesi etkin, bir denetleyici için iskelesini oluşturabilirsiniz `Blog` varlık.</span><span class="sxs-lookup"><span data-stu-id="8998f-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="8998f-184">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**</span><span class="sxs-lookup"><span data-stu-id="8998f-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="8998f-185">Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="8998f-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="8998f-186">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="8998f-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="8998f-187">Tıklayın **Ekle**</span><span class="sxs-lookup"><span data-stu-id="8998f-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="8998f-188">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="8998f-188">Run the application</span></span>

<span data-ttu-id="8998f-189">Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8998f-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="8998f-190">**Hata ayıklama, hata ayıklama olmadan Başlat ->**</span><span class="sxs-lookup"><span data-stu-id="8998f-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="8998f-191">Uygulama oluşturacak ve bir web tarayıcısında Aç</span><span class="sxs-lookup"><span data-stu-id="8998f-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="8998f-192">Gidin `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="8998f-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="8998f-193">Tıklayın **Yeni Oluştur**</span><span class="sxs-lookup"><span data-stu-id="8998f-193">Click **Create New**</span></span>
* <span data-ttu-id="8998f-194">Girin bir **Url** tıklayın ve yeni blog için **oluştur**</span><span class="sxs-lookup"><span data-stu-id="8998f-194">Enter a **Url** for the new blog and click **Create**</span></span>

![görüntü](_static/create.png)

![görüntü](_static/index-existing-db.png)
