---
title: EF Core 1,0 ' deki yenilikler-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655852"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="a068d-102">EF Core 1,0 ' de bulunan özellikler</span><span class="sxs-lookup"><span data-stu-id="a068d-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="a068d-103">Platformlar</span><span class="sxs-lookup"><span data-stu-id="a068d-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="a068d-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="a068d-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="a068d-105">Konsol, WPF, WinForms, ASP.NET 4 vb. içerir.</span><span class="sxs-lookup"><span data-stu-id="a068d-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="a068d-106">.NET Standard 1,3</span><span class="sxs-lookup"><span data-stu-id="a068d-106">.NET Standard 1.3</span></span>

<span data-ttu-id="a068d-107">Hem .NET Framework hem de .NET Core 'u Windows, OSX ve Linux üzerinde hedefleme ASP.NET Core dahil.</span><span class="sxs-lookup"><span data-stu-id="a068d-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="a068d-108">Modelleme</span><span class="sxs-lookup"><span data-stu-id="a068d-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="a068d-109">Temel modelleme</span><span class="sxs-lookup"><span data-stu-id="a068d-109">Basic modelling</span></span>

<span data-ttu-id="a068d-110">Ortak skaler türlerin (`int`, `string`, vb.) Get/Set özellikleriyle POCO varlıklarını temel alır.</span><span class="sxs-lookup"><span data-stu-id="a068d-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="a068d-111">İlişkiler ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="a068d-111">Relationships and navigation properties</span></span>

<span data-ttu-id="a068d-112">Bire çok ve bire sıfır veya-bir ilişki yabancı anahtara göre modelde belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="a068d-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="a068d-113">Basit koleksiyon veya başvuru türlerinin gezinti özellikleri, bu ilişkilerle ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a068d-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="a068d-114">Yerleşik kurallar</span><span class="sxs-lookup"><span data-stu-id="a068d-114">Built-in conventions</span></span>

<span data-ttu-id="a068d-115">Bu, varlık sınıflarının şekline göre bir başlangıç modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a068d-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="a068d-116">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="a068d-116">Fluent API</span></span>

<span data-ttu-id="a068d-117">Kural tarafından bulunan modeli daha fazla yapılandırmak için, bağlamdaki `OnModelCreating` yöntemi geçersiz kılmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="a068d-118">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a068d-118">Data annotations</span></span>

<span data-ttu-id="a068d-119">, Varlık sınıflarınıza/özelliklerine eklenebilen özniteliklerdir ve EF modelini etkiler.</span><span class="sxs-lookup"><span data-stu-id="a068d-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="a068d-120">Örneğin, `[Required]` eklemek EF 'in bir özelliğin gerekli olduğunu bilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="a068d-121">İlişkisel tablo eşleme</span><span class="sxs-lookup"><span data-stu-id="a068d-121">Relational Table mapping</span></span>

<span data-ttu-id="a068d-122">Varlıkların tablolarla/sütunlarla eşleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="a068d-123">Anahtar değeri oluşturma</span><span class="sxs-lookup"><span data-stu-id="a068d-123">Key value generation</span></span>

<span data-ttu-id="a068d-124">İstemci tarafı oluşturma ve veritabanı oluşturma dahil.</span><span class="sxs-lookup"><span data-stu-id="a068d-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="a068d-125">Veritabanı tarafından oluşturulan değerler</span><span class="sxs-lookup"><span data-stu-id="a068d-125">Database generated values</span></span>

<span data-ttu-id="a068d-126">Ekleme (varsayılan değerler) veya Update (hesaplanan sütunlar) üzerinde veritabanı tarafından oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a068d-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="a068d-127">SQL Server diziler</span><span class="sxs-lookup"><span data-stu-id="a068d-127">Sequences in SQL Server</span></span>

<span data-ttu-id="a068d-128">Model içinde sıralama nesnelerinin tanımlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="a068d-129">Benzersiz kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="a068d-129">Unique constraints</span></span>

<span data-ttu-id="a068d-130">Alternatif anahtarların tanımına ve bu anahtarı hedefleyen ilişkiler tanımlamasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="a068d-131">Dizinlerde</span><span class="sxs-lookup"><span data-stu-id="a068d-131">Indexes</span></span>

