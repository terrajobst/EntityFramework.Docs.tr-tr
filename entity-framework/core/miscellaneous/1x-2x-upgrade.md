---
title: "EF çekirdek 2 - EF çekirdek önceki sürümlerinden yükseltme"
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 0bd1ea2476621f826cca7d4a526a49a1b902acf8
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Uygulamaları EF çekirdek 2.0 önceki sürümlerinden yükseltme

## <a name="procedures-common-to-all-applications"></a>Tüm uygulamalar için yordamlar ortak

EF çekirdek 2.0 varolan bir uygulamaya güncelleştirme gerektirebilir:

1. Uygulama standart .NET 2.0 destekleyen bir hedef .NET platformu yükseltiliyor. Bkz: [desteklenen platformlar](../platforms/index.md) daha fazla ayrıntı için.

2. EF çekirdek 2.0 ile uyumlu olan, hedef veritabanı için bir sağlayıcı tanımlar. Bkz: [EF çekirdek 2.0 gerektirir 2.0 veritabanı sağlayıcısı](#ef-core-20-requires-a-20-database-provider) aşağıda.

3. Tüm EF çekirdek paketleri (çalışma zamanı ve araçları) 2.0 için yükseltme. Başvurmak [yükleme EF çekirdek](../get-started/install/index.md) daha fazla ayrıntı için.

4. Yeni değişiklikler dengelemek için gerekli kod değişiklikleri yapın. Bkz: [son değişiklikler](#breaking-changes) daha fazla ayrıntı için bölüm aşağıda.

## <a name="aspnet-core-applications"></a>ASP.NET Core uygulamaları

1. Özellikle bkz [uygulamanın hizmet sağlayıcısı başlatma için yeni düzeni](#new-way-of-getting-application-services) aşağıda açıklanmıştır.

> [!TIP]  
> 2.0 uygulamaları güncelleştirme önerilir ve çalışmaya sırayla Entity Framework Çekirdek geçişler gibi ürün özellikleri için gereken bu yeni düzeni benimseme. Ortak bir alternatif [uygulamak *IDesignTimeDbContextFactory\<TContext >*](configuring-dbcontext.md#using-idesigntimedbcontextfactorytcontext).

2. ASP.NET Core 2.0 hedefleme uygulamaları EF çekirdek 2.0 üçüncü taraf veritabanı sağlayıcıları yanı sıra ek bağımlılıklar olmadan kullanabilir. Ancak, ASP.NET Core önceki sürümlerini hedefleyen uygulamalar EF çekirdek 2.0 kullanmak için ASP.NET Core 2.0 sürümüne yükseltmek gerekir. ASP.NET Core uygulamaları 2.0 yükseltme hakkında daha fazla bilgi için bkz [konu ASP.NET Core belgelerine](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Yeni Değişiklikler

Biz önemli ölçüde bizim mevcut API'lerini ve 2.0 davranışlarını iyileştirmek için fırsatın gerçekleştirmişsiniz. Mevcut uygulama kodunun değiştirme gerektirebilir birkaç geliştirmeleri vardır, uygulamaların çoğu için inanılmaktadır rağmen etkisi düşük yalnızca yeniden derlenmek ve eski API'ları değiştirmek için küçük destekli değişiklikler gerektiren çoğu durumda olacaktır.

### <a name="new-way-of-getting-application-services"></a>Uygulama Hizmetleri alma yeni yolu

ASP.NET Core web uygulamaları için önerilen desenini 2.0 EF çekirdek 1.x içinde kullanılan tasarım zamanı mantığı ihlal şekilde güncelleştirildi. Tasarım zamanında, çağrılacak EF çekirdek önceden isteriz `Startup.ConfigureServices` uygulamanın hizmet sağlayıcısı doğrudan erişmek için. ASP.NET Core 2. 0'dışında yapılandırma başlatılmadan `Startup` sınıfı. EF çekirdek genellikle kullanarak uygulamaları kendi bağlantı dizesi bu nedenle yapılandırmasından erişim `Startup` kendisi tarafından artık yeterli değildir. Bir ASP.NET Core 1.x uygulama yükseltirseniz, EF çekirdek araçlarını kullanırken aşağıdaki hata iletisini alabilirsiniz.

> Parametresiz bir kurucusu 'ApplicationContext üzerinde' bulundu. 'ApplicationContext' parametresiz bir oluşturucu ekleyin veya uygulaması Ekle ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' 'ApplicationContext' ile aynı bütünleştirilmiş kodda

Yeni bir tasarım zamanı kancası ASP.NET Core 2. 0'ın varsayılan şablonda eklendi. Statik `Program.BuildWebHost` yöntemi EF tasarım zamanında uygulamanın hizmet sağlayıcısına erişmek çekirdek sağlar. Bir ASP.NET Core 1.x uygulama yükseltiyorsanız, güncelleştirmeniz gerekecektir `Program` aşağıdakine benzeyecek şekilde sınıfı.

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

### <a name="idbcontextfactory-renamed"></a>Idbcontextfactory yeniden adlandırıldı

Farklı uygulama düzenleri desteği ve kullanıcıları nasıl üzerinde daha fazla denetime için kendi `DbContext` kullanılan tasarım zamanında, geçmişte, sağlanan sahibiz `IDbContextFactory<TContext>` arabirimi. Tasarım zamanında, EF çekirdek araçları projenizdeki bu arabirim uygulamaları bulmak ve oluşturmak için kullanın `DbContext` nesneleri.

Bu arabirim, bazı kullanıcılar için başka bir yeniden kullanmayı deneyin yanıltmaya çok genel bir adı değiştirilmiş `DbContext`-senaryolar oluşturma. EF araçları sonra uygulamalarının tasarım zamanında kullanmaya çalıştığında koruma devre dışı yakalanan ve komutları gibi neden `Update-Database` veya `dotnet ef database update` başarısız.

Bu arabirim güçlü tasarım zamanı semantiği iletişim kurmak için şu şekilde yeniden adlandırdınız `IDesignTimeDbContextFactory<TContext>`.

2.0 için sürüm `IDbContextFactory<TContext>` hala var, ancak kullanımdan kaldırılmış olarak işaretlenmiş.

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions kaldırıldı

Yukarıda açıklanan ASP.NET Core 2.0 değişiklikler nedeniyle, bulduk `DbContextFactoryOptions` artık yeni gerekti `IDesignTimeDbContextFactory<TContext>` arabirimi. Burada, bunun yerine kullanarak alternatifleri bulunmaktadır.

DbContextFactoryOptions | Alternatifi
--- | ---
ApplicationBasePath | AppContext.BaseDirectory
ContentRootPath | Directory.GetCurrentDirectory()
EnvironmentName | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")

### <a name="design-time-working-directory-changed"></a>Tasarım zamanı çalışma dizini değiştirildi

ASP.NET Core 2.0 değişiklik de tarafından kullanılan çalışma dizini gerekli `dotnet ef` , uygulamanızı çalıştırırken Visual Studio tarafından kullanılan çalışma dizini ile hizalamak için. Bu bir observable yan etkisi olması için kullandıkları gibi dosya adları proje dizininin ve çıktı dizini göreli sunulmuştur bu SQLite ' dir.

### <a name="ef-core-20-requires-a-20-database-provider"></a>2.0 veritabanı sağlayıcısı EF çekirdek 2.0 gerektirir

EF çekirdek 2.0 için biz birçok basitleştirme ve iş yolu veritabanı sağlayıcıları geliştirmeler yaptık. Başka bir deyişle, 1.0.x ve 1.1.x sağlayıcıları EF çekirdek 2.0 ile çalışmaz.

SQL Server ve SQLite sağlayıcıları EF ekibi tarafından gönderilen ve 2.0 sürümleri kullanılabilir olacak 2.0 bir parçası olarak bırakın. Açık kaynak üçüncü taraf sağlayıcılar için [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), ve [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) 2.0 için güncelleştirilmiştir. Diğer tüm sağlayıcıları, sağlayıcı yazan temasa geçin.

### <a name="logging-and-diagnostics-events-have-changed"></a>Günlüğe kaydetme ve tanılama olayları değişti

Not: Bu değişikliklerin çoğu uygulama kodu etkisi değil.

Gönderilen iletiler için olay kimliklerinin bir [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) 2.0 değiştirilmiştir. Olay kimlikleri, şimdi EF çekirdek kodu arasında benzersizdir. Bu iletiler de artık yapılandırılmış günlük kullandığı, örneğin, MVC için standart yol izler.

Günlükçü kategoriler de değiştirilmiştir. Kategoriler iyi bilinen bir dizi üzerinden erişilen artık yoktur [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) olaylarını şimdi ilgili olarak aynı olay kimliği adları kullanma `ILogger` iletileri. Olay yükü türetilen tüm nominal türleridir [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Olay kimlikleri, yükü türleri ve kategorileri konusunda belgelenir [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) ve [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) sınıfları.

Kimlikleri de Microsoft.EntityFrameworkCore.Infraestructure yeni Microsoft.EntityFrameworkCore.Diagnostics ad alanına taşınmış.

### <a name="ef-core-relational-metadata-api-changes"></a>EF çekirdek ilişkisel API meta veri değişiklikleri

EF çekirdek 2.0 artık farklı bir oluşturacaksınız [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) kullanılan her farklı bir sağlayıcı için. Bu, genellikle uygulamanız için saydamdır. Erişim herhangi bir şekilde bu alt düzey meta verilerin API'leri basitleştirme sayesinde kolaylaşır _ortak ilişkisel meta verileri kavramlar_ her zaman bir çağrı aracılığıyla yapılan `.Relational` yerine `.SqlServer`, `.Sqlite`vb. Örneğin, 1.1.x kodu şuna benzer:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Şimdi şu şekilde yazılmalıdır:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Yöntemler gibi kullanmak yerine `ForSqlServerToTable`, genişletme yöntemleri şu anda kullanımda geçerli sağlayıcı göre koşullu kod yazmak kullanılabilir durumda. Örneğin:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Bu değişikliği yalnızca API/için tanımlanan meta veri uygular Not _tüm_ ilişkisel sağlayıcıları. API ve meta verileri aynı kalır yalnızca tek bir sağlayıcıya özel olduğunda. Örneğin, kümelenmiş dizinler SQL Server için belirli şekilde `ForSqlServerIsClustered` ve `.SqlServer().IsClustered()` hala kullanılması gerekir.

### <a name="dont-take-control-of-the-ef-service-provider"></a>EF hizmet sağlayıcısı'nın denetimini ele yok

EF çekirdek kullanan bir iç `IServiceProvider` (örn. bir bağımlılık ekleme kapsayıcısını) kendi iç uygulama. Uygulamalar oluşturmak ve özel durumlar dışında bu sağlayıcıyı yönetmek EF çekirdek izin vermeniz gerekir. Kesinlikle hiçbir çağrı kaldırmayı düşünün `UseInternalServiceProvider`. Bir uygulamayı çağırmak gerekiyorsa `UseInternalServiceProvider`, lütfen düşünün [bir sorun dosyalama](https://github.com/aspnet/EntityFramework/Issues) senaryonuz işlemek için diğer yolları inceleme yapmak amacıyla.

Çağırma `AddEntityFramework`, `AddEntityFrameworkSqlServer`, vb. gerekli değildir uygulama kodu tarafından sürece `UseInternalServiceProvider` olarak da adlandırılır. Var olan tüm çağrıları kaldırın `AddEntityFramework` veya `AddEntityFrameworkSqlServer`, vb. `AddDbContext` hala aynı şekilde eskisi kullanılmalıdır.

### <a name="in-memory-databases-must-be-named"></a>Bellek içi veritabanları olarak adlandırılmalıdır

Genel adlandırılmamış bellek içi veritabanına kaldırıldı ve bunun yerine tüm bellek içi veritabanlarını şeklinde adlandırılmalıdır. Örneğin:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Bu oluşturur / "Veritabanım" adda bir veritabanı kullanır. Varsa `UseInMemoryDatabase` aynı ada sahip birden fazla bağlam örnekleri tarafından paylaşılmasına izin vererek aynı bellek içi veritabanı kullanılacak sonra yeniden adlandırılır.

### <a name="read-only-api-changes"></a>Salt okunur API değişiklikleri

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, ve `IsStoreGeneratedAlways` geçersiz ve değiştirilecek [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) ve [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Bu davranışların herhangi bir özelliği (yalnızca depoda üretilmiş özellikleri) uygulamak ve bir veritabanı satıra eklerken özelliğinin değeri nasıl kullanılacağını belirlemek (`BeforeSaveBehavior`) veya ne zaman mevcut bir güncelleştirme izin ver veritabanı satır (`AfterSaveBehavior`).

Özellikleri olarak işaretli [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (örn. hesaplanan sütunlar için) varsayılan olarak özelliği ayarlanmış herhangi bir değer göz ardı eder. Bu, depoda üretilmiş bir değer olup herhangi bir değer ayarlamak veya bırakıldığı izlenen varlık üzerinde değişiklik bakılmaksızın her zaman alınacaktır anlamına gelir. Bu farklı bir ayarlayarak değiştirilebilir `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Yeni ClientSetNull silme davranışı

Önceki sürümlerde [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) sahip varlıklar için bir davranış izlenen kapsamında daha fazla eşleşen kapalı `SetNull` semantiği. EF çekirdek 2. 0'da, yeni bir `ClientSetNull` davranışı, isteğe bağlı ilişkiler için varsayılan olarak sunulmuş. Bu davranışı `SetNull` izlenen varlık anlamları ve `Restrict` EF çekirdek kullanılarak oluşturulmuş veritabanları için davranış. İzlenen varlıkları ve veritabanı için beklenen/faydalı davranışları bunlar deneyimi bizim. `DeleteBehavior.Restrict`Şimdi için isteğe bağlı bir ilişki ayarlandığında izlenen varlıklar için uygulanır.

### <a name="provider-design-time-packages-removed"></a>Sağlayıcı tasarım zamanı paketleri kaldırıldı

`Microsoft.EntityFrameworkCore.Relational.Design` Paket kaldırıldı. İçeriğiyle birleştirilmiş içine `Microsoft.EntityFrameworkCore.Relational` ve `Microsoft.EntityFrameworkCore.Design`.

Bu sağlayıcı tasarım zamanı paketlere yayar. Bu paketleri (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, vs.) kaldırıldı ve içeriklerini birleştirilmiş ana sağlayıcıyı paketlere.

Etkinleştirmek için `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold` EF çekirdek 2. 0'yalnızca tek sağlayıcısı paketi başvuru gerekir:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
