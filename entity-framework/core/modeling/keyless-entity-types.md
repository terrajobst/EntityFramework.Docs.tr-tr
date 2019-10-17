---
title: Keyless varlık türleri-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 3dbc2700fc9bb277eb90885dfc2506c250ae21f1
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445938"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="6380f-102">Anahtarsız Varlık Türleri</span><span class="sxs-lookup"><span data-stu-id="6380f-102">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="6380f-103">Bu özellik, sorgu türlerinin adı altında EF Core 2,1 ' ye eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="6380f-103">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="6380f-104">EF Core 3,0 ' de kavram, anahtarsız varlık türleri olarak yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="6380f-104">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="6380f-105">Normal varlık türlerine ek olarak, bir EF Core modeli, anahtar değerleri içermeyen verilere karşı veritabanı sorgularını yürütmek için kullanılabilen, _daha seyrek varlık türleri_içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6380f-105">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="6380f-106">Keyless varlık türleri özellikleri</span><span class="sxs-lookup"><span data-stu-id="6380f-106">Keyless entity types characteristics</span></span>

<span data-ttu-id="6380f-107">Keyless varlık türleri, devralma eşlemesi ve gezinti özellikleri gibi normal varlık türleriyle aynı eşleme özelliklerinin çoğunu destekler.</span><span class="sxs-lookup"><span data-stu-id="6380f-107">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="6380f-108">İlişkisel depolarda, hedef veritabanı nesnelerini ve sütunları Fluent API Yöntemler veya veri ek açıklamaları aracılığıyla yapılandırabilirler.</span><span class="sxs-lookup"><span data-stu-id="6380f-108">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="6380f-109">Ancak bunlar, normal varlık türlerinden farklıdır:</span><span class="sxs-lookup"><span data-stu-id="6380f-109">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="6380f-110">Tanımlı bir anahtar olamaz.</span><span class="sxs-lookup"><span data-stu-id="6380f-110">Cannot have a key defined.</span></span>
- <span data-ttu-id="6380f-111">, _DbContext_ 'teki değişiklikler için hiçbir şekilde izlenmez ve bu nedenle veritabanında hiçbir şekilde eklenmemiş, güncellenmez veya silinmez.</span><span class="sxs-lookup"><span data-stu-id="6380f-111">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="6380f-112">Hiçbir şekilde kural tarafından keşfedilir.</span><span class="sxs-lookup"><span data-stu-id="6380f-112">Are never discovered by convention.</span></span>
- <span data-ttu-id="6380f-113">Yalnızca bir gezinti eşleme özellikleri alt kümesini destekler, özellikle:</span><span class="sxs-lookup"><span data-stu-id="6380f-113">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="6380f-114">Bir ilişkinin asıl ucu olarak hiçbir şekilde davranmayabilir.</span><span class="sxs-lookup"><span data-stu-id="6380f-114">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="6380f-115">Sahip oldukları varlıkların gezginlerine sahip olmayabilir</span><span class="sxs-lookup"><span data-stu-id="6380f-115">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="6380f-116">Yalnızca normal varlıkların işaret eden başvuru gezinti özelliklerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6380f-116">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="6380f-117">Varlıklar, anahtarsız varlık türlerine gezinti özellikleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="6380f-117">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="6380f-118">@No__t-0 yöntem çağrısıyla yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6380f-118">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="6380f-119">, _Tanımlayan bir sorguyla_eşleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="6380f-119">May be mapped to a _defining query_.</span></span> <span data-ttu-id="6380f-120">Tanımlama sorgusu, modelde tanımlanan ve anahtarsız varlık türü için veri kaynağı olarak davranan bir sorgudur.</span><span class="sxs-lookup"><span data-stu-id="6380f-120">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="6380f-121">Kullanım senaryoları</span><span class="sxs-lookup"><span data-stu-id="6380f-121">Usage scenarios</span></span>

