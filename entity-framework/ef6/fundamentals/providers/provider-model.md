---
title: Entity Framework 6 sağlayıcı modeli-EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: 8bda3f51e8934f2add862c30e60f1185f068c515
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181604"
---
# <a name="the-entity-framework-6-provider-model"></a>Entity Framework 6 sağlayıcı modeli

Entity Framework Sağlayıcı modeli, Entity Framework farklı veritabanı sunucusu türleriyle kullanılmasına izin verir. Örneğin, bir sağlayıcının Microsoft SQL Server karşı kullanılmasına izin vermek için, başka bir sağlayıcının Microsoft SQL Server Compact sürümünde kullanılmasına izin vermek üzere takılabileceği şekilde erişilebilir olması için bir sağlayıcı takılabilir. Bildiğimiz EF6 sağlayıcıları [Entity Framework sağlayıcılar](~/ef6/fundamentals/providers/index.md) sayfasında bulunabilir.

EF 'in açık kaynaklı bir lisans altında serbest bırakılacağını sağlamak için gerekli olan belirli değişiklikler, aşv ile etkileşime girer. Bu değişiklikler, sağlayıcının kaydına yönelik yeni mekanizmalarla birlikte EF6 Derlemeleriyle EF sağlayıcılarının yeniden oluşturulması gerekir.

## <a name="rebuilding"></a>Oluşturmak

EF6 ile .NET Framework daha önce bir parçası olan çekirdek kod artık bant dışı (OOB) derlemeler olarak sevk edilir. EF6 'e yönelik uygulamalar oluşturma hakkındaki ayrıntılar, [EF6 için uygulamaları güncelleştirme](~/ef6/what-is-new/upgrading-to-ef6.md) sayfasında bulunabilir. Ayrıca bu yönergeler kullanılarak sağlayıcıların yeniden oluşturulması gerekir.

## <a name="provider-types-overview"></a>Sağlayıcı türlerine genel bakış

EF sağlayıcısı gerçekten, bu hizmetlerin genişletecek (bir temel sınıf için) veya uygulama (bir arabirim için) CLR türleri tarafından tanımlanan sağlayıcıya özgü hizmetlerden oluşan bir koleksiyondur. Bu hizmetlerden ikisi temel ve EF 'in hiç çalışması için gereklidir. Diğerleri isteğe bağlıdır ve yalnızca belirli işlevler gerekliyse ve/veya hedeflenen belirli veritabanı sunucusu için bu hizmetlerin varsayılan uygulamaları işe gerekmediği takdirde uygulanmalıdır.

## <a name="fundamental-provider-types"></a>Temel sağlayıcı türleri

### <a name="dbproviderfactory"></a>DbProviderFactory

