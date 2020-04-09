---
title: EF6'dan EF Core'a taşıma - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416952"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="04a6c-102">EF6’dan EF Core’a taşıma</span><span class="sxs-lookup"><span data-stu-id="04a6c-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="04a6c-103">EF Core'daki temel değişiklikler nedeniyle, değişikliği yapmak için zorlayıcı bir nedeniniz yoksa bir EF6 uygulamasını EF Core'a taşımayı denemenizi önermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="04a6c-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="04a6c-104">EF6'dan EF Core'a geçişi yükseltme yerine bir bağlantı noktası olarak görüntülemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="04a6c-105">Taşıma işlemine başlamadan önce, EF Core'un uygulamanız için veri erişim gereksinimlerini karşıladığını doğrulamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="04a6c-106">Eksik özellikler</span><span class="sxs-lookup"><span data-stu-id="04a6c-106">Missing features</span></span>

<span data-ttu-id="04a6c-107">EF Core'un uygulamanızda kullanmanız gereken tüm özelliklere sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="04a6c-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="04a6c-108">EF Core'da ayarlanan özelliğin EF6 ile karşılaştırıldığında nasıl olduğunu ayrıntılı bir karşılaştırma için [Özellik Karşılaştırması'na](xref:efcore-and-ef6/index) bakın.</span><span class="sxs-lookup"><span data-stu-id="04a6c-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="04a6c-109">Gerekli özellikler yoksa, EF Core'a geçmeden önce bu özelliklerin eksikliğini telafi edebileceğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="04a6c-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="04a6c-110">Davranış değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="04a6c-110">Behavior changes</span></span>

<span data-ttu-id="04a6c-111">Bu, EF6 ve EF Core arasındaki davranış değişikliklerinin kapsamlı olmayan bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="04a6c-112">Uygulamanızın nasıl bir hareket olduğunu değiştirebileceğinden, ancak EF Core'a geçtikten sonra derleme hatası olarak gösterilmeyeceğine dikkat etmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="04a6c-113">DbSet.Ekle/Ekle ve grafik davranışı</span><span class="sxs-lookup"><span data-stu-id="04a6c-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="04a6c-114">EF6'da, `DbSet.Add()` bir varlığı çağırmak, gezinti özelliklerinde başvurulan tüm varlıklar için özyinelemeli bir arama yla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="04a6c-115">Bulunan ve bağlam tarafından zaten izlenmeyen tüm varlıklar da eklenmiş olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="04a6c-116">`DbSet.Attach()`tüm varlıklar değişmeden işaretlenmiş olması dışında aynı şekilde kalır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="04a6c-117">**EF Core benzer özyinelemeli arama yapar, ancak bazı biraz farklı kurallar ile.**</span><span class="sxs-lookup"><span data-stu-id="04a6c-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="04a6c-118">Kök varlık her zaman istenen durumdadır `DbSet.Add` (için `DbSet.Attach`eklenen ve değiştirilmemiş).</span><span class="sxs-lookup"><span data-stu-id="04a6c-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="04a6c-119">**Gezinti özelliklerinin özyinelemeli araması sırasında bulunan varlıklar için:**</span><span class="sxs-lookup"><span data-stu-id="04a6c-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="04a6c-120">**Varlığın birincil anahtarı depo oluşturulursa**</span><span class="sxs-lookup"><span data-stu-id="04a6c-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="04a6c-121">Birincil anahtar bir değere ayarlanmazsa, durum eklenecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="04a6c-122">Özellik türü için CLR varsayılan değeri atanırsa birincil anahtar değeri "ayarlanmaz" `null` `string`olarak kabul edilir (örneğin, `0` için, `int`vb.).</span><span class="sxs-lookup"><span data-stu-id="04a6c-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="04a6c-123">Birincil anahtar bir değere ayarlanmışsa, durum değişmeden ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="04a6c-124">Birincil anahtar veritabanı oluşturulmazsa, varlık kökle aynı duruma sokulur.</span><span class="sxs-lookup"><span data-stu-id="04a6c-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="04a6c-125">Kod İlk veritabanı başlatma</span><span class="sxs-lookup"><span data-stu-id="04a6c-125">Code First database initialization</span></span>

<span data-ttu-id="04a6c-126">**EF6, veritabanı bağlantısını seçme ve veritabanını başlatma etrafında gerçekleştirdiği önemli miktarda büyüye sahiptir. Bu kurallardan bazıları şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="04a6c-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="04a6c-127">Yapılandırma yapılmazsa, EF6 SQL Express veya LocalDb'da bir veritabanı seçer.</span><span class="sxs-lookup"><span data-stu-id="04a6c-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="04a6c-128">Uygulamalar `App/Web.config` dosyasında bağlamla aynı ada sahip bir bağlantı dizesi varsa, bu bağlantı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="04a6c-129">Veritabanı yoksa, oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04a6c-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="04a6c-130">Modeldeki tablolardan hiçbiri veritabanında yoksa, geçerli modelin şeması veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="04a6c-131">Geçişler etkinse, veritabanını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="04a6c-132">Veritabanı varsa ve EF6 şemayı daha önce oluşturmuşsa, şema geçerli modelle uyumluluk için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="04a6c-133">Şema oluşturulduğundan beri model değiştiyse bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="04a6c-134">**EF Core bu sihri gerçekleştirmez.**</span><span class="sxs-lookup"><span data-stu-id="04a6c-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="04a6c-135">Veritabanı bağlantısı açıkça kod olarak yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="04a6c-136">Başlatma işlemi yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="04a6c-136">No initialization is performed.</span></span> <span data-ttu-id="04a6c-137">Geçişleri `DbContext.Database.Migrate()` uygulamak (veya `DbContext.Database.EnsureCreated()` geçişleri `EnsureDeleted()` kullanmadan veritabanını oluşturmak/silmek) için kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="04a6c-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="04a6c-138">Kod İlk tablo adlandırma kuralı</span><span class="sxs-lookup"><span data-stu-id="04a6c-138">Code First table naming convention</span></span>

<span data-ttu-id="04a6c-139">EF6, varlığın eşlenen varsayılan tablo adını hesaplamak için varlık sınıf adını çoğullaştırma hizmeti aracılığıyla çalıştırAr.</span><span class="sxs-lookup"><span data-stu-id="04a6c-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="04a6c-140">EF Core, varlığın `DbSet` türetilmiş bağlamda maruz kalandığı özelliğin adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="04a6c-141">Varlığın bir `DbSet` özelliği yoksa, sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="04a6c-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