<span data-ttu-id="6380f-122">Anahtarsız varlık türlerine yönelik ana kullanım senaryolarından bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6380f-122">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="6380f-123">[Ham SQL sorguları](xref:core/querying/raw-sql)için dönüş türü olarak hizmet sunma.</span><span class="sxs-lookup"><span data-stu-id="6380f-123">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="6380f-124">Birincil anahtar içermeyen veritabanı görünümlerine eşleme.</span><span class="sxs-lookup"><span data-stu-id="6380f-124">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="6380f-125">Birincil anahtarı tanımlanmış olmayan tablolarla eşleme.</span><span class="sxs-lookup"><span data-stu-id="6380f-125">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="6380f-126">Modelde tanımlanan sorgularla eşleme.</span><span class="sxs-lookup"><span data-stu-id="6380f-126">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="6380f-127">Veritabanı nesneleriyle eşleme</span><span class="sxs-lookup"><span data-stu-id="6380f-127">Mapping to database objects</span></span>

<span data-ttu-id="6380f-128">@No__t-0 veya `ToView` Fluent API kullanılarak bir anahtarsız varlık türünü bir veritabanı nesnesiyle eşleme yapılır.</span><span class="sxs-lookup"><span data-stu-id="6380f-128">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="6380f-129">EF Core perspektifinden, bu yöntemde belirtilen veritabanı nesnesi bir _görünümdir_, yani salt okunurdur bir sorgu kaynağı olarak kabul edilir ve güncelleştirme, ekleme veya silme işlemlerinin hedefi olamaz.</span><span class="sxs-lookup"><span data-stu-id="6380f-129">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="6380f-130">Ancak bu, veritabanı nesnesinin gerçekten bir veritabanı görünümü olması gerektiği anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="6380f-130">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="6380f-131">Alternatif olarak, salt okunurdur olarak değerlendirilecek bir veritabanı tablosu olabilir.</span><span class="sxs-lookup"><span data-stu-id="6380f-131">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="6380f-132">Buna karşılık, normal varlık türleri için EF Core, `ToTable` yönteminde belirtilen bir veritabanı nesnesinin _tablo_olarak değerlendirilebileceği anlamına gelir, yani bir sorgu kaynağı olarak kullanılabilecek ancak aynı zamanda Update, DELETE ve INSERT işlemleri tarafından hedeflenebilir.</span><span class="sxs-lookup"><span data-stu-id="6380f-132">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="6380f-133">Aslında `ToTable` ' da bir veritabanı görünümü adı belirtebilirsiniz ve görünümün veritabanında güncelleştirimek üzere yapılandırıldığı sürece her şey iyi çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6380f-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="6380f-134">`ToView` nesnenin veritabanında zaten var olduğunu ve geçişler tarafından oluşturulmayacak olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="6380f-134">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="6380f-135">Örnek</span><span class="sxs-lookup"><span data-stu-id="6380f-135">Example</span></span>

<span data-ttu-id="6380f-136">Aşağıdaki örnek, bir veritabanı görünümünü sorgulamak için anahtarsız varlık türlerinin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="6380f-136">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="6380f-137">Bu makalenin [örneğini](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6380f-137">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="6380f-138">İlk olarak, basit bir blog ve gönderi modeli tanımladık:</span><span class="sxs-lookup"><span data-stu-id="6380f-138">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="6380f-139">Ardından, her bloga ait gönderi sayısını sorgulamanızı sağlayacak basit bir veritabanı görünümü tanımladık:</span><span class="sxs-lookup"><span data-stu-id="6380f-139">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="6380f-140">Sonra, veritabanı görünümünden elde edilen sonucu barındıracak bir sınıf tanımlayacağız:</span><span class="sxs-lookup"><span data-stu-id="6380f-140">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="6380f-141">Daha sonra, `HasNoKey` API kullanarak _onmodelyaratırken_ anahtarsız varlık türünü yapılandıracağız.</span><span class="sxs-lookup"><span data-stu-id="6380f-141">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="6380f-142">Anahtarsız varlık türü için eşlemeyi yapılandırmak üzere floent Yapılandırma API 'sini kullanıyoruz:</span><span class="sxs-lookup"><span data-stu-id="6380f-142">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="6380f-143">Sonra, `DbContext` `DbSet<T>` ' i içerecek şekilde yapılandırdık:</span><span class="sxs-lookup"><span data-stu-id="6380f-143">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="6380f-144">Son olarak, veritabanı görünümünü standart şekilde sorgulayabilir:</span><span class="sxs-lookup"><span data-stu-id="6380f-144">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="6380f-145">Ayrıca, bu tür bir sorgu için kök görevi gören bir bağlam düzeyi sorgu özelliği (DbSet) tanımladık.</span><span class="sxs-lookup"><span data-stu-id="6380f-145">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
