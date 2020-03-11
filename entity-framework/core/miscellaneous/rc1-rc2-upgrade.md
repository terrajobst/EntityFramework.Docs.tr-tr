---
title: EF Core 1,0 RC1 'den RC2 'ye yükseltme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416612"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>EF Core 1,0 RC1 'ten 1,0 RC2 'ye yükseltme

Bu makalede, RC1 paketleriyle derlenmiş bir uygulamayı RC2 'ye taşımaya yönelik rehberlik sunulmaktadır.

## <a name="package-names-and-versions"></a>Paket adları ve sürümleri

RC1 ve RC2 arasında "Entity Framework 7" iken "Entity Framework Core" olarak değiştiriyoruz. [Scott Hanselman tarafından bu postadaki](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)değişikliğin nedenleri hakkında daha fazla bilgi edinebilirsiniz. Bu değişiklik nedeniyle, paket adlarımız `EntityFramework.*` 'den `Microsoft.EntityFrameworkCore.*` ve sürümlerimize `7.0.0-rc1-final` `1.0.0-rc2-final` (ya da araç için `1.0.0-preview1-final`) olarak değiştirilmiştir.

**RC1 paketlerini tamamen kaldırmanız ve ardından RC2 'yi yüklemeniz gerekir.** Bazı ortak paketlere yönelik eşleme aşağıda verilmiştir.

| RC1 paketi                                               | RC2 eşdeğeri                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework. MicrosoftSqlServer 7.0.0-RC1-son | Microsoft. EntityFrameworkCore. SqlServer 1.0.0-RC2-final      |
| EntityFramework. SQLite 7.0.0-RC1-son | Microsoft. EntityFrameworkCore. SQLite 1.0.0-RC2-final      |
| EntityFramework7. Npgsql 3.1.0-RC1-3     | NpgSql. EntityFrameworkCore. Postgres <to be advised>      |
| EntityFramework. SqlServerCompact35 7.0.0-RC1-final | EntityFrameworkCore. SqlServerCompact35 1.0.0-RC2-final      |
| EntityFramework. SqlServerCompact40 7.0.0-RC1-final | EntityFrameworkCore. SqlServerCompact40 1.0.0-RC2-final      |
| EntityFramework. InMemory 7.0.0-RC1-son | Microsoft. EntityFrameworkCore. InMemory 1.0.0-RC2-final      |
| EntityFramework. IBMDataServer 7.0.0-Beta1     | Henüz RC2 için kullanılamaz                                            |
| EntityFramework. Commands 7.0.0-RC1-final | Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1 'i-final |
| EntityFramework. MicrosoftSqlServer. Design 7.0.0-RC1-final | Microsoft. EntityFrameworkCore. SqlServer. Design 1.0.0-RC2-final      |

## <a name="namespaces"></a>Ad Alanları

Paket adlarıyla birlikte `Microsoft.Data.Entity.*` ad alanları `Microsoft.EntityFrameworkCore.*`olarak değiştirilir. Bu değişikliği, `using Microsoft.EntityFrameworkCore``using Microsoft.Data.Entity` Bul/Değiştir ile işleyebilirsiniz.

## <a name="table-naming-convention-changes"></a>Tablo adlandırma kuralı değişiklikleri

RC2 'de yaptığımız önemli bir işlevsel değişiklik, belirli bir varlık için `DbSet<TEntity>` özelliğinin adını yalnızca sınıf adı yerine, eşlendiği tablo adı olarak kullanmaktır. [İlgili duyuru sorununu](https://github.com/aspnet/Announcements/issues/167)bu değişiklik hakkında daha fazla bilgi edinebilirsiniz.

Mevcut RC1 uygulamaları için, RC1 adlandırma stratejisini korumak üzere `OnModelCreating` yönteminizin başlangıcına aşağıdaki kodu eklemeniz önerilir:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Yeni adlandırma stratejisini benimsemek istiyorsanız, Yükseltme adımlarının geri kalanını başarıyla tamamladık ve sonra kodu kaldırarak ve tablo yeniden adlandırmalarını uygulamak için bir geçiş oluşturmanız önerilir.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext/Startup.cs Changes (yalnızca ASP.NET Core projeleri)

RC1 'de, uygulama hizmeti sağlayıcısına Entity Framework Hizmetleri eklemeniz gerekiyordu-`Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

