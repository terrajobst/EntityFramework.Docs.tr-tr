---
title: ASP.NET Core - var olan veritabanı - EF Çekirdeğinde Başlarken
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="16ad9-102">Varolan bir veritabanını ile ASP.NET Core EF Çekirdeğinde ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="16ad9-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="16ad9-103">Bu kılavuzda, Entity Framework kullanarak temel veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="16ad9-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="16ad9-104">Varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için ters mühendislik kullanır.</span><span class="sxs-lookup"><span data-stu-id="16ad9-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="16ad9-105">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) github'da.</span><span class="sxs-lookup"><span data-stu-id="16ad9-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16ad9-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="16ad9-106">Prerequisites</span></span>

<span data-ttu-id="16ad9-107">Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:</span><span class="sxs-lookup"><span data-stu-id="16ad9-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="16ad9-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="16ad9-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="16ad9-109">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="16ad9-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="16ad9-110">**.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="16ad9-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="16ad9-111">[.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="16ad9-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="16ad9-112">Blog veritabanı</span><span class="sxs-lookup"><span data-stu-id="16ad9-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="16ad9-113">Blog veritabanı</span><span class="sxs-lookup"><span data-stu-id="16ad9-113">Blogging database</span></span>

<span data-ttu-id="16ad9-114">Bu öğretici kullanan bir **blog** veritabanı, yerel veritabanı örneğinde var olan veritabanı olarak.</span><span class="sxs-lookup"><span data-stu-id="16ad9-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="16ad9-115">Zaten oluşturduysanız **blog** veritabanı başka bir öğretici bir parçası olarak, bu adımı atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ad9-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="16ad9-116">Açık Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16ad9-116">Open Visual Studio</span></span>
* <span data-ttu-id="16ad9-117">**Araçlar -> veritabanına bağlan...**</span><span class="sxs-lookup"><span data-stu-id="16ad9-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="16ad9-118">Seçin **Microsoft SQL Server** tıklatıp **devam et**</span><span class="sxs-lookup"><span data-stu-id="16ad9-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="16ad9-119">Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**</span><span class="sxs-lookup"><span data-stu-id="16ad9-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="16ad9-120">Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="16ad9-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="16ad9-121">Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="16ad9-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="16ad9-122">Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="16ad9-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="16ad9-123">Aşağıda, sorgu Düzenleyicisi'ne komut dosyasını kopyalayın</span><span class="sxs-lookup"><span data-stu-id="16ad9-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="16ad9-124">Üzerinde sorgu Düzenleyicisi'ni sağ tıklatın ve seçin **çalıştırma**</span><span class="sxs-lookup"><span data-stu-id="16ad9-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="16ad9-125">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="16ad9-125">Create a new project</span></span>

* <span data-ttu-id="16ad9-126">Open Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="16ad9-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="16ad9-127">**Dosya -> Yeni Proje ->...**</span><span class="sxs-lookup"><span data-stu-id="16ad9-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="16ad9-128">Sol menüden seçin **yüklü şablonları -> Visual C# -> Web ->**</span><span class="sxs-lookup"><span data-stu-id="16ad9-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="16ad9-129">Seçin **ASP.NET çekirdek Web uygulaması (.NET Core)** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="16ad9-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="16ad9-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span><span class="sxs-lookup"><span data-stu-id="16ad9-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="16ad9-131">Bekle **çekirdek yeni bir ASP.NET Web uygulaması** görünmesi iletişim</span><span class="sxs-lookup"><span data-stu-id="16ad9-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="16ad9-132">Altında **ASP.NET Core şablonları 2.0** seçin **Web uygulaması (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="16ad9-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="16ad9-133">Emin **kimlik doğrulaması** ayarlanır **doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="16ad9-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="16ad9-134">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16ad9-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="16ad9-135">Entity Framework'ü yükleme</span><span class="sxs-lookup"><span data-stu-id="16ad9-135">Install Entity Framework</span></span>

