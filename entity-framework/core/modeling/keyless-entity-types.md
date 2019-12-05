---
title: Keyless varlık türleri-EF Core
description: Entity Framework Core kullanarak anahtarsız varlık türlerini yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 129e24b154ba32583435aeb742dbf478350344e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824667"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="7495a-103">Anahtarsız Varlık Türleri</span><span class="sxs-lookup"><span data-stu-id="7495a-103">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="7495a-104">Bu özellik, sorgu türlerinin adı altında EF Core 2,1 ' ye eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="7495a-104">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="7495a-105">EF Core 3,0 ' de kavram, anahtarsız varlık türleri olarak yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="7495a-105">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="7495a-106">Normal varlık türlerine ek olarak, bir EF Core modeli, anahtar değerleri içermeyen verilere karşı veritabanı sorgularını yürütmek için kullanılabilen, _daha seyrek varlık türleri_içerebilir.</span><span class="sxs-lookup"><span data-stu-id="7495a-106">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="7495a-107">Keyless varlık türleri özellikleri</span><span class="sxs-lookup"><span data-stu-id="7495a-107">Keyless entity types characteristics</span></span>

<span data-ttu-id="7495a-108">Keyless varlık türleri, devralma eşlemesi ve gezinti özellikleri gibi normal varlık türleriyle aynı eşleme özelliklerinin çoğunu destekler.</span><span class="sxs-lookup"><span data-stu-id="7495a-108">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="7495a-109">İlişkisel depolarını, bunlar hedef veritabanı nesneleri ve sütunları fluent API yöntemleri veya veri ek açıklamaları üzerinden yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7495a-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="7495a-110">Ancak bunlar, normal varlık türlerinden farklıdır:</span><span class="sxs-lookup"><span data-stu-id="7495a-110">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="7495a-111">Tanımlı bir anahtar olamaz.</span><span class="sxs-lookup"><span data-stu-id="7495a-111">Cannot have a key defined.</span></span>
- <span data-ttu-id="7495a-112">, _DbContext_ 'teki değişiklikler için hiçbir şekilde izlenmez ve bu nedenle veritabanında hiçbir şekilde eklenmemiş, güncellenmez veya silinmez.</span><span class="sxs-lookup"><span data-stu-id="7495a-112">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="7495a-113">Kural gereği hiçbir zaman bulunur.</span><span class="sxs-lookup"><span data-stu-id="7495a-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="7495a-114">Yalnızca bir gezinti eşleme özellikleri alt kümesini destekler, özellikle:</span><span class="sxs-lookup"><span data-stu-id="7495a-114">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="7495a-115">Bunlar, hiçbir zaman bir ilişkisinin birincil ucu çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="7495a-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="7495a-116">Sahip oldukları varlıkların gezginlerine sahip olmayabilir</span><span class="sxs-lookup"><span data-stu-id="7495a-116">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="7495a-117">Yalnızca normal varlıkların işaret eden başvuru gezinti özelliklerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="7495a-117">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="7495a-118">Varlıklar, anahtarsız varlık türlerine gezinti özellikleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="7495a-118">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="7495a-119">`.HasNoKey()` yöntemi çağrısıyla birlikte yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7495a-119">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="7495a-120">, _Tanımlayan bir sorguyla_eşleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="7495a-120">May be mapped to a _defining query_.</span></span> <span data-ttu-id="7495a-121">Tanımlama sorgusu, modelde tanımlanan ve anahtarsız varlık türü için veri kaynağı olarak davranan bir sorgudur.</span><span class="sxs-lookup"><span data-stu-id="7495a-121">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="7495a-122">Kullanım senaryoları</span><span class="sxs-lookup"><span data-stu-id="7495a-122">Usage scenarios</span></span>