RC2 'de `AddEntityFramework()`, `AddSqlServer()`, vb. çağrıları kaldırabilirsiniz:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Ayrıca, bağlam seçeneklerini alan ve bunları temel oluşturucuya ileten türetilmiş bağlamına bir Oluşturucu eklemeniz gerekir. Bu, tüm Sary Magic 'in arka planda yer aldığı bir kısmını kaldırdığımız için gereklidir:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>IServiceProvider 'a geçirme

Bir `IServiceProvider` bağlama sahip olan RC1 kodunuz varsa, bu artık ayrı bir oluşturucu parametresi yerine `DbContextOptions`' a taşınır. Hizmet sağlayıcısını ayarlamak için `DbContextOptionsBuilder.UseInternalServiceProvider(...)` kullanın.

### <a name="testing"></a>Test Etme

Bunu yapmanın en yaygın senaryosu, test edilirken bir InMemory veritabanının kapsamını denetmektir. Bunu RC2 ile yapmakla ilgili bir örnek için güncelleştirilmiş [Test](testing/index.md) makalesine bakın.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Uygulama hizmeti sağlayıcısından Iç Hizmetleri çözme (yalnızca ASP.NET Core projeleri)

Bir ASP.NET Core uygulamanız varsa ve uygulama hizmeti sağlayıcısından iç Hizmetleri çözümlemek istiyorsanız, bunu yapılandırmanıza izin veren `AddDbContext` aşırı yüklemesi vardır:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> İç EF hizmetlerini uygulama hizmeti sağlayıcınızda birleştirmek için bir nedeniniz yoksa, EF 'in kendi hizmetlerini dahili olarak yönetmesine izin vermesini öneririz. Bunu yapmak isteyebileceğiniz başlıca neden, uygulama hizmeti sağlayıcınızı EF 'in dahili olarak kullandığı hizmetleri değiştirmek için kullanmaktır

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX komutları = > .NET CLı (yalnızca ASP.NET Core projeleri)

Daha önce ASP.NET 5 projeleri için `dnx ef` komutlarını kullandıysanız, bunlar artık `dotnet ef` komutlara taşınmıştır. Aynı komut söz dizimi hala geçerlidir. Sözdizimi bilgileri için `dotnet ef --help` kullanabilirsiniz.

DNX 'in .NET CLı tarafından değiştirilmeleri nedeniyle, komutların kayıtlı olduğu Yöntem RC2 'de değiştirilmiştir. Komutlar artık `project.json`bir `tools` bölümüne kaydedilir:

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
> Visual Studio kullanıyorsanız artık ASP.NET Core projelerine yönelik EF komutlarını çalıştırmak için Package Manager konsolunu kullanabilirsiniz (Bu, RC1 'de desteklenmez). Bunu yapmak için, `project.json` `tools` bölümündeki komutları kaydetmeniz gerekir.

## <a name="package-manager-commands-require-powershell-5"></a>Paket Yöneticisi komutları PowerShell 5 gerektirir

Visual Studio 'da Paket Yöneticisi konsolundaki Entity Framework komutlarını kullanırsanız, PowerShell 5 ' in yüklü olduğundan emin olmanız gerekir. Bu, sonraki sürümde kaldırılacak geçici bir gereksinimdir (daha fazla ayrıntı için bkz. [sorun #5327](https://github.com/aspnet/EntityFramework/issues/5327) ).

## <a name="using-imports-in-projectjson"></a>Project. JSON içinde "Imports" kullanma

EF Core bağımlılıklarından bazıları henüz .NET Standard desteklemez. .NET Standard ve .NET Core projelerindeki EF Core, geçici bir geçici çözüm olarak Project. json ' a "Imports" eklenmesini gerektirebilir.

EF eklendiğinde, NuGet geri yükleme şu hata iletisini görüntüler:

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

Geçici çözüm olarak, "Portable-net451 + Win8" taşınabilir profilini el ile içeri aktarırsınız. Bu, NuGet 'in bu ikili dosyaları, .NET Standard ile eşleşen bu ikilileri, uyumlu bir çerçeve olarak kabul etmeye zorlar, ancak olmasa da. "Portable-net451 + Win8 100", .NET Standard ile uyumlu olmasa da, PCL 'den .NET Standard geçişe yeterince uyumlu olur. EF 'in bağımlılıkları sonunda .NET Standard 'e yükselttiğinizde içeri aktarmalar kaldırılabilir.

Dizi sözdiziminde "Imports" öğesine birden çok çerçeve eklenebilir. Projenize ek kitaplıklar eklerseniz diğer içeri aktarmalar da gerekli olabilir.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Bkz. [sorun #5176](https://github.com/aspnet/EntityFramework/issues/5176).
