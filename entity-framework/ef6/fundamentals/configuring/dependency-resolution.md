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
# <a name="dependency-resolution"></a><span data-ttu-id="1ec36-102">Bağımlılık çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="1ec36-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="1ec36-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1ec36-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="1ec36-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="1ec36-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="1ec36-105">EF6 ile başlayarak Entity Framework, gereken hizmet uygulamalarını almak için genel amaçlı bir mekanizma içerir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="1ec36-106">Diğer bir deyişle, EF bazı arabirimlerin veya temel sınıfların bir örneğini kullandığında, arabirim veya temel sınıf için somut bir uygulama ister.</span><span class="sxs-lookup"><span data-stu-id="1ec36-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="1ec36-107">Bu, ıdbdependencyresolver arabirimi kullanılarak sağlanır:</span><span class="sxs-lookup"><span data-stu-id="1ec36-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="1ec36-108">GetService yöntemi genellikle EF tarafından çağrılır ve EF ya da uygulama tarafından verilen bir ıdbdependencyresolver uygulaması tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="1ec36-109">Çağrıldığında tür bağımsız değişkeni, istenen hizmetin arabirimi veya temel sınıf türüdür ve anahtar nesne null ya da istenen hizmet hakkında bağlamsal bilgiler sağlayan bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="1ec36-110">Aksi belirtilmediği sürece, döndürülen herhangi bir nesne, tek bir olarak kullanılabilmesi için iş parçacığı açısından güvenli olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="1ec36-111">Çoğu durumda, döndürülen nesne bir fabrikadır, ancak her kullanım için fabrikadan yeni bir örnek istendiğinden, fabrikadan döndürülen nesnenin iş parçacığı açısından güvenli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1ec36-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="1ec36-112">Bu makale, ıdbdependencyresolver 'ın nasıl uygulanacağı hakkında tam ayrıntı içermez, ancak bunun yerine, EF 'in GetService 'i çağırdığı ve bunların her biri için anahtar nesnesinin semantiğinin hizmet türleri (yani, arabirim ve temel sınıf türleri) için başvuru görevi görür aramalarda.</span><span class="sxs-lookup"><span data-stu-id="1ec36-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="1ec36-113">System. Data. Entity. ıdatabasebaşlatıcısı < TContext\></span><span class="sxs-lookup"><span data-stu-id="1ec36-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="1ec36-114">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-115">**Döndürülen nesne**: belirtilen bağlam türü için bir veritabanı başlatıcısı</span><span class="sxs-lookup"><span data-stu-id="1ec36-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="1ec36-116">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="1ec36-117">Func < System. Data. Entity. geçişler. Sql. MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="1ec36-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="1ec36-118">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="1ec36-119">**Döndürülen nesne**: geçiş için kullanılabilen ve veritabanı başlatıcılarla veritabanı oluşturma gibi bir veritabanının oluşturulmasına neden olan diğer eylemler için KULLANıLABILEN bir SQL Oluşturucu oluşturmaya yönelik bir fabrika.</span><span class="sxs-lookup"><span data-stu-id="1ec36-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="1ec36-120">**Anahtar**: SQL oluşturulacak veritabanının türünü belirten ADO.NET sağlayıcısı değişmez adını içeren bir dize.</span><span class="sxs-lookup"><span data-stu-id="1ec36-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="1ec36-121">Örneğin, "System. Data. SqlClient" anahtarı için SQL Server SQL Oluşturucu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1ec36-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-122">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="1ec36-123">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="1ec36-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="1ec36-124">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-125">**Döndürülen nesne**: belirli bir sağlayıcı sabit adı IÇIN kullanılacak EF sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="1ec36-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="1ec36-126">**Anahtar**: bir sağlayıcının gerektirdiği veritabanının türünü belirten ADO.NET sağlayıcısı değişmez adını içeren bir dize.</span><span class="sxs-lookup"><span data-stu-id="1ec36-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="1ec36-127">Örneğin, SQL Server sağlayıcı "System. Data. SqlClient" anahtarı için döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1ec36-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-128">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="1ec36-129">System. Data. Entity. Infrastructure. ıdbconnectionfactory</span><span class="sxs-lookup"><span data-stu-id="1ec36-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="1ec36-130">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-131">**Döndürülen nesne**: EF, kurala göre bir veritabanı bağlantısı oluşturduğunda kullanılacak bağlantı fabrikası.</span><span class="sxs-lookup"><span data-stu-id="1ec36-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="1ec36-132">Diğer bir deyişle, bir bağlantı veya bağlantı dizesinin EF 'e verilme ve App. config veya Web. config dosyasında hiçbir bağlantı dizesinin bulunamaması durumunda bu hizmet, kurala göre bir bağlantı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="1ec36-133">Bağlantı fabrikasının değiştirilmesi, EF 'in varsayılan olarak farklı türde bir veritabanı (örneğin, SQL Server Compact Edition) kullanmasına izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="1ec36-134">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-135">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="1ec36-136">System. Data. Entity. Infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="1ec36-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="1ec36-137">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-138">**Döndürülen nesne**: bir bağlantıdan sağlayıcı bildirim belirteci oluşturabilen bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="1ec36-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="1ec36-139">Bu hizmet genellikle iki şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-139">This service is typically used in two ways.</span></span> <span data-ttu-id="1ec36-140">İlk olarak, bir model oluştururken veritabanına bağlantı Code First önlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="1ec36-141">İkincisi, belirli bir veritabanı sürümü için bir model oluşturmak üzere Code First zorlamak için kullanılabilir (örneğin, bazen SQL Server 2008 kullanılıyorsa bile, bir modeli SQL Server 2005 için zorlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="1ec36-142">**Nesne ömrü**: tek--aynı nesne birden çok kez ve farklı iş parçacıkları tarafından aynı anda kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="1ec36-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="1ec36-143">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="1ec36-144">System. Data. Entity. Infrastructure. ıdbproviderfactoryservice</span><span class="sxs-lookup"><span data-stu-id="1ec36-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="1ec36-145">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-146">**Döndürülen nesne**: belirli bir bağlantıdan sağlayıcı fabrikası elde eden bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="1ec36-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="1ec36-147">.NET 4,5 ' de, sağlayıcıya genel olarak bağlantıdan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="1ec36-148">.NET 4 ' te bu hizmetin varsayılan uygulanması, eşleşen sağlayıcıyı bulmak için bazı buluşsal yöntemler kullanır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="1ec36-149">Bunlar başarısız olursa, uygun bir çözüm sağlamak için bu hizmetin yeni bir uygulama kaydı yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="1ec36-150">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="1ec36-151">Func < DbContext, System. Data. Entity. Infrastructure. ıdbmodelcachekey\></span><span class="sxs-lookup"><span data-stu-id="1ec36-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="1ec36-152">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-153">**Döndürülen nesne**: belirli bir bağlam için model önbellek anahtarı oluşturacak bir fabrika.</span><span class="sxs-lookup"><span data-stu-id="1ec36-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="1ec36-154">Varsayılan olarak, EF, sağlayıcı başına her DbContext türü için bir modeli önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="1ec36-155">Bu hizmetin farklı bir uygulanması, şema adı gibi diğer bilgileri önbellek anahtarına eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="1ec36-156">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="1ec36-157">System. Data. Entity. uzamsal. DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="1ec36-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="1ec36-158">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-159">**Döndürülen nesne**: Coğrafya ve geometri uzamsal türler IÇIN temel EF sağlayıcısına destek ekleyen bir EF uzamsal sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="1ec36-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="1ec36-160">**Anahtar**: dbsptialservices 'in iki şekilde istenir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="1ec36-161">İlk olarak, sağlayıcıya özgü uzamsal hizmetler, anahtar olarak bir DBProviderInfo nesnesi (Sabit adı ve bildirim belirteci içerir) kullanılarak istenir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="1ec36-162">İkinci olarak, DbSpatialServices hiçbir anahtar olmadan istenebilir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="1ec36-163">Bu, tek başına Dbcoğrafya veya DbGeometry türleri oluştururken kullanılan "küresel uzamsal sağlayıcıyı" çözümlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-164">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="1ec36-165">Func < System. Data. Entity. Infrastructure. ıdbexecutionstrateji\></span><span class="sxs-lookup"><span data-stu-id="1ec36-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="1ec36-166">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-167">**Döndürülen nesne**: bir sağlayıcının, sorgular ve komutlar veritabanına göre yürütüldüğünde yeniden denemeler veya diğer davranışları uygulamasına izin veren bir hizmet oluşturmak için bir fabrika.</span><span class="sxs-lookup"><span data-stu-id="1ec36-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="1ec36-168">Uygulama sağlanmazsa, EF komutları yürütür ve oluşturulan özel durumları yayacaktır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="1ec36-169">SQL Server için bu hizmet, SQL Azure gibi bulut tabanlı veritabanı sunucularında çalışırken yararlı olan bir yeniden deneme ilkesi sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="1ec36-170">**Anahtar**: sağlayıcı sabit adı ve isteğe bağlı olarak yürütme stratejisi kullanılacak bir sunucu adı Içeren bir Executionstratejik jkey nesnesi.</span><span class="sxs-lookup"><span data-stu-id="1ec36-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-171">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="1ec36-172">Func < DbConnection, String, System. Data. Entity. geçişler. history. Geçmişcontext\></span><span class="sxs-lookup"><span data-stu-id="1ec36-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="1ec36-173">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-174">**Döndürülen nesne**: bir sağlayıcının, IBir IBU bağlamın EŞLEMESINI, EF geçişi tarafından kullanılan `__MigrationHistory` tablosuna yapılandırmasını sağlayan bir fabrika.</span><span class="sxs-lookup"><span data-stu-id="1ec36-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="1ec36-175">Bu bir DbContext Code First ve tablo adı ve sütun eşleme belirtimleri gibi şeyleri değiştirmek için normal Fluent API kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ec36-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="1ec36-176">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-177">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="1ec36-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="1ec36-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="1ec36-179">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-180">**Döndürülen nesne**: belirli bir sağlayıcı sabit adı için kullanılacak ADO.NET sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="1ec36-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="1ec36-181">**Anahtar**: ADO.NET sağlayıcısı değişmez adını içeren bir dize</span><span class="sxs-lookup"><span data-stu-id="1ec36-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-182">Varsayılan uygulama normal ADO.NET sağlayıcısı kaydını kullandığından, bu hizmet genellikle doğrudan değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="1ec36-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="1ec36-183">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="1ec36-184">System. Data. Entity. Infrastructure. IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="1ec36-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="1ec36-185">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-186">**Döndürülen nesne**: belirli bir DbProviderFactory türü için sağlayıcı sabit adını belirlemede kullanılan bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="1ec36-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="1ec36-187">Bu hizmetin varsayılan uygulanması ADO.NET sağlayıcı kaydını kullanır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="1ec36-188">Bu, DbProviderFactory EF tarafından çözümlendiği için ADO.NET sağlayıcısı normal şekilde kaydedilmemişse, bu hizmetin çözümlenmesi için de gerekli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="1ec36-189">**Anahtar**: sabit adının gerekli olduğu DbProviderFactory örneği.</span><span class="sxs-lookup"><span data-stu-id="1ec36-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="1ec36-190">EF6 içindeki sağlayıcıyla ilgili hizmetler hakkında daha fazla bilgi için bkz. [EF6 Provider model](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ec36-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="1ec36-191">System. Data. Entity. Core. Mapping. ViewGeneration. ıviewassemblycache</span><span class="sxs-lookup"><span data-stu-id="1ec36-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="1ec36-192">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-193">**Döndürülen nesne**: önceden oluşturulmuş görünümleri içeren derlemelerin bir önbelleği.</span><span class="sxs-lookup"><span data-stu-id="1ec36-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="1ec36-194">Bir değiştirme işlemi, hiçbir keşfi yapmadan, EF 'in hangi derlemelerin önceden oluşturulmuş görünümleri içerdiğini bilmesini sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="1ec36-195">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="1ec36-196">System. Data. Entity. Infrastructure. pluralization. IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="1ec36-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="1ec36-197">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-198">**Döndürülen nesne**: adları plurmak ve sıngumize etmek için EF tarafından kullanılan bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="1ec36-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="1ec36-199">Varsayılan olarak, Ingilizce plurselleştirme hizmeti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="1ec36-200">**Anahtar**: kullanılmıyor; null olur</span><span class="sxs-lookup"><span data-stu-id="1ec36-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="1ec36-201">System. Data. Entity. Infrastructure. yakalayıcısı. ıdbyakalayıcısı</span><span class="sxs-lookup"><span data-stu-id="1ec36-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="1ec36-202">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="1ec36-203">**Döndürülen nesneler**: uygulama başladığında kaydedilmesi gereken her bir dinleyici.</span><span class="sxs-lookup"><span data-stu-id="1ec36-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="1ec36-204">Bu nesnelerin GetServices çağrısı kullanılarak istendiğine ve herhangi bir bağımlılık Çözümleyicisi tarafından döndürülen tüm yakalayıcılar kaydettirildiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1ec36-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="1ec36-205">**Anahtar**: kullanılmıyor; null olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="1ec36-206">Func < System. Data. Entity. DbContext, ACTION < String\>, System. Data. Entity. Infrastructure. Yakation. DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="1ec36-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="1ec36-207">**Sunulan sürüm**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="1ec36-208">**Döndürülen nesne**: bağlam sırasında kullanılacak veritabanı günlük biçimlendirici öğesini oluşturmak için kullanılacak bir fabrika. Database. Log özelliği verilen bağlamda ayarlandı.</span><span class="sxs-lookup"><span data-stu-id="1ec36-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="1ec36-209">**Anahtar**: kullanılmıyor; null olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1ec36-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="1ec36-210">Func < System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="1ec36-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="1ec36-211">**Sunulan sürüm**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="1ec36-212">**Döndürülen nesne**: bağlam, erişilebilir parametresiz bir oluşturucuya sahip olmadığında geçişler için bağlam örnekleri oluşturmak için kullanılacak bir fabrika.</span><span class="sxs-lookup"><span data-stu-id="1ec36-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="1ec36-213">**Anahtar**: bir fabrika için gereken türetilmiş DbContext türünün türü nesne.</span><span class="sxs-lookup"><span data-stu-id="1ec36-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="1ec36-214">Func < System. Data. Entity. Core. Metadata. Edm. ımetadataannotationserializer\></span><span class="sxs-lookup"><span data-stu-id="1ec36-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="1ec36-215">**Sunulan sürüm**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="1ec36-216">**Döndürülen nesne**: Code First Migrations ' de kullanılmak üzere serileştirilmek ve hedeflenebilir özel açıklamaların serileştirilmesi için kullanılan bir fabrika.</span><span class="sxs-lookup"><span data-stu-id="1ec36-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="1ec36-217">**Anahtar**: seri hale getirilen veya seri durumdan çıkarılan ek açıklamanın adı.</span><span class="sxs-lookup"><span data-stu-id="1ec36-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="1ec36-218">Func < System. Data. Entity. Infrastructure. TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="1ec36-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="1ec36-219">**Sunulan sürüm**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="1ec36-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="1ec36-220">**Döndürülen nesne**: işleme başarısızlıklarını işleme gibi durumlar için özel işlemenin uygulanabilmesi için, işlemler için işleyiciler oluşturmak üzere kullanılacak bir fabrika.</span><span class="sxs-lookup"><span data-stu-id="1ec36-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="1ec36-221">**Anahtar**: sağlayıcının sabit adını ve isteğe bağlı olarak işlem işleyicisinin kullanılacağı sunucu adını Içeren bir Executionstratejik jkey nesnesi.</span><span class="sxs-lookup"><span data-stu-id="1ec36-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
