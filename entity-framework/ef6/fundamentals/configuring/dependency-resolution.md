---
title: Bağımlılık çözümlemesi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417864"
---
# <a name="dependency-resolution"></a>Bağımlılık çözümlemesi
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

EF6 ile başlayarak Entity Framework, gereken hizmet uygulamalarını almak için genel amaçlı bir mekanizma içerir. Diğer bir deyişle, EF bazı arabirimlerin veya temel sınıfların bir örneğini kullandığında, arabirim veya temel sınıf için somut bir uygulama ister. Bu, ıdbdependencyresolver arabirimi kullanılarak sağlanır:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

GetService yöntemi genellikle EF tarafından çağrılır ve EF ya da uygulama tarafından verilen bir ıdbdependencyresolver uygulaması tarafından işlenir. Çağrıldığında tür bağımsız değişkeni, istenen hizmetin arabirimi veya temel sınıf türüdür ve anahtar nesne null ya da istenen hizmet hakkında bağlamsal bilgiler sağlayan bir nesnedir.  

Aksi belirtilmediği sürece, döndürülen herhangi bir nesne, tek bir olarak kullanılabilmesi için iş parçacığı açısından güvenli olmalıdır. Çoğu durumda, döndürülen nesne bir fabrikadır, ancak her kullanım için fabrikadan yeni bir örnek istendiğinden, fabrikadan döndürülen nesnenin iş parçacığı açısından güvenli olması gerekmez.

Bu makale, ıdbdependencyresolver 'ın nasıl uygulanacağı hakkında tam ayrıntı içermez, ancak bunun yerine, EF 'in GetService 'i çağırdığı ve bunların her biri için anahtar nesnesinin semantiğinin hizmet türleri (yani, arabirim ve temel sınıf türleri) için başvuru görevi görür aramalarda.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System. Data. Entity. ıdatabasebaşlatıcısı < TContext\>  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: belirtilen bağlam türü için bir veritabanı başlatıcısı  

**Anahtar**: kullanılmıyor; null olur  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System. Data. Entity. geçişler. Sql. MigrationSqlGenerator\>  

**Sunulan sürüm**: EF 6.0.0

**Döndürülen nesne**: geçiş için kullanılabilen ve veritabanı başlatıcılarla veritabanı oluşturma gibi bir veritabanının oluşturulmasına neden olan diğer eylemler için KULLANıLABILEN bir SQL Oluşturucu oluşturmaya yönelik bir fabrika.  

**Anahtar**: SQL oluşturulacak veritabanının türünü belirten ADO.NET sağlayıcısı değişmez adını içeren bir dize. Örneğin, "System. Data. SqlClient" anahtarı için SQL Server SQL Oluşturucu döndürülür.  

>[!NOTE]
> EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="systemdataentitycorecommondbproviderservices"></a>System. Data. Entity. Core. Common. DbProviderServices  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: belirli bir sağlayıcı sabit adı IÇIN kullanılacak EF sağlayıcısı  

**Anahtar**: bir sağlayıcının gerektirdiği veritabanının türünü belirten ADO.NET sağlayıcısı değişmez adını içeren bir dize. Örneğin, SQL Server sağlayıcı "System. Data. SqlClient" anahtarı için döndürülür.  

>[!NOTE]
> EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System. Data. Entity. Infrastructure. ıdbconnectionfactory  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: EF, kurala göre bir veritabanı bağlantısı oluşturduğunda kullanılacak bağlantı fabrikası. Diğer bir deyişle, bir bağlantı veya bağlantı dizesinin EF 'e verilme ve App. config veya Web. config dosyasında hiçbir bağlantı dizesinin bulunamaması durumunda bu hizmet, kurala göre bir bağlantı oluşturmak için kullanılır. Bağlantı fabrikasının değiştirilmesi, EF 'in varsayılan olarak farklı türde bir veritabanı (örneğin, SQL Server Compact Edition) kullanmasına izin verebilir.  

**Anahtar**: kullanılmıyor; null olur  

