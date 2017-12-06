---
title: "ASP.NET Core - var olan veritabanı - EF Çekirdeğinde Başlarken"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="b27f9-102">Varolan bir veritabanını ile ASP.NET Core EF Çekirdeğinde ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b27f9-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="b27f9-103">[.NET Core SDK](https://www.microsoft.com/net/download/core) artık `project.json` veya Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="b27f9-103">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="b27f9-104">Herkes .NET Core geliştirme yapılması önerilir [project.json için csproj geçirmek](https://docs.microsoft.com/dotnet/articles/core/migration/) ve [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b27f9-104">Everyone doing .NET Core development is encouraged to [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) and [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

<span data-ttu-id="b27f9-105">Bu kılavuzda, Entity Framework kullanarak temel veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b27f9-105">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="b27f9-106">Varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için ters mühendislik kullanır.</span><span class="sxs-lookup"><span data-stu-id="b27f9-106">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="b27f9-107">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) github'da.</span><span class="sxs-lookup"><span data-stu-id="b27f9-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b27f9-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b27f9-108">Prerequisites</span></span>

<span data-ttu-id="b27f9-109">Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:</span><span class="sxs-lookup"><span data-stu-id="b27f9-109">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="b27f9-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="b27f9-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="b27f9-111">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="b27f9-111">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="b27f9-112">**.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="b27f9-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="b27f9-113">[.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="b27f9-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="b27f9-114">Blog veritabanı</span><span class="sxs-lookup"><span data-stu-id="b27f9-114">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="b27f9-115">Blog veritabanı</span><span class="sxs-lookup"><span data-stu-id="b27f9-115">Blogging database</span></span>

<span data-ttu-id="b27f9-116">Bu öğretici kullanan bir **blog** veritabanı, yerel veritabanı örneğinde var olan veritabanı olarak.</span><span class="sxs-lookup"><span data-stu-id="b27f9-116">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="b27f9-117">Zaten oluşturduysanız **blog** veritabanı başka bir öğretici bir parçası olarak, bu adımı atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b27f9-117">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="b27f9-118">Açık Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b27f9-118">Open Visual Studio</span></span>
* <span data-ttu-id="b27f9-119">**Araçlar -> veritabanına bağlan...**</span><span class="sxs-lookup"><span data-stu-id="b27f9-119">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="b27f9-120">Seçin **Microsoft SQL Server** tıklatıp **devam et**</span><span class="sxs-lookup"><span data-stu-id="b27f9-120">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="b27f9-121">Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**</span><span class="sxs-lookup"><span data-stu-id="b27f9-121">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="b27f9-122">Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="b27f9-122">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="b27f9-123">Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="b27f9-123">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="b27f9-124">Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="b27f9-124">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="b27f9-125">Aşağıda, sorgu Düzenleyicisi'ne komut dosyasını kopyalayın</span><span class="sxs-lookup"><span data-stu-id="b27f9-125">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="b27f9-126">Üzerinde sorgu Düzenleyicisi'ni sağ tıklatın ve seçin **çalıştırma**</span><span class="sxs-lookup"><span data-stu-id="b27f9-126">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="b27f9-127">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="b27f9-127">Create a new project</span></span>

* <span data-ttu-id="b27f9-128">Visual Studio 2017 Aç</span><span class="sxs-lookup"><span data-stu-id="b27f9-128">Open Visual Studio 2017</span></span>
* <span data-ttu-id="b27f9-129">**Dosya -> Yeni Proje ->...**</span><span class="sxs-lookup"><span data-stu-id="b27f9-129">**File -> New -> Project...**</span></span>
* <span data-ttu-id="b27f9-130">Sol menüden seçin **yüklü şablonları -> Visual C# -> Web ->**</span><span class="sxs-lookup"><span data-stu-id="b27f9-130">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="b27f9-131">Seçin **ASP.NET çekirdek Web uygulaması (.NET Core)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="b27f9-131">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="b27f9-132">Girin **EFGetStarted.AspNetCore.ExistingDb** tıklatın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="b27f9-132">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="b27f9-133">Bekle **çekirdek yeni bir ASP.NET Web uygulaması** görünmesi iletişim</span><span class="sxs-lookup"><span data-stu-id="b27f9-133">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="b27f9-134">Altında **ASP.NET Core şablonları 2.0** seçin **Web uygulaması (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="b27f9-134">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="b27f9-135">Emin **kimlik doğrulaması** ayarlanır **doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="b27f9-135">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="b27f9-136">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b27f9-136">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="b27f9-137">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="b27f9-137">Install Entity Framework</span></span>

<span data-ttu-id="b27f9-138">EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="b27f9-138">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="b27f9-139">Bu kılavuz, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="b27f9-139">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="b27f9-140">Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="b27f9-140">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="b27f9-141">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="b27f9-141">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="b27f9-142">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="b27f9-142">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="b27f9-143">Veritabanından bir model oluşturmak için size bazı Entity Framework araçları kullanarak.</span><span class="sxs-lookup"><span data-stu-id="b27f9-143">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="b27f9-144">Böylece biz de araçları paketini yükleyecek:</span><span class="sxs-lookup"><span data-stu-id="b27f9-144">So we will install the tools package as well:</span></span>

* <span data-ttu-id="b27f9-145">Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="b27f9-145">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="b27f9-146">Daha sonra denetleyicileri ve görünümleri oluşturmak için size bazı ASP.NET Core İskele araçlarını kullanma.</span><span class="sxs-lookup"><span data-stu-id="b27f9-146">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="b27f9-147">Böylece biz de bu tasarım paket yükleyecek:</span><span class="sxs-lookup"><span data-stu-id="b27f9-147">So we will install this design package as well:</span></span>

* <span data-ttu-id="b27f9-148">Çalıştırma`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="b27f9-148">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="b27f9-149">Tersine mühendislik modelinizi</span><span class="sxs-lookup"><span data-stu-id="b27f9-149">Reverse engineer your model</span></span>

<span data-ttu-id="b27f9-150">Şimdi, varolan bir veritabanını temel alan EF modeli oluşturmak için zaman yapılır.</span><span class="sxs-lookup"><span data-stu-id="b27f9-150">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="b27f9-151">**Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="b27f9-151">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="b27f9-152">Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b27f9-152">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="b27f9-153">Belirten bir hata alırsanız, `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, ardından kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="b27f9-153">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="b27f9-154">Varlıklar için ekleyerek oluşturmak istediğiniz tabloları belirtebilirsiniz `-Tables` yukarıdaki komut bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="b27f9-154">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="b27f9-155">Örneğin</span><span class="sxs-lookup"><span data-stu-id="b27f9-155">E.g.</span></span> <span data-ttu-id="b27f9-156">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="b27f9-156">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="b27f9-157">Ters mühendislik işlemi oluşturulan sınıflar (`Blog.cs` & `Post.cs`) ve türetilmiş bir içerik (`BloggingContext.cs`) var olan veritabanı şemasını temel alan.</span><span class="sxs-lookup"><span data-stu-id="b27f9-157">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="b27f9-158">Sınıflar, sorgulama kaydetme ve verilerini temsil eden basit C# nesneleridir.</span><span class="sxs-lookup"><span data-stu-id="b27f9-158">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="b27f9-159">Bağlam veritabanı oturumla temsil eder ve sorgu ve varlık sınıfları kaydetmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b27f9-159">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="b27f9-160">İçeriğiniz bağımlılık ekleme ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="b27f9-160">Register your context with dependency injection</span></span>

<span data-ttu-id="b27f9-161">Bağımlılık ekleme kavramı ASP.NET Core taşımaktadır.</span><span class="sxs-lookup"><span data-stu-id="b27f9-161">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="b27f9-162">Hizmetler (gibi `BloggingContext`) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b27f9-162">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="b27f9-163">Bu Hizmetleri (örneğin, MVC denetleyicileri) gerektiren bileşenler daha sonra bu hizmetlere Oluşturucu parametreleri veya özellikleri yoluyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b27f9-163">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="b27f9-164">Bağımlılık ekleme hakkında daha fazla bilgi için bkz: [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) makale ASP.NET sitesi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="b27f9-164">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="b27f9-165">Satır içi içeriği yapılandırmasını Kaldır</span><span class="sxs-lookup"><span data-stu-id="b27f9-165">Remove inline context configuration</span></span>

<span data-ttu-id="b27f9-166">ASP.NET çekirdek yapılandırması genellikle içinde gerçekleştirilir **haline**.</span><span class="sxs-lookup"><span data-stu-id="b27f9-166">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="b27f9-167">Bu desene uyacak şekilde biz veritabanı sağlayıcısı yapılandırmasını taşınır **haline**.</span><span class="sxs-lookup"><span data-stu-id="b27f9-167">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="b27f9-168">Açık`Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="b27f9-168">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="b27f9-169">Silme `OnConfiguring(...)` yöntemi</span><span class="sxs-lookup"><span data-stu-id="b27f9-169">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="b27f9-170">Bağımlılık ekleme tarafından bağlamına geçirilecek yapılandırma sağlayacak aşağıdaki oluşturucuyu ekleyin</span><span class="sxs-lookup"><span data-stu-id="b27f9-170">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="b27f9-171">Kaydolun ve içeriğiniz haline içinde yapılandırın</span><span class="sxs-lookup"><span data-stu-id="b27f9-171">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="b27f9-172">Bizim MVC denetleyicileri yapmak için sırayla kullanımı `BloggingContext` bir hizmet olarak kaydetmek için kalacaklarını.</span><span class="sxs-lookup"><span data-stu-id="b27f9-172">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="b27f9-173">Açık **haline**</span><span class="sxs-lookup"><span data-stu-id="b27f9-173">Open **Startup.cs**</span></span>
* <span data-ttu-id="b27f9-174">Aşağıdakileri ekleyin `using` dosyasının başındaki deyimleri</span><span class="sxs-lookup"><span data-stu-id="b27f9-174">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="b27f9-175">Artık biz kullanarak `AddDbContext(...)` bir hizmet olarak kaydetmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="b27f9-175">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="b27f9-176">Bulun `ConfigureServices(...)` yöntemi</span><span class="sxs-lookup"><span data-stu-id="b27f9-176">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="b27f9-177">Bağlam bir hizmet olarak kaydetmek için aşağıdaki kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="b27f9-177">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="b27f9-178">Gerçek bir uygulamada genellikle bir yapılandırma dosyasında bağlantı dizesini koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b27f9-178">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="b27f9-179">Basitleştirmek amacıyla, biz bu kodda tanımlıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="b27f9-179">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="b27f9-180">Daha fazla bilgi için bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="b27f9-180">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="b27f9-181">Bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="b27f9-181">Create a controller</span></span>

<span data-ttu-id="b27f9-182">Ardından, yapı iskelesi bizim projesinde etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="b27f9-182">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="b27f9-183">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**</span><span class="sxs-lookup"><span data-stu-id="b27f9-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="b27f9-184">Seçin **tam bağımlılıkları** tıklatıp **Ekle**</span><span class="sxs-lookup"><span data-stu-id="b27f9-184">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="b27f9-185">' Ndaki yönergeleri yoksayabilirsiniz `ScaffoldingReadMe.txt` açar dosyası</span><span class="sxs-lookup"><span data-stu-id="b27f9-185">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="b27f9-186">Askılama özelliğinin etkinleştirildiğinden, bir denetleyici için iskele `Blog` varlık.</span><span class="sxs-lookup"><span data-stu-id="b27f9-186">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="b27f9-187">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**</span><span class="sxs-lookup"><span data-stu-id="b27f9-187">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="b27f9-188">Seçin **görünümleri ile MVC Entity Framework kullanarak denetleyicisi** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="b27f9-188">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="b27f9-189">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfı** için **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="b27f9-189">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="b27f9-190">Tıklatın **Ekle**</span><span class="sxs-lookup"><span data-stu-id="b27f9-190">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="b27f9-191">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b27f9-191">Run the application</span></span>

<span data-ttu-id="b27f9-192">Şimdi eylemde görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b27f9-192">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="b27f9-193">**Hata ayıklama, hata ayıklama olmadan Başlat ->**</span><span class="sxs-lookup"><span data-stu-id="b27f9-193">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="b27f9-194">Uygulama oluşturma ve bir web tarayıcısında Aç</span><span class="sxs-lookup"><span data-stu-id="b27f9-194">The application will build and open in a web browser</span></span>
* <span data-ttu-id="b27f9-195">Gidin`/Blogs`</span><span class="sxs-lookup"><span data-stu-id="b27f9-195">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="b27f9-196">Tıklatın **Yeni Oluştur**</span><span class="sxs-lookup"><span data-stu-id="b27f9-196">Click **Create New**</span></span>
* <span data-ttu-id="b27f9-197">Girin bir **Url** tıklatın ve yeni blog **oluştur**</span><span class="sxs-lookup"><span data-stu-id="b27f9-197">Enter a **Url** for the new blog and click **Create**</span></span>

![görüntü](_static/create.png)

![görüntü](_static/index-existing-db.png)
