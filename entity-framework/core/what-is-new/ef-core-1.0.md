---
title: EF Core 1.0'daki yenilikler - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417526"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="78c2e-102">EF Core 1.0'da yer alan özellikler</span><span class="sxs-lookup"><span data-stu-id="78c2e-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="78c2e-103">Platformlar</span><span class="sxs-lookup"><span data-stu-id="78c2e-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="78c2e-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="78c2e-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="78c2e-105">Konsol, WPF, WinForms, ASP.NET 4, vb içerir</span><span class="sxs-lookup"><span data-stu-id="78c2e-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="78c2e-106">.NET Standart 1.3</span><span class="sxs-lookup"><span data-stu-id="78c2e-106">.NET Standard 1.3</span></span>

<span data-ttu-id="78c2e-107">Windows, OSX ve Linux'ta .NET Framework ve .NET Core'u hedefleyen ASP.NET Core dahil.</span><span class="sxs-lookup"><span data-stu-id="78c2e-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="78c2e-108">Modelleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="78c2e-109">Temel modelleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-109">Basic modelling</span></span>

<span data-ttu-id="78c2e-110">Ortak skaler türlerinin (,`int`vb.) `string`get/set özelliklerine sahip POCO varlıklarına dayanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="78c2e-111">İlişkiler ve gezinme özellikleri</span><span class="sxs-lookup"><span data-stu-id="78c2e-111">Relationships and navigation properties</span></span>

<span data-ttu-id="78c2e-112">Modelde yabancı bir anahtara dayalı bire bir ve bire sıfır veya bir ilişkileri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="78c2e-113">Basit toplama veya başvuru türlerinin gezinme özellikleri bu ilişkilerle ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="78c2e-114">Yerleşik sözleşmeler</span><span class="sxs-lookup"><span data-stu-id="78c2e-114">Built-in conventions</span></span>

<span data-ttu-id="78c2e-115">Bunlar, varlık sınıflarının şekline dayalı bir başlangıç modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="78c2e-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="78c2e-116">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="78c2e-116">Fluent API</span></span>

<span data-ttu-id="78c2e-117">Kuralı tarafından keşfedilen `OnModelCreating` modeli daha da yapılandırmak için bağlamınızdaki yöntemi geçersiz kılmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="78c2e-118">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="78c2e-118">Data annotations</span></span>

<span data-ttu-id="78c2e-119">Varlık sınıflarınıza/özelliklerinize eklenebilecek ve EF modelini etkileyecek özniteliklerdir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="78c2e-120">Örneğin, ekleme, `[Required]` EF'ye bir özelliğin gerekli olduğunu bilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="78c2e-121">İlişkisel Tablo eşleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-121">Relational Table mapping</span></span>

<span data-ttu-id="78c2e-122">Varlıkların tablolara/sütunlara eşlenemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="78c2e-123">Anahtar değer oluşturma</span><span class="sxs-lookup"><span data-stu-id="78c2e-123">Key value generation</span></span>

<span data-ttu-id="78c2e-124">İstemci tarafı oluşturma ve veritabanı oluşturma dahil.</span><span class="sxs-lookup"><span data-stu-id="78c2e-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="78c2e-125">Veritabanı oluşturulan değerler</span><span class="sxs-lookup"><span data-stu-id="78c2e-125">Database generated values</span></span>

<span data-ttu-id="78c2e-126">Ekler (varsayılan değerler) veya güncelleştirme (hesaplanmış sütunlar) veritabanı tarafından oluşturulacak değerlerin sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="78c2e-127">SQL Server'da Diziler</span><span class="sxs-lookup"><span data-stu-id="78c2e-127">Sequences in SQL Server</span></span>

<span data-ttu-id="78c2e-128">Dizi nesnelerinin modelde tanımlanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="78c2e-129">Benzersiz kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="78c2e-129">Unique constraints</span></span>

<span data-ttu-id="78c2e-130">Alternatif anahtarların tanımına ve bu anahtarı hedefleyen ilişkileri tanımlama yeteneğine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="78c2e-131">Dizinler</span><span class="sxs-lookup"><span data-stu-id="78c2e-131">Indexes</span></span>

