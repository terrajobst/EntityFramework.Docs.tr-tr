---
title: Bağımlılık çözümlemesi - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 45681bb0cedecd502b1968b90b7f682d3257dd23
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998169"
---
# <a name="dependency-resolution"></a><span data-ttu-id="5a80a-102">Bağımlılık çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="5a80a-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="5a80a-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="5a80a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="5a80a-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="5a80a-105">EF6 ile başlayarak, Entity Framework gerekli hizmetleri, uygulamaları alma genel amaçlı bir mekanizma içerir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="5a80a-106">Diğer bir deyişle, EF bazı arabirimleri ya da taban sınıfların örneği kullandığında bunu kullanmak için somut bir uygulama arabirim veya temel sınıfının sorar.</span><span class="sxs-lookup"><span data-stu-id="5a80a-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="5a80a-107">Bu, IDbDependencyResolver arabirimi kullanılmasıyla elde edilir:</span><span class="sxs-lookup"><span data-stu-id="5a80a-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="5a80a-108">GetService yöntemi genellikle EF tarafından çağrılır ve EF veya uygulama tarafından sağlanan IDbDependencyResolver uygulaması tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="5a80a-109">Çağrıldığında, tür bağımsız değişkeni istenen hizmet arabirimi veya temel sınıf türünde olduğundan ve anahtar nesnesi null ya da istenen hizmeti hakkında bağlamsal bilgiler sağlayan bir nesne değil.</span><span class="sxs-lookup"><span data-stu-id="5a80a-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="5a80a-110">Bu makalede IDbDependencyResolver uygulamak tam birleştiremiyorsa içermiyor, ancak bunun yerine, EF GetService anahtar nesnesi semantiği bunların her biri için çağırır ve hizmet türleri (diğer bir deyişle, arabirimi ve temel sınıf türleri) için bir başvuru olarak davranır çağırır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="5a80a-111">Ek hizmetler eklendikçe bu belgeyi güncel tutulacak.</span><span class="sxs-lookup"><span data-stu-id="5a80a-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="5a80a-112">Çözümlenen hizmetler</span><span class="sxs-lookup"><span data-stu-id="5a80a-112">Services resolved</span></span>  