<span data-ttu-id="7495a-123">Anahtarsız varlık türlerine yönelik ana kullanım senaryolarından bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="7495a-123">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="7495a-124">[Ham SQL sorguları](xref:core/querying/raw-sql)için dönüş türü olarak hizmet sunma.</span><span class="sxs-lookup"><span data-stu-id="7495a-124">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="7495a-125">Birincil anahtar içermeyen veritabanı görünümlerine eşleme.</span><span class="sxs-lookup"><span data-stu-id="7495a-125">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="7495a-126">Tanımlı bir birincil anahtarı olmayan tablolar için eşleme.</span><span class="sxs-lookup"><span data-stu-id="7495a-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="7495a-127">Eşleme için modelde tanımlı sorgular.</span><span class="sxs-lookup"><span data-stu-id="7495a-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="7495a-128">Veritabanı nesneleri eşleme</span><span class="sxs-lookup"><span data-stu-id="7495a-128">Mapping to database objects</span></span>

<span data-ttu-id="7495a-129">`ToTable` veya `ToView` Fluent API kullanılarak, bir anahtarsız varlık türünü bir veritabanı nesnesiyle eşleme elde edilir.</span><span class="sxs-lookup"><span data-stu-id="7495a-129">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="7495a-130">EF Core açısından bakıldığında, bu yöntemde belirtilen veritabanı nesnesi olan bir _görünümü_, yani bir salt okunur sorgu kaynağı olarak kabul edilir ve güncelleştirme işleminin hedefi, ekleme ya da silme işlemleri.</span><span class="sxs-lookup"><span data-stu-id="7495a-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="7495a-131">Ancak bu, veritabanı nesnesinin gerçekten bir veritabanı görünümü olması gerektiği anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="7495a-131">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="7495a-132">Alternatif olarak, salt okunurdur olarak değerlendirilecek bir veritabanı tablosu olabilir.</span><span class="sxs-lookup"><span data-stu-id="7495a-132">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="7495a-133">Buna karşılık, normal varlık türleri için EF Core, `ToTable` yönteminde belirtilen bir veritabanı nesnesinin _tablo_olarak değerlendirilebileceği anlamına gelir, yani bir sorgu kaynağı olarak kullanılabilecek ancak aynı zamanda Update, DELETE ve INSERT işlemleri tarafından hedeflenebilir.</span><span class="sxs-lookup"><span data-stu-id="7495a-133">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="7495a-134">Aslında, bir veritabanı görünümü'nde adını belirtebilirsiniz. `ToTable` ve görünümü veritabanında güncelleştirilebilir olarak yapılandırılmış olduğu sürece her şeyin düzgün çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7495a-134">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="7495a-135">`ToView`, nesnenin veritabanında zaten var olduğunu ve geçişler tarafından oluşturulmayacak olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="7495a-135">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="7495a-136">Örnek</span><span class="sxs-lookup"><span data-stu-id="7495a-136">Example</span></span>

<span data-ttu-id="7495a-137">Aşağıdaki örnek, bir veritabanı görünümünü sorgulamak için anahtarsız varlık türlerinin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7495a-137">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="7495a-138">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="7495a-138">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="7495a-139">İlk olarak, basit bir Blog ve gönderi modeli tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="7495a-139">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="7495a-140">Ardından, bize her blog ile ilişkili gönderi sayısı sorgulamaya izin verecek basit veritabanı görünümü tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="7495a-140">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="7495a-141">Ardından, veritabanı görünümü sonucu için bir sınıf tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="7495a-141">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="7495a-142">Daha sonra, `HasNoKey` API kullanarak _onmodelyaratırken_ anahtarsız varlık türünü yapılandıracağız.</span><span class="sxs-lookup"><span data-stu-id="7495a-142">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="7495a-143">Anahtarsız varlık türü için eşlemeyi yapılandırmak üzere floent Yapılandırma API 'sini kullanıyoruz:</span><span class="sxs-lookup"><span data-stu-id="7495a-143">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="7495a-144">Sonra, `DbContext` `DbSet<T>`içerecek şekilde yapılandırdık:</span><span class="sxs-lookup"><span data-stu-id="7495a-144">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="7495a-145">Son olarak, biz veritabanı görünümü standart şekilde sorgulayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7495a-145">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="7495a-146">Ayrıca, bu tür bir sorgu için kök görevi gören bir bağlam düzeyi sorgu özelliği (DbSet) tanımladık.</span><span class="sxs-lookup"><span data-stu-id="7495a-146">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