<span data-ttu-id="a068d-132">Modeldeki dizinlerin tanımlanması veritabanındaki dizinleri otomatik olarak tanıtır.</span><span class="sxs-lookup"><span data-stu-id="a068d-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="a068d-133">Benzersiz dizinler de desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a068d-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="a068d-134">Gölge durumu özellikleri</span><span class="sxs-lookup"><span data-stu-id="a068d-134">Shadow state properties</span></span>

<span data-ttu-id="a068d-135">Özelliklerin bildirilmemiş ve .NET sınıfında depolanmayan, ancak EF Core tarafından izlenip güncelleştirilebilecek modelde tanımlanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a068d-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="a068d-136">Genellikle yabancı anahtar özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a068d-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="a068d-137">Tablo başına devralma düzeni</span><span class="sxs-lookup"><span data-stu-id="a068d-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="a068d-138">Bir devralma hiyerarşisindeki varlıkların veritabanındaki belirli bir kayıt için varlık türünü belirlemek üzere bir Ayrıştırıcı sütunu kullanılarak tek bir tabloya kaydedilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a068d-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="a068d-139">Model doğrulaması</span><span class="sxs-lookup"><span data-stu-id="a068d-139">Model validation</span></span>

<span data-ttu-id="a068d-140">Modelde geçersiz desenler algılar ve yararlı hata iletileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="a068d-141">Change tracking</span><span class="sxs-lookup"><span data-stu-id="a068d-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="a068d-142">Anlık görüntü değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="a068d-142">Snapshot change tracking</span></span>

<span data-ttu-id="a068d-143">Geçerli durum özgün durumun bir kopyasına (anlık görüntü) göre karşılaştırılırken varlıklarda yapılan değişikliklerin otomatik olarak algılanabilmesi için izin verir.</span><span class="sxs-lookup"><span data-stu-id="a068d-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="a068d-144">Bildirim değişikliği izleme</span><span class="sxs-lookup"><span data-stu-id="a068d-144">Notification change tracking</span></span>

<span data-ttu-id="a068d-145">Özellik değerleri değiştirildiğinde varlıklarınızın değişiklik izleyicide bilgilendirmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="a068d-146">İzlenen duruma erişme</span><span class="sxs-lookup"><span data-stu-id="a068d-146">Accessing tracked state</span></span>

<span data-ttu-id="a068d-147">`DbContext.Entry` ve `DbContext.ChangeTracker`aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="a068d-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="a068d-148">Ayrılmış varlıkları/grafikleri iliştirme</span><span class="sxs-lookup"><span data-stu-id="a068d-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="a068d-149">Yeni `DbContext.AttachGraph` API 'SI, yeni/değiştirilmiş varlıkların kaydedileceği varlıkları bir bağlama yeniden iliştirmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a068d-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="a068d-150">Verileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="a068d-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="a068d-151">Temel kaydetme işlevselliği</span><span class="sxs-lookup"><span data-stu-id="a068d-151">Basic save functionality</span></span>

<span data-ttu-id="a068d-152">Varlık örneklerindeki değişikliklerin veritabanına kalıcı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="a068d-153">İyimser Eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="a068d-153">Optimistic Concurrency</span></span>

<span data-ttu-id="a068d-154">Verilerin veritabanından getirilmesi bu yana başka bir kullanıcı tarafından yapılan değişikliklerin üzerine yazılmasına karşı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="a068d-155">Zaman uyumsuz SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a068d-155">Async SaveChanges</span></span>

<span data-ttu-id="a068d-156">, Veritabanı `SaveChanges`verilen komutları işlerken diğer istekleri işlemek için geçerli iş parçacığını serbest bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a068d-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="a068d-157">Veritabanı Işlemleri</span><span class="sxs-lookup"><span data-stu-id="a068d-157">Database Transactions</span></span>

<span data-ttu-id="a068d-158">`SaveChanges` her zaman atomik olduğu anlamına gelir (yani tamamen başarılı olur ya da veritabanında değişiklik yapılmaz).</span><span class="sxs-lookup"><span data-stu-id="a068d-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="a068d-159">Ayrıca bağlam örnekleri arasında işleme işlemlerine izin veren işlem ile ilgili API 'Ler vardır.</span><span class="sxs-lookup"><span data-stu-id="a068d-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="a068d-160">İlişkisel: deyimlerin toplu işi</span><span class="sxs-lookup"><span data-stu-id="a068d-160">Relational: Batching of statements</span></span>