EF, tüm düşük düzeydeki veritabanı erişimini gerçekleştirmek için [System. Data. Common. DbProviderFactory](https://msdn.microsoft.com/library/system.data.common.dbproviderfactory.aspx) öğesinden türetilmiş bir türe sahip olmaya bağlıdır. DbProviderFactory gerçekte EF 'in bir parçası değildir ancak bunun yerine, ADO.NET sağlayıcıları için EF, other/RMs veya doğrudan bağlantı, komut, parametre örnekleri elde eden bir uygulama tarafından kullanılabilecek bir giriş noktası sunan bir sınıf .NET Framework. bir sağlayıcının belirsiz şekilde diğer ADO.NET soyutlamaları. DbProviderFactory hakkında daha fazla bilgi [Için MSDN belgelerinde ADO.net](https://msdn.microsoft.com/library/a6cd7c08.aspx)bulabilirsiniz.

### <a name="dbproviderservices"></a>DbProviderServices

EF, ADO.NET sağlayıcısı tarafından zaten sağlanan işlevselliğin üzerine gereken ek işlevselliği sağlamak için DbProviderServices 'den türetilmiş bir türe sahip olmaya bağlıdır. EF 'in daha eski sürümlerinde DbProviderServices sınıfı .NET Framework bir parçasıdır ve System. Data. Common Ad alanında bulunur. EF6 ile başlayarak bu sınıf artık EntityFramework. dll ' nin bir parçasıdır ve System. Data. Entity. Core. Common ad alanıdır.

DbProviderServices uygulamasının temel işlevleriyle ilgili daha fazla ayrıntı [MSDN](https://msdn.microsoft.com/library/ee789835.aspx)'de bulunabilir. Ancak, kavramların büyük bir kısmının hala geçerli olmasına rağmen, bu bilgileri yazmanın zaman EF6 için güncellenmediğini unutmayın. DbProviderServices 'ın SQL Server ve SQL Server Compact uygulamaları da [Açık kaynaklı kod](https://github.com/aspnet/EntityFramework6/) tabanına iade edilir ve diğer uygulamalar için yararlı başvurular olarak hizmet verebilir.

EF 'in daha eski sürümlerinde kullanılacak DbProviderServices uygulamasının bir ADO.NET sağlayıcısından doğrudan edinildiği. Bu, DbProviderFactory IServiceProvider 'a bağlanarak ve GetService yöntemi çağrılırken yapılır. Bu, EF sağlayıcısını DbProviderFactory 'e sıkı bir şekilde bağlanmış. Bu kuponun .NET Framework dışına taşınmasını engelledi ve bu nedenle EF6 bu sıkı bir şekilde kaldırılmıştır ve DbProviderServices uygulaması artık doğrudan uygulamanın yapılandırma dosyasına veya kod tabanlı olarak kaydedilir yapılandırma, aşağıdaki _DbProviderServices kaydı_ hakkında daha ayrıntılı bilgi bölümünde açıklanmıştır.

## <a name="additional-services"></a>Ek hizmetler

Yukarıda açıklanan temel hizmetlere ek olarak, EF tarafından her zaman veya bazen sağlayıcıya özgü olan çok sayıda diğer hizmet de mevcuttur. Bu hizmetlerin varsayılan sağlayıcıya özgü uygulamaları, bir DbProviderServices uygulaması tarafından sağlanabilir. Uygulamalar Ayrıca bu hizmetlerin uygulamalarını geçersiz kılabilir veya bir DbProviderServices türü için varsayılan değer sağlamadığı durumlarda uygulamalar sağlayabilir. Bu, aşağıdaki _ek hizmetleri çözme_ bölümünde daha ayrıntılı olarak açıklanmıştır.

Bir sağlayıcının bir sağlayıcıya ilgi çekici olabileceği ek hizmet türleri aşağıda listelenmiştir. Bu hizmet türlerinin her biri hakkında daha fazla ayrıntı, API belgelerinde bulunabilir.

### <a name="idbexecutionstrategy"></a>Idbexecutionstrategy

Bu, bir sağlayıcının sorgular ve komutlar veritabanına göre yürütüldüğünde yeniden denemeler veya diğer davranışları uygulamasına izin veren isteğe bağlı bir hizmettir. Uygulama sağlanmazsa, EF komutları yürütür ve oluşturulan özel durumları yayacaktır. SQL Server için bu hizmet, SQL Azure gibi bulut tabanlı veritabanı sunucularında çalışırken yararlı olan bir yeniden deneme ilkesi sağlamak için kullanılır.

### <a name="idbconnectionfactory"></a>Idbconnectionfactory

Bu, bir sağlayıcının yalnızca bir veritabanı adı verildiğinde bir kurala göre DbConnection nesneleri oluşturmasına olanak sağlayan isteğe bağlı bir hizmettir. Bu hizmet bir DbProviderServices uygulamasıyla çözülebildiğinden, EF 4,1 ' den bu yana var olan ve ayrıca yapılandırma dosyasında ya da kodda açıkça ayarlanmış olabilir. Sağlayıcı yalnızca varsayılan sağlayıcı olarak kayıtlıysa (varsayılan _sağlayıcıya_ bakın) ve bir varsayılan bağlantı fabrikası başka bir yerde ayarlanmamışsa bu hizmetin çözümlenme şansı elde eder.

### <a name="dbspatialservices"></a>DbSpatialServices

Bu, bir sağlayıcının Coğrafya ve geometri uzamsal türler için destek eklemesini sağlayan isteğe bağlı bir hizmetlerdir. Bir uygulamanın uzamsal türler içeren EF kullanması için bu hizmetin uygulanması sağlanmalıdır. DbSptialServices için iki şekilde istenir. İlk olarak, sağlayıcıya özgü uzamsal hizmetler, anahtar olarak bir DBProviderInfo nesnesi (Sabit adı ve bildirim belirteci içerir) kullanılarak istenir. İkinci olarak, DbSpatialServices hiçbir anahtar olmadan istenebilir. Bu, tek başına Dbcoğrafya veya DbGeometry türleri oluştururken kullanılan "küresel uzamsal sağlayıcıyı" çözümlemek için kullanılır.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Bu, Code First tarafından veritabanı şemaları oluştururken ve değiştirirken kullanılan SQL üretimi için EF geçişlerinin kullanılmasına izin veren isteğe bağlı bir hizmettir. Geçişleri desteklemek için bir uygulama gerekir. Bir uygulama sağlanmışsa, veritabanları veritabanı başlatıcıları veya Database. Create yöntemi kullanılarak oluşturulduğunda de kullanılacaktır.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, String, geçmişini contextFactory >

Bu, bir sağlayıcının,, @no__t, bir sağlayıcının,,,,,, Bu bir DbContext Code First ve tablo adı ve sütun eşleme belirtimleri gibi şeyleri değiştirmek için normal Fluent API kullanılarak yapılandırılabilir. Tüm varsayılan tablo ve sütun eşlemeleri bu sağlayıcı tarafından destekleniyorsa, tüm sağlayıcılar için EF tarafından döndürülen bu hizmetin varsayılan uygulanması belirli bir veritabanı sunucusu için çalışabilir. Böyle bir durumda, sağlayıcının bu hizmetin bir uygulamasını sağlaması gerekmez.

### <a name="idbproviderfactoryresolver"></a>Idbproviderfactoryresolver

Bu, belirli bir DbConnection nesnesinden doğru DbProviderFactory elde etmek için isteğe bağlı bir hizmettir. Tüm sağlayıcılar için EF tarafından döndürülen bu hizmetin varsayılan uygulamasının tüm sağlayıcılar için çalışması amaçlanır. Ancak, .NET 4 ' te çalışırken DbProviderFactory, DbConnections ise herkese açık bir şekilde erişemez. Bu nedenle, EF bir eşleşme bulmak için kayıtlı sağlayıcıları aramak üzere bazı buluşsal yöntemler kullanır. Bazı sağlayıcılardan bu buluşsal yöntemler başarısız olur ve bu gibi durumlarda sağlayıcı yeni bir uygulama sağlamalıdır.

## <a name="registering-dbproviderservices"></a>DbProviderServices kaydediliyor

Kullanılacak DbProviderServices uygulaması, uygulamanın yapılandırma dosyasında (App. config veya Web. config) ya da kod tabanlı yapılandırma kullanılarak kaydedilebilir. Her iki durumda da kayıt, sağlayıcının "sabit adı" nı anahtar olarak kullanır. Bu, birden çok sağlayıcının tek bir uygulamada kaydedilmesini ve kullanılmasını sağlar. EF kayıtları için kullanılan sabit ad, ADO.NET sağlayıcı kaydı ve bağlantı dizeleri için kullanılan sabit adla aynıdır. Örneğin, SQL Server için "System. Data. SqlClient" sabit adı kullanılır.

### <a name="config-file-registration"></a>Yapılandırma dosyası kaydı

Kullanılacak DbProviderServices türü, uygulamanın yapılandırma dosyasının entityFramework bölümünün sağlayıcılar listesinde bir sağlayıcı öğesi olarak kaydedilir. Örneğin:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

_Tür_ dizesi, kullanılacak DbProviderServices uygulamasının derleme nitelikli tür adı olmalıdır.

### <a name="code-based-registration"></a>Kod tabanlı kayıt

EF6 sağlayıcılardan itibaren, kod kullanılarak da kaydedilebilir. Bu, bir EF sağlayıcısının uygulamanın yapılandırma dosyasında herhangi bir değişiklik yapılmadan kullanılmasına izin verir. Kod tabanlı yapılandırmayı kullanmak için bir uygulamanın, [kod tabanlı yapılandırma belgelerinde](https://msdn.com/data/jj680699)açıklandığı gibi bir DBConfiguration sınıfı oluşturması gerekir. DbConfiguration sınıfının Oluşturucusu, EF sağlayıcısını kaydettirmek için SetProviderServices öğesini çağırmalıdır. Örneğin:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Ek hizmetler çözümleniyor

_Sağlayıcı türlerine genel bakış_ bölümünde belirtildiği gibi, ek hizmetleri çözümlemek için de bir DbProviderServices sınıfı kullanılabilir. DbProviderServices ıdbdependencyresolver uyguladığından ve kayıtlı her DbProviderServices türü "varsayılan çözümleyici" olarak eklendiğinden, bu mümkündür. Idbdpendencyresolver mekanizması, [bağımlılık çözümlemesinde](~/ef6/fundamentals/configuring/dependency-resolution.md)daha ayrıntılı olarak açıklanmıştır. Ancak, bir sağlayıcıdaki ek hizmetleri çözümlemek için bu belirtideki tüm kavramların anlaşılması gerekli değildir.

Bir sağlayıcının ek hizmetleri çözümlemek için en yaygın yolu, DbProviderServices sınıfının oluşturucusunda her bir hizmet için DbProviderServices. AddDependencyResolver öğesini çağırmaktır. Örneğin, SqlProviderServices (SQL Server için EF Provider), başlatma için buna benzer bir kod içerir:

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Bu Oluşturucu aşağıdaki yardımcı sınıfları kullanır:

*   SingletonDependencyResolver: tek Hizmetleri çözümlemek için basit bir yol sağlar — diğer bir deyişle, GetService 'in her çağrılışında aynı örneğin döndürüldüğü hizmetler. Geçici hizmetler genellikle isteğe bağlı olarak geçici örnekler oluşturmak için kullanılacak tek bir fabrika olarak kaydedilir.
*   Executionstratejik Cayresolver: ıexecutionstrateji uygulamalarının döndürülmesi için özel bir çözümleyici.

DbProviderServices. AddDependencyResolver kullanmak yerine, DbProviderServices. GetService ' i geçersiz kılmak ve ek hizmetleri doğrudan çözümlemek da mümkündür. Bu yöntem, EF belirli bir tür tarafından tanımlanan bir hizmete ihtiyaç duyduğunda ve bazı durumlarda belirli bir anahtar için çağrılır. Yöntemi, hizmet tarafından döndürülmeden hizmeti döndürmelidir veya hizmeti döndürmekten vazgeçmek için null döndürebilir ve bunun yerine başka bir sınıfın bunu çözümlemesine izin verir. Örneğin, varsayılan bağlantı fabrikasını çözümlemek için GetService 'teki kod şuna benzer görünebilir:

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Kayıt sırası

Birden çok DbProviderServices uygulaması, bir uygulamanın yapılandırma dosyasına kaydedildiğinde, bunların listelendikleri sırayla ikincil çözümleyiciler olarak eklenir. Çözümleyiciler her zaman ikincil çözümleyici zincirinin üst kısmına eklendiğinden, bu, listenin sonundaki sağlayıcının diğerlerinden önce bağımlılıkları çözme şansı alabileceği anlamına gelir. (Bu, ilk başta çok az sayıda sayaç görünebilir, ancak her sağlayıcıyı listeden ayırarak ve mevcut sağlayıcıların üzerine yığarak bu durum üzerinde işlem yapar.)

Bu sıralama genellikle çoğu sağlayıcı hizmetinin sağlayıcıya özgü ve sağlayıcı sabit adı tarafından anahtarlamalı olması nedeniyle değildir. Ancak, sağlayıcı sabit adına veya sağlayıcıya özgü başka bir anahtara göre anahtarınmayan hizmetler için, hizmet bu sıralamaya göre çözümlenir. Örneğin, başka bir yerde açıkça farklı ayarlanmamışsa, varsayılan bağlantı fabrikası zincirdeki en üstteki sağlayıcıdan gelir.

## <a name="additional-config-file-registrations"></a>Ek yapılandırma dosyası kayıtları

Yukarıda açıklanan ek sağlayıcı hizmetlerinden bazılarını doğrudan bir uygulamanın yapılandırma dosyasında kaydetmek mümkündür. Bu tamamlandığında yapılandırma dosyasındaki kayıt, DbProviderServices uygulamasının GetService yöntemi tarafından döndürülen herhangi bir şey yerine kullanılacaktır.

### <a name="registering-the-default-connection-factory"></a>Varsayılan bağlantı fabrikası kaydediliyor

EF5 ile başlayarak EntityFramework NuGet paketi otomatik olarak SQL Express bağlantı fabrikası veya yapılandırma dosyasında LocalDb bağlantı fabrikası olarak kaydedilir.

Örneğin:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

_Tür_ , varsayılan bağlantı fabrikası Için ıdbconnectionfactory uygulaması gereken derleme nitelikli tür adıdır.

Bir sağlayıcı NuGet paketinin varsayılan bağlantı fabrikası yüklenirken bu şekilde ayarlanması önerilir. Aşağıdaki _sağlayıcılar Için NuGet paketlerine_ bakın.

## <a name="additional-ef6-provider-changes"></a>Ek EF6 sağlayıcısı değişiklikleri

### <a name="spatial-provider-changes"></a>Uzamsal sağlayıcı değişiklikleri

Uzamsal türleri destekleyen sağlayıcılar artık Dbspatıaldatareader ' dan türetilen sınıflarda bazı ek yöntemler gerçekleştirmelidir:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Varsayılan uygulamalar, zaman uyumsuz yöntemlere temsilci olarak verilmediğinden geçersiz kılınmasının önerildiği mevcut yöntemlerin yeni zaman uyumsuz sürümleri de vardır ve bu nedenle zaman uyumsuz olarak yürütülmez:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Sıralanabilir. Contains için yerel destek

EF6, sıralanabilir. LINQ sorgularında bulunan performans sorunlarına yönelik olarak eklenen DbInExpression adlı yeni bir ifade türü sunar. DbProviderManifest sınıfı yeni bir sanal yönteme sahiptir ve bir sağlayıcının yeni ifade türünü işlediğini tespit etmek için EF tarafından çağrılan, SupportsInExpression. Mevcut sağlayıcı uygulamalarıyla uyumluluk için, yöntem false döndürür. Bu iyileştirmelerden yararlanmak için, EF6 sağlayıcısı DbInExpression 'ı işlemek üzere kod ekleyebilir ve Supportsınexpression 'ı true döndürecek şekilde geçersiz kılar. DbInExpression örneği DbExpressionBuilder.In yöntemi çağırarak oluşturulabilir. Bir DbInExpression örneği, genellikle bir tablo sütununu temsil eden bir DbExpression ve bir eşleşme için test edilecek bir DbConstantExpression listesi oluşur.

## <a name="nuget-packages-for-providers"></a>Sağlayıcılar için NuGet paketleri

EF6 sağlayıcıyı kullanılabilir hale getirmenin bir yolu, bir NuGet paketi olarak yayınlanmanız. NuGet paketinin kullanılması aşağıdaki avantajları içerir:

*   Sağlayıcının kaydını uygulamanın yapılandırma dosyasına eklemek için NuGet kullanmak kolaydır
*   Kural tarafından yapılan bağlantıların kayıtlı sağlayıcıyı kullanması için varsayılan bağlantı fabrikasını ayarlamak üzere yapılandırma dosyasında ek değişiklikler yapılabilir
*   NuGet, EF6 sağlayıcısı yeni bir EF paketi serbest bırakıldıktan sonra bile çalışmaya devam etmesi için bağlama yeniden yönlendirmeleri eklemeyi işler

Bu, [açık kaynak kod](https://github.com/aspnet/entityframework6)tabanına dahil edilen EntityFramework. sqlservercompact Package örneğidir. Bu paket, EF sağlayıcısı NuGet paketleri oluşturmak için iyi bir şablon sağlar.

### <a name="powershell-commands"></a>PowerShell komutları

EntityFramework NuGet paketi yüklendiğinde, sağlayıcı paketleri için çok faydalı olan iki komut içeren bir PowerShell modülü kaydettirir:

*   Add-EFProvider, hedef projenin yapılandırma dosyasında sağlayıcı için yeni bir varlık ekler ve kayıtlı sağlayıcılar listesinin sonunda olduğundan emin olur.
*   Add-EFDefaultConnectionFactory, hedef projenin yapılandırma dosyasında defaultConnectionFactory kaydını ekler ya da güncelleştirir.

Bu komutların her ikisi de bir entityFramework bölümünü yapılandırma dosyasına ekleme ve gerekirse bir sağlayıcı koleksiyonu ekleme işlemini gerçekleştirir.

Bu komutların install. ps1 NuGet betiğinden çağrılması amaçlanmıştır. Örneğin, SQL Compact sağlayıcısı için Install. ps1 şuna benzer:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Bu komutlar hakkında daha fazla bilgi, Paket Yöneticisi konsol penceresinde Get-Help kullanılarak elde edilebilir.

## <a name="wrapping-providers"></a>Sarmalama sağlayıcıları

Sarmalama sağlayıcısı, var olan bir sağlayıcıyı, profil oluşturma veya izleme yetenekleri gibi diğer işlevlerle genişletmek için sarmalayan bir EF ve/veya ADO.NET sağlayıcısıdır. Sarmalama sağlayıcıları normal şekilde kaydedilebilir, ancak sağlayıcıya yönelik hizmetlerin çözümlenmesini kesintiye uğratan, sarmalama sağlayıcısının çalışma zamanında kurulması genellikle daha uygundur. DbConfiguration sınıfındaki statik olay OnLockingConfiguration bunu yapmak için kullanılabilir.

EF olduktan sonra OnLockingConfiguration çağrılır ve uygulama etki alanı için tüm EF yapılandırmasının, kullanım için kilitlenmesinden önce nereden elde edildiğimize göre belirlenir. Uygulama başlangıcında (EF kullanılmadan önce) uygulamanın bu olay için bir olay işleyicisi kaydetmesi gerekir. (Bu işleyiciyi yapılandırma dosyasına kaydetmek için destek eklemeyi düşünüyoruz ancak henüz desteklenmiyor.) Olay işleyicisi daha sonra sarmalanması gereken her hizmet için bir ReplaceService çağrısı yapmanız gerekir.  

Örneğin, ıdbconnectionfactory ve DbProviderService 'i kaydırmak için şuna benzer bir işleyici kaydedilmelidir:

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

Çözümlenmiş olan ve artık hizmeti çözümlemek için kullanılan anahtarla birlikte sarmalanmış olan hizmet işleyiciye iletilir. İşleyici daha sonra bu hizmeti sarmalanabilir ve döndürülen hizmeti sarmalanmış sürümle değiştirebilir.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Bir DbProviderFactory 'ı EF ile çözme

DbProviderFactory, yukarıdaki _sağlayıcı türleri genel bakış_ bölümünde AÇıKLANDıĞı gibi EF tarafından gereken temel sağlayıcı türlerinden biridir. Zaten belirtildiği gibi, bir EF türü değildir ve kayıt genellikle EF yapılandırmasının bir parçası değildir, ancak bunun yerine Machine. config dosyasında ve/veya uygulamanın yapılandırma dosyasında normal ADO.NET sağlayıcı kaydı olur.

Bu EF hala, bir DbProviderFactory öğesini kullanmak için ararken normal bağımlılık çözüm mekanizmasını kullanıyor olsa da. Varsayılan çözümleyici yapılandırma dosyalarında normal ADO.NET kaydını kullanır ve bu nedenle genellikle saydamdır. Ancak normal bağımlılık çözümleme mekanizması kullanıldığı için, normal ADO.NET kaydı yapılmadığında bile bir ıdbdependencyresolver 'ın bir DbProviderFactory çözümlemek için kullanılabileceği anlamına gelir.

DbProviderFactory 'in bu şekilde çözülmesi birçok etkilere sahiptir:

*   Kod tabanlı yapılandırma kullanan bir uygulama, uygun DbProviderFactory 'yi kaydetmek için DbConfiguration sınıfında çağrı ekleyebilir. Bu, özellikle herhangi bir dosya tabanlı yapılandırmayı kullanmak istemediğiniz (veya yapamayan) uygulamalar için yararlıdır.
*   Bu hizmet, yukarıdaki _sarmalama sağlayıcıları_ bölümünde açıklandığı gibi replaceservice kullanılarak sarmalanabilir veya değiştirilebilir
*   Teorik olarak, bir DbProviderServices uygulaması bir DbProviderFactory çözümleyebilir.

Bunlardan herhangi birini yapmakla ilgili dikkat edilmesi gereken önemli nokta, yalnızca DbProviderFactory 'ın EF ile olan aramasını etkiler. Diğer EF olmayan kod, ADO.NET sağlayıcısı 'nın normal şekilde kaydedilmesini bekleyebilir ve kayıt bulunamazsa başarısız olabilir. Bu nedenle, DbProviderFactory 'nin normal ADO.NET şekilde kaydedilmesi genellikle daha iyidir.

### <a name="related-services"></a>İlgili hizmetler

Bir DbProviderFactory çözümlemek için EF kullanılırsa, ayrıca IProviderInvariantName ve ıdbproviderfactoryresolver hizmetlerini de çözmelidir.

IProviderInvariantName, belirli bir DbProviderFactory türü için sağlayıcı sabit adını belirlemede kullanılan bir hizmettir. Bu hizmetin varsayılan uygulanması ADO.NET sağlayıcı kaydını kullanır. Bu, DbProviderFactory EF tarafından çözümlendiği için ADO.NET sağlayıcısı normal şekilde kaydedilmemişse, bu hizmetin çözümlenmesi için de gerekli olacaktır. DbConfiguration. SetProviderFactory yöntemi kullanılırken bu hizmet için bir çözümleyici 'nin otomatik olarak eklendiğini unutmayın.

Yukarıdaki _sağlayıcı türleri genel bakış_ bölümünde açıklandığı gibi, ıdbproviderfactoryresolver, belirli bir DbConnection nesnesinden doğru DbProviderFactory elde etmek için kullanılır. .NET 4 ' te çalışırken bu hizmetin varsayılan uygulanması ADO.NET sağlayıcısı kaydını kullanır. Bu, DbProviderFactory EF tarafından çözümlendiği için ADO.NET sağlayıcısı normal şekilde kaydedilmemişse, bu hizmetin çözümlenmesi için de gerekli olacaktır.
