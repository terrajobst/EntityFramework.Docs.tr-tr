---
title: Bağlantı dizelerini - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: c306f9ca7a51fc9e3db18e883fd44f56dd1a3cb4
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286456"
---
# <a name="connection-strings"></a>Bağlantı Dizeleri

Çoğu veritabanı sağlayıcısı veritabanına bağlanmak için bağlantı dizesi biçimi gerektirir. Bazen bu bağlantı dizesi, korunması gereken hassas bilgiler içerir. Uygulamanızı geliştirme, test ve üretim ortamları arasında taşırken bağlantı dizesini değiştirmeniz gerekebilir.

## <a name="net-framework-applications"></a>.NET framework uygulamaları

.NET framework uygulamaları, WinForms, WPF, konsol ve ASP.NET 4 gibi denenmiş ve test edilmiş bağlantı dize deseni vardır. Bağlantı dizesi (ASP.NET kullanıyorsanız, Web.config), uygulamanızın App.config dosyasına eklenmelidir. Bağlantı dizenizi kullanıcı adı ve parola gibi hassas bilgileri içeriyorsa yapılandırma dosyası kullanarak içerikleri koruyabilir [korumalı yapılandırma](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> `providerName` Ayarı veritabanı sağlayıcısı kod yapılandırıldığından App.config dosyasında depolanan EF Core bağlantı dizelerini gerekli değildir.

Daha sonra bağlantı dizesini kullanarak okuyabilir `ConfigurationManager` , bağlamı'nın API'SİNDE `OnConfiguring` yöntemi. Bir başvuru eklemeniz gerekebilir `System.Configuration` bu API'yi kullanabilmek için framework derlemesi.

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

Bir UWP uygulamasındaki bağlantı dizeleri, genellikle yalnızca yerel bir dosya adı belirten bir SQLite bağlantı cihazlardır. Bunlar genellikle hassas bilgi içermez ve bir uygulama dağıtıldığında değiştirilmesi gerekmez. Bu nedenle, bu bağlantı dizeleri aşağıda gösterildiği gibi kodda bırakılması genellikle bir sakınca yoktur. Kodların dışına taşımak istiyorsanız UWP ayarları kavramını destekler, bkz: [UWP belgelerin uygulama ayarları bölümü](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Ayrıntılar için.

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

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core yapılandırma sistemi çok esnektir ve bağlantı dizesini içinde depolanacak `appsettings.json`, bir ortam değişkeni, kullanıcı gizli dizi deposu veya başka bir yapılandırma kaynağı. Bkz: [yapılandırma bölümü, ASP.NET Core belgeleri](https://docs.asp.net/en/latest/fundamentals/configuration.html) daha fazla ayrıntı için. Aşağıdaki örnek, depolanan bağlantı dizesini gösterir `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Bağlam genellikle yapılandırılan `Startup.cs` yapılandırmasından okunan bağlantı dizesine sahip. Not `GetConnectionString()` yöntemi görünür olan anahtar için bir yapılandırma değeri `ConnectionStrings:<connection string name>`. İçeri aktarmak gereken [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) bu genişletme yöntemi için kullanılacak ad.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
