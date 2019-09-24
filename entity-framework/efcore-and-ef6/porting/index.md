---
title: EF6’dan EF Core’a taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 42e40ce769a67a987883027e1807ec7eaeb4ad7a
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198032"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="06ed8-102">EF6’dan EF Core’a taşıma</span><span class="sxs-lookup"><span data-stu-id="06ed8-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="06ed8-103">EF Core temel değişiklikler nedeniyle, değişikliği yapmak için etkileyici bir nedeniniz olmadığı takdirde, bir EF6 uygulamasını EF Core taşımaya çalışmak önerilmez.</span><span class="sxs-lookup"><span data-stu-id="06ed8-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="06ed8-104">EF6 ' dan EF Core bir yükseltme yerine bir bağlantı noktası olarak ilerletirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06ed8-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06ed8-105">Taşıma işlemine başlamadan önce, EF Core uygulamanızın veri erişim gereksinimlerini karşıladığını doğrulamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="06ed8-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="06ed8-106">Eksik Özellikler</span><span class="sxs-lookup"><span data-stu-id="06ed8-106">Missing features</span></span>

<span data-ttu-id="06ed8-107">EF Core uygulamanızda kullanmak için gereken tüm özelliklere sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="06ed8-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="06ed8-108">EF Core özellik kümesinin EF6 ile nasıl Karşılaştırıldığı hakkında ayrıntılı bir karşılaştırma için bkz. [Özellik Karşılaştırması](xref:efcore-and-ef6/index) .</span><span class="sxs-lookup"><span data-stu-id="06ed8-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="06ed8-109">Gerekli özelliklerden herhangi biri eksikse, EF Core 'e geçmeden önce bu özelliklerin eksik olup olmadığını dengelemediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="06ed8-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="06ed8-110">Davranış değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="06ed8-110">Behavior changes</span></span>

<span data-ttu-id="06ed8-111">Bu, EF6 ve EF Core arasındaki davranıştaki bazı değişikliklerin ayrıntılı olmayan bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="06ed8-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="06ed8-112">Uygulamanızın davranış şeklini değiştirebilecekleri, ancak EF Core takas edildikten sonra derleme hatası olarak göstermeyecek olan bağlantı noktası olarak bunları aklınızda bulundurmanız önemlidir.</span><span class="sxs-lookup"><span data-stu-id="06ed8-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="06ed8-113">DbSet. Add/Attach ve Graph davranışı</span><span class="sxs-lookup"><span data-stu-id="06ed8-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="06ed8-114">EF6 ' de, `DbSet.Add()` bir varlığa çağırmak, gezinti özelliklerinde başvurulan tüm varlıkların özyinelemeli aramasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="06ed8-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="06ed8-115">Bulunan ve bağlam tarafından henüz izlenmeyen varlıklar de eklenmiş olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="06ed8-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="06ed8-116">`DbSet.Attach()`tüm varlıkların değiştirilmemiş olarak işaretlenmesi dışında, aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="06ed8-117">**EF Core benzer özyinelemeli arama gerçekleştirir, ancak biraz farklı kurallara sahiptir.**</span><span class="sxs-lookup"><span data-stu-id="06ed8-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="06ed8-118">Kök varlık her zaman istenen durumda (için eklendi `DbSet.Add` ve `DbSet.Attach`değiştirilmez).</span><span class="sxs-lookup"><span data-stu-id="06ed8-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="06ed8-119">**Gezinti özelliklerinin özyinelemeli araması sırasında bulunan varlıklar için:**</span><span class="sxs-lookup"><span data-stu-id="06ed8-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="06ed8-120">**Varlığın birincil anahtarı mağaza oluşturulduysa**</span><span class="sxs-lookup"><span data-stu-id="06ed8-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="06ed8-121">Birincil anahtar bir değere ayarlanmamışsa, durum eklendi olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="06ed8-122">Özellik türü için clr varsayılan değeri atanırsa, birincil anahtar değeri "ayarlanmadı" `0` olarak değerlendirilir (örneğin `int` `null` `string`, için, vb.).</span><span class="sxs-lookup"><span data-stu-id="06ed8-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="06ed8-123">Birincil anahtar bir değere ayarlanmışsa durum Unchanged olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="06ed8-124">Birincil anahtar veritabanı oluşturulmadığından, varlık kökle aynı duruma konur.</span><span class="sxs-lookup"><span data-stu-id="06ed8-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="06ed8-125">Code First veritabanı başlatma</span><span class="sxs-lookup"><span data-stu-id="06ed8-125">Code First database initialization</span></span>

<span data-ttu-id="06ed8-126">**EF6, veritabanı bağlantısını seçip veritabanını başlatırken önemli miktarda Magic 'e sahiptir. Bu kurallardan bazıları şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="06ed8-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="06ed8-127">Hiçbir yapılandırma gerçekleştirildiyse, EF6 SQL Express veya LocalDb üzerinde bir veritabanı seçer.</span><span class="sxs-lookup"><span data-stu-id="06ed8-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="06ed8-128">Bağlamla aynı ada sahip bir bağlantı dizesi uygulamalar `App/Web.config` dosyasında ise, bu bağlantı kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="06ed8-129">Veritabanı yoksa, oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="06ed8-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="06ed8-130">Modeldeki tablolardan hiçbiri veritabanında yoksa, geçerli modelin şeması veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="06ed8-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="06ed8-131">Geçişler etkinse, veritabanını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="06ed8-132">Veritabanı varsa ve EF6 daha önce şemayı oluşturduysanız, şema geçerli modelle uyumluluk için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="06ed8-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="06ed8-133">Şema oluşturulduktan sonra model değiştiyse bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="06ed8-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="06ed8-134">**EF Core Bu Magic 'in hiçbirini gerçekleştirmez.**</span><span class="sxs-lookup"><span data-stu-id="06ed8-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="06ed8-135">Veritabanı bağlantısı kodda açıkça yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="06ed8-136">Başlatma yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="06ed8-136">No initialization is performed.</span></span> <span data-ttu-id="06ed8-137">Geçişleri uygulamak için `DbContext.Database.Migrate()` kullanmanız gerekir ( `EnsureDeleted()` veya `DbContext.Database.EnsureCreated()` , geçişleri kullanmadan veritabanını oluşturmak/silmek için).</span><span class="sxs-lookup"><span data-stu-id="06ed8-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="06ed8-138">Code First tablo adlandırma kuralı</span><span class="sxs-lookup"><span data-stu-id="06ed8-138">Code First table naming convention</span></span>

<span data-ttu-id="06ed8-139">EF6, varlığın eşlendiği varsayılan tablo adını hesaplamak için bir çoğullaştırma hizmeti aracılığıyla varlık sınıfı adını çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="06ed8-140">EF Core, varlığın türetilmiş bağlamda kullanıma `DbSet` sunulmuş olduğu özelliğin adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="06ed8-141">Varlığın bir `DbSet` özelliği yoksa, sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="06ed8-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
