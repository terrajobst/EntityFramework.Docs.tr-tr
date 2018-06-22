---
title: Bağlantı dizeleri - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054099"
---
# <a name="connection-strings"></a><span data-ttu-id="12621-102">Bağlantı dizeleri</span><span class="sxs-lookup"><span data-stu-id="12621-102">Connection Strings</span></span>

<span data-ttu-id="12621-103">Çoğu veritabanı sağlayıcısı veritabanına bağlanmak için bağlantı dizesi çeşit gerektirir.</span><span class="sxs-lookup"><span data-stu-id="12621-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="12621-104">Bazen bu bağlantı dizesi korunması gereken hassas bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="12621-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="12621-105">Uygulamanızı geliştirme, test ve üretim gibi ortamlar arasında hareket ederken bağlantı dizesini değiştirin gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="12621-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="12621-106">.NET framework uygulamaları</span><span class="sxs-lookup"><span data-stu-id="12621-106">.NET Framework Applications</span></span>

<span data-ttu-id="12621-107">WinForms, WPF, konsol ve ASP.NET 4 gibi .NET framework uygulamaları denenmiş ve test edilen bağlantı dizesi deseni vardır.</span><span class="sxs-lookup"><span data-stu-id="12621-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="12621-108">Bağlantı dizesi uygulamaları App.config dosyasına (ASP.NET kullanıyorsanız, Web.config) eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="12621-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="12621-109">Bağlantı dizenizi kullanıcı adı ve parola gibi hassas bilgiler içeriyorsa yapılandırma dosyası kullanarak içerikleri koruyabilir [korumalı yapılandırma](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="12621-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="12621-110">`providerName` Ayarı veritabanı sağlayıcısı kodu aracılığıyla yapılandırıldığından App.config dosyasında depolanan EF çekirdek bağlantı dizelerini gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="12621-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="12621-111">Bağlantı dizesi kullanarak ardından okuyabilirsiniz `ConfigurationManager` , bağlamın API `OnConfiguring` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="12621-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="12621-112">Bir başvuru eklemeniz gerekebilir `System.Configuration` framework derleme bu API kullanmanız mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="12621-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="12621-113">Evrensel Windows Platformu (UWP)</span><span class="sxs-lookup"><span data-stu-id="12621-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="12621-114">Bağlantı bir UWP uygulamasında genellikle yalnızca yerel bir dosya adı belirten bir SQLite bağlantı dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="12621-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="12621-115">Bunlar genellikle hassas bilgileri içermez ve bir uygulamanın dağıtıldığını olarak değiştirilmesine gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="12621-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="12621-116">Bu nedenle, bu bağlantı dizeleri aşağıda gösterildiği gibi kodda bırakılması genellikle iyi.</span><span class="sxs-lookup"><span data-stu-id="12621-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="12621-117">Kodun dışına taşımak isterseniz, UWP ayarları kavramını destekler, bkz: [UWP belgelerine uygulama ayarları bölümünde](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="12621-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="12621-118">ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="12621-118">ASP.NET Core</span></span>

<span data-ttu-id="12621-119">ASP.NET çekirdek yapılandırma sistemi çok esnektir ve bağlantı dizesini alanında depolanacak `appsettings.json`, bir ortam değişkeni, kullanıcı gizli deposu veya başka bir yapılandırma kaynağı.</span><span class="sxs-lookup"><span data-stu-id="12621-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="12621-120">Bkz: [ASP.NET Core belgeleri yapılandırma bölümünü](https://docs.asp.net/en/latest/fundamentals/configuration.html) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="12621-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="12621-121">Aşağıdaki örnek, depolanan bağlantı dizesini gösterir `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="12621-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="12621-122">Bağlam genellikle yapılandırılan `Startup.cs` yapılandırmasından okunan bağlantı dizesine sahip.</span><span class="sxs-lookup"><span data-stu-id="12621-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="12621-123">Not `GetConnectionString()` yöntemi görünür olan anahtar için bir yapılandırma değeri `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="12621-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
