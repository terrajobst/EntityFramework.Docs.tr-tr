---
title: Bağımlılık çözümlemesi - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 76bb350f2407e0e33a20d0c4f6961ba57d646818
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914217"
---
# <a name="dependency-resolution"></a>Bağımlılık çözümlemesi
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

EF6 ile başlayarak, Entity Framework gerekli hizmetleri, uygulamaları alma genel amaçlı bir mekanizma içerir. Diğer bir deyişle, EF bazı arabirimleri ya da taban sınıfların örneği kullandığında bunu kullanmak için somut bir uygulama arabirim veya temel sınıfının sorar. Bu, IDbDependencyResolver arabirimi kullanılmasıyla elde edilir:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

GetService yöntemi genellikle EF tarafından çağrılır ve EF veya uygulama tarafından sağlanan IDbDependencyResolver uygulaması tarafından işlenir. Çağrıldığında, tür bağımsız değişkeni istenen hizmet arabirimi veya temel sınıf türünde olduğundan ve anahtar nesnesi null ya da istenen hizmeti hakkında bağlamsal bilgiler sağlayan bir nesne değil.  

Bu makalede IDbDependencyResolver uygulamak tam birleştiremiyorsa içermiyor, ancak bunun yerine, kendisi için EF GetService semantiği anahtar nesnesinin her biri bu çağrılar için çağırır ve hizmet türleri (yani, arabirim ve temel sınıf türleri) için bir başvuru olarak görev yapar. Ek hizmetler eklendikçe bu belgeyi güncel tutulacak.  

## <a name="services-resolved"></a>Çözümlenen hizmetler  

Tekil olarak kullanılabildiğinden döndürülen herhangi bir nesne iş parçacığı açısından güvenli olmalıdır aksi belirtilmediği sürece. Çoğu durumda nesne bir factory, bu durumda, döndürülen iş parçacığı açısından güvenli Fabrika olmalıdır ancak fabrikadan döndürülen nesne, her kullanım için Fabrika öğesinden yeni bir örneğini istediğinden itibaren geçen iş parçacığı açısından güvenli olması gerekmez.  

### <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: Belirtilen içerik türü için bir veritabanı Başlatıcısı  

**Anahtar**: kullanılan; null  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>FUNC < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Kullanıma sunulan sürümü**: EF6.0.0

**Döndürülen nesne**: geçişler ve veritabanı başlatıcılar ile veritabanı oluşturma gibi oluşturulması için bir veritabanı neden diğer eylemler için kullanılabilir bir SQL oluşturucuyu oluşturmak için bir üreteci.  

**Anahtar**: SQL oluşturulacak veritabanı türünü belirleyen ADO.NET sağlayıcısı değişmez adını içeren bir dize. Örneğin, SQL Server SQL Oluşturucu "System.Data.SqlClient" anahtarı için döndürülür.  

>[!NOTE]
> EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: verilen sağlayıcı değişmez adı için kullanılacak EF sağlayıcısı  

**Anahtar**: bir sağlayıcı gereklidir veritabanı türünü belirleyen ADO.NET sağlayıcısı değişmez adını içeren bir dize. Örneğin, SQL Server sağlayıcısı için "System.Data.SqlClient" anahtar döndürülür.  

>[!NOTE]
> EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: EF veritabanı bağlantısı oluşturduğunda, kural olarak kullanılacak bağlantı üreteci. Diğer bir deyişle, hiçbir bağlantı veya bağlantı dizesi için EF verilir ve bağlantı dizesi app.config veya web.config içinde bulunabilir, sonra bu hizmet bir bağlantı oluşturmak için kural olarak kullanılır. Bağlantı üreteci değiştirme (örneğin SQL Server Compact Edition) veritabanının farklı bir türü varsayılan olarak kullanmak üzere EF izin verebilirsiniz.  

**Anahtar**: kullanılan; null  

>[!NOTE]
> EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: bağlantı sağlayıcısı bildirimi belirteci oluşturan bir hizmet. Bu hizmet, genellikle iki şekilde kullanılır. İlk olarak, Code First modeli oluştururken veritabanına bağlanırken önlemek için kullanılabilir. İkinci olarak, SQL Server 2008 bazen kullanıldığında bile bir modeli SQL Server 2005'te zorlamak için belirli bir veritabanı sürümü için--örneğin, bir model oluşturmak için Code First zorlamak için kullanılabilir.  

**Nesne ömrü**: tekil--aynı nesneye kullanılan birden çok kez ve aynı anda farklı iş parçacıkları tarafından olabilir  

**Anahtar**: kullanılan; null  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: bir hizmet sağlayıcı üreteci belirli bir bağlantı edinebilirsiniz. .NET 4.5 üzerinde sağlayıcı bağlantısı genel olarak erişilebilir. .NET 4'te bu hizmetin varsayılan uygulama eşleşen sağlayıcı bulunamadı bazı buluşsal yöntemler kullanır. Bunlar daha sonra bu hizmetin yeni bir uygulama başarısız olursa, uygun bir çözüm sağlamak için kaydedilebilir.  

**Anahtar**: kullanılan; null  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>FUNC < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: belirli bir bağlam için bir model önbellek anahtarını oluşturan bir üreteci. Varsayılan olarak EF DbContext tür sağlayıcı başına başına bir model önbelleğe alır. Bu hizmetin farklı bir uygulama için önbellek anahtarını şema adı gibi diğer bilgilerini eklemek için kullanılabilir.  

**Anahtar**: kullanılan; null  

### <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: temel EF sağlayıcı coğrafya ve geometri uzamsal türler için destek ekleyen bir EF uzamsal sağlayıcısı.  

