---
title: Önceki sürümlerden EF Core 2 ' ye yükseltme-EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 42e59b47f569ef6fcf72fc5bd5f94d3e9d807a24
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813567"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Önceki sürümlerden uygulamaları EF Core 2,0 ' ye yükseltme

2,0 ' de var olan API 'Leri ve davranışları önemli ölçüde geliştirmeye yönelik fırsat aldık. Mevcut uygulama kodu üzerinde değişiklik yapılmasını gerektirebilecek birkaç iyileştirme vardır; ancak, uygulamanın büyük bir bölümü için en düşük durumda, yalnızca yeniden derleme ve kullanılmayan API 'Leri değiştirmek için en düşük düzeyde yapılan değişiklikler gerekir.

Mevcut bir uygulamayı EF Core 2,0 ' ye güncelleştirme şunları gerektirebilir:

1. Uygulamanın hedef .NET uygulamasını .NET Standard 2,0 ' i destekleyen bir sürümüne yükseltme. Daha fazla bilgi için bkz. [desteklenen .NET uygulamaları](../platforms/index.md) .

2. Hedef veritabanı için EF Core 2,0 ile uyumlu bir sağlayıcı belirler. Bkz. [EF Core 2,0, aşağıda 2,0 veritabanı sağlayıcısı gerektirir](#ef-core-20-requires-a-20-database-provider) .

3. Tüm EF Core paketlerini (çalışma zamanı ve araçları) 2,0 ' ye yükseltme. Daha fazla ayrıntı için [EF Core yükleme](../get-started/install/index.md) bölümüne bakın.

4. Bu belgenin geri kalanında açıklanan son değişiklikleri dengelemek için gerekli kod değişikliklerini yapın.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core artık EF Core içeriyor

ASP.NET Core 2,0 ' i hedefleyen uygulamalar, EF Core 2,0 ' i üçüncü taraf veritabanı sağlayıcılarının yanında ek bağımlılıklar olmadan kullanabilir. Ancak, önceki ASP.NET Core sürümlerinin hedeflediği uygulamaların EF Core 2,0 ' i kullanabilmesi için ASP.NET Core 2,0 ' e yükseltilmesi gerekir. ASP.NET Core Uygulamaları 2,0 ' ye yükseltme hakkında daha fazla ayrıntı için [, konusunun ASP.NET Core belgelerine](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/)bakın.

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Uygulama hizmetlerini ASP.NET Core almanın yeni yolu

ASP.NET Core Web uygulamaları için önerilen model, 1. x içinde kullanılan tasarım zamanı mantığını EF Core bir şekilde 2,0 için güncelleştirilmiştir. Daha önce tasarım zamanında EF Core, uygulamanın hizmet sağlayıcısına erişmek için `Startup.ConfigureServices` doğrudan çağırmayı deneyebilir. ASP.NET Core 2,0 ' de, yapılandırma `Startup` sınıfının dışında başlatılır. EF Core kullanan uygulamalar genellikle bağlantı dizesine, bu nedenle `Startup` kendisi artık yeterli olmaz. Bir ASP.NET Core 1. x uygulamasını yükseltirseniz EF Core araçlarını kullanırken aşağıdaki hatayı alabilirsiniz.

> ' ApplicationContext ' üzerinde parametresiz oluşturucu bulunamadı. ' ApplicationContext ' öğesine parametresiz bir Oluşturucu ekleyin ya da ' ApplicationContext ' ile aynı derlemede ' ıdesigntimedbcontextfactory&lt;ApplicationContext&gt;' uygulamasını ekleyin

ASP.NET Core 2.0 'ın varsayılan şablonuna yeni bir tasarım zamanı kancası eklenmiştir. Statik `Program.BuildWebHost` Yöntem EF Core, tasarım zamanında uygulamanın hizmet sağlayıcısına erişmesini sağlar. Bir ASP.NET Core 1. x uygulamasını yükseltiyorsanız, `Program` sınıfı aşağıdakine benzeyecek şekilde güncelleştirmeniz gerekecektir.

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

Uygulamaları 2,0 ' e güncelleştirirken bu yeni düzeni benimseme işlemi, Entity Framework Core geçişlerinin çalışması gibi ürün özelliklerinin olması için gereklidir ve gereklidir. Diğer yaygın alternatif, [ *\<ıdesigntimedbcontextfactory tcontext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory)uygulamasıdır.

## <a name="idbcontextfactory-renamed"></a>Idbcontextfactory yeniden adlandırıldı

Farklı uygulama düzenlerini desteklemek ve kullanıcılara tasarım zamanında nasıl kullanıldıkları hakkında `DbContext` daha fazla denetim sağlamak için, daha önce `IDbContextFactory<TContext>` arabirimi sağladık. Tasarım zamanında EF Core araçları projenizdeki bu arabirimin uygulamalarını bulacak ve nesne oluşturmak `DbContext` için kullanacaktır.

Bu arabirimin çok genel bir adı vardı, bu da bazı kullanıcıların diğer `DbContext`oluşturma senaryolarında yeniden kullanılmasını denemeye neden olur. EF araçları, daha sonra kendi uygulamasını tasarım zamanında kullanmaya çalıştık ve `Update-Database` veya `dotnet ef database update` başarısız olan komutlara neden olan komutları koruma dışı bırakır.

Bu arabirimin güçlü tasarım zamanı semantiğini iletmek için bunu olarak `IDesignTimeDbContextFactory<TContext>`yeniden adlandırdık.

2,0 sürümü için hala var `IDbContextFactory<TContext>` , ancak eski olarak işaretlendi.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions kaldırıldı

Yukarıda açıklanan ASP.NET Core 2,0 değişikliği nedeniyle, yeni `DbContextFactoryOptions` `IDesignTimeDbContextFactory<TContext>` arabirimde artık gerekli olmadığını bulduk. Bunun yerine kullanmanız gereken alternatifler aşağıda verilmiştir.

| DbContextFactoryOptions | Yapıyı                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext. BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment. GetEnvironmentVariable ("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Tasarım zamanı çalışma dizini değişti

ASP.NET Core 2,0 değişiklikleri, uygulamanızı çalıştırırken Visual Studio tarafından kullanılan çalışma `dotnet ef` diziniyle hizalamak için tarafından kullanılan çalışma dizinini de gerektirir. Bunun bir observable yan etkisi, SQLite dosya adlarının artık proje dizinine göre ve bu şekilde kullanıldıkları gibi çıkış dizinine göre değil.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0, 2,0 veritabanı sağlayıcısı gerektiriyor

EF Core 2,0 için, veritabanı sağlayıcılarının çalışması sırasında birçok basitleştirme ve geliştirme yaptık. Bu, 1.0. x ve 1.1. x sağlayıcılarının EF Core 2,0 ile çalışmayacağı anlamına gelir.

SQL Server ve SQLite sağlayıcıları EF ekibi tarafından sevk edilir ve 2,0 sürümleri 2,0 sürümünün bir parçası olarak sunulacaktır. [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)ve [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) için açık kaynaklı üçüncü taraf sağlayıcıları 2,0 için güncelleştiriliyor. Diğer tüm sağlayıcılar için lütfen sağlayıcı yazıcı ile iletişime geçin.

## <a name="logging-and-diagnostics-events-have-changed"></a>Günlüğe kaydetme ve tanılama olayları değişti

Not: Bu değişiklikler çoğu uygulama kodunu etkilememelidir.

Bir [ILogger](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger) 'a gönderilen iletiler Için olay kimlikleri 2,0 içinde değişmiştir. Olay kimlikleri artık EF Core kod genelinde benzersizdir. Bu iletiler artık, örneğin MVC gibi kullanılan yapılandırılmış günlük için standart kalıbı de izler.

Günlükçü kategorileri de değişmiştir. Artık [Dbloggercategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs)aracılığıyla erişilen iyi bilinen bir kategori kümesi vardır.

[Diagnosticsource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) olayları artık karşılık gelen `ILogger` iletilerle aynı olay kimliği adlarını kullanır. Olay yükleri, [eventdata](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata)'dan türetilmiş tüm nominal türlerdir.

Olay kimlikleri, yük türleri ve kategoriler [Coreeventıd](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) ve [Relationaleventıd](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) sınıflarında belgelenmiştir.

Kimlikler ayrıca Microsoft. EntityFrameworkCore. Infrastructure ' dan yeni Microsoft. EntityFrameworkCore. Diagnostics ad alanına de taşınmıştır.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core ilişkisel meta veri API 'SI değişiklikleri

EF Core 2,0, artık kullanılan her farklı sağlayıcı için farklı bir [IModel](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) oluşturacak. Bu genellikle uygulama için saydamdır. Bu, alt düzey meta veri API 'lerinin basitleştirdiğini, _ortak ilişkisel meta veri kavramlarının_ her zaman `.Relational` `.SqlServer`, `.Sqlite`, vb. bir çağrısıyla yapılan bir çağrı aracılığıyla yapılması için kolaylaştırmıştır. Örneğin, 1.1. x kodu şöyle:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Şu şekilde yazılmış olmalıdır:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Uzantı yöntemleri, gibi `ForSqlServerToTable`yöntemleri kullanmak yerine artık kullanımda olan geçerli sağlayıcıyı temel alarak koşullu kod yazmak için kullanılabilir. Örneğin:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Bu değişikliğin yalnızca _Tüm_ ilişkisel sağlayıcılar Için tanımlanan API 'ler/meta veriler için geçerli olduğunu unutmayın. API ve meta veriler yalnızca tek bir sağlayıcıya özgü olduğunda aynı kalır. Örneğin, kümelenmiş dizinler SQL Server 'a özgüdür, bu nedenle `ForSqlServerIsClustered` `.SqlServer().IsClustered()` yine de kullanılmalıdır.

## <a name="dont-take-control-of-the-ef-service-provider"></a>EF hizmeti sağlayıcısı denetimi yapmayın

EF Core iç uygulama için `IServiceProvider` bir iç (bağımlılık ekleme kapsayıcısı) kullanır. Uygulamalar, EF Core özel durumlar dışında bu sağlayıcıyı oluşturmasına ve yönetmesine izin verir. Tüm çağrıları kaldırmayı kesin olarak `UseInternalServiceProvider`düşünün. Bir uygulamanın çağrı `UseInternalServiceProvider`yapması gerekiyorsa, senaryonuzu işlemenin diğer yollarını araştırabilmemiz için lütfen [bir sorunu](https://github.com/aspnet/EntityFramework/Issues) yerleştirmeyi düşünün.

Çağırılmadan, `AddEntityFramework` `UseInternalServiceProvider` , vb. çağırma uygulama kodu için gerekli değildir. `AddEntityFrameworkSqlServer` Ya `AddDbContext` `AddEntityFramework` da'ayönelikmevcutçağrıların,dahaönceileaynışekildekullanılmasıgerekir.`AddEntityFrameworkSqlServer`

## <a name="in-memory-databases-must-be-named"></a>Bellek içi veritabanlarının adlandırılması gerekir

Genel adlandırılmamış bellek içi veritabanı kaldırılmıştır ve bunun yerine tüm bellek içi veritabanlarının adlandırılması gerekir. Örneğin:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Bu, "MyDatabase" adlı bir veritabanını oluşturur/kullanır. `UseInMemoryDatabase` Aynı adla yeniden çağrılırsa, aynı bellek içi veritabanı kullanılır ve bu da birden çok bağlam örneği tarafından paylaşılmak üzere olur.

## <a name="read-only-api-changes"></a>Salt okunurdur API değişiklikleri

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, ve `IsStoreGeneratedAlways` kullanımdan kaldırılmıştır ve [beforesavebehavior](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) ve [aftersavebehavior](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior)ile değiştirilmiştir. Bu davranışlar her özellik için geçerlidir (yalnızca depo tarafından üretilen Özellikler değil) ve bir veritabanı satırına (`BeforeSaveBehavior`) eklenirken veya var olan bir veritabanı`AfterSaveBehavior`satırı () güncelleştirilirken özelliğin değerinin nasıl kullanılacağı belirlenir.

[Valuegenerated. OnAddOrUpdate](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (örneğin, hesaplanan sütunlar için) olarak işaretlenen özellikler, varsayılan olarak, özelliğinde ayarlanmış olan tüm değerleri yoksayar. Bu, izlenen varlık üzerinde herhangi bir değerin ayarlanmış veya değiştirilmiş olmasından bağımsız olarak her zaman bir mağaza tarafından oluşturulan değerin elde edilmeyeceği anlamına gelir. Bu, farklı `Before\AfterSaveBehavior`ayarlanarak değiştirilebilir.

## <a name="new-clientsetnull-delete-behavior"></a>Yeni ClientSetNull silme davranışı

Önceki sürümlerde, [DeleteBehavior. restrict](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.deletebehavior) bağlam tarafından izlenen varlıklar için daha fazla eşleşen `SetNull` semantiğini daha fazla kapatan bir davranışa sahipti. EF Core 2,0 ' de, isteğe `ClientSetNull` bağlı ilişkiler için varsayılan olarak yeni bir davranış sunulmuştur. Bu davranışın, `SetNull` EF Core kullanılarak oluşturulan veritabanlarının izlenen `Restrict` varlıkları ve davranışları için semantiği vardır. Deneyimimizde, izlenen varlıklar ve veritabanı için en çok beklenen/kullanışlı davranışlar vardır. `DeleteBehavior.Restrict`, isteğe bağlı ilişkiler için ayarlandığında, izlenen varlıklar için artık kabul edilir.

## <a name="provider-design-time-packages-removed"></a>Sağlayıcı tasarım-zaman paketleri kaldırıldı

`Microsoft.EntityFrameworkCore.Relational.Design` Paket kaldırılmıştır. İçeriği `Microsoft.EntityFrameworkCore.Relational` ve ile `Microsoft.EntityFrameworkCore.Design`birleştirildi.

Bu, sağlayıcı tasarım zamanı paketlerine yayar. Bu paketler (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, vb.) kaldırılmıştır ve içerikleri ana sağlayıcı paketlerine birleştirilir.

EF Core 2,0 `Scaffold-DbContext` ' `dotnet ef dbcontext scaffold` de etkinleştirmek için, yalnızca tek sağlayıcı paketine başvurmanız gerekir:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
