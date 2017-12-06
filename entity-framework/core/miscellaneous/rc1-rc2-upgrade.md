---
title: "RC2 - EF çekirdek EF çekirdek 1.0 RC1 ' yükseltme"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: ae5077c30642e3f40f51adee429821978f194460
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>EF çekirdek 1.0 RC1 1.0 RC2'ye yükseltme

Bu makalede RC2 için RC1 paketleri ile oluşturulmuş bir uygulama taşınması için yönergeler sağlar.

## <a name="package-names-and-versions"></a>Paket adları ve sürümleri

RC1 ve RC2 arasında biz "İçin Entity Framework Çekirdek" "Entity Framework' 7" değiştirildi. Daha fazla bilgiyi değişikliği nedenleri hakkında [bu posta Scott Hanselman yoluyla](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Bu değişikliği nedeniyle bizim paket adları değiştirildi `EntityFramework.*` için `Microsoft.EntityFrameworkCore.*` ve bizim sürümlerden `7.0.0-rc1-final` için `1.0.0-rc2-final` (veya `1.0.0-preview1-final` araçları için).

**Tamamen RC1 paketlerini kaldırın ve ardından RC2'yi yüklemeniz gerekir olanları.** Burada, bazı ortak paketler için eşleme verilmiştir.

| RC1 paketi                                               | RC2 eşdeğeri                                                       |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres<to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | RC2 için henüz kullanılamıyor                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Ad Alanları

Ad alanları değiştirildi paket adları ile birlikte `Microsoft.Data.Entity.*` için `Microsoft.EntityFrameworkCore.*`. Bu değişiklikle bulun ve değiştirin işleyebilir `using Microsoft.Data.Entity` ile `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Adlandırma kuralı değişiklikleri tablosu

Biz sürdü RC2 önemli bir işlev değişiklik adını kullanılmasıydır `DbSet<TEntity>` özelliği tablo adı olarak belirli bir varlık için eşleneceği, yalnızca sınıf adını yerine. Daha fazla bilgiyi bu değişiklik hakkında [ilgili duyuru sorunu](https://github.com/aspnet/Announcements/issues/167).

Varolan RC1 uygulamalar için aşağıdaki kod başlangıcına ekleme olan öneririz, `OnModelCreating` yöntemi RC1 adlandırma stratejisi tutmak için:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Yeni adlandırma stratejisi benimsemeyi istiyorsanız, başarılı bir şekilde yükseltme adımları ve ardından kod kaldırma ve tablo uygulamak için bir geçiş oluşturma rest Tamamlanıyor yeniden adlandırır öneririz.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / haline değiştirir (yalnızca ASP.NET Core projeler)

Uygulama hizmeti sağlayıcısı - Entity Framework Hizmetleri eklemek gerekiyordu RC1 içinde `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

RC2'yi çağrıları kaldırabilirsiniz `AddEntityFramework()`, `AddSqlServer()`vb..:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Bağlam seçeneklerini alır ve temel oluşturucuya aktaran türetilmiş içeriğiniz, bir oluşturucu eklemeniz gerekir. Biz bunları arka planda snuck korkutucu Sihirli bazıları kaldırılmış olduğundan bu gereklidir:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Bir IServiceProvider geçirme

Geçirir RC1 kodunuz varsa bir `IServiceProvider` bağlamına bu artık e taşınmıştır `DbContextOptions`, ayrı Oluşturucu parametresini olmak yerine. Kullanım `DbContextOptionsBuilder.UseInternalServiceProvider(...)` hizmet sağlayıcısı ayarlamak için.

### <a name="testing"></a>Test etme

Bunu yapmak için en yaygın senaryo test edilirken bir Inmemory veritabanı kapsamını denetlemek için oluştu. Güncelleştirilmiş bkz [test](testing/index.md) makale RC2 ile bunu bir örnek.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Uygulama hizmeti sağlayıcısı (yalnızca ASP.NET Core projeleri) iç Hizmetleri'nden çözme

Bir ASP.NET Core uygulama varsa ve uygulama servis sağlayıcı iç Hizmetleri'nden çözümlemek için EF istediğiniz bir aşırı yüklemesini yoktur `AddDbContext` , böylece bunu yapılandırmak:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Uygulama hizmet sağlayıcınıza iç EF Hizmetleri birleştirmek için bir nedeniniz yoksa dahili olarak kendi hizmetleri yönetmek EF izin vererek öneririz. Bunu yapmak isteyebilirsiniz ana nedeni, uygulama hizmet sağlayıcınıza EF dahili olarak kullandığı hizmetleri değiştirmek için kullanmaktır

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX komutları = > .NET CLI (yalnızca ASP.NET Core projeler)

Daha önce kullandıysanız `dnx ef` ASP.NET 5 projeleri için komutları, bunlar artık taşınmış için `dotnet ef` komutları. Aynı komut sözdizimini hala geçerlidir. Kullanabileceğiniz `dotnet ef --help` sözdizimi bilgi.

Komutları kayıtlı şekilde .NET CLI tarafından değiştirilen DNX nedeniyle RC2 değişti. İçinde artık komutları kayıtlı bir `tools` bölümüne `project.json`:

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
> Visual Studio kullanıyorsanız (Bu RC1 desteklenmiyordu) ASP.NET Core projeleri için EF komutları çalıştırmak için Paket Yöneticisi konsolu şimdi kullanabilirsiniz. Hala komutlar kaydetmeniz gerekir `tools` bölümünü `project.json` Bunu yapmak için.

## <a name="package-manager-commands-require-powershell-5"></a>Paket Yöneticisi komutları PowerShell 5 gerektirir

Visual Studio'da Paket Yöneticisi konsolunda Entity Framework komutları kullanıyorsanız PowerShell 5 yüklü emin olmak gerekir. Bu, sonraki sürümde kaldırılacak, geçici bir gereksinimdir (bkz [sorun #5327](https://github.com/aspnet/EntityFramework/issues/5327) daha fazla ayrıntı için).

## <a name="using-imports-in-projectjson"></a>Project.json içinde "Imports" kullanma

EF çekirdek'ın bağımlılıkları bazıları .NET standart henüz desteklemez. EF çekirdek .NET standart ve .NET Core projelerinde "project.json geçici bir çözüm olarak içeri aktarır" ekleme gerektirebilir.

EF eklerken NuGet restore bu hata iletisini görüntüler:

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

Taşınabilir profil "taşınabilir net451 + olduğu win8" el ile almak için geçici bir çözüm değildir. Olmadıkları olsa bile bu zorlar bu eşleşen bu ikili dosyalarını işlemek için NuGet standart .NET ile uyumlu bir çerçeve olarak sağlayın. "Taşınabilir net451 + olduğu win8" % 100 standart .NET ile uyumlu olmamasına rağmen geçiş PCL .NET standart için yeterince uyumlu değil. EF'ın bağımlılıkları sonunda .NET standart yükselttiğinizde içeri aktarmalar kaldırılabilir.

Birden çok çerçeveyi "içeri aktarmalar için" dizi sözdiziminde eklenebilir. Diğer içeri aktarmalar ek kitaplıklarını projenize eklemek durumlarda gerekli olabilir.

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
