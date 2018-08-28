---
title: EF Core 1.0 RC1'den RC2 - EF Core yükseltme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996903"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>EF Core 1.0 RC1'den 1.0 RC2'ye yükseltme

Bu makalede RC1 paketler RC2 ile oluşturulmuş bir uygulamada taşımak için yönergeler sağlar.

## <a name="package-names-and-versions"></a>Paket adları ve sürümler

RC1 ile RC2 arasında "İçin Entity Framework Core" "Entity Framework' 7" değiştirdik. Daha fazla değişiklik nedenleri hakkında [Scott Hanselman tarafından Bu gönderiyi](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Bu değişiklik nedeniyle, bizim paket adları değiştirildi `EntityFramework.*` için `Microsoft.EntityFrameworkCore.*` ve bizim sürümlerinden `7.0.0-rc1-final` için `1.0.0-rc2-final` (veya `1.0.0-preview1-final` araçları için).

**Tamamen RC1 paketlerini kaldırın ve ardından RC2 yüklemek ihtiyacınız olanları.** Bazı yaygın paketler için eşleme aşağıda verilmiştir.

| RC1'de paket                                               | RC2'yi eşdeğerine                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | RC2 için henüz kullanılamıyor                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Ad Alanları

Paket adları ile birlikte ad değiştirildi `Microsoft.Data.Entity.*` için `Microsoft.EntityFrameworkCore.*`. Bu değişiklikle birlikte Bul/Değiştir işleyebilir `using Microsoft.Data.Entity` ile `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Adlandırma kuralı değişikliklerinin tablo

RC2'deki attığımız önemli işlevsel bir değişiklik adını kullanılmasıydır `DbSet<TEntity>` özelliği tablo adını olarak belirli bir varlık için eşleneceği, yalnızca sınıf adı yerine. Daha fazla bilgi bu değişiklik hakkında [ilgili duyuru sorunu](https://github.com/aspnet/Announcements/issues/167).

Mevcut RC1 uygulamalar için aşağıdaki kodu ekleyerek başlangıcına kadar olan öneririz, `OnModelCreating` RC1 adlandırma stratejisi tutmak yöntemi:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Yeni adlandırma stratejisi benimsemek isterseniz başarıyla yeniden adlandırır ve yükseltme adımları ve ardından kod kaldırma tablo uygulamak için bir geçiş oluşturmaya rest Tamamlanıyor öneririz.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs değişiklikler (yalnızca ASP.NET Core projeleri için)

RC1'de uygulama hizmet sağlayıcısı - Entity Framework Hizmetleri eklemek zorunda `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

RC2'deki çağrıları kaldırabilirsiniz `AddEntityFramework()`, `AddSqlServer()`vb..:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Bağlamı seçenekleri alır ve bunları temel oluşturucuya geçirir, türetilmiş bağlamı, bir oluşturucu eklemeniz gerekir. Bunları arka planda snuck scary Sihirli bazıları kaldırdık çünkü bu gereklidir:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>İçinde bir IServiceProvider geçirme

Geçirir RC1 kodunuz varsa bir `IServiceProvider` bağlamına, bunu şimdi e taşınmıştır `DbContextOptions`, ayrı Oluşturucu parametresi olan yerine. Kullanım `DbContextOptionsBuilder.UseInternalServiceProvider(...)` hizmet sağlayıcısı ayarlanamadı.

### <a name="testing"></a>Sınama

Bunu yapmak için en yaygın senaryo test ederken Inmemory veritabanı kapsamını denetlemek için oluştu. Güncelleştirilmiş bkz [test](testing/index.md) makale RC2 ile bunu bir örnek.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Uygulama hizmeti sağlayıcısı (yalnızca ASP.NET Core projeleri için) iç hizmetlerinden çözümleme

Bir ASP.NET Core uygulaması varsa ve uygulama servis sağlayıcısından iç hizmetlerin çözümlenmesi için EF istediğiniz bir aşırı yüklemesini yoktur `AddDbContext` bu yapılandırmanıza olanak tanır:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Uygulama hizmet sağlayıcınıza iç EF Hizmetleri birleştirmek için bir nedeniniz yoksa dahili olarak kendi Servisleri yönetilecek EF izin vererek öneririz. Bunu yapmak isteyebileceğiniz temel nedeni uygulama hizmet sağlayıcınıza EF dahili olarak kullandığı hizmetleri değiştirilecek kullanmaktır

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX komutlarını = > .NET CLI (yalnızca ASP.NET Core projeleri için)

Daha önce kullandıysanız `dnx ef` komutları ASP.NET 5 projeleri için bunlar artık taşınmış için `dotnet ef` komutları. Aynı komut sözdizimi hala geçerlidir. Kullanabileceğiniz `dotnet ef --help` söz dizimi bilgi.

.NET CLI tarafından değiştirilen DNX nedeniyle, RC2'deki komutları kayıtlı şekilde değiştirdi. İçinde artık komutları kayıtlı bir `tools` konusundaki `project.json`:

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Visual Studio kullanıyorsanız, Paket Yöneticisi konsolu artık (Bu RC1'de desteklenmiyordu) ASP.NET Core projeleri için EF komutlarını çalıştırmak için kullanabilirsiniz. Komutlar kaydetmek yine `tools` bölümünü `project.json` Bunu yapmak için.

## <a name="package-manager-commands-require-powershell-5"></a>Paket Yöneticisi komutları PowerShell 5 gerektirir

Entity Framework komutlar Visual Studio'da Paket Yöneticisi konsolu kullanıyorsanız, PowerShell 5'in yüklü olduğundan emin olmak gerekir. Bu, sonraki sürümde kaldırılacak, geçici bir gereksinimdir (bkz [sorun #5327](https://github.com/aspnet/EntityFramework/issues/5327) daha fazla ayrıntı için).

## <a name="using-imports-in-projectjson"></a>Project.json içinde "içeri aktarmalar" kullanma

EF Core'nın bağımlılıkları bazıları .NET Standard henüz desteklemiyor. EF Core .NET Standard ve .NET Core projelerinde, "project.json geçici bir çözüm olarak alır" ekleyerek gerektirebilir.

NuGet geri yükleme EF eklerken, bu hata iletisini görüntüler:

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

Taşınabilir profil "taşınabilir net451 + win8" el ile içeri aktarmak için çözüm olabilir. Olmadıkları olsa bile bu zorlar bu eşleşen bu ikili dosyalarını işlemek için NuGet ile .NET Standard uyumlu çerçeve sağlar. "Taşınabilir net451 + win8" %100 .NET Standard ile uyumlu olmamasına karşın, geçiş PCL .NET Standard için yeteri kadar uyumlu. EF'ın bağımlılıkları sonunda .NET Standard yükselttiğinizde içeri aktarmalar kaldırılabilir.

Birden çok çerçeveyi "Alınanlar için" dizi sözdiziminde eklenebilir. Diğer içeri aktarmalar ek kitaplıklar projenize eklediğiniz durumlarda gerekli olabilir.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Bkz: [#5176 sorun](https://github.com/aspnet/EntityFramework/issues/5176).
