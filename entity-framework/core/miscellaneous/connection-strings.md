---
title: Bağlantı dizeleri-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149124"
---
# <a name="connection-strings"></a><span data-ttu-id="e2a4d-102">Bağlantı Dizeleri</span><span class="sxs-lookup"><span data-stu-id="e2a4d-102">Connection Strings</span></span>

<span data-ttu-id="e2a4d-103">Veritabanı sağlayıcılarının çoğu, veritabanına bağlanmak için bir bağlantı dizesi biçimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="e2a4d-104">Bazen bu bağlantı dizesi korunması gereken hassas bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="e2a4d-105">Uygulamanızı geliştirme, test ve üretim gibi ortamlar arasında taşırken bağlantı dizesini de değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="e2a4d-106">WinForms & WPF uygulamaları</span><span class="sxs-lookup"><span data-stu-id="e2a4d-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="e2a4d-107">WinForms, WPF ve ASP.NET 4 uygulamaları, denenen ve sınanan bir bağlantı dizesi düzenine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="e2a4d-108">Bağlantı dizesi uygulamanızın App. config dosyasına eklenmelidir (ASP.NET kullanıyorsanız Web. config).</span><span class="sxs-lookup"><span data-stu-id="e2a4d-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="e2a4d-109">Bağlantı dizeniz Kullanıcı adı ve parola gibi hassas bilgiler içeriyorsa, yapılandırma dosyasının içeriğini [gizli yönetici aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)kullanarak koruyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span></span>

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> <span data-ttu-id="e2a4d-110">Veritabanı sağlayıcısı kod aracılığıyla yapılandırıldığı için App. config dosyasında depolanan EF Core bağlantı dizeleri üzerinde bu ayargereklideğildir.`providerName`</span><span class="sxs-lookup"><span data-stu-id="e2a4d-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="e2a4d-111">Daha sonra bağlantı dizesini `ConfigurationManager` `OnConfiguring` bağlam yöntemindeki API 'yi kullanarak okuyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="e2a4d-112">Bu API 'yi kullanabilmeniz için `System.Configuration` Framework derlemesine bir başvuru eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="e2a4d-113">Evrensel Windows Platformu (UWP)</span><span class="sxs-lookup"><span data-stu-id="e2a4d-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="e2a4d-114">UWP uygulamasındaki bağlantı dizeleri genellikle yerel bir dosya adı belirten bir SQLite bağlantıdır.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="e2a4d-115">Genellikle hassas bilgiler içermez ve bir uygulama dağıtıldığında değiştirilmeleri gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="e2a4d-116">Bu şekilde, bu bağlantı dizeleri genellikle aşağıda gösterildiği gibi, kodda ayrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="e2a4d-117">Kod dışına taşımak isterseniz, UWP ayar kavramını destekler, Ayrıntılar için [UWP belgelerinin uygulama ayarları bölümüne](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) bakın.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a><span data-ttu-id="e2a4d-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2a4d-118">ASP.NET Core</span></span>

<span data-ttu-id="e2a4d-119">ASP.NET Core yapılandırma sistemi çok esnektir ve bağlantı dizesi, bir ortam değişkeni, Kullanıcı gizli dizisi veya `appsettings.json`başka bir yapılandırma kaynağı içinde depolanabilir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="e2a4d-120">Daha fazla bilgi için [ASP.NET Core belgelerinin yapılandırma bölümüne](https://docs.asp.net/en/latest/fundamentals/configuration.html) bakın.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="e2a4d-121">Aşağıdaki örnek, içinde `appsettings.json`depolanan bağlantı dizesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="e2a4d-122">Bağlam genellikle, ' de `Startup.cs` yapılandırmadan okunmakta olan bağlantı dizesi ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="e2a4d-123">Yöntemi, `GetConnectionString()` `ConnectionStrings:<connection string name>`anahtarı olan bir yapılandırma değeri arar.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="e2a4d-124">Bu genişletme yöntemini kullanmak için [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) ad alanını içeri aktarmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e2a4d-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