<span data-ttu-id="78c2e-132">Modeldeki dizinleri tanımlamak veritabanında dizinleri otomatik olarak tanıtıyor.</span><span class="sxs-lookup"><span data-stu-id="78c2e-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="78c2e-133">Benzersiz dizinler de desteklenir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="78c2e-134">Gölge durumu özellikleri</span><span class="sxs-lookup"><span data-stu-id="78c2e-134">Shadow state properties</span></span>

<span data-ttu-id="78c2e-135">Modelde belirtilmeyen ve .NET sınıfında depolanmayan ancak EF Core tarafından izlenebilir ve güncelleştirilebilen özelliklerin tanımlanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="78c2e-136">Bunları nesnede teşhir ederken yabancı anahtar özellikleri için yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="78c2e-137">Tablo-Hiyerarşi Başına kalıtım deseni</span><span class="sxs-lookup"><span data-stu-id="78c2e-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="78c2e-138">Devralma hiyerarşisindeki varlıkların, veritabanında belirli bir kayıt için varlık türünü tanımlamak için ayırıcı sütun kullanarak tek bir tabloya kaydedilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="78c2e-139">Model doğrulaması</span><span class="sxs-lookup"><span data-stu-id="78c2e-139">Model validation</span></span>

<span data-ttu-id="78c2e-140">Modeldeki geçersiz desenleri algılar ve yararlı hata iletileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="78c2e-141">Değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="78c2e-142">Anlık görüntü değiştirme izleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-142">Snapshot change tracking</span></span>

<span data-ttu-id="78c2e-143">Geçerli durumu özgün durumu kopyalayan (anlık görüntü) ile karşılaştırarak varlıklardaki değişikliklerin otomatik olarak algılanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="78c2e-144">Bildirim değişikliği izleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-144">Notification change tracking</span></span>

<span data-ttu-id="78c2e-145">Özellik değerleri değiştirildiğinde, varlıklarınızın değişiklik izleyicisini bildirmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="78c2e-146">İzlenen duruma erişme</span><span class="sxs-lookup"><span data-stu-id="78c2e-146">Accessing tracked state</span></span>

<span data-ttu-id="78c2e-147">Via `DbContext.Entry` `DbContext.ChangeTracker`ve .</span><span class="sxs-lookup"><span data-stu-id="78c2e-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="78c2e-148">Ayrılmış varlıkları/grafikleri ekleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="78c2e-149">Yeni `DbContext.AttachGraph` API, yeni/değiştirilmiş varlıkları kaydetmek için varlıkları bir içeriğe yeniden eklemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="78c2e-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="78c2e-150">Verileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="78c2e-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="78c2e-151">Temel kaydetme işlevi</span><span class="sxs-lookup"><span data-stu-id="78c2e-151">Basic save functionality</span></span>

<span data-ttu-id="78c2e-152">Varlık örneklerindeki değişikliklerin veritabanında kalıcı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="78c2e-153">İyimser Eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="78c2e-153">Optimistic Concurrency</span></span>

<span data-ttu-id="78c2e-154">Veriler veritabanından getirildiğinden, başka bir kullanıcı tarafından yapılan aşırı yazım değişikliklerine karşı korur.</span><span class="sxs-lookup"><span data-stu-id="78c2e-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="78c2e-155">Async SaveChanges</span><span class="sxs-lookup"><span data-stu-id="78c2e-155">Async SaveChanges</span></span>

<span data-ttu-id="78c2e-156">Veritabanı' dan verilen komutları işlerken, diğer istekleri işlemek `SaveChanges`için geçerli iş parçacığı serbest</span><span class="sxs-lookup"><span data-stu-id="78c2e-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="78c2e-157">Veritabanı İşlemleri</span><span class="sxs-lookup"><span data-stu-id="78c2e-157">Database Transactions</span></span>

<span data-ttu-id="78c2e-158">Her `SaveChanges` zaman atomik olduğu anlamına gelir (ya tamamen başarılı olur veya veritabanında herhangi bir değişiklik yapılmaz).</span><span class="sxs-lookup"><span data-stu-id="78c2e-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="78c2e-159">İçerik örnekleri vb. arasında işlem paylaşımına izin vermek için işlemle ilgili API'ler de vardır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="78c2e-160">İlişkisel: İfadelerin toplu olarak gruplanması</span><span class="sxs-lookup"><span data-stu-id="78c2e-160">Relational: Batching of statements</span></span>