>[!NOTE]
> EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System. Data. Entity. Infrastructure. IManifestTokenService  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: bir bağlantıdan sağlayıcı bildirim belirteci oluşturabilen bir hizmet. Bu hizmet genellikle iki şekilde kullanılır. İlk olarak, bir model oluştururken veritabanına bağlantı Code First önlemek için kullanılabilir. İkincisi, belirli bir veritabanı sürümü için bir model oluşturmak üzere Code First zorlamak için kullanılabilir (örneğin, bazen SQL Server 2008 kullanılıyorsa bile, bir modeli SQL Server 2005 için zorlamak için kullanılır.  

**Nesne ömrü**: tek--aynı nesne birden çok kez ve farklı iş parçacıkları tarafından aynı anda kullanılabilir  

**Anahtar**: kullanılmıyor; null olur  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System. Data. Entity. Infrastructure. ıdbproviderfactoryservice  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: belirli bir bağlantıdan sağlayıcı fabrikası elde eden bir hizmet. .NET 4,5 ' de, sağlayıcıya genel olarak bağlantıdan erişilebilir. .NET 4 ' te bu hizmetin varsayılan uygulanması, eşleşen sağlayıcıyı bulmak için bazı buluşsal yöntemler kullanır. Bunlar başarısız olursa, uygun bir çözüm sağlamak için bu hizmetin yeni bir uygulama kaydı yapılabilir.  

**Anahtar**: kullanılmıyor; null olur  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System. Data. Entity. Infrastructure. ıdbmodelcachekey\>  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: belirli bir bağlam için model önbellek anahtarı oluşturacak bir fabrika. Varsayılan olarak, EF, sağlayıcı başına her DbContext türü için bir modeli önbelleğe alır. Bu hizmetin farklı bir uygulanması, şema adı gibi diğer bilgileri önbellek anahtarına eklemek için kullanılabilir.  

**Anahtar**: kullanılmıyor; null olur  

## <a name="systemdataentityspatialdbspatialservices"></a>System. Data. Entity. uzamsal. DbSpatialServices  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: Coğrafya ve geometri uzamsal türler IÇIN temel EF sağlayıcısına destek ekleyen bir EF uzamsal sağlayıcı.  

**Anahtar**: dbsptialservices 'in iki şekilde istenir. İlk olarak, sağlayıcıya özgü uzamsal hizmetler, anahtar olarak bir DBProviderInfo nesnesi (Sabit adı ve bildirim belirteci içerir) kullanılarak istenir. İkinci olarak, DbSpatialServices hiçbir anahtar olmadan istenebilir. Bu, tek başına Dbcoğrafya veya DbGeometry türleri oluştururken kullanılan "küresel uzamsal sağlayıcıyı" çözümlemek için kullanılır.  

>[!NOTE]
> EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System. Data. Entity. Infrastructure. ıdbexecutionstrateji\>  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: bir sağlayıcının, sorgular ve komutlar veritabanına göre yürütüldüğünde yeniden denemeler veya diğer davranışları uygulamasına izin veren bir hizmet oluşturmak için bir fabrika. Uygulama sağlanmazsa, EF komutları yürütür ve oluşturulan özel durumları yayacaktır. SQL Server için bu hizmet, SQL Azure gibi bulut tabanlı veritabanı sunucularında çalışırken yararlı olan bir yeniden deneme ilkesi sağlamak için kullanılır.  

**Anahtar**: sağlayıcı sabit adı ve isteğe bağlı olarak yürütme stratejisi kullanılacak bir sunucu adı Içeren bir Executionstratejik jkey nesnesi.  

>[!NOTE]
> EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, String, System. Data. Entity. geçişler. history. Geçmişcontext\>  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: bir sağlayıcının, IBir IBU bağlamın EŞLEMESINI, EF geçişi tarafından kullanılan `__MigrationHistory` tablosuna yapılandırmasını sağlayan bir fabrika. Bu bir DbContext Code First ve tablo adı ve sütun eşleme belirtimleri gibi şeyleri değiştirmek için normal Fluent API kullanılarak yapılandırılabilir.  

**Anahtar**: kullanılmıyor; null olur  

>[!NOTE]
> EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="systemdatacommondbproviderfactory"></a>System. Data. Common. DbProviderFactory  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: belirli bir sağlayıcı sabit adı için kullanılacak ADO.NET sağlayıcısı.  

**Anahtar**: ADO.NET sağlayıcısı değişmez adını içeren bir dize  

>[!NOTE]
> Varsayılan uygulama normal ADO.NET sağlayıcısı kaydını kullandığından, bu hizmet genellikle doğrudan değiştirilmez. EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System. Data. Entity. Infrastructure. IProviderInvariantName  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: belirli bir DbProviderFactory türü için sağlayıcı sabit adını belirlemede kullanılan bir hizmet. Bu hizmetin varsayılan uygulanması ADO.NET sağlayıcı kaydını kullanır. Bu, DbProviderFactory EF tarafından çözümlendiği için ADO.NET sağlayıcısı normal şekilde kaydedilmemişse, bu hizmetin çözümlenmesi için de gerekli olacaktır.  

**Anahtar**: sabit adının gerekli olduğu DbProviderFactory örneği.  

>[!NOTE]
> EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System. Data. Entity. Core. Mapping. ViewGeneration. ıviewassemblycache  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: önceden oluşturulmuş görünümleri içeren derlemelerin bir önbelleği. Bir değiştirme işlemi, hiçbir keşfi yapmadan, EF 'in hangi derlemelerin önceden oluşturulmuş görünümleri içerdiğini bilmesini sağlamak için kullanılır.  

**Anahtar**: kullanılmıyor; null olur  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System. Data. Entity. Infrastructure. pluralization. IPluralizationService

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: adları plurmak ve sıngumize etmek için EF tarafından kullanılan bir hizmet. Varsayılan olarak, Ingilizce plurselleştirme hizmeti kullanılır.  

**Anahtar**: kullanılmıyor; null olur  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System. Data. Entity. Infrastructure. yakalayıcısı. ıdbyakalayıcısı  

**Sunulan sürüm**: EF 6.0.0

**Döndürülen nesneler**: uygulama başladığında kaydedilmesi gereken her bir dinleyici. Bu nesnelerin GetServices çağrısı kullanılarak istendiğine ve herhangi bir bağımlılık Çözümleyicisi tarafından döndürülen tüm yakalayıcılar kaydettirildiğine unutmayın.

**Anahtar**: kullanılmıyor; null olacaktır.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System. Data. Entity. DbContext, ACTION < String\>, System. Data. Entity. Infrastructure. Yakation. DatabaseLogFormatter\>  

**Sunulan sürüm**: EF 6.0.0  

**Döndürülen nesne**: bağlam sırasında kullanılacak veritabanı günlük biçimlendirici öğesini oluşturmak için kullanılacak bir fabrika. Database. Log özelliği verilen bağlamda ayarlandı.  

**Anahtar**: kullanılmıyor; null olacaktır.  

## <a name="funcsystemdataentitydbcontext"></a>Func < System. Data. Entity. DbContext\>  

**Sunulan sürüm**: EF 6.1.0  

**Döndürülen nesne**: bağlam, erişilebilir parametresiz bir oluşturucuya sahip olmadığında geçişler için bağlam örnekleri oluşturmak için kullanılacak bir fabrika.  

**Anahtar**: bir fabrika için gereken türetilmiş DbContext türünün türü nesne.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System. Data. Entity. Core. Metadata. Edm. ımetadataannotationserializer\>  

**Sunulan sürüm**: EF 6.1.0  

**Döndürülen nesne**: Code First Migrations ' de kullanılmak üzere serileştirilmek ve hedeflenebilir özel açıklamaların serileştirilmesi için kullanılan bir fabrika.  

**Anahtar**: seri hale getirilen veya seri durumdan çıkarılan ek açıklamanın adı.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System. Data. Entity. Infrastructure. TransactionHandler\>  

**Sunulan sürüm**: EF 6.1.0  

**Döndürülen nesne**: işleme başarısızlıklarını işleme gibi durumlar için özel işlemenin uygulanabilmesi için, işlemler için işleyiciler oluşturmak üzere kullanılacak bir fabrika.  

**Anahtar**: sağlayıcının sabit adını ve isteğe bağlı olarak işlem işleyicisinin kullanılacağı sunucu adını Içeren bir Executionstratejik jkey nesnesi.  
