---
title: Önceki sürümlerden EF Core 2 ' ye yükseltme-EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: b27c09fdb6210dd7c6aa0c8bc912a8bd183c16b9
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824421"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Önceki sürümlerden uygulamaları EF Core 2,0 ' ye yükseltme

2,0 ' de var olan API 'Leri ve davranışları önemli ölçüde geliştirmeye yönelik fırsat aldık. Mevcut uygulama kodu üzerinde değişiklik yapılmasını gerektirebilecek birkaç iyileştirme vardır; ancak, uygulamanın büyük bir bölümü için en düşük durumda, yalnızca yeniden derleme ve kullanılmayan API 'Leri değiştirmek için en düşük düzeyde yapılan değişiklikler gerekir.

Mevcut bir uygulamayı EF Core 2,0 ' ye güncelleştirme şunları gerektirebilir:

1. Uygulamanın hedef .NET uygulamasını .NET Standard 2,0 ' i destekleyen bir sürümüne yükseltme. Daha fazla bilgi için bkz. [desteklenen .NET uygulamaları](xref:core/platforms/index) .

2. Hedef veritabanı için EF Core 2,0 ile uyumlu bir sağlayıcı belirler. Bkz. [EF Core 2,0, aşağıda 2,0 veritabanı sağlayıcısı gerektirir](#ef-core-20-requires-a-20-database-provider) .

3. Tüm EF Core paketlerini (çalışma zamanı ve araçları) 2,0 ' ye yükseltme. Daha fazla ayrıntı için [EF Core yükleme](xref:core/get-started/install/index) bölümüne bakın.

4. Bu belgenin geri kalanında açıklanan son değişiklikleri dengelemek için gerekli kod değişikliklerini yapın.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core artık EF Core içeriyor

ASP.NET Core 2,0 ' i hedefleyen uygulamalar, EF Core 2,0 ' i üçüncü taraf veritabanı sağlayıcılarının yanında ek bağımlılıklar olmadan kullanabilir. Ancak, önceki ASP.NET Core sürümlerinin hedeflediği uygulamaların EF Core 2,0 ' i kullanabilmesi için ASP.NET Core 2,0 ' e yükseltilmesi gerekir. ASP.NET Core Uygulamaları 2,0 ' ye yükseltme hakkında daha fazla ayrıntı için [, konusunun ASP.NET Core belgelerine](/aspnet/core/migration/1x-to-2x/)bakın.

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Uygulama hizmetlerini ASP.NET Core almanın yeni yolu

ASP.NET Core Web uygulamaları için önerilen model, 1. x içinde kullanılan tasarım zamanı mantığını EF Core bir şekilde 2,0 için güncelleştirilmiştir. Daha önce tasarım zamanında EF Core, uygulamanın hizmet sağlayıcısına erişmek için `Startup.ConfigureServices` doğrudan çağırmayı deneyebilir. ASP.NET Core 2,0 ' de, yapılandırma `Startup` sınıfının dışında başlatılır. EF Core kullanan uygulamalar genellikle bağlantı dizesine yapılandırma ile erişir, bu nedenle `Startup` kendisi artık yeterli değildir. Bir ASP.NET Core 1. x uygulamasını yükseltirseniz EF Core araçlarını kullanırken aşağıdaki hatayı alabilirsiniz.

> ' ApplicationContext ' üzerinde parametresiz oluşturucu bulunamadı. ' ApplicationContext ' öğesine parametresiz bir Oluşturucu ekleyin ya da ' ApplicationContext ' ile aynı derlemede ' ıdesigntimedbcontextfactory&lt;ApplicationContext&gt;' uygulamasını ekleyin

ASP.NET Core 2.0 'ın varsayılan şablonuna yeni bir tasarım zamanı kancası eklenmiştir. Statik `Program.BuildWebHost` yöntemi, EF Core uygulamanın hizmet sağlayıcısına tasarım zamanında erişmesine olanak sağlar. ASP.NET Core 1. x uygulamasını yükseltiyorsanız, `Program` sınıfını aşağıdakine benzer şekilde güncelleştirmeniz gerekir.

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

Uygulamaları 2,0 ' e güncelleştirirken bu yeni düzeni benimseme işlemi, Entity Framework Core geçişlerinin çalışması gibi ürün özelliklerinin olması için gereklidir ve gereklidir. Diğer yaygın alternatif, [ *ıdesigntimedbcontextfactory\<tcontext >* uygulamaktır](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>Idbcontextfactory yeniden adlandırıldı

Farklı uygulama düzenlerini desteklemek ve kullanıcılara tasarım zamanında `DbContext` nasıl kullanıldıkları hakkında daha fazla denetim sağlamak için, geçmişte, `IDbContextFactory<TContext>` arabirimini sağladık. Tasarım zamanında EF Core araçları projenizdeki bu arabirimin uygulamalarını keşfeder ve `DbContext` nesneleri oluşturmak için onu kullanır.

Bu arabirim, bazı kullanıcıların diğer `DbContext`oluşturma senaryolarında onu yeniden kullanmaya denemesini sağlayan çok genel bir ada sahipti. EF araçları, daha sonra bu uygulamaları tasarım zamanında kullanmaya çalıştık ve `Update-Database` ya da `dotnet ef database update` gibi komutların başarısız olmasına neden olan korumaların korumasını yakaladı.

Bu arabirimin güçlü tasarım zamanı semantiğini iletmek için, `IDesignTimeDbContextFactory<TContext>`olarak yeniden adlandırdık.

2,0 sürümü için `IDbContextFactory<TContext>` hala var, ancak artık kullanılmıyor olarak işaretlendi.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions kaldırıldı

Yukarıda açıklanan ASP.NET Core 2,0 değişikliği nedeniyle, `DbContextFactoryOptions` yeni `IDesignTimeDbContextFactory<TContext>` arabiriminde artık gerekli olmadığını bulduk. Bunun yerine kullanmanız gereken alternatifler aşağıda verilmiştir.

| DbContextFactoryOptions | Yapıyı                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext. BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| environmentName         | Environment. GetEnvironmentVariable ("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Tasarım zamanı çalışma dizini değişti

ASP.NET Core 2,0 değişiklikleri, uygulamanızı çalıştırırken Visual Studio tarafından kullanılan çalışma diziniyle hizalamak için `dotnet ef` tarafından kullanılan çalışma dizini de gereklidir. Bunun bir observable yan etkisi, SQLite dosya adlarının artık proje dizinine göre ve bu şekilde kullanıldıkları gibi çıkış dizinine göre değil.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0, 2,0 veritabanı sağlayıcısı gerektiriyor

EF Core 2,0 için, veritabanı sağlayıcılarının çalışması sırasında birçok basitleştirme ve geliştirme yaptık. Bu, 1.0. x ve 1.1. x sağlayıcılarının EF Core 2,0 ile çalışmayacağı anlamına gelir.

SQL Server ve SQLite sağlayıcıları EF ekibi tarafından sevk edilir ve 2,0 sürümleri 2,0 sürümünün bir parçası olarak sunulacaktır. [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)ve [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) için açık kaynaklı üçüncü taraf sağlayıcıları 2,0 için güncelleştiriliyor. Diğer tüm sağlayıcılar için lütfen sağlayıcı yazıcı ile iletişime geçin.

## <a name="logging-and-diagnostics-events-have-changed"></a>Günlüğe kaydetme ve tanılama olayları değişti

Not: Bu değişiklikler çoğu uygulama kodunu etkilememelidir.

Bir [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) 'a gönderilen iletiler Için olay kimlikleri 2,0 içinde değişmiştir. Olay kimlikleri artık EF Core kod genelinde benzersizdir. Bu iletiler artık, örneğin MVC gibi kullanılan yapılandırılmış günlük için standart kalıbı de izler.

Günlükçü kategorileri de değişmiştir. Artık [Dbloggercategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs)aracılığıyla erişilen iyi bilinen bir kategori kümesi vardır.

[Diagnosticsource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) olayları artık karşılık gelen `ILogger` iletileriyle aynı olay kimliği adlarını kullanır. Olay yükleri, [eventdata](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata)'dan türetilmiş tüm nominal türlerdir.

Olay kimlikleri, yük türleri ve kategoriler [Coreeventıd](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) ve [Relationaleventıd](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) sınıflarında belgelenmiştir.

Kimlikler ayrıca Microsoft. EntityFrameworkCore. Infrastructure ' dan yeni Microsoft. EntityFrameworkCore. Diagnostics ad alanına de taşınmıştır.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core ilişkisel meta veri API 'SI değişiklikleri

EF Core 2,0, artık kullanılan her farklı sağlayıcı için farklı bir [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) oluşturacak. Bu genellikle uygulama için saydamdır. Bu, alt düzey meta veri API 'Lerinin basitleştirdiğini, _yaygın ilişkisel meta veri kavramlarını_ her zaman `.SqlServer`, `.Sqlite`vb. yerine `.Relational` çağrısıyla yapılır. Örneğin, 1.1. x kodu şöyle:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Şu şekilde yazılmış olmalıdır:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

`ForSqlServerToTable`gibi yöntemleri kullanmak yerine, genişletme yöntemleri artık kullanımda olan geçerli sağlayıcıyı temel alarak koşullu kod yazmak için kullanılabilir. Örneğin:

```csharp
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Bu değişikliğin yalnızca _Tüm_ ilişkisel sağlayıcılar Için tanımlanan API 'ler/meta veriler için geçerli olduğunu unutmayın. API ve meta veriler yalnızca tek bir sağlayıcıya özgü olduğunda aynı kalır. Örneğin, kümelenmiş dizinler SQL Server 'a özgüdür, bu nedenle `ForSqlServerIsClustered` ve `.SqlServer().IsClustered()` kullanılmaya devam etmelidir.

## <a name="dont-take-control-of-the-ef-service-provider"></a>EF hizmeti sağlayıcısı denetimi yapmayın

EF Core iç uygulama için bir iç `IServiceProvider` (bağımlılık ekleme kapsayıcısı) kullanır. Uygulamalar, EF Core özel durumlar dışında bu sağlayıcıyı oluşturmasına ve yönetmesine izin verir. `UseInternalServiceProvider`çağrıları kaldırmayı kesin olarak düşünün. Bir uygulamanın `UseInternalServiceProvider`çağırması gerekiyorsa, senaryonuzu işlemek için kullanabileceğiniz diğer yolları araştırmak için lütfen [bir sorunu](https://github.com/aspnet/EntityFramework/Issues) yerleştirmeyi düşünün.

`UseInternalServiceProvider` de çağrılmadığı müddetçe `AddEntityFramework`, `AddEntityFrameworkSqlServer`vb. çağırma uygulama kodu için gerekli değildir. `AddEntityFramework` veya `AddEntityFrameworkSqlServer`var olan tüm çağrıları kaldırın, vb. `AddDbContext` yine de ile aynı şekilde kullanılmalıdır.

## <a name="in-memory-databases-must-be-named"></a>Bellek içi veritabanlarının adlandırılması gerekir

Genel adlandırılmamış bellek içi veritabanı kaldırılmıştır ve bunun yerine tüm bellek içi veritabanlarının adlandırılması gerekir. Örneğin:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Bu, "MyDatabase" adlı bir veritabanını oluşturur/kullanır. `UseInMemoryDatabase` aynı adla yeniden çağrılırsa, aynı bellek içi veritabanı kullanılır ve bu da birden çok bağlam örneği tarafından paylaşılmak üzere olur.

## <a name="read-only-api-changes"></a>Salt okunurdur API değişiklikleri

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`ve `IsStoreGeneratedAlways` kullanımdan kaldırılmıştır ve [Beforesavebehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) ve [aftersavebehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior)ile değiştirilmiştir. Bu davranışlar her özellik için geçerlidir (yalnızca depo tarafından üretilen Özellikler değil) ve bir veritabanı satırına (`BeforeSaveBehavior`) eklenirken veya var olan bir veritabanı satırı (`AfterSaveBehavior`) güncelleştirilirken özelliğin değerinin nasıl kullanılacağı belirlenir.

[Valuegenerated. OnAddOrUpdate](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (örneğin, hesaplanan sütunlar için) olarak işaretlenen özellikler, varsayılan olarak, özelliğinde ayarlanmış olan tüm değerleri yoksayar. Bu, izlenen varlık üzerinde herhangi bir değerin ayarlanmış veya değiştirilmiş olmasından bağımsız olarak her zaman bir mağaza tarafından oluşturulan değerin elde edilmeyeceği anlamına gelir. Bu, farklı bir `Before\AfterSaveBehavior`ayarlanarak değiştirilebilir.

## <a name="new-clientsetnull-delete-behavior"></a>Yeni ClientSetNull silme davranışı

Önceki sürümlerde, [DeleteBehavior. restrict](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) , daha fazla eşleşen `SetNull` semantiğini daha fazla kapatan bağlam tarafından izlenen varlıklar için bir davranışa sahipti. EF Core 2,0 ' de, isteğe bağlı ilişkiler için varsayılan olarak yeni bir `ClientSetNull` davranışı sunulmuştur. Bu davranışın, EF Core kullanılarak oluşturulan veritabanları için izlenen varlıklar ve `Restrict` davranışı için `SetNull` semantiği vardır. Deneyimimizde, izlenen varlıklar ve veritabanı için en çok beklenen/kullanışlı davranışlar vardır. `DeleteBehavior.Restrict`, isteğe bağlı ilişkiler için ayarlandığında, izlenen varlıklar için artık kabul edilir.

## <a name="provider-design-time-packages-removed"></a>Sağlayıcı tasarım-zaman paketleri kaldırıldı

`Microsoft.EntityFrameworkCore.Relational.Design` paketi kaldırılmıştır. İçerikleri `Microsoft.EntityFrameworkCore.Relational` ve `Microsoft.EntityFrameworkCore.Design`olarak birleştirildi.

Bu, sağlayıcı tasarım zamanı paketlerine yayar. Bu paketler (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, vb.) kaldırılmıştır ve içerikleri ana sağlayıcı paketlerine birleştirilir.

EF Core 2,0 ' de `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold` etkinleştirmek için yalnızca tek sağlayıcı paketine başvurmanız gerekir:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
