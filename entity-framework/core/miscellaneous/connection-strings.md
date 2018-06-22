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
# <a name="connection-strings"></a>Bağlantı dizeleri

Çoğu veritabanı sağlayıcısı veritabanına bağlanmak için bağlantı dizesi çeşit gerektirir. Bazen bu bağlantı dizesi korunması gereken hassas bilgiler içerir. Uygulamanızı geliştirme, test ve üretim gibi ortamlar arasında hareket ederken bağlantı dizesini değiştirin gerekebilir.

## <a name="net-framework-applications"></a>.NET framework uygulamaları

WinForms, WPF, konsol ve ASP.NET 4 gibi .NET framework uygulamaları denenmiş ve test edilen bağlantı dizesi deseni vardır. Bağlantı dizesi uygulamaları App.config dosyasına (ASP.NET kullanıyorsanız, Web.config) eklenmesi gerekir. Bağlantı dizenizi kullanıcı adı ve parola gibi hassas bilgiler içeriyorsa yapılandırma dosyası kullanarak içerikleri koruyabilir [korumalı yapılandırma](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> `providerName` Ayarı veritabanı sağlayıcısı kodu aracılığıyla yapılandırıldığından App.config dosyasında depolanan EF çekirdek bağlantı dizelerini gerekli değildir.

Bağlantı dizesi kullanarak ardından okuyabilirsiniz `ConfigurationManager` , bağlamın API `OnConfiguring` yöntemi. Bir başvuru eklemeniz gerekebilir `System.Configuration` framework derleme bu API kullanmanız mümkün olmayacaktır.

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

## <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

Bağlantı bir UWP uygulamasında genellikle yalnızca yerel bir dosya adı belirten bir SQLite bağlantı dizelerdir. Bunlar genellikle hassas bilgileri içermez ve bir uygulamanın dağıtıldığını olarak değiştirilmesine gerek yoktur. Bu nedenle, bu bağlantı dizeleri aşağıda gösterildiği gibi kodda bırakılması genellikle iyi. Kodun dışına taşımak isterseniz, UWP ayarları kavramını destekler, bkz: [UWP belgelerine uygulama ayarları bölümünde](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Ayrıntılar için.

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

## <a name="aspnet-core"></a>ASP.NET Çekirdeği

ASP.NET çekirdek yapılandırma sistemi çok esnektir ve bağlantı dizesini alanında depolanacak `appsettings.json`, bir ortam değişkeni, kullanıcı gizli deposu veya başka bir yapılandırma kaynağı. Bkz: [ASP.NET Core belgeleri yapılandırma bölümünü](https://docs.asp.net/en/latest/fundamentals/configuration.html) daha fazla ayrıntı için. Aşağıdaki örnek, depolanan bağlantı dizesini gösterir `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Bağlam genellikle yapılandırılan `Startup.cs` yapılandırmasından okunan bağlantı dizesine sahip. Not `GetConnectionString()` yöntemi görünür olan anahtar için bir yapılandırma değeri `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