<span data-ttu-id="a068d-161">Birden fazla ekleme/GÜNCELLEŞTIRME/SILME komutunu veritabanına tek bir gidiş dönüş halinde toplu olarak oluşturarak daha iyi performans sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="a068d-162">Sorgu</span><span class="sxs-lookup"><span data-stu-id="a068d-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="a068d-163">Temel LINQ desteği</span><span class="sxs-lookup"><span data-stu-id="a068d-163">Basic LINQ support</span></span>

<span data-ttu-id="a068d-164">Veritabanından veri almak için LINQ kullanma özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="a068d-165">Karma istemci/sunucu değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="a068d-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="a068d-166">Sorguların veritabanında değerlendirilemeyen mantığı içermesini sağlar ve bu nedenle veriler belleğe alındıktan sonra değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a068d-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="a068d-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="a068d-167">NoTracking</span></span>

<span data-ttu-id="a068d-168">Bağlam, varlık örneklerine yapılan değişiklikleri izlemeye gerek olmadığında daha hızlı sorgu yürütmeye olanak sağlar (sonuçlar salt okunurdur, bu faydalıdır).</span><span class="sxs-lookup"><span data-stu-id="a068d-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="a068d-169">Ekip yükleme</span><span class="sxs-lookup"><span data-stu-id="a068d-169">Eager loading</span></span>

<span data-ttu-id="a068d-170">, ' İ sorgularken de getirilmesi gereken ilgili verileri belirlemek için `Include` ve `ThenInclude` yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="a068d-171">Zaman uyumsuz sorgu</span><span class="sxs-lookup"><span data-stu-id="a068d-171">Async query</span></span>

<span data-ttu-id="a068d-172">, Veritabanı sorguyu işlerken diğer istekleri işlemek için geçerli iş parçacığını (ve ilişkili kaynakları) serbest bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a068d-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="a068d-173">Ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="a068d-173">Raw SQL queries</span></span>

<span data-ttu-id="a068d-174">Veri getirmek için ham SQL sorguları kullanmak üzere `DbSet.FromSql` yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a068d-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="a068d-175">Bu sorgular, LINQ kullanılarak da oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="a068d-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="a068d-176">Veritabanı şeması yönetimi</span><span class="sxs-lookup"><span data-stu-id="a068d-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="a068d-177">Veritabanı oluşturma/silme API 'Leri</span><span class="sxs-lookup"><span data-stu-id="a068d-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="a068d-178">Genellikle, geçişleri kullanmadan veritabanını hızlı bir şekilde oluşturmak/silmek istediğiniz test için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a068d-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="a068d-179">İlişkisel veritabanı geçişleri</span><span class="sxs-lookup"><span data-stu-id="a068d-179">Relational database migrations</span></span>

<span data-ttu-id="a068d-180">İlişkisel bir veritabanı şemasının, modelinizde değişiklik yaptığı fazla mesaiyi geliştirmeye izin verin.</span><span class="sxs-lookup"><span data-stu-id="a068d-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="a068d-181">Veritabanından tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="a068d-181">Reverse engineer from database</span></span>

<span data-ttu-id="a068d-182">Var olan bir ilişkisel veritabanı şemasını temel alan bir EF modeli yapı iskelesi.</span><span class="sxs-lookup"><span data-stu-id="a068d-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="a068d-183">Veritabanı sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="a068d-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="a068d-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a068d-184">SQL Server</span></span>

<span data-ttu-id="a068d-185">Microsoft SQL Server 2008 ve sonraki sürümleri bağlanır.</span><span class="sxs-lookup"><span data-stu-id="a068d-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="a068d-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="a068d-186">SQLite</span></span>

<span data-ttu-id="a068d-187">SQLite 3 veritabanına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="a068d-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="a068d-188">Bellek içi</span><span class="sxs-lookup"><span data-stu-id="a068d-188">In-Memory</span></span>

<span data-ttu-id="a068d-189">, Gerçek bir veritabanına bağlanmadan test etmeyi kolayca etkinleştirmek üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a068d-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="a068d-190">3\. taraf sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="a068d-190">3rd party providers</span></span>

<span data-ttu-id="a068d-191">Diğer veritabanı motorları için çeşitli sağlayıcılar mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="a068d-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="a068d-192">Tüm liste için bkz. [veritabanı sağlayıcıları](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="a068d-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
