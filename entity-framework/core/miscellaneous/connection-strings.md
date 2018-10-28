---
title: Bağlantı dizelerini - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 7bb39d260f700e5087673e92a50377dc68151710
ms.sourcegitcommit: 85ccc9ed42d4aaf7525c6312058c5c9ebdaed3ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2018
ms.locfileid: "50191348"
---
# <a name="connection-strings"></a><span data-ttu-id="e3220-102">Bağlantı dizeleri</span><span class="sxs-lookup"><span data-stu-id="e3220-102">Connection Strings</span></span>

<span data-ttu-id="e3220-103">Çoğu veritabanı sağlayıcısı veritabanına bağlanmak için bağlantı dizesi biçimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e3220-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="e3220-104">Bazen bu bağlantı dizesi, korunması gereken hassas bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="e3220-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="e3220-105">Uygulamanızı geliştirme, test ve üretim ortamları arasında taşırken bağlantı dizesini değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e3220-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="e3220-106">.NET framework uygulamaları</span><span class="sxs-lookup"><span data-stu-id="e3220-106">.NET Framework Applications</span></span>

<span data-ttu-id="e3220-107">.NET framework uygulamaları, WinForms, WPF, konsol ve ASP.NET 4 gibi denenmiş ve test edilmiş bağlantı dize deseni vardır.</span><span class="sxs-lookup"><span data-stu-id="e3220-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="e3220-108">Bağlantı dizesi, uygulamalar için sık sorulan sorular App.config dosyasında (Web.config ASP.NET kullanıyorsanız) eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e3220-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="e3220-109">Bağlantı dizenizi kullanıcı adı ve parola gibi hassas bilgileri içeriyorsa yapılandırma dosyası kullanarak içerikleri koruyabilir [korumalı yapılandırma](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="e3220-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="e3220-110">`providerName` Ayarı veritabanı sağlayıcısı kod yapılandırıldığından App.config dosyasında depolanan EF Core bağlantı dizelerini gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="e3220-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="e3220-111">Daha sonra bağlantı dizesini kullanarak okuyabilir `ConfigurationManager` , bağlamı'nın API'SİNDE `OnConfiguring` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e3220-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="e3220-112">Bir başvuru eklemeniz gerekebilir `System.Configuration` bu API'yi kullanabilmek için framework derlemesi.</span><span class="sxs-lookup"><span data-stu-id="e3220-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="e3220-113">Evrensel Windows Platformu (UWP)</span><span class="sxs-lookup"><span data-stu-id="e3220-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="e3220-114">Bir UWP uygulamasındaki bağlantı dizeleri, genellikle yalnızca yerel bir dosya adı belirten bir SQLite bağlantı cihazlardır.</span><span class="sxs-lookup"><span data-stu-id="e3220-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="e3220-115">Bunlar genellikle hassas bilgi içermez ve bir uygulama dağıtıldığında değiştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e3220-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="e3220-116">Bu nedenle, bu bağlantı dizeleri aşağıda gösterildiği gibi kodda bırakılması genellikle bir sakınca yoktur.</span><span class="sxs-lookup"><span data-stu-id="e3220-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="e3220-117">Kodların dışına taşımak istiyorsanız UWP ayarları kavramını destekler, bkz: [UWP belgelerin uygulama ayarları bölümü](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="e3220-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="e3220-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3220-118">ASP.NET Core</span></span>

<span data-ttu-id="e3220-119">ASP.NET Core yapılandırma sistemi çok esnektir ve bağlantı dizesini içinde depolanacak `appsettings.json`, bir ortam değişkeni, kullanıcı gizli dizi deposu veya başka bir yapılandırma kaynağı.</span><span class="sxs-lookup"><span data-stu-id="e3220-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="e3220-120">Bkz: [yapılandırma bölümü, ASP.NET Core belgeleri](https://docs.asp.net/en/latest/fundamentals/configuration.html) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="e3220-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="e3220-121">Aşağıdaki örnek, depolanan bağlantı dizesini gösterir `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="e3220-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="e3220-122">Bağlam genellikle yapılandırılan `Startup.cs` yapılandırmasından okunan bağlantı dizesine sahip.</span><span class="sxs-lookup"><span data-stu-id="e3220-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="e3220-123">Not `GetConnectionString()` yöntemi görünür olan anahtar için bir yapılandırma değeri `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="e3220-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="e3220-124">İçeri aktarmak gereken [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) bu genişletme yöntemi için kullanılacak ad.</span><span class="sxs-lookup"><span data-stu-id="e3220-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