<span data-ttu-id="16ad9-136">EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="16ad9-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="16ad9-137">Bu kılavuz, SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="16ad9-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="16ad9-138">Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="16ad9-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="16ad9-139">**Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="16ad9-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="16ad9-140">`Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="16ad9-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="16ad9-141">Veritabanından bir model oluşturmak için size bazı Entity Framework araçları kullanarak.</span><span class="sxs-lookup"><span data-stu-id="16ad9-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="16ad9-142">Böylece biz de araçları paketini yükleyecek:</span><span class="sxs-lookup"><span data-stu-id="16ad9-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="16ad9-143">`Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="16ad9-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="16ad9-144">Daha sonra denetleyicileri ve görünümleri oluşturmak için size bazı ASP.NET Core İskele araçlarını kullanma.</span><span class="sxs-lookup"><span data-stu-id="16ad9-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="16ad9-145">Böylece biz de bu tasarım paket yükleyecek:</span><span class="sxs-lookup"><span data-stu-id="16ad9-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="16ad9-146">`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="16ad9-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="16ad9-147">Tersine mühendislik modelinizi</span><span class="sxs-lookup"><span data-stu-id="16ad9-147">Reverse engineer your model</span></span>

<span data-ttu-id="16ad9-148">Şimdi, varolan bir veritabanını temel alan EF modeli oluşturmak için zaman yapılır.</span><span class="sxs-lookup"><span data-stu-id="16ad9-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="16ad9-149">**Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="16ad9-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="16ad9-150">Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="16ad9-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="16ad9-151">Belirten bir hata alırsanız, `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, ardından kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="16ad9-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="16ad9-152">Varlıklar için ekleyerek oluşturmak istediğiniz tabloları belirtebilirsiniz `-Tables` yukarıdaki komut bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="16ad9-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="16ad9-153">Örneğin</span><span class="sxs-lookup"><span data-stu-id="16ad9-153">E.g.</span></span> <span data-ttu-id="16ad9-154">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="16ad9-154">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="16ad9-155">Ters mühendislik işlemi oluşturulan sınıflar (`Blog.cs` & `Post.cs`) ve türetilmiş bir içerik (`BloggingContext.cs`) var olan veritabanı şemasını temel alan.</span><span class="sxs-lookup"><span data-stu-id="16ad9-155">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="16ad9-156">Sınıflar, sorgulama kaydetme ve verilerini temsil eden basit C# nesneleridir.</span><span class="sxs-lookup"><span data-stu-id="16ad9-156">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="16ad9-157">Bağlam veritabanı oturumla temsil eder ve sorgu ve varlık sınıfları kaydetmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="16ad9-157">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="16ad9-158">İçeriğiniz bağımlılık ekleme ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="16ad9-158">Register your context with dependency injection</span></span>

<span data-ttu-id="16ad9-159">Bağımlılık ekleme kavramı ASP.NET Core taşımaktadır.</span><span class="sxs-lookup"><span data-stu-id="16ad9-159">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="16ad9-160">Hizmetler (gibi `BloggingContext`) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="16ad9-160">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="16ad9-161">Bu Hizmetleri (örneğin, MVC denetleyicileri) gerektiren bileşenler daha sonra bu hizmetlere Oluşturucu parametreleri veya özellikleri yoluyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="16ad9-161">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="16ad9-162">Bağımlılık ekleme hakkında daha fazla bilgi için bkz: [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) makale ASP.NET sitesi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="16ad9-162">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="16ad9-163">Satır içi içeriği yapılandırmasını Kaldır</span><span class="sxs-lookup"><span data-stu-id="16ad9-163">Remove inline context configuration</span></span>

