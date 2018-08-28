---
title: Üzerinde ASP.NET Core - mevcut veritabanı - EF Core kullanmaya başlama
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 79a73e38fdc9c4268c21de66571d6272f33e9457
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997042"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="1e0b6-102">Mevcut bir veritabanı ile ASP.NET Core üzerinde EF Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1e0b6-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="1e0b6-103">Bu öğreticide, Entity Framework Core kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="1e0b6-104">Entity Framework modelini oluşturmak için varolan bir veritabanını mühendisi ters.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-104">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="1e0b6-105">[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="1e0b6-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e0b6-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="1e0b6-106">Prerequisites</span></span>

<span data-ttu-id="1e0b6-107">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="1e0b6-107">Install the following software:</span></span>

* <span data-ttu-id="1e0b6-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) bu iş yükleri ile:</span><span class="sxs-lookup"><span data-stu-id="1e0b6-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="1e0b6-109">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="1e0b6-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="1e0b6-110">**.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)</span><span class="sxs-lookup"><span data-stu-id="1e0b6-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="1e0b6-111">[.NET core SDK'sını 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="1e0b6-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="1e0b6-112">Günlük veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1e0b6-112">Create Blogging database</span></span>

<span data-ttu-id="1e0b6-113">Bu öğreticide bir **blog** LocalDb örneğiniz mevcut veritabanı olarak veritabanı.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="1e0b6-114">Zaten oluşturduysanız **blog** veritabanı başka bir öğreticinin bir parçası olarak, bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-114">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="1e0b6-115">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="1e0b6-115">Open Visual Studio</span></span>
* <span data-ttu-id="1e0b6-116">**Araçlar -> veritabanına bağlan...**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-116">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="1e0b6-117">Seçin **Microsoft SQL Server** tıklatıp **devam et**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-117">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="1e0b6-118">Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="1e0b6-119">Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-119">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="1e0b6-120">Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="1e0b6-121">Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="1e0b6-122">Sorgu Düzenleyicisi'ne aşağıdaki betiği kopyalayın</span><span class="sxs-lookup"><span data-stu-id="1e0b6-122">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="1e0b6-123">Sorgu düzenleyicisini sağ tıklayıp **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="1e0b6-124">Yeni bir proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="1e0b6-124">Create a new project</span></span>

* <span data-ttu-id="1e0b6-125">Açık Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1e0b6-125">Open Visual Studio 2017</span></span>
* <span data-ttu-id="1e0b6-126">**Dosya > Yeni > Proje...**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-126">**File > New > Project...**</span></span>
* <span data-ttu-id="1e0b6-127">Sol menüden **yüklü > Visual C# > Web**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-127">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="1e0b6-128">Seçin **ASP.NET Core Web uygulaması** proje şablonu</span><span class="sxs-lookup"><span data-stu-id="1e0b6-128">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="1e0b6-129">Girin **EFGetStarted.AspNetCore.ExistingDb** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-129">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="1e0b6-130">Bekle **yeni ASP.NET Core Web uygulaması** görüntülenecek iletişim</span><span class="sxs-lookup"><span data-stu-id="1e0b6-130">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="1e0b6-131">Hedef framework açılan ayarlandığından emin olun **.NET Core**, ve sürüm açılan kümesine **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-131">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="1e0b6-132">Seçin **Web uygulaması (Model-View-Controller)** şablonu</span><span class="sxs-lookup"><span data-stu-id="1e0b6-132">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="1e0b6-133">Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="1e0b6-134">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-134">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="1e0b6-135">Entity Framework Core yükleme</span><span class="sxs-lookup"><span data-stu-id="1e0b6-135">Install Entity Framework Core</span></span>

<span data-ttu-id="1e0b6-136">EF Core yüklemek için hedeflemek istediğiniz EF Core veritabanı sağlayıcı(lar) için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-136">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="1e0b6-137">Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1e0b6-137">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="1e0b6-138">Bu öğreticide, öğretici, SQL Server kullandığından sağlayıcı paketi yüklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-138">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="1e0b6-139">SQL Server sağlayıcı paketi eklenmiştir [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="1e0b6-139">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="1e0b6-140">Modelinizi tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="1e0b6-140">Reverse engineer your model</span></span>

<span data-ttu-id="1e0b6-141">Artık, mevcut bir veritabanını temel alan EF modeli oluşturma zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-141">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="1e0b6-142">**Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-142">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="1e0b6-143">Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1e0b6-143">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="1e0b6-144">Belirten bir hata alırsanız `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, ardından kapatın ve Visual Studio'yu yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-144">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="1e0b6-145">Varlıklar için ekleyerek oluşturmak istediğiniz tabloları belirtebilirsiniz `-Tables` bağımsız değişkeni için yukarıdaki komutu.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-145">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="1e0b6-146">Örneğin, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-146">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="1e0b6-147">Oluşturulan varlık sınıfları ters mühendislik süreci (`Blog.cs` & `Post.cs`) ve türetilmiş bir içerik (`BloggingContext.cs`) var olan veritabanı şemasını temel alan.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-147">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="1e0b6-148">Varlık sınıfları, sorgulama ve kaydetme verilerini temsil eden basit C# nesneleridir.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-148">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="1e0b6-149">İşte `Blog` ve `Post` varlık sınıfları:</span><span class="sxs-lookup"><span data-stu-id="1e0b6-149">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="1e0b6-150">Yavaş yükleniyor etkinleştirmek için Gezinti özellikleri yapabilirsiniz `virtual` (Blog.Post ve Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="1e0b6-150">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="1e0b6-151">İçerik veritabanı ile bir oturumu temsil eder ve sorgu ve varlık sınıflarının örneklerini kaydetmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-151">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="1e0b6-152">Bağlamınızı bağımlılık ekleme ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="1e0b6-152">Register your context with dependency injection</span></span>

<span data-ttu-id="1e0b6-153">ASP.NET Core için bağımlılık ekleme kavramının taşımaktadır.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-153">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="1e0b6-154">Hizmetler (gibi `BloggingContext`) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-154">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="1e0b6-155">Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler sonra hizmetlerin Oluşturucu parametresi veya özellikleri aracılığıyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-155">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="1e0b6-156">Bağımlılık ekleme hakkında daha fazla bilgi için bkz. [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) ASP.NET sitesinde makalesi.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-156">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="1e0b6-157">Kaydetme ve Bağlamınızı Startup.cs içinde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1e0b6-157">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="1e0b6-158">Yapmak `BloggingContext` MVC denetleyicileri için kullanılabilir bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="1e0b6-159">Açık **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-159">Open **Startup.cs**</span></span>
* <span data-ttu-id="1e0b6-160">Aşağıdaki `using` deyimlerini dosyanın başında</span><span class="sxs-lookup"><span data-stu-id="1e0b6-160">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="1e0b6-161">Artık `AddDbContext(...)` hizmet olarak kaydetmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-161">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="1e0b6-162">Bulun `ConfigureServices(...)` yöntemi</span><span class="sxs-lookup"><span data-stu-id="1e0b6-162">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="1e0b6-163">Bağlam bir hizmet olarak kaydetmek için aşağıdaki vurgulanmış kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="1e0b6-163">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="1e0b6-164">Gerçek bir uygulamada genellikle bir yapılandırma dosyası veya ortam değişkenine bağlantı dizesini koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-164">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="1e0b6-165">Basitleştirmek amacıyla, Bu öğretici, kod içinde tanımlamanıza sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-165">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="1e0b6-166">Daha fazla bilgi için [bağlantı dizeleri](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="1e0b6-166">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="1e0b6-167">Bir denetleyici ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="1e0b6-167">Create a controller and views</span></span>

* <span data-ttu-id="1e0b6-168">Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="1e0b6-169">Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="1e0b6-170">Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="1e0b6-171">Tıklayın **Ekle**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-171">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="1e0b6-172">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="1e0b6-172">Run the application</span></span>

<span data-ttu-id="1e0b6-173">Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e0b6-173">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="1e0b6-174">**Hata ayıklama, hata ayıklama olmadan Başlat ->**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-174">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="1e0b6-175">Uygulama oluşturur ve bir web tarayıcısında açılır</span><span class="sxs-lookup"><span data-stu-id="1e0b6-175">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="1e0b6-176">Gidin `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="1e0b6-176">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="1e0b6-177">Tıklayın **Yeni Oluştur**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-177">Click **Create New**</span></span>
* <span data-ttu-id="1e0b6-178">Girin bir **Url** tıklayın ve yeni blog için **oluştur**</span><span class="sxs-lookup"><span data-stu-id="1e0b6-178">Enter a **Url** for the new blog and click **Create**</span></span>

![görüntü](_static/create.png)

![görüntü](_static/index-existing-db.png)
