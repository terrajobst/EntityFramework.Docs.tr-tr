---
title: Entity Framework 6 sağlayıcı modeli - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: d07a8689fe968bb1512095a59a61abc7ac346a31
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022330"
---
# <a name="the-entity-framework-6-provider-model"></a>Entity Framework 6 sağlayıcı modeli

Entity Framework sağlayıcısı model Entity Framework, farklı türde veritabanı sunucusu ile kullanılacak sağlar. Örneğin, bir sağlayıcı başka bir sağlayıcı uygulamasına Microsoft SQL Server Compact Edition karşı kullanılmak üzere EF izin vermek için takılabilen sırasında Microsoft SQL Server karşı kullanılmak üzere EF izin vermek için takılabilir. Farkında duyuyoruz EF6 için sağlayıcılar bulunabilir [Entity Framework sağlayıcıları](~/ef6/fundamentals/providers/index.md) sayfası.

Bazı değişiklikleri bir açık kod Lisansı kapsamında yayımlanacak EF izin vermek için sağlayıcıları EF etkileşimde şekilde gerekiyordu. Bu değişiklikler, sağlayıcı kaydı için yeni mekanizmaları birlikte EF6 derlemelere karşı EF sağlayıcılarının yeniden oluşturmayı gerektirir.

## <a name="rebuilding"></a>Yeniden oluşturma

EF6 ile daha önce .NET Framework'ün bir parçası olan çekirdek kod artık bant dışı (OOB) derlemeler sevk edilen. EF6'yı kullanan uygulamalar oluşturma hakkında ayrıntılı bilgi bulunabilir [EF6 için uygulamaları güncelleştirme](~/ef6/what-is-new/upgrading-to-ef6.md) sayfası. Sağlayıcıları, ayrıca bu yönergeleri kullanarak yeniden oluşturulması gerekir.

## <a name="provider-types-overview"></a>Sağlayıcı türlerine genel bakış

EF sağlayıcısı gerçekten bu hizmetler gelen (için temel sınıf) genişletin veya (bir arabirim için) ve CLR türleri tarafından tanımlanan sağlayıcıya özgü Hizmetleri koleksiyonudur. Bu hizmetlerin iki temel ve EF çalışabilmesi gerekli. Diğerleri isteğe bağlıdır ve yalnızca belirli işlevleri gereklidir ve/veya bu hizmetler varsayılan uygulamaları için hedeflenen belirli bir veritabanı sunucusu çalışmıyor uygulanması gerekir.

## <a name="fundamental-provider-types"></a>Temel sağlayıcı türleri

### <a name="dbproviderfactory"></a>DbProviderFactory