<span data-ttu-id="16ad9-164">ASP.NET çekirdek yapılandırması genellikle içinde gerçekleştirilir **haline**.</span><span class="sxs-lookup"><span data-stu-id="16ad9-164">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="16ad9-165">Bu desene uyacak şekilde biz veritabanı sağlayıcısı yapılandırmasını taşınır **haline**.</span><span class="sxs-lookup"><span data-stu-id="16ad9-165">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="16ad9-166">Açık `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="16ad9-166">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="16ad9-167">Silme `OnConfiguring(...)` yöntemi</span><span class="sxs-lookup"><span data-stu-id="16ad9-167">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="16ad9-168">Bağımlılık ekleme tarafından bağlamına geçirilecek yapılandırma sağlayacak aşağıdaki oluşturucuyu ekleyin</span><span class="sxs-lookup"><span data-stu-id="16ad9-168">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="16ad9-169">Kaydolun ve içeriğiniz haline içinde yapılandırın</span><span class="sxs-lookup"><span data-stu-id="16ad9-169">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="16ad9-170">Bizim MVC denetleyicileri yapmak için sırayla kullanımı `BloggingContext` bir hizmet olarak kaydetmek için kalacaklarını.</span><span class="sxs-lookup"><span data-stu-id="16ad9-170">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="16ad9-171">Açık **haline**</span><span class="sxs-lookup"><span data-stu-id="16ad9-171">Open **Startup.cs**</span></span>
* <span data-ttu-id="16ad9-172">Aşağıdakileri ekleyin `using` dosyasının başındaki deyimleri</span><span class="sxs-lookup"><span data-stu-id="16ad9-172">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="16ad9-173">Artık biz kullanarak `AddDbContext(...)` bir hizmet olarak kaydetmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="16ad9-173">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="16ad9-174">Bulun `ConfigureServices(...)` yöntemi</span><span class="sxs-lookup"><span data-stu-id="16ad9-174">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="16ad9-175">Bağlam bir hizmet olarak kaydetmek için aşağıdaki kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="16ad9-175">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="16ad9-176">Gerçek bir uygulamada genellikle bir yapılandırma dosyasında bağlantı dizesini koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ad9-176">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="16ad9-177">Basitleştirmek amacıyla, biz bu kodda tanımlıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="16ad9-177">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="16ad9-178">Daha fazla bilgi için bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="16ad9-178">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="16ad9-179">Bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="16ad9-179">Create a controller</span></span>

<span data-ttu-id="16ad9-180">Ardından, yapı iskelesi bizim projesinde etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="16ad9-180">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="16ad9-181">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**</span><span class="sxs-lookup"><span data-stu-id="16ad9-181">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="16ad9-182">Seçin **tam bağımlılıkları** tıklatıp **Ekle**</span><span class="sxs-lookup"><span data-stu-id="16ad9-182">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="16ad9-183">' Ndaki yönergeleri yoksayabilirsiniz `ScaffoldingReadMe.txt` açar dosyası</span><span class="sxs-lookup"><span data-stu-id="16ad9-183">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="16ad9-184">Askılama özelliğinin etkinleştirildiğinden, bir denetleyici için iskele `Blog` varlık.</span><span class="sxs-lookup"><span data-stu-id="16ad9-184">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="16ad9-185">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**</span><span class="sxs-lookup"><span data-stu-id="16ad9-185">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="16ad9-186">Seçin **görünümleri ile MVC Entity Framework kullanarak denetleyicisi** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="16ad9-186">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="16ad9-187">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfı** için **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="16ad9-187">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="16ad9-188">Tıklatın **Ekle**</span><span class="sxs-lookup"><span data-stu-id="16ad9-188">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="16ad9-189">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="16ad9-189">Run the application</span></span>

<span data-ttu-id="16ad9-190">Şimdi eylemde görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ad9-190">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="16ad9-191">**Hata ayıklama, hata ayıklama olmadan Başlat ->**</span><span class="sxs-lookup"><span data-stu-id="16ad9-191">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="16ad9-192">Uygulama oluşturma ve bir web tarayıcısında Aç</span><span class="sxs-lookup"><span data-stu-id="16ad9-192">The application will build and open in a web browser</span></span>
* <span data-ttu-id="16ad9-193">Gidin `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="16ad9-193">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="16ad9-194">Tıklatın **Yeni Oluştur**</span><span class="sxs-lookup"><span data-stu-id="16ad9-194">Click **Create New**</span></span>
* <span data-ttu-id="16ad9-195">Girin bir **Url** tıklatın ve yeni blog **oluştur**</span><span class="sxs-lookup"><span data-stu-id="16ad9-195">Enter a **Url** for the new blog and click **Create**</span></span>

![görüntü](_static/create.png)

![görüntü](_static/index-existing-db.png)
