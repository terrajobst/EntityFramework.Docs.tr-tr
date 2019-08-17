---
title: EF6 'den EF Core 'e taşıma-gereksinimleri doğrulama
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565351"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="2e98a-102">EF6 'den EF Core 'e aktarmadan önce: Uygulamanızın gereksinimlerini doğrulama</span><span class="sxs-lookup"><span data-stu-id="2e98a-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="2e98a-103">Taşıma işlemine başlamadan önce, EF Core uygulamanızın veri erişim gereksinimlerini karşıladığını doğrulamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="2e98a-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="2e98a-104">Eksik Özellikler</span><span class="sxs-lookup"><span data-stu-id="2e98a-104">Missing features</span></span>

<span data-ttu-id="2e98a-105">EF Core uygulamanızda kullanmak için gereken tüm özelliklere sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="2e98a-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="2e98a-106">EF Core özellik kümesinin EF6 ile nasıl Karşılaştırıldığı hakkında ayrıntılı bir karşılaştırma için bkz. [Özellik Karşılaştırması](../features.md) .</span><span class="sxs-lookup"><span data-stu-id="2e98a-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="2e98a-107">Gerekli özelliklerden herhangi biri eksikse, EF Core 'e geçmeden önce bu özelliklerin eksik olup olmadığını dengelemediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="2e98a-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="2e98a-108">Davranış değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2e98a-108">Behavior changes</span></span>

<span data-ttu-id="2e98a-109">Bu, EF6 ve EF Core arasındaki davranıştaki bazı değişikliklerin ayrıntılı olmayan bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="2e98a-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="2e98a-110">Uygulamanızın davranış şeklini değiştirebilecekleri, ancak EF Core takas edildikten sonra derleme hatası olarak göstermeyecek olan bağlantı noktası olarak bunları aklınızda bulundurmanız önemlidir.</span><span class="sxs-lookup"><span data-stu-id="2e98a-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="2e98a-111">DbSet. Add/Attach ve Graph davranışı</span><span class="sxs-lookup"><span data-stu-id="2e98a-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="2e98a-112">EF6 ' de, `DbSet.Add()` bir varlığa çağırmak, gezinti özelliklerinde başvurulan tüm varlıkların özyinelemeli aramasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2e98a-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="2e98a-113">Bulunan ve bağlam tarafından henüz izlenmeyen varlıklar de eklenmiş olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="2e98a-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="2e98a-114">`DbSet.Attach()`tüm varlıkların değiştirilmemiş olarak işaretlenmesi dışında, aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="2e98a-115">**EF Core benzer özyinelemeli arama gerçekleştirir, ancak biraz farklı kurallara sahiptir.**</span><span class="sxs-lookup"><span data-stu-id="2e98a-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="2e98a-116">Kök varlık her zaman istenen durumda (için eklendi `DbSet.Add` ve `DbSet.Attach`değiştirilmez).</span><span class="sxs-lookup"><span data-stu-id="2e98a-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="2e98a-117">**Gezinti özelliklerinin özyinelemeli araması sırasında bulunan varlıklar için:**</span><span class="sxs-lookup"><span data-stu-id="2e98a-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="2e98a-118">**Varlığın birincil anahtarı mağaza oluşturulduysa**</span><span class="sxs-lookup"><span data-stu-id="2e98a-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="2e98a-119">Birincil anahtar bir değere ayarlanmamışsa, durum eklendi olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="2e98a-120">Özellik türü için clr varsayılan değeri atanırsa, birincil anahtar değeri "ayarlanmadı" `0` olarak değerlendirilir (örneğin `int` `null` `string`, için, vb.).</span><span class="sxs-lookup"><span data-stu-id="2e98a-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="2e98a-121">Birincil anahtar bir değere ayarlanmışsa durum Unchanged olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="2e98a-122">Birincil anahtar veritabanı oluşturulmadığından, varlık kökle aynı duruma konur.</span><span class="sxs-lookup"><span data-stu-id="2e98a-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="2e98a-123">Code First veritabanı başlatma</span><span class="sxs-lookup"><span data-stu-id="2e98a-123">Code First database initialization</span></span>

<span data-ttu-id="2e98a-124">**EF6, veritabanı bağlantısını seçip veritabanını başlatırken önemli miktarda Magic 'e sahiptir. Bu kurallardan bazıları şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="2e98a-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="2e98a-125">Hiçbir yapılandırma gerçekleştirildiyse, EF6 SQL Express veya LocalDb üzerinde bir veritabanı seçer.</span><span class="sxs-lookup"><span data-stu-id="2e98a-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="2e98a-126">Bağlamla aynı ada sahip bir bağlantı dizesi uygulamalar `App/Web.config` dosyasında ise, bu bağlantı kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="2e98a-127">Veritabanı yoksa, oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e98a-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="2e98a-128">Modeldeki tablolardan hiçbiri veritabanında yoksa, geçerli modelin şeması veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="2e98a-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="2e98a-129">Geçişler etkinse, veritabanını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="2e98a-130">Veritabanı varsa ve EF6 daha önce şemayı oluşturduysanız, şema geçerli modelle uyumluluk için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="2e98a-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="2e98a-131">Şema oluşturulduktan sonra model değiştiyse bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e98a-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="2e98a-132">**EF Core Bu Magic 'in hiçbirini gerçekleştirmez.**</span><span class="sxs-lookup"><span data-stu-id="2e98a-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="2e98a-133">Veritabanı bağlantısı kodda açıkça yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="2e98a-134">Başlatma yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="2e98a-134">No initialization is performed.</span></span> <span data-ttu-id="2e98a-135">Geçişleri uygulamak için `DbContext.Database.Migrate()` kullanmanız gerekir ( `EnsureDeleted()` veya `DbContext.Database.EnsureCreated()` , geçişleri kullanmadan veritabanını oluşturmak/silmek için).</span><span class="sxs-lookup"><span data-stu-id="2e98a-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="2e98a-136">Code First tablo adlandırma kuralı</span><span class="sxs-lookup"><span data-stu-id="2e98a-136">Code First table naming convention</span></span>

<span data-ttu-id="2e98a-137">EF6, varlığın eşlendiği varsayılan tablo adını hesaplamak için bir çoğullaştırma hizmeti aracılığıyla varlık sınıfı adını çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="2e98a-138">EF Core, varlığın türetilmiş bağlamda kullanıma `DbSet` sunulmuş olduğu özelliğin adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="2e98a-139">Varlığın bir `DbSet` özelliği yoksa, sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e98a-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