EF bağlıdır öğesinden türetilmiş bir tür olması [System.Data.Common.DbProviderFactory](https://msdn.microsoft.com/library/system.data.common.dbproviderfactory.aspx) tüm alt düzey veritabanı erişimi gerçekleştirme. DbProviderFactory EF aslında bir parçası değil ancak bunun yerine bir giriş noktası için ADO.NET sağlayıcıları hizmet veren .NET Framework sınıfında EF, diğer O/RMs tarafından veya doğrudan bir uygulama tarafından bağlantıları, komutlar, Parametreler örneğini almak için kullanılabilir ve Diğer ADO.NET soyutlama sağlayıcıyıda dilden bağımsız bir şekilde. DbProviderFactory hakkında daha fazla bilgi bir bulunabilecek [ADO.NET için MSDN belgelerine](https://msdn.microsoft.com/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

ADO.NET sağlayıcısı tarafından zaten sağlanan işlevselliği üzerine EF tarafından gereken ek işlevsellik sağlamak için DbProviderServices türetilmiş bir tür EF bağlıdır. EF'ın eski sürümlerinde DbProviderServices sınıf .NET Framework'ün bir parçası olan ve System.Data.Common Ad alanında bulunamadı. Bu sınıf EF6'ile başlayan EntityFramework.dll artık parçasıdır ve System.Data.Entity.Core.Common ad alanındadır.

Temel işlevselliğini DbProviderServices uygulama hakkında daha fazla ayrıntı bulunabilir [MSDN](https://msdn.microsoft.com/library/ee789835.aspx). Ancak, çoğu kavramları hala geçerli olduğu halde bu bilgileri yazma saati itibarıyla EF6 için güncelleştirilmez unutmayın. DbProviderServices SQL Server ve SQL Server Compact uygulamalarını da içinde denetlenmiş olan [açık kaynak kod tabanı](https://github.com/aspnet/EntityFramework6/) ve diğer uygulamalar için kullanışlı bir başvuru olarak hizmet verebilir.

EF eski sürümlerinde DbProviderServices uygulamasını kullanmak için doğrudan bir ADO.NET Sağlayıcısı'ndan edinilen. Bu, DbProviderFactory IServiceProvider için atama ve GetService metodunu çağırarak yapıldı. Bu EF sağlayıcısı için DbProviderFactory sıkı şekilde bağlı. Bu bağlantı, .NET Framework dışında taşınmış EF engellendi, bu nedenle bu sıkı bağ EF6 için kaldırılmıştır ve DbProviderServices uygulaması artık doğrudan uygulamanın yapılandırma dosyası veya kod tabanlı kayıtlı daha ayrıntılı olarak açıklandığı gibi yapılandırma _kaydetme DbProviderServices_ bölümüne bakın.

## <a name="additional-services"></a>Ek hizmetler

Yukarıda açıklanan temel hizmetlerin yanı sıra ayrıca her zaman veya bazen sağlayıcıya özgü olan EF tarafından kullanılan pek çok diğer hizmet mevcuttur. Bu hizmetlerin varsayılan sağlayıcıya özgü uygulamaları DbProviderServices uygulama tarafından sağlanabilir. Uygulamalar Ayrıca bu hizmetlerin uygulamalarını geçersiz kılmak veya DbProviderServices türü varsayılan bir sağlamaz uygulamaları belirtin. Bu daha ayrıntılı olarak açıklanan _ek hizmetler çözümleme_ bölümüne bakın.

Sağlayıcı için bir sağlayıcı ilgilendirebilecek diğer hizmet türleri aşağıda listelenmiştir. Bu hizmet türlerinin her biri hakkında daha fazla ayrıntı için API belgelerinde bulunabilir.

### <a name="idbexecutionstrategy"></a>Idbexecutionstrategy

Bu veritabanında sorgulara ve komutlara yürütüldüğünde, yeniden deneme veya diğer davranışı uygulamak bir sağlayıcı sağlayan isteğe bağlı bir hizmettir. Uygulaması sağlanırsa, ardından EF yalnızca şu komutları çalıştırın ve oluşturulan tüm özel durumları yayar. SQL Server için SQL Azure gibi bulut tabanlı veritabanı sunucularına karşı çalışırken kullanışlı olan bir yeniden deneme ilkesi sağlamak için bu hizmeti kullanılır.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Bu yalnızca bir veritabanı adı verildiğinde kurala göre DbConnection nesneleri oluşturmak bir sağlayıcı sağlayan isteğe bağlı bir hizmettir. Bu hizmet çalışırken EF 4.1 beri var olmuştur ve aynı zamanda açıkça kod veya yapılandırma dosyasında ayarlanabilir DbProviderServices uygulaması tarafından çözümlenen unutmayın. Sağlayıcı, yalnızca varsayılan sağlayıcı olarak kaydettiyseniz, bu hizmet çözmek için imkanına sahip olur (bkz _varsayılan sağlayıcı_ aşağıda) ve varsayılan bağlantı üretecini başka bir yerde ayarlanmadı.

### <a name="dbspatialservices"></a>DbSpatialServices

Coğrafi konum ve geometri uzamsal türler için destek eklemek bir sağlayıcı sağlayan isteğe bağlı bir hizmet budur. Bu hizmet uygulaması, bir uygulamanın EF uzamsal türler ile kullanılacak sağlanmalıdır. DbSptialServices için iki yolla istenir. İlk olarak, sağlayıcıya özgü uzamsal hizmetler DbProviderInfo nesnesi kullanılarak istenir (değişmez değer içeren ad ve bildirim belirteci) anahtarı olarak. İkinci olarak, DbSpatialServices için hiçbir anahtar ile onaylayanlara sorulabilir. Bu, "tek başına DbGeography veya DbGeometry türleri oluştururken kullanılan genel uzamsal sağlayıcı" çözmek için kullanılır.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

EF geçişleri, oluşturmak ve veritabanı şemalarını değiştirme kullanılan SQL üretimi için Code First tarafından kullanılacak sağlayan isteğe bağlı bir hizmettir. Uygulamaya geçişlerini desteklemek için gereklidir. Uygulamaya sağlanmazsa veritabanı başlatıcılar veya Database.Create yöntemi kullanılarak veritabanları oluşturulduğunda ardından onu da kullanılır.

### <a name="funcdbconnection-string-historycontextfactory"></a>FUNC < DbConnection, dize, HistoryContextFactory >

Bu eşleme için HistoryContext yapılandırmak bir sağlayıcı sağlayan, isteğe bağlı bir hizmettir `__MigrationHistory` EF geçişleri tarafından kullanılan tablo. HistoryContext kod ilk DbContext olduğundan ve tablo ve sütun eşleme belirtimlerini adı gibi şeyleri değiştirmek için normal fluent API'si kullanılarak yapılandırılabilir. Bu sağlayıcı tarafından desteklenen tüm varsayılan tablo ve sütun eşlemelerini bile varsayılan uygulamasını EF tarafından döndürülen tüm sağlayıcıları için bu hizmet için belirtilen veritabanı sunucusu çalışabilir. Böyle bir durumda sağlayıcısı bu hizmet uygulaması sağlamanız gerekmez.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Bu, belirli bir DbConnection nesneden doğru DbProviderFactory alma isteğe bağlı bir hizmettir. Varsayılan uygulamasını EF tarafından döndürülen tüm sağlayıcıları için bu hizmet, tüm sağlayıcılar için amaçlanmıştır. Ancak .NET 4'te çalışan olduğunda DbProviderFactory bir IF genel olarak erişilebilir değil, DbConnections. Bu nedenle, EF eşleştirme bulmak üzere kayıtlı sağlayıcılardan aramak için bazı buluşsal yöntemler kullanır. Bazı sağlayıcıları için bu buluşsal yöntemler başarısız olur ve bu gibi durumlarda, yeni bir uygulama sağlayıcısı sunmalıdır mümkündür.

## <a name="registering-dbproviderservices"></a>DbProviderServices kaydediliyor

DbProviderServices uygulamasını kullanmak için uygulamanın yapılandırma dosyası (app.config veya web.config) veya kod tabanlı yapılandırma kullanılarak kaydedilebilir. Her iki durumda da kayıt sağlayıcının "değişmez adı" bir anahtar olarak kullanır. Bu, kayıtlı ve tek bir uygulamada kullanılan birden çok sağlayıcı sağlar. EF kayıtları için kullanılan değişmez adı ADO.NET Sağlayıcısı kaydı ve bağlantı dizeleri için kullanılan değişmez adı ile aynıdır. Örneğin, SQL Server için sabit adı "System.Data.SqlClient" kullanılır.

### <a name="config-file-registration"></a>Yapılandırma dosyası kayıt

Kullanılacak DbProviderServices türünü, uygulamanın yapılandırma dosyasında entityFramework bölümünü sağlayıcıları listesinden bir sağlayıcı öğesi olarak kaydedilir. Örneğin:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

_Türü_ dize DbProviderServices uygulamasını kullanmak için derleme nitelikli tür adı olmalıdır.

### <a name="code-based-registration"></a>Kod tabanlı kayıt

EF6 sağlayıcıları ile başlayarak, ayrıca kod kullanılarak kaydedilebilir. Bu, uygulamanın yapılandırma dosyası herhangi bir değişiklik olmadan kullanılacak bir EF sağlayıcı sağlar. DbConfiguration sınıfı bölümünde anlatıldığı gibi bir uygulama oluşturmalısınız kod tabanlı yapılandırma kullanılacak [kod tabanlı yapılandırma belgeleri](https://msdn.com/data/jj680699). DbConfiguration sınıfının oluşturucusu, ardından EF sağlayıcıyı kaydetmek için SetProviderServices çağırmalıdır. Örneğin:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Ek hizmetler çözümleme

Yukarıda belirtildiği gibi _sağlayıcısı türlerinin genel bakış_ bölümünde, bir DbProviderServices sınıfı ayrıca ek hizmetlerin çözümlenmesi için kullanılabilir. Bu, çünkü DbProviderServices IDbDependencyResolver uygular ve her kayıtlı DbProviderServices türü "Varsayılan çözümleyici" eklenir mümkündür. IDbDpendencyResolver mekanizması, daha ayrıntılı olarak açıklanan [bağımlılık çözümlemesi](~/ef6/fundamentals/configuring/dependency-resolution.md). Ancak, bu özellikteki sağlayıcıyıda ek hizmetlerin çözümlenmesi için tüm kavramları anlamak gerekli değildir.

Ek hizmetlerin çözümlenmesi sağlayıcı için en yaygın yolu DbProviderServices sınıfın oluşturucusundaki her hizmet için DbProviderServices.AddDependencyResolver çağırmaktır. Örneğin, başlatma için buna benzer kod SqlProviderServices (SQL Server için EF sağlayıcısı) vardır:

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

Bu oluşturucu, aşağıdaki yardımcı sınıfları kullanır:

*   SingletonDependencyResolver: Tekil hizmetin çözümlemek için basit bir yol sağlar — diğer bir deyişle, hizmetler için aynı örneğini getirilen her zaman GetService çağrılır. Geçici Hizmetleri genellikle isteğe bağlı olarak geçici örnekleri oluşturmak için kullanılacak bir singleton üreteci olarak kaydedilir.
*   ExecutionStrategyResolver: bir çözümleyici IExecutionStrategy uygulamaları döndürmek için belirli.

DbProviderServices.AddDependencyResolver kullanmak yerine DbProviderServices.GetService geçersiz kılmak ve ek hizmetler doğrudan çözmek mümkün. Belirli bir tür tarafından ve bazı durumlarda, belirli bir anahtar için tanımlanmış bir hizmeti EF ihtiyacı olduğunda, bu yöntem çağrılır. Yöntemi, bu olabilir veya hizmet verme ayrılma null döndürür ve bunun yerine bu sorunu çözmek başka bir sınıf izin verirseniz hizmet döndürmesi gerekir. Örneğin, varsayılan bağlantı üretecini çözmek için GetService kodu aşağıdakine benzeyebilir:

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

Bir uygulamanın yapılandırma dosyasında birden çok DbProviderServices uygulamaları kayıtlı olduğunda ikincil Çözümleyicileri listelendikleri sırayla olarak eklenir. Çözümleyiciler her zaman ikincil çözümleyici zinciri üstüne listesinin sonuna sağlayıcısında buna eklenir beri diğerlerinden önce bağımlılıkları çözümlemek için imkanına sahip olur. (Bu bir biraz counter-intuitive ilk görünebilir, ancak her bir sağlayıcı listesi dışında alma ve üstünde mevcut sağlayıcıları yığınlama hayal ederseniz mantıklıdır.)

Bu sıralama, genellikle çoğu sağlayıcısı hizmetler sağlayıcıya özgü ve sağlayıcının değişmez adı tarafından Anahtarlanan olduğundan farketmez. Ancak, hizmetler için sağlayıcının değişmez adı ya da bu sıralama bağlı hizmet çözümlenir bazı diğer sağlayıcıya özgü anahtar tarafından Anahtarlanan değil. Bu açıkça farklı bir yerde başka ayarlanmazsa, örneğin, ardından varsayılan bağlantı üretecini zincirindeki en üstteki sağlayıcısından gelir.

## <a name="additional-config-file-registrations"></a>Ek yapılandırma dosyası kayıtları

Doğrudan uygulamanın yapılandırma dosyasında yukarıda açıklanan ek sağlayıcısı hizmetlerinden bazılarını kaydetmek mümkündür. Bu yapılandırma dosyasında kaydı yapıldığında DbProviderServices uygulama GetService yöntemi tarafından döndürülen herhangi bir şey yerine kullanılacaktır.

### <a name="registering-the-default-connection-factory"></a>Varsayılan bağlantı üretecini kaydediliyor

İle EF5 EntityFramework NuGet paketini otomatik olarak başlatılmasını SQL Express bağlantı üreteci ya da LocalDb bağlantı üreteci yapılandırma dosyasında kayıtlı.

Örneğin:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

_Türü_ IDbConnectionFactory uygulamalıdır varsayılan bağlantı üreteci için derleme nitelikli tür adı.

Sağlayıcı NuGet paketi yüklendiğinde bu şekilde varsayılan bağlantı üretecini ayarlamanız önerilir. Bkz: _sağlayıcıları için NuGet paketlerini_ aşağıda.

## <a name="additional-ef6-provider-changes"></a>Ek EF6 sağlayıcısı değişiklikler

### <a name="spatial-provider-changes"></a>Uzamsal sağlayıcısı değişiklikleri

Uzamsal türlerini destekleyen sağlayıcıları artık DbSpatialDataReader türevi olan sınıflarda bazı ek yöntemleri uygulamanız gerekir:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Yeni zaman uyumsuz sürümlerini varsayılan uygulamaları zaman uyumlu metotlar için temsilci seçme ve bu nedenle zaman uyumsuz olarak Yürütülmeyen geçersiz kılınacak önerilen mevcut yöntemleri vardır:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Enumerable.Contains için yerel destek

EF6 Enumerable.Contains LINQ sorguları kullanımını etrafında performans sorunlarını gidermek için eklenen DbInExpression yeni ifade türü sunar. Yeni bir sanal yöntemle, yeni ifade türü bir sağlayıcı tarafından işlendiğini varsa belirlemek amacıyla EF tarafından çağrılan SupportsInExpression DbProviderManifest sınıfı vardır. Mevcut bir sağlayıcı uygulamaları ile uyumluluk için yöntem false döndürür. Bu gelişmeleri yararlanmak için bir EF6 sağlayıcısı DbInExpression işlemek ve SupportsInExpression true döndürmek için geçersiz kılmak için kod ekleyebilirsiniz. DbExpressionBuilder.In yöntemi çağırarak DbInExpression örneği oluşturulabilir. Genellikle bir tablo sütunu ve test etmek için bir eşleşme için DbConstantExpression listesini temsil eden, bir Dbexpression'dan DbInExpression örneği oluşur.

## <a name="nuget-packages-for-providers"></a>NuGet paketlerini sağlayıcıları

EF6 sağlayıcısı kullanılabilir duruma getirmenin bir yolu, bir NuGet paketi olarak yayın sağlamaktır. Bir NuGet paketi kullanarak aşağıdaki avantajlara sahiptir:

*   Sağlayıcı kaydı uygulamanın yapılandırma dosyasına eklemek için kullanımı kolay NuGet olduğu
*   Varsayılan bağlantı üretecini kurala göre yapılan bağlantılar kayıtlı sağlayıcı kullanacak şekilde ayarlamak için yapılandırma dosyasına ek değişiklik yapılamaz
*   NuGet tanıtıcıları bağlama yeniden yönlendirmeleri ekleyerek EF6 sağlayıcısı bile yeni EF paketi kullanıma sunulduktan sonra çalışmaya devam etmesi gerekir

Buna örnek olarak dahil edilen EntityFramework.SqlServerCompact pakettir [açık kaynak kod tabanı](http://github.com/aspnet/entityframework6). Bu paket, NuGet paketlerini EF sağlayıcısı oluşturmak için iyi bir şablonu sağlar.

### <a name="powershell-commands"></a>PowerShell komutları

EntityFramework NuGet paketi yüklendiğinde sağlayıcısı paketleri için çok kullanışlı olan iki komutları içeren bir PowerShell modülü kaydeder:

*   Ekleme EFProvider hedef projenin yapılandırma dosyasında sağlayıcının yeni bir varlık ekler ve kayıtlı sağlayıcılar listesi sonunda olduğundan emin olur.
*   Ekleme EFDefaultConnectionFactory ekler veya hedef projenin yapılandırma dosyasında defaultConnectionFactory kaydı güncelleştirir.

Hem bu komutları gerekirse sağlayıcıları koleksiyonu ekleme ve yapılandırma dosyasına bir entityFramework bölümü eklemeyi ve dikkatli olun.

Bu komutlar NuGet betik install.ps1 çağrılması amaçlanmıştır. Örneğin, SQL Compact sağlayıcısı install.ps1 şuna benzer:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Bu komutlar hakkında daha fazla bilgi Paket Yöneticisi konsolu penceresinde get-help kullanılarak elde edilebilir.

## <a name="wrapping-providers"></a>Kaydırma sağlayıcıları

Kaydırma genişletmek için profil oluşturma veya izleme özellikleri gibi diğer işlevlere sahip mevcut bir sağlayıcı sarmalayan bir EF ve/veya ADO.NET sağlayıcısı sağlayıcısıdır. Sağlayıcıları sarmalama normal yolla kaydedilebilir, ancak bu genellikle daha fazla çözüm sağlayıcısı ile ilgili hizmetlerin uğratarak zamanında sarmalama Sağlayıcısı Kurulumu uygun. Bunu yapmak için statik olay OnLockingConfiguration DbConfiguration sınıfı üzerinde kullanılabilir.

OnLockingConfiguration EF burada uygulama etki alanı için tüm EF yapılandırma öğesinden elde edileceği belirledi sonra ancak kullanım için kilitlenmeden önce çağrılır. Uygulama başlangıcında (EF kullanılmadan önce) uygulama bu olay için bir olay işleyicisi kaydolmalıdır. (Bu henüz desteklenmiyor ancak Biz bu işleyici yapılandırma dosyasında kaydetmek için destek ekleme değerlendiriyorsanız.) Olay işleyicisi, ardından bir çağrı ReplaceService sarmalamak için gereken her hizmet için olmanız gerekir.  

Örneğin, IDbConnectionFactory ve DbProviderService kaydırmak için şuna benzeyen bir işleyici kaydedilmelidir:

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

Çözümlenmiş ve hizmet çözümlemek için kullanılan anahtarı ile birlikte artık sarılacağını hizmeti işleyiciye geçirilir. İşleyici sarmalamak bu hizmet ve döndürülen hizmet Sarmalanan bir sürümle değiştirin.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>DbProviderFactory EF ile çözümleme

DbProviderFactory olduğu EF tarafından açıklanan şekilde gerekli temel sağlayıcı türlerinden birini _sağlayıcısı türlerinin genel bakış_ yukarıdaki bölümde. Zaten belirtilen EF türü değilse ve kayıt genellikle EF yapılandırmasının bir parçası değildir, ancak bunun yerine normal ADO.NET Sağlayıcısı kaydı machine.config dosyasının ve/veya uygulamanın yapılandırma dosyası olduğu.

Bu EF rağmen yine de, normal bağımlılık çözümleme mekanizması olduğunda DbProviderFactory için arama kullanmak için kullanır. Varsayılan çözümleyici normal ADO.NET kayıt yapılandırma dosyalarında kullanır ve bu nedenle bu genellikle saydamdır. Ancak normal bağımlılık çözümlemesi nedeniyle mekanizması olarak kullanıldığı bir IDbDependencyResolver bile normal ADO.NET kaydı yapılmadı olduğunda DbProviderFactory gidermek için kullanılabilir anlamına gelir.

Bu şekilde DbProviderFactory çözme, birkaç olası etkilere sahiptir:

*   Kod tabanlı yapılandırma kullanarak uygulama uygun DbProviderFactory kaydedilecek DbConfiguration sınıfında çağrılar ekleyebilirsiniz. Etmek istiyor musunuz (veya açılamıyor) uygulamalar için özellikle kullanışlıdır yapmak hiç içeren herhangi bir dosya tabanlı yapılandırmayı kullanın.
*   Hizmet sarmalanabilir veya ReplaceService makalesinde açıklanan şekilde kullanarak yerine _sağlayıcıları sarmalama_ yukarıdaki bölümde
*   Teorik olarak, DbProviderServices uygulama DbProviderFactory çözümlenemiyor.

Bunlardan herhangi birini yapma hakkında dikkat edilecek önemli olan nokta, bunların yalnızca EF tarafından DbProviderFactory arama etkiler ' dir. Diğer EF olmayan kod hala normal bir şekilde kaydedilecek ADO.NET sağlayıcısı bekleyebilir ve kayıt bulunamaması durumunda başarısız olabilir. Bu nedenle, normal bir ADO.NET şekilde kaydedilecek DbProviderFactory için normalde daha iyi olacaktır.

### <a name="related-services"></a>İlgili hizmetler

Ardından EF DbProviderFactory çözmek için kullanılıyorsa IProviderInvariantName ve IDbProviderFactoryResolver Hizmetleri çözümlenmelidir.

IProviderInvariantName DbProviderFactory belirli bir tür bir sağlayıcı değişmez adını belirlemek için kullanılan bir hizmettir. Bu hizmet, varsayılan uygulama, ADO.NET Sağlayıcısı kaydı kullanır. Bu DbProviderFactory tarafından EF çözümlendiğinden ADO.NET sağlayıcısı normal bir şekilde kayıtlı değil, daha sonra Ayrıca bu hizmet çözmek gerekli anlamına gelir. Bu hizmet için bir çözümleyici DbConfiguration.SetProviderFactory yöntemi kullanırken otomatik olarak eklendiğini unutmayın.

Bölümünde anlatıldığı gibi _sağlayıcısı türlerinin genel bakış_ yukarıda bölümünde, IDbProviderFactoryResolver doğru DbProviderFactory belirli bir DbConnection nesneden elde etmek için kullanılır. Bu hizmet, .NET 4'te çalışan ADO.NET Sağlayıcısı kaydı kullandığında varsayılan uygulaması. Bu DbProviderFactory tarafından EF çözümlendiğinden ADO.NET sağlayıcısı normal bir şekilde kayıtlı değil, daha sonra Ayrıca bu hizmet çözmek gerekli anlamına gelir.
