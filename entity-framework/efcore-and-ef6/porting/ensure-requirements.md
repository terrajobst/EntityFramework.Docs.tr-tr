---
title: "EF çekirdek - EF6 bağlantı noktası oluşturma gereksinimlerini doğrulayın"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="8a8cd-102">EF çekirdek EF6 bağlantı noktası oluşturma önce: uygulamanızın gereksinimlerini doğrulayın</span><span class="sxs-lookup"><span data-stu-id="8a8cd-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="8a8cd-103">Taşıma işlemine başlamadan önce EF çekirdek uygulamanız için veri erişim gereksinimlerini karşıladığını doğrulamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="8a8cd-104">Eksik Özellikler</span><span class="sxs-lookup"><span data-stu-id="8a8cd-104">Missing features</span></span>

<span data-ttu-id="8a8cd-105">EF çekirdek, uygulamanızda kullanmak için gereken tüm özellikler sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="8a8cd-106">Bkz: [özellik karşılaştırması](../features.md) nasıl EF6 için özellik EF çekirdek kümesini karşılaştırır, ayrıntılı bir karşılaştırma için.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="8a8cd-107">Tüm gerekli özellikleri eksikse, bu özellikler olmaması için EF çekirdek taşıma önce dengeleyebilirsiniz emin olun.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="8a8cd-108">Davranış değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="8a8cd-108">Behavior changes</span></span>

<span data-ttu-id="8a8cd-109">Bu davranış EF6 EF çekirdek arasındaki bazı değişiklikler kapsamlı olmayan bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="8a8cd-110">Bunlar uygulamanızı davranır, ancak derleme hataları takas sonra EF çekirdek gösterilmez şekilde değişiklik gösterebileceği için bağlantı noktası olarak uygulamanız bu aklınızda tutmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="8a8cd-111">DbSet.Add/Attach ve grafik davranışı</span><span class="sxs-lookup"><span data-stu-id="8a8cd-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="8a8cd-112">EF6 içinde arama `DbSet.Add()` Gezinti özelliklerini başvurulan tüm varlıklar için yinelemeli bir aramasını varlık sonuçlarında üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="8a8cd-113">Bulunan ve bağlam tarafından izlenmez herhangi bir varlık da olduğu eklenmiş olarak işaretlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="8a8cd-114">`DbSet.Attach()`Tüm varlıklar işaretlenmiş dışında aynı şekilde davranır olarak değişmez.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="8a8cd-115">**EF çekirdek benzer bir özyinelemeli arama gerçekleştirir, ancak bazı biraz farklı kuralları ile.**</span><span class="sxs-lookup"><span data-stu-id="8a8cd-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="8a8cd-116">Kök varlık her zaman istenen durumda (için eklenen `DbSet.Add` ve için değişmemiştir `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="8a8cd-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="8a8cd-117">**Gezinti özellikleri özyinelemeli arama sırasında bulunan varlıklar için:**</span><span class="sxs-lookup"><span data-stu-id="8a8cd-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="8a8cd-118">**Varlığın birincil anahtarı oluşturulan depo ise**</span><span class="sxs-lookup"><span data-stu-id="8a8cd-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="8a8cd-119">Birincil anahtar değerine ayarlı değilse durumu için eklenen ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="8a8cd-120">Özellik türü için CLR varsayılan değer atanmışsa birincil anahtar değerine "ayarlı değil" olarak kabul edilir (yani `0` için `int`, `null` için `string`, vb..).</span><span class="sxs-lookup"><span data-stu-id="8a8cd-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (i.e. `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="8a8cd-121">Birincil anahtar değerine ayarlı ise, durumu değişmeden ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="8a8cd-122">Birincil anahtar oluşturulan veritabanı değilse, varlık kök ile aynı duruma getirilir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="8a8cd-123">İlk veritabanı başlatma kod</span><span class="sxs-lookup"><span data-stu-id="8a8cd-123">Code First database initialization</span></span>

<span data-ttu-id="8a8cd-124">**EF6 veritabanı bağlantısını seçerek ve veritabanı başlatılırken geçici gerçekleştirir Sihirli önemli miktarda sahiptir. Bu kurallar şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="8a8cd-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="8a8cd-125">Hiçbir yapılandırma gerçekleştirilirse, EF6 SQL Express veya yerel veritabanı bir veritabanı seçin.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="8a8cd-126">Bağlam aynı ada sahip bir bağlantı dizesi uygulamalarda ise `App/Web.config` dosyası, bu bağlantısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="8a8cd-127">Veritabanı mevcut değilse oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="8a8cd-128">Model tablolardan hiçbiri veritabanında yoksa, geçerli model için şema veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="8a8cd-129">Geçişler etkinleştirilirse, ardından bunlar veritabanı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="8a8cd-130">Veritabanı varsa ve EF6 daha önce şema oluşturduysanız, geçerli model ile uyumluluk için şema denetlenir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="8a8cd-131">Model şeması oluşturulmasından bu yana değiştiyse özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="8a8cd-132">**EF çekirdek bu Sihirli gerçekleştirmez.**</span><span class="sxs-lookup"><span data-stu-id="8a8cd-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="8a8cd-133">Veritabanı bağlantısı kodda açıkça yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="8a8cd-134">Hiçbir başlatma gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-134">No initialization is performed.</span></span> <span data-ttu-id="8a8cd-135">Kullanmalısınız `DbContext.Database.Migrate()` geçişleri uygulamak için (veya `DbContext.Database.EnsureCreated()` ve `EnsureDeleted()` oluşturma / veritabanı geçişler kullanmadan silme için).</span><span class="sxs-lookup"><span data-stu-id="8a8cd-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="8a8cd-136">Kod ilk tablonun adlandırma</span><span class="sxs-lookup"><span data-stu-id="8a8cd-136">Code First table naming convention</span></span>

<span data-ttu-id="8a8cd-137">EF6 varlık sınıfı adı varlık eşlenmiş varsayılan tablo adı hesaplamak için çoğullaştırma hizmeti ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="8a8cd-138">EF çekirdek adını kullanan `DbSet` varlık de türetilmiş bağlamda sağlanmaktadır özelliği.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="8a8cd-139">Varlık yoksa bir `DbSet` özelliği, ardından sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a8cd-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