<span data-ttu-id="5a80a-113">Tekil olarak kullanılabildiğinden döndürülen herhangi bir nesne iş parçacığı açısından güvenli olmalıdır aksi belirtilmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="5a80a-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="5a80a-114">Çoğu durumda nesne bir factory, bu durumda, döndürülen iş parçacığı açısından güvenli Fabrika olmalıdır ancak fabrikadan döndürülen nesne, her kullanım için Fabrika öğesinden yeni bir örneğini istediğinden itibaren geçen iş parçacığı açısından güvenli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5a80a-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="5a80a-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="5a80a-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="5a80a-116">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-117">**Döndürülen nesne**: Belirtilen içerik türü için bir veritabanı Başlatıcısı</span><span class="sxs-lookup"><span data-stu-id="5a80a-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="5a80a-118">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="5a80a-119">FUNC < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="5a80a-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="5a80a-120">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="5a80a-121">**Döndürülen nesne**: geçişler ve veritabanı başlatıcılar ile veritabanı oluşturma gibi oluşturulması için bir veritabanı neden diğer eylemler için kullanılabilir bir SQL oluşturucuyu oluşturmak için bir üreteci.</span><span class="sxs-lookup"><span data-stu-id="5a80a-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="5a80a-122">**Anahtar**: SQL oluşturulacak veritabanı türünü belirleyen ADO.NET sağlayıcısı değişmez adını içeren bir dize.</span><span class="sxs-lookup"><span data-stu-id="5a80a-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="5a80a-123">Örneğin, SQL Server SQL Oluşturucu "System.Data.SqlClient" anahtarı için döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5a80a-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-124">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="5a80a-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="5a80a-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="5a80a-126">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-127">**Döndürülen nesne**: verilen sağlayıcı değişmez adı için kullanılacak EF sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="5a80a-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="5a80a-128">**Anahtar**: bir sağlayıcı gereklidir veritabanı türünü belirleyen ADO.NET sağlayıcısı değişmez adını içeren bir dize.</span><span class="sxs-lookup"><span data-stu-id="5a80a-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="5a80a-129">Örneğin, SQL Server sağlayıcısı için "System.Data.SqlClient" anahtar döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5a80a-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-130">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="5a80a-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="5a80a-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="5a80a-132">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-133">**Döndürülen nesne**: EF veritabanı bağlantısı oluşturduğunda, kural olarak kullanılacak bağlantı üreteci.</span><span class="sxs-lookup"><span data-stu-id="5a80a-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="5a80a-134">Diğer bir deyişle, hiçbir bağlantı veya bağlantı dizesi için EF verilir ve bağlantı dizesi app.config veya web.config içinde bulunabilir, sonra bu hizmet bir bağlantı oluşturmak için kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="5a80a-135">Bağlantı üreteci değiştirme EF farklı türde bir veritabanı (örneğin, SQL Server Compact Edition) kullanmak varsayılan olarak izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a80a-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="5a80a-136">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-137">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="5a80a-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="5a80a-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="5a80a-139">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-140">**Döndürülen nesne**: bağlantı sağlayıcısı bildirimi belirteci oluşturan bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="5a80a-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="5a80a-141">Bu hizmet, genellikle iki şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-141">This service is typically used in two ways.</span></span> <span data-ttu-id="5a80a-142">İlk olarak, Code First modeli oluştururken veritabanına bağlanırken önlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="5a80a-143">İkinci olarak, SQL Server 2008 bazen kullanıldığında bile bir modeli SQL Server 2005'te zorlamak için belirli bir veritabanı sürümü için--örneğin, bir model oluşturmak için Code First zorlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="5a80a-144">**Nesne ömrü**: tekil--aynı nesneye kullanılan birden çok kez ve aynı anda farklı iş parçacıkları tarafından olabilir</span><span class="sxs-lookup"><span data-stu-id="5a80a-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="5a80a-145">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="5a80a-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="5a80a-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="5a80a-147">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-148">**Döndürülen nesne**: bir hizmet sağlayıcı üreteci belirli bir bağlantı edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5a80a-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="5a80a-149">.NET 4.5 üzerinde sağlayıcı bağlantısı genel olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="5a80a-150">.NET 4'te bu hizmetin varsayılan uygulama eşleşen sağlayıcı bulunamadı bazı buluşsal yöntemler kullanır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="5a80a-151">Bunlar daha sonra bu hizmetin yeni bir uygulama başarısız olursa, uygun bir çözüm sağlamak için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="5a80a-152">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="5a80a-153">FUNC < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="5a80a-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="5a80a-154">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-155">**Döndürülen nesne**: belirli bir bağlam için bir model önbellek anahtarını oluşturan bir üreteci.</span><span class="sxs-lookup"><span data-stu-id="5a80a-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="5a80a-156">Varsayılan olarak EF DbContext tür sağlayıcı başına başına bir model önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="5a80a-157">Bu hizmetin farklı bir uygulama için önbellek anahtarını şema adı gibi diğer bilgilerini eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="5a80a-158">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="5a80a-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="5a80a-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="5a80a-160">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-161">**Döndürülen nesne**: temel EF sağlayıcı coğrafya ve geometri uzamsal türler için destek ekleyen bir EF uzamsal sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="5a80a-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="5a80a-162">**Anahtar**: DbSptialServices sorular için iki yolla.</span><span class="sxs-lookup"><span data-stu-id="5a80a-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="5a80a-163">İlk olarak, sağlayıcıya özgü uzamsal hizmetler DbProviderInfo nesnesi kullanılarak istenir (değişmez değer içeren ad ve bildirim belirteci) anahtarı olarak.</span><span class="sxs-lookup"><span data-stu-id="5a80a-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="5a80a-164">İkinci olarak, DbSpatialServices için hiçbir anahtar ile onaylayanlara sorulabilir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="5a80a-165">Bu, "tek başına DbGeography veya DbGeometry türleri oluştururken kullanılan genel uzamsal sağlayıcı" çözmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-166">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="5a80a-167">FUNC < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="5a80a-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="5a80a-168">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-169">**Döndürülen nesne**: veritabanına karşı sorgulara ve komutlara yürütüldüğünde, yeniden deneme veya diğer davranışı uygulamak bir sağlayıcı sağlayan bir hizmet oluşturmak için bir üreteci.</span><span class="sxs-lookup"><span data-stu-id="5a80a-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="5a80a-170">Uygulaması sağlanırsa, ardından EF yalnızca şu komutları çalıştırın ve oluşturulan tüm özel durumları yayar.</span><span class="sxs-lookup"><span data-stu-id="5a80a-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="5a80a-171">SQL Server için SQL Azure gibi bulut tabanlı veritabanı sunucularına karşı çalışırken kullanışlı olan bir yeniden deneme ilkesi sağlamak için bu hizmeti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="5a80a-172">**Anahtar**: sağlayıcının değişmez adı ve isteğe bağlı yürütme stratejisi kullanılacak bir sunucu adı içeren bir ExecutionStrategyKey nesne.</span><span class="sxs-lookup"><span data-stu-id="5a80a-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-173">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="5a80a-174">FUNC < DbConnection, dize, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="5a80a-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="5a80a-175">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-176">**Döndürülen nesne**: HistoryContext için eşlemeyi yapılandırmak bir sağlayıcı izin veren bir Fabrika `__MigrationHistory` EF geçişleri tarafından kullanılan tablo.</span><span class="sxs-lookup"><span data-stu-id="5a80a-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="5a80a-177">HistoryContext kod ilk DbContext olduğundan ve tablo ve sütun eşleme belirtimlerini adı gibi şeyleri değiştirmek için normal fluent API'si kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="5a80a-178">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-179">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="5a80a-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="5a80a-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="5a80a-181">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-182">**Döndürülen nesne**: ADO.NET sağlayıcısı belirtilen sağlayıcı değişmez adı için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="5a80a-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="5a80a-183">**Anahtar**: ADO.NET sağlayıcısı değişmez adını içeren bir dize</span><span class="sxs-lookup"><span data-stu-id="5a80a-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-184">Varsayılan uygulama normal ADO.NET Sağlayıcısı kaydı kullandığından bu hizmet genellikle doğrudan değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="5a80a-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="5a80a-185">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="5a80a-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="5a80a-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="5a80a-187">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-188">**Döndürülen nesne**: DbProviderFactory belirli bir tür bir sağlayıcı değişmez adını belirlemek için kullanılan bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="5a80a-189">Bu hizmet, varsayılan uygulama, ADO.NET Sağlayıcısı kaydı kullanır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="5a80a-190">Bu DbProviderFactory tarafından EF çözümlendiğinden ADO.NET sağlayıcısı normal bir şekilde kayıtlı değil, daha sonra Ayrıca bu hizmet çözmek gerekli anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="5a80a-191">**Anahtar**: DbProviderFactory örneği için bir değişmez adı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5a80a-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="5a80a-192">EF6 Hizmetleri Sağlayıcısı ile ilgili daha fazla ayrıntı görmek için [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5a80a-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="5a80a-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="5a80a-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="5a80a-194">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-195">**Döndürülen nesne**: önceden üretilmiş görünümleri içeren derlemeler, bir önbellek.</span><span class="sxs-lookup"><span data-stu-id="5a80a-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="5a80a-196">Bir değiştirme, genellikle tüm keşfi olmadan hangi derlemelerin önceden üretilmiş görünümleri içeren bilmeniz EF izin vermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="5a80a-197">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="5a80a-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="5a80a-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="5a80a-199">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-200">**Döndürülen nesne**: pluralize ve adları singularize EF tarafından kullanılan bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="5a80a-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="5a80a-201">Varsayılan olarak İngilizce çoğullaştırma hizmeti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="5a80a-202">**Anahtar**: kullanılan; null</span><span class="sxs-lookup"><span data-stu-id="5a80a-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="5a80a-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="5a80a-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="5a80a-204">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="5a80a-205">**Döndürülen nesne**: uygulama başlatıldığında, kaydedilmesi gereken tüm dinleyicileri.</span><span class="sxs-lookup"><span data-stu-id="5a80a-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="5a80a-206">Herhangi bir bağımlılık çözümleyici tarafından döndürülen tüm dinleyicileri GetServices kullanarak bu nesneleri istenen ve Not kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="5a80a-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="5a80a-207">**Anahtar**: kullanılan; null olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="5a80a-208">FUNC < System.Data.Entity.DbContext, eylem < dize\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="5a80a-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="5a80a-209">**Kullanıma sunulan sürümü**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="5a80a-210">**Döndürülen nesne**: olacak veritabanı günlük biçimlendirici oluşturmak için kullanılan Fabrika kullanılabilir bağlamı. Belirtilen bağlama Database.Log özelliği ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="5a80a-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="5a80a-211">**Anahtar**: kullanılan; null olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5a80a-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="5a80a-212">FUNC < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="5a80a-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="5a80a-213">**Kullanıma sunulan sürümü**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="5a80a-214">**Döndürülen nesne**: bağlam erişilebilir parametresiz bir oluşturucu yoksa, geçişler için bağlam örnekleri oluşturmak için kullanılacak bir Üreteç.</span><span class="sxs-lookup"><span data-stu-id="5a80a-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="5a80a-215">**Anahtar**: bir Fabrika gereklidir türetilmiş DbContext tür için tür nesnesi.</span><span class="sxs-lookup"><span data-stu-id="5a80a-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="5a80a-216">FUNC < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="5a80a-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="5a80a-217">**Kullanıma sunulan sürümü**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="5a80a-218">**Döndürülen nesne**: serileştiricileri seri hale getirme türü kesin belirlenmiş özel ek açıklamaları, sıralanabilir ve XML olarak kullanılmak üzere Code First Migrations desterilized şekilde oluşturmak için kullanılan bir Üreteç.</span><span class="sxs-lookup"><span data-stu-id="5a80a-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="5a80a-219">**Anahtar**: yüklenmekte olan ek açıklama adı serileştirilecek veya serisi.</span><span class="sxs-lookup"><span data-stu-id="5a80a-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="5a80a-220">FUNC < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="5a80a-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="5a80a-221">**Kullanıma sunulan sürümü**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="5a80a-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="5a80a-222">**Döndürülen nesne**: işleyicileri işlemler için özel işlem işleme hataları işleme gibi durumlar için uygulanabilir olacak şekilde oluşturmak için kullanılan bir Üreteç.</span><span class="sxs-lookup"><span data-stu-id="5a80a-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="5a80a-223">**Anahtar**: sağlayıcının değişmez adı ve isteğe bağlı olarak hareket işleyicisi kullanılacak bir sunucu adı içeren bir ExecutionStrategyKey nesne.</span><span class="sxs-lookup"><span data-stu-id="5a80a-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
