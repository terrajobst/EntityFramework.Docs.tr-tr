---
title: EF Core 2 - EF Core önceki sürümlerinden yükseltme
author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 6df57b04808307238287094c285727ececc6c18d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996820"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Uygulamaları, EF Core 2.0 için önceki sürümlerinden yükseltme

## <a name="procedures-common-to-all-applications"></a>Tüm uygulamalar için genel yordamlar

EF Core 2.0 için mevcut bir uygulamayı güncelleştirme gerektirebilir:

1. .NET Standard 2.0 destekleyen bir uygulamanın hedef .NET platformu yükseltiliyor. Bkz: [desteklenen platformlar](../platforms/index.md) daha fazla ayrıntı için.

2. EF Core 2.0 ile uyumlu olan hedef veritabanı için bir sağlayıcı tanımlar. Bkz: [EF Core 2.0 gerektirir 2.0 veritabanı sağlayıcısı](#ef-core-20-requires-a-20-database-provider) aşağıda.

3. Tüm EF Core paketleri (çalışma zamanı ve araç) 2.0 için yükseltiliyor. Başvurmak [yükleme EF Core](../get-started/install/index.md) daha fazla ayrıntı için.

4. Önemli değişiklikler için dengelemek için gereken kod değişikliklerini yapın. Bkz: [bozucu değişiklikleri](#breaking-changes) bölümünde daha fazla ayrıntı için.

## <a name="aspnet-core-applications"></a>ASP.NET Core uygulamaları

1. Özellikle bkz [uygulamanın hizmet sağlayıcısı başlatılırken yeni Düzen](#new-way-of-getting-application-services) aşağıda açıklanmıştır.

> [!TIP]  
> 2.0 uygulamaları güncelleştirme önemle tavsiye edilir ve çalışılabilmesi Entity Framework Code Migrations gibi ürün özellikleri için gerekli olan bu yeni düzen benimsenmesini. Yaygın bir alternatif [uygulamak *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. ASP.NET Core 2.0 hedefleyen uygulamalar, üçüncü taraf veritabanı sağlayıcıları yanı sıra ek bağımlılıkları olmadan EF Core 2.0 kullanabilirsiniz. Ancak, ASP.NET Core'nın önceki sürümlerini hedefleyen uygulamalar EF Core 2.0 kullanmak için ASP.NET Core 2. 0'ı yükseltmeniz gerekir. ASP.NET Core 2.0 uygulamaları yükseltme hakkında daha fazla ayrıntı görmek için [konu üzerinde ASP.NET Core belgeleri](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Yeni Değişiklikler

Biz, önemli ölçüde bizim mevcut API'lere ve 2.0 davranışlarını iyileştirmek için Fırsat yönlendirdik. Uygulamaların çoğu için inanıyoruz, ancak etkileri düşük yeniden derleme ve eski API'ler değiştirilecek destekli küçük değişiklikler gerektiren çoğu durumda, mevcut uygulama kodunun değiştirilmesi gereken birkaç geliştirmeleri vardır.

### <a name="new-way-of-getting-application-services"></a>Uygulama Hizmetleri yeni yöntemi

ASP.NET Core web uygulamaları için önerilen Düzen 2.0 EF Core 1.x içinde kullanılan tasarım zamanı mantığını kesildi şekilde güncelleştirildi. Tasarım zamanında, çağırmak daha önce EF Core isteriz `Startup.ConfigureServices` doğrudan uygulamanın hizmet sağlayıcısı erişebilmek için. ASP.NET Core 2.0 sürümünde, yapılandırma dışında başlatılır `Startup` sınıfı. EF Core genellikle kullanan uygulamalar, bağlantı dizesi bu nedenle yapılandırmasından erişim `Startup` kendisi tarafından artık yeterli değil. Bir ASP.NET Core 1.x uygulaması yükseltirseniz, EF Core araçlarını kullanırken aşağıdaki hatayı alabilirsiniz.

> Parametresiz bir Oluşturucusu 'ApplicationContext' üzerinde bulunamadı. Uygulaması ekleyin veya parametresiz bir Oluşturucu 'ApplicationContext için' Ekle ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' aynı bütünleştirilmiş kodun 'ApplicationContext' olarak

ASP.NET Core 2.0'ın varsayılan şablonda yeni bir tasarım zamanı kanca eklendi. Statik `Program.BuildWebHost` EF Core, tasarım zamanında uygulamanın hizmet sağlayıcısına erişmek yöntemi etkinleştirir. Bir ASP.NET Core 1.x uygulaması yükseltiyorsanız, güncelleştirmeniz gerekecektir `Program` aşağıdakine benzeyecek şekilde sınıfı.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

### <a name="idbcontextfactory-renamed"></a>IDbContextFactory yeniden adlandırıldı

Farklı uygulama kullanım düzenini ve kullanıcıların nasıl üzerinde daha fazla denetim sağlamak için bunların `DbContext` kullanılan tasarım zamanında, geçmişte, sağlanan sahibiz `IDbContextFactory<TContext>` arabirimi. Tasarım zamanında, EF Core araçları, projenizdeki bu arabirimin uygulamaları bulmak ve oluşturmak için bunu kullanın `DbContext` nesneleri.

Bu arabirim, bazı kullanıcılar için başka bir yeniden kullanmayı deneyin yanıltmaya çok genel bir ad olduğu `DbContext`-senaryolar oluşturma. Tasarım zamanında, geliştirdikleri kullanılacak EF araçları ardından çalıştığında guard'ı yakalandı ve komutlar gibi neden `Update-Database` veya `dotnet ef database update` başarısız.

Bu arabirim güçlü tasarım zamanı semantiği iletişim kurmak için biz olarak yeniden adlandırıldı `IDesignTimeDbContextFactory<TContext>`.

2.0 sürüm `IDbContextFactory<TContext>` hala var, ancak eski olarak işaretlendi.

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions kaldırıldı

Yukarıda açıklanan ASP.NET Core 2.0 değişiklikler nedeniyle, bulduk `DbContextFactoryOptions` artık yeni gerekti `IDesignTimeDbContextFactory<TContext>` arabirimi. Bunun yerine kullanarak seçenekleri aşağıda verilmiştir.

| DbContextFactoryOptions | Alternatif                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

### <a name="design-time-working-directory-changed"></a>Tasarım zamanı çalışma dizini değiştirildi

ASP.NET Core 2.0 değişiklikleri de kullandığı çalışma dizinini gerekli `dotnet ef` uygulamanız çalışırken Visual Studio tarafından kullanılan çalışma dizini ile hizalamak için. Bu gözlemlenebilir bir yan etkisi olması için kullandıkları gibi dosya adları şimdi proje dizinine ve çıkış dizini göreli olan bu SQLite ' dir.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 2.0 veritabanı sağlayıcısı gerektirir

EF Core 2.0 için birçok basitleştirme ve çalışma biçimini veritabanı sağlayıcıları geliştirmeleri yaptık. Başka bir deyişle, 1.0.x kullanılır ve 1.1.x sağlayıcıları EF Core 2.0 ile çalışmaz.

SQL Server ve SQLite sağlayıcıları EF ekibi tarafından sağlanan ve 2.0 sürümlerinde kullanılabilir 2.0 bir parçası olarak bırakın. Açık kaynak üçüncü taraf sağlayıcılar için [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), ve [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) 2.0 için güncelleştirilir. Diğer tüm sağlayıcıları için sağlayıcı Yazar temasa geçin.

### <a name="logging-and-diagnostics-events-have-changed"></a>Günlüğe kaydetme ve tanılama olayları değiştirildi

Not: Bu değişikliklerin çoğu uygulama kodu etkilememesi gerekir.

' % S'olay kimlikleri için gönderilen iletiler için bir [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) 2.0 sürümünde değiştirilmiştir. Olay kimlikleri, artık EF Core kod arasında benzersizdir. Bu iletiler şimdi de, örneğin, MVC kullanılan yapılandırılmış günlük kaydı için standart deseni izleyin.

Günlükçü kategoriler de değiştirilmiştir. İyi bilinen bir kategori kümesini aracılığıyla erişilen artık yoktur [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) olayları artık ilgili olarak aynı olay kimliği adları kullanma `ILogger` iletileri. Olay yükü türetilen tüm nominal türleridir [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Olay kimlikleri, yükü türleri ve kategorileri bölümünde belgelenmiştir [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) ve [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) sınıfları.

Kimlikleri, ayrıca yeni Microsoft.EntityFrameworkCore.Diagnostics ad alanına Microsoft.EntityFrameworkCore.Infraestructure taşındı.

### <a name="ef-core-relational-metadata-api-changes"></a>EF Core ilişkisel meta veri API'si değişiklikleri

EF Core 2.0 artık farklı bir derler [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) için kullanılan her farklı bir sağlayıcı. Bu genellikle uygulamaya saydamdır. Tüm erişimi olacak şekilde bu düşük düzeyli meta verileri API'leri bir basitleştirme kolaylaştırılan _ortak ilişkisel meta veri kavramlarını_ her zaman bir çağrı yoluyla yapılan `.Relational` yerine `.SqlServer`, `.Sqlite`, vb. Örneğin, 1.1.x kodu şöyle:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Artık şu şekilde yazılmalıdır:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Gibi yöntemleri kullanmak yerine `ForSqlServerToTable`, genişletme yöntemleri geçerli sağlayıcı kullanımda göre koşullu kodu yazmak kullanılabilen şimdi. Örneğin:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Bu değişiklik yalnızca tanımlanmış API'ler/meta geçerlidir Not _tüm_ ilişkisel sağlayıcıları. API ve meta verileri aynı kalır, yalnızca tek bir sağlayıcıya özel olduğunda. Örneğin, SQL Server için belirli Kümelenmiş dizinler için `ForSqlServerIsClustered` ve `.SqlServer().IsClustered()` yine de kullanılması gerekir.

### <a name="dont-take-control-of-the-ef-service-provider"></a>EF hizmet sağlayıcısı'nın denetimini ele geçirmesine yok

EF Core kullanan bir iç `IServiceProvider` (bir bağımlılık ekleme kapsayıcısını) için kendi iç uygulama. Uygulamaları oluşturmak ve özel durumlar dışında bu sağlayıcıyı yönetmek EF Core izin vermelidir. Çağrıları kaldırma kesin düşünün `UseInternalServiceProvider`. Bir uygulama çağırmak gerekli olmaması halinde `UseInternalServiceProvider`, lütfen göz önünde bulundurun [soruna dosyalama](https://github.com/aspnet/EntityFramework/Issues) biz senaryonuz işlemek için kullanabileceğiniz diğer yöntemler araştırabileceği şekilde.

Çağırma `AddEntityFramework`, `AddEntityFrameworkSqlServer`, vb. gerekli değildir uygulama kodu tarafından sürece `UseInternalServiceProvider` olarak da adlandırılır. Mevcut tüm çağrıları kaldırın `AddEntityFramework` veya `AddEntityFrameworkSqlServer`, vs. `AddDbContext` yine de aynı şekilde önceki gibi kullanılmalıdır.

### <a name="in-memory-databases-must-be-named"></a>Bellek içi veritabanı olarak adlandırılmalıdır

Genel adlandırılmamış bellek içi veritabanı kaldırıldı ve bunun yerine tüm bellek içi veritabanı olarak adlandırılmalıdır. Örneğin:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Bu oluşturur / "Veritabanım" adlı bir veritabanı kullanır. Varsa `UseInMemoryDatabase` aynı ada sahip birden fazla bağlam örneği tarafından paylaşılmasına izin vererek aynı bellek içi veritabanı kullanılır yeniden adlandırılır.

### <a name="read-only-api-changes"></a>Salt okunur API'SİNDEKİ değişiklikler

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, ve `IsStoreGeneratedAlways` geçersiz kılınmış ve yerine [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) ve [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Bu davranışların herhangi bir özelliği (yalnızca depoda üretilmiş Özellikler) uygulayın ve özelliğinin değeri bir veritabanı satır ekleme yaparken nasıl kullanılması gerektiği belirlemek (`BeforeSaveBehavior`) veya ne zaman mevcut bir veritabanı güncelleniyor satır (`AfterSaveBehavior`).

Özellikler olarak işaretlenmiş [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (örneğin, hesaplanan sütunlar) özelliği ayarlanmış herhangi bir değeri varsayılan olarak göz ardı eder. Bu depoda üretilmiş bir değer olup herhangi bir değere ayarlayın veya bırakıldığı izlenen varlık üzerinde değişiklik bakılmaksızın her zaman alınacaktır anlamına gelir. Bu farklı bir ayarlayarak değiştirilebilir `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Yeni ClientSetNull silme davranışı

Önceki sürümlerde, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) sahip varlıklar için bir davranış izlenen bağlam tarafından daha fazla eşleşen kapalı `SetNull` semantiği. EF Core 2.0, yeni bir `ClientSetNull` davranışı, isteğe bağlı bir ilişki için varsayılan olarak sunulmuş. Bu davranışı `SetNull` izlenen varlık semantiği ve `Restrict` EF Core kullanılarak oluşturulan veritabanları için davranış. Deneyimimizde, bu izlenen varlıkları ve veritabanı için beklenen/faydalı davranışları vardır. `DeleteBehavior.Restrict` Şimdi isteğe bağlı ilişki için ayarlandığında izlenen varlıklar için uygulanır.

### <a name="provider-design-time-packages-removed"></a>Sağlayıcı tasarım zamanı paketleri kaldırıldı

`Microsoft.EntityFrameworkCore.Relational.Design` Paket kaldırıldı. İçeriğiyle birleştirilmiş içine `Microsoft.EntityFrameworkCore.Relational` ve `Microsoft.EntityFrameworkCore.Design`.

Bu sağlayıcı tasarım zamanı paketler yayar. Bu paketleri (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, vs.) kaldırıldı ve içeriklerini ana sağlayıcısı paketlerine birleştirilir.

Etkinleştirmek için `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold` EF Core 2.0 sürümünde, yalnızca tek bir sağlayıcı paketi başvurusu gerekir:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
