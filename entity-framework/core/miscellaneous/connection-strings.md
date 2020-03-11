---
title: Bağlantı dizeleri-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416591"
---
# <a name="connection-strings"></a>Bağlantı Dizeleri

Veritabanı sağlayıcılarının çoğu, veritabanına bağlanmak için bir bağlantı dizesi biçimi gerektirir. Bazen bu bağlantı dizesi korunması gereken hassas bilgiler içerir. Uygulamanızı geliştirme, test ve üretim gibi ortamlar arasında taşırken bağlantı dizesini de değiştirmeniz gerekebilir.

## <a name="winforms--wpf-applications"></a>WinForms & WPF uygulamaları

WinForms, WPF ve ASP.NET 4 uygulamaları, denenen ve sınanan bir bağlantı dizesi düzenine sahiptir. Bağlantı dizesi uygulamanızın App. config dosyasına eklenmelidir (ASP.NET kullanıyorsanız Web. config). Bağlantı dizeniz Kullanıcı adı ve parola gibi hassas bilgiler içeriyorsa, yapılandırma dosyasının içeriğini [gizli yönetici aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)kullanarak koruyabilirsiniz.

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
> Veritabanı sağlayıcısı kod aracılığıyla yapılandırıldığından, App. config dosyasında depolanan EF Core bağlantı dizeleri `providerName` ayarı gerekli değildir.

Daha sonra `ConfigurationManager` API 'sini kullanarak bağlantı dizesini bağlam `OnConfiguring` yönteminde okuyabilirsiniz. Bu API 'yi kullanabilmek için `System.Configuration` Framework derlemesine bir başvuru eklemeniz gerekebilir.

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

UWP uygulamasındaki bağlantı dizeleri genellikle yerel bir dosya adı belirten bir SQLite bağlantıdır. Genellikle hassas bilgiler içermez ve bir uygulama dağıtıldığında değiştirilmeleri gerekmez. Bu şekilde, bu bağlantı dizeleri genellikle aşağıda gösterildiği gibi, kodda ayrılmalıdır. Kod dışına taşımak isterseniz, UWP ayar kavramını destekler, Ayrıntılar için [UWP belgelerinin uygulama ayarları bölümüne](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) bakın.

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

ASP.NET Core yapılandırma sistemi çok esnektir ve bağlantı dizesi `appsettings.json`, bir ortam değişkeni, Kullanıcı gizli dizisi ya da başka bir yapılandırma kaynağı içinde depolanabilir. Daha fazla bilgi için [ASP.NET Core belgelerinin yapılandırma bölümüne](https://docs.asp.net/en/latest/fundamentals/configuration.html) bakın. Aşağıdaki örnek, `appsettings.json`depolanan bağlantı dizesini gösterir.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Bağlam genellikle bağlantı dizesinin yapılandırmadan okunmakta olan `Startup.cs` içinde yapılandırılır. `GetConnectionString()` yöntemi, anahtarı `ConnectionStrings:<connection string name>`olan bir yapılandırma değeri arar. Bu genişletme yöntemini kullanmak için [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) ad alanını içeri aktarmanız gerekir.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
