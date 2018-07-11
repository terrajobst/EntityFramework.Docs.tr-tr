---
title: EF6'dan EF Core'a - taşıma gereksinimleri doğrulama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949120"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="8065c-102">EF6'dan EF Core'a taşıma önce: uygulamanızın gereksinimlerini doğrulayın</span><span class="sxs-lookup"><span data-stu-id="8065c-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="8065c-103">Taşıma işlemine başlamadan önce EF Core uygulamanız için veri erişim gereksinimleri karşıladığını doğrulamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="8065c-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="8065c-104">Eksik özellikleri</span><span class="sxs-lookup"><span data-stu-id="8065c-104">Missing features</span></span>

<span data-ttu-id="8065c-105">EF Core uygulamanızda kullanmak için ihtiyacınız olan tüm özellikleri sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8065c-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="8065c-106">Bkz: [özellik karşılaştırması](../features.md) nasıl EF6 için özellik EF Core kümesini karşılaştırır, ayrıntılı bir karşılaştırması için.</span><span class="sxs-lookup"><span data-stu-id="8065c-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="8065c-107">Tüm gerekli özellikleri yoksa, bu özelliklerin olmaması için önce EF Core'a taşıma dengeleyebilir emin olun.</span><span class="sxs-lookup"><span data-stu-id="8065c-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="8065c-108">Davranış değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="8065c-108">Behavior changes</span></span>

<span data-ttu-id="8065c-109">Kapsamlı olmayan bazı değişiklikler EF6 ve EF Core arasındaki davranış listesini budur.</span><span class="sxs-lookup"><span data-stu-id="8065c-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="8065c-110">Bunlar uygulamanızı davranır, ancak derleme değiştirdikten sonra EF Core gösterilmez şekilde değişiklik gösterebileceği için bağlantı noktası olarak uygulamanız bu aklınızda tutmanız önemlidir.</span><span class="sxs-lookup"><span data-stu-id="8065c-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="8065c-111">DbSet.Add/Attach ve grafik davranışı</span><span class="sxs-lookup"><span data-stu-id="8065c-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="8065c-112">EF6 içinde arama `DbSet.Add()` yinelemeli arama Gezinti özelliklerini başvurulan tüm varlıklar için bir varlık sonuçları üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8065c-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="8065c-113">Aynı zamanda bulunur ve bağlam tarafından izlenmez herhangi bir varlık olan eklenen olarak işaretlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="8065c-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="8065c-114">`DbSet.Attach()` Tüm varlıklar işaretlenmiş dışında aynı şekilde davranır ancak olarak değişmez.</span><span class="sxs-lookup"><span data-stu-id="8065c-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="8065c-115">**EF Core benzer bir yinelemeli arama gerçekleştirir ancak bazı biraz farklı kuralları.**</span><span class="sxs-lookup"><span data-stu-id="8065c-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="8065c-116">Kök varlığın her zaman istenen durumda (eklenen `DbSet.Add` ve için değişmemiştir `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="8065c-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="8065c-117">**Gezinti özellikleri yinelemeli arama sırasında bulunan varlıkları için:**</span><span class="sxs-lookup"><span data-stu-id="8065c-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="8065c-118">**Varlığın birincil anahtarı oluşturulan depo ise**</span><span class="sxs-lookup"><span data-stu-id="8065c-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="8065c-119">Birincil anahtar değerine ayarlanmazsa durum için eklenen ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8065c-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="8065c-120">Özellik türü için CLR varsayılan değer atanmışsa birincil anahtar değeri "ayarlı değil" olarak kabul edilir (örneğin, `0` için `int`, `null` için `string`vb..).</span><span class="sxs-lookup"><span data-stu-id="8065c-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="8065c-121">Birincil anahtarı bir değere ayarlarsanız, durumu değişmeden ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8065c-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="8065c-122">Birincil anahtarı oluşturulan veritabanı değil, varlığın kök ile aynı duruma konur.</span><span class="sxs-lookup"><span data-stu-id="8065c-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="8065c-123">İlk veritabanı başlatma kodu</span><span class="sxs-lookup"><span data-stu-id="8065c-123">Code First database initialization</span></span>

<span data-ttu-id="8065c-124">**EF6 veritabanı bağlantısını seçerek ve veritabanı başlatma etrafında gerçekleştirir Sihirli önemli miktarda sahiptir. Bu kuralların bazıları şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="8065c-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="8065c-125">Yapılandırma gerçekleştirilirse, bir veritabanında SQL Express ya da LocalDb EF6 seçer.</span><span class="sxs-lookup"><span data-stu-id="8065c-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="8065c-126">İçerik olarak aynı ada sahip bir bağlantı dizesi uygulamalar ise `App/Web.config` dosya, bu bağlantı kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="8065c-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="8065c-127">Veritabanı yoksa, oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8065c-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="8065c-128">Modelden tablolardan veritabanında varsa, geçerli model için şema veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="8065c-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="8065c-129">Geçişleri etkinse, sonra bunlar veritabanını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8065c-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="8065c-130">Veritabanı varsa ve EF6 şemayı daha önce oluşturmuştunuz şema geçerli modeli ile uyumluluk için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="8065c-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="8065c-131">Model şeması oluşturulmasından bu yana değişmişse bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8065c-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="8065c-132">**EF Core bu Sihirli gerçekleştirmez.**</span><span class="sxs-lookup"><span data-stu-id="8065c-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="8065c-133">Veritabanı bağlantısı kodda açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8065c-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="8065c-134">Hiçbir başlatma gerçekleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="8065c-134">No initialization is performed.</span></span> <span data-ttu-id="8065c-135">Kullanmalısınız `DbContext.Database.Migrate()` geçişler uygulamak için (veya `DbContext.Database.EnsureCreated()` ve `EnsureDeleted()` oluşturma / veritabanı geçişleri kullanmadan silme için).</span><span class="sxs-lookup"><span data-stu-id="8065c-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="8065c-136">Kod ilk tabloda adlandırma kuralı</span><span class="sxs-lookup"><span data-stu-id="8065c-136">Code First table naming convention</span></span>

<span data-ttu-id="8065c-137">EF6 varlık sınıfı adı varlığa eşlendi varsayılan tablo adı hesaplamak için çoğullaştırma hizmeti aracılığıyla çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="8065c-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="8065c-138">EF Core kullanan adını `DbSet` varlık de türetilmiş bağlamda sağlanmaktadır özelliği.</span><span class="sxs-lookup"><span data-stu-id="8065c-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="8065c-139">Varlık yoksa bir `DbSet` özelliği ve ardından sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8065c-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