<span data-ttu-id="78c2e-161">Birden çok INSERT/UPDATE/DELETE komutunu veritabanına tek bir gidiş dönüşe ekleyerek daha iyi performans sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="78c2e-162">Sorgu</span><span class="sxs-lookup"><span data-stu-id="78c2e-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="78c2e-163">Temel LINQ desteği</span><span class="sxs-lookup"><span data-stu-id="78c2e-163">Basic LINQ support</span></span>

<span data-ttu-id="78c2e-164">Veritabanından veri almak için LINQ'yi kullanma olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="78c2e-165">Karışık istemci/sunucu değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="78c2e-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="78c2e-166">Sorguların veritabanında değerlendirilemeyen bir mantık içermesini sağlar ve bu nedenle veriler belleğe alındıktan sonra değerlendirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="78c2e-167">Notracking</span><span class="sxs-lookup"><span data-stu-id="78c2e-167">NoTracking</span></span>

<span data-ttu-id="78c2e-168">Bağlam varlık örneklerideğişiklikleri için izlemek gerekmez (sonuçlar salt okunursa bu yararlıdır) sorgular daha hızlı sorgu yürütme sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="78c2e-169">Istekli yükleme</span><span class="sxs-lookup"><span data-stu-id="78c2e-169">Eager loading</span></span>

<span data-ttu-id="78c2e-170">`Include` Sorguyaparken `ThenInclude` alınması gereken ilgili verileri tanımlamak için ve yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="78c2e-171">Async sorgusu</span><span class="sxs-lookup"><span data-stu-id="78c2e-171">Async query</span></span>

<span data-ttu-id="78c2e-172">Veritabanı sorguyu işlerken diğer istekleri işlemek için geçerli iş parçacığı (ve ilişkili kaynaklar) kadar serbest olabilir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="78c2e-173">Ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="78c2e-173">Raw SQL queries</span></span>

<span data-ttu-id="78c2e-174">Verileri `DbSet.FromSql` almak için ham SQL sorgularını kullanma yöntemini sağlar.</span><span class="sxs-lookup"><span data-stu-id="78c2e-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="78c2e-175">Bu sorgular LINQ kullanılarak da oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="78c2e-176">Veritabanı şema yönetimi</span><span class="sxs-lookup"><span data-stu-id="78c2e-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="78c2e-177">Veritabanı oluşturma/silme API'leri</span><span class="sxs-lookup"><span data-stu-id="78c2e-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="78c2e-178">Geçişleri kullanmadan veritabanını hızla oluşturmak/silmek istediğiniz leri sınamak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="78c2e-179">İlişkisel veritabanı geçişleri</span><span class="sxs-lookup"><span data-stu-id="78c2e-179">Relational database migrations</span></span>

<span data-ttu-id="78c2e-180">İlişkisel veritabanı şemasının modeliniz değiştikçe fazla mesaiye dönüşmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="78c2e-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="78c2e-181">Veritabanından ters mühendis</span><span class="sxs-lookup"><span data-stu-id="78c2e-181">Reverse engineer from database</span></span>

<span data-ttu-id="78c2e-182">İskele, varolan bir ilişkisel veritabanı şemasına dayalı bir EF modelini şekillendiricidir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="78c2e-183">Veritabanı sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="78c2e-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="78c2e-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="78c2e-184">SQL Server</span></span>

<span data-ttu-id="78c2e-185">Microsoft SQL Server 2008'e bağlanır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="78c2e-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="78c2e-186">SQLite</span></span>

<span data-ttu-id="78c2e-187">Bir SQLite 3 veritabanına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="78c2e-188">Bellek Içi</span><span class="sxs-lookup"><span data-stu-id="78c2e-188">In-Memory</span></span>

<span data-ttu-id="78c2e-189">Gerçek bir veritabanına bağlanmadan testi kolayca etkinleştirmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="78c2e-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="78c2e-190">3. taraf sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="78c2e-190">3rd party providers</span></span>

<span data-ttu-id="78c2e-191">Diğer veritabanı motorları için çeşitli sağlayıcılar kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="78c2e-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="78c2e-192">Tam liste için [Veritabanı Sağlayıcıları'na](../providers/index.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="78c2e-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