**Anahtar**: DbSptialServices sorular için iki yolla. İlk olarak, sağlayıcıya özgü uzamsal hizmetler DbProviderInfo nesnesi kullanılarak istenir (değişmez değer içeren ad ve bildirim belirteci) anahtarı olarak. İkinci olarak, DbSpatialServices için hiçbir anahtar ile onaylayanlara sorulabilir. Bu, "tek başına DbGeography veya DbGeometry türleri oluştururken kullanılan genel uzamsal sağlayıcı" çözmek için kullanılır.  

>[!NOTE]
> EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>FUNC < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: veritabanına karşı sorgulara ve komutlara yürütüldüğünde, yeniden deneme veya diğer davranışı uygulamak bir sağlayıcı sağlayan bir hizmet oluşturmak için bir üreteci. Uygulaması sağlanırsa, ardından EF yalnızca şu komutları çalıştırın ve oluşturulan tüm özel durumları yayar. SQL Server için SQL Azure gibi bulut tabanlı veritabanı sunucularına karşı çalışırken kullanışlı olan bir yeniden deneme ilkesi sağlamak için bu hizmeti kullanılır.  

**Anahtar**: sağlayıcının değişmez adı ve isteğe bağlı yürütme stratejisi kullanılacak bir sunucu adı içeren bir ExecutionStrategyKey nesne.  

>[!NOTE]
> EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>FUNC < DbConnection, dize, System.Data.Entity.Migrations.History.HistoryContext\>  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: HistoryContext için eşlemeyi yapılandırmak bir sağlayıcı izin veren bir Fabrika `__MigrationHistory` EF geçişleri tarafından kullanılan tablo. HistoryContext kod ilk DbContext olduğundan ve tablo ve sütun eşleme belirtimlerini adı gibi şeyleri değiştirmek için normal fluent API'si kullanılarak yapılandırılabilir.  

**Anahtar**: kullanılan; null  

>[!NOTE]
> EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="systemdatacommondbproviderfactory"></a>System.Data.Common.DbProviderFactory  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: ADO.NET sağlayıcısı belirtilen sağlayıcı değişmez adı için kullanılacak.  

**Anahtar**: ADO.NET sağlayıcısı değişmez adını içeren bir dize  

>[!NOTE]
> Varsayılan uygulama normal ADO.NET Sağlayıcısı kaydı kullandığından bu hizmet genellikle doğrudan değiştirilemez. EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: DbProviderFactory belirli bir tür bir sağlayıcı değişmez adını belirlemek için kullanılan bir hizmettir. Bu hizmet, varsayılan uygulama, ADO.NET Sağlayıcısı kaydı kullanır. Bu DbProviderFactory tarafından EF çözümlendiğinden ADO.NET sağlayıcısı normal bir şekilde kayıtlı değil, daha sonra Ayrıca bu hizmet çözmek gerekli anlamına gelir.  

**Anahtar**: DbProviderFactory örneği için bir değişmez adı gereklidir.  

>[!NOTE]
> EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: önceden üretilmiş görünümleri içeren derlemeler, bir önbellek. Bir değiştirme, genellikle tüm keşfi olmadan hangi derlemelerin önceden üretilmiş görünümleri içeren bilmeniz EF izin vermek için kullanılır.  

**Anahtar**: kullanılan; null  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: pluralize ve adları singularize EF tarafından kullanılan bir hizmet. Varsayılan olarak İngilizce çoğullaştırma hizmeti kullanılır.  

**Anahtar**: kullanılan; null  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Kullanıma sunulan sürümü**: EF6.0.0

**Döndürülen nesne**: uygulama başlatıldığında, kaydedilmesi gereken tüm dinleyicileri. Herhangi bir bağımlılık çözümleyici tarafından döndürülen tüm dinleyicileri GetServices kullanarak bu nesneleri istenen ve Not kayıtlı.

**Anahtar**: kullanılan; null olacaktır.  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>FUNC < System.Data.Entity.DbContext, eylem < dize\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Kullanıma sunulan sürümü**: EF6.0.0  

**Döndürülen nesne**: olacak veritabanı günlük biçimlendirici oluşturmak için kullanılan Fabrika kullanılabilir bağlamı. Belirtilen bağlama Database.Log özelliği ayarlanmış.  

**Anahtar**: kullanılan; null olacaktır.  

### <a name="funcsystemdataentitydbcontext"></a>FUNC < System.Data.Entity.DbContext\>  

**Kullanıma sunulan sürümü**: EF6.1.0  

**Döndürülen nesne**: bağlam erişilebilir parametresiz bir oluşturucu yoksa, geçişler için bağlam örnekleri oluşturmak için kullanılacak bir Üreteç.  

**Anahtar**: bir Fabrika gereklidir türetilmiş DbContext tür için tür nesnesi.  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>FUNC < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Kullanıma sunulan sürümü**: EF6.1.0  

**Döndürülen nesne**: serileştiricileri seri hale getirme türü kesin belirlenmiş özel ek açıklamaları, sıralanabilir ve XML olarak kullanılmak üzere Code First Migrations desterilized şekilde oluşturmak için kullanılan bir Üreteç.  

**Anahtar**: yüklenmekte olan ek açıklama adı serileştirilecek veya serisi.  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>FUNC < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Kullanıma sunulan sürümü**: EF6.1.0  

**Döndürülen nesne**: işleyicileri işlemler için özel işlem işleme hataları işleme gibi durumlar için uygulanabilir olacak şekilde oluşturmak için kullanılan bir Üreteç.  

**Anahtar**: sağlayıcının değişmez adı ve isteğe bağlı olarak hareket işleyicisi kullanılacak bir sunucu adı içeren bir ExecutionStrategyKey nesne.  
