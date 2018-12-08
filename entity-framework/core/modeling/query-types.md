---
title: Sorgu türleri - EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: cb391343e6f24092ae0874003c0ef2935dd4e03f
ms.sourcegitcommit: 8dd71a57a01c439431164c163a0722877d0e5cd8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028186"
---
# <a name="query-types"></a><span data-ttu-id="ed507-102">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="ed507-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="ed507-103">Bu EF Core 2.1 içinde yeni bir özelliktir</span><span class="sxs-lookup"><span data-stu-id="ed507-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="ed507-104">Varlık türlerine ek olarak, EF Core modeli içerebilir _sorgu türü_, varlık türlerine eşlenmediği verilere karşı veritabanı sorgularını yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ed507-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

## <a name="compare-query-types-to-entity-types"></a><span data-ttu-id="ed507-105">Sorgu türleri varlık türleri için karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="ed507-105">Compare query types to entity types</span></span>

<span data-ttu-id="ed507-106">Sorgu türleri gibi varlık türleri, bunlar:</span><span class="sxs-lookup"><span data-stu-id="ed507-106">Query types are like entity types in that they:</span></span>

- <span data-ttu-id="ed507-107">Modele ya da eklenebilir, `OnModelCreating` veya türetilmiş "set" özellik aracılığıyla _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="ed507-107">Can be added to the model either in `OnModelCreating` or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="ed507-108">Devralma eşleme ve gezinti özellikleri gibi aynı Haritalama özellikleri çoğunu destekler.</span><span class="sxs-lookup"><span data-stu-id="ed507-108">Support many of the same mapping capabilities, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="ed507-109">İlişkisel depolarını, bunlar hedef veritabanı nesneleri ve sütunları fluent API yöntemleri veya veri ek açıklamaları üzerinden yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed507-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="ed507-110">Ancak, varlıktan farklı türlerden bunlar:</span><span class="sxs-lookup"><span data-stu-id="ed507-110">However, they are different from entity types in that they:</span></span>

- <span data-ttu-id="ed507-111">Tanımlanacak bir anahtarı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ed507-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="ed507-112">Üzerinde değişiklikler için hiçbir zaman izlenir _DbContext_ ve bu nedenle hiçbir zaman eklenen, güncelleştirilen veya veritabanında silindi.</span><span class="sxs-lookup"><span data-stu-id="ed507-112">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="ed507-113">Kural gereği hiçbir zaman bulunur.</span><span class="sxs-lookup"><span data-stu-id="ed507-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="ed507-114">Yalnızca bir alt kümesini Gezinti Haritalama özellikleri - özellikle destekler:</span><span class="sxs-lookup"><span data-stu-id="ed507-114">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="ed507-115">Bunlar, hiçbir zaman bir ilişkisinin birincil ucu çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="ed507-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="ed507-116">Bunlar yalnızca varlıklara işaret eden başvuru Gezinti özellikleri de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="ed507-116">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="ed507-117">Varlıklar için sorgu türleri Gezinti özellikleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="ed507-117">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="ed507-118">Üzerinde ele _ModelBuilder_ kullanarak `Query` yöntemi yerine `Entity` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ed507-118">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="ed507-119">Üzerinde eşlenen _DbContext_ türü özellikleri aracılığıyla `DbQuery<T>` yerine `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="ed507-119">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="ed507-120">Kullanarak veritabanı nesneleri için eşlenmiş `ToView` yöntemi yerine `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="ed507-120">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="ed507-121">Eşlenen bir _sorgu tanımlama_ - bir sorgu tanımlanıyor bir sorgu türü için bir veri kaynağı görevi gören modelinde bildirilen ikincil bir sorgu verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ed507-121">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="ed507-122">Kullanım senaryoları</span><span class="sxs-lookup"><span data-stu-id="ed507-122">Usage scenarios</span></span>

<span data-ttu-id="ed507-123">Sorgu türleri için ana kullanım senaryoları bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ed507-123">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="ed507-124">Dönüş türü için geçici hizmet `FromSql()` sorgular.</span><span class="sxs-lookup"><span data-stu-id="ed507-124">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="ed507-125">Veritabanı görünümleriyle eşleme.</span><span class="sxs-lookup"><span data-stu-id="ed507-125">Mapping to database views.</span></span>
- <span data-ttu-id="ed507-126">Tanımlı bir birincil anahtarı olmayan tablolar için eşleme.</span><span class="sxs-lookup"><span data-stu-id="ed507-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="ed507-127">Eşleme için modelde tanımlı sorgular.</span><span class="sxs-lookup"><span data-stu-id="ed507-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="ed507-128">Veritabanı nesneleri eşleme</span><span class="sxs-lookup"><span data-stu-id="ed507-128">Mapping to database objects</span></span>

<span data-ttu-id="ed507-129">Bir sorgu türü için bir veritabanı nesnesi eşleme gerçekleştirilir kullanarak `ToView` fluent API'si.</span><span class="sxs-lookup"><span data-stu-id="ed507-129">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="ed507-130">EF Core açısından bakıldığında, bu yöntemde belirtilen veritabanı nesnesi olan bir _görünümü_, yani bir salt okunur sorgu kaynağı olarak kabul edilir ve güncelleştirme işleminin hedefi, ekleme ya da silme işlemleri.</span><span class="sxs-lookup"><span data-stu-id="ed507-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="ed507-131">Ancak, bu gelmez veritabanı nesnesi veritabanı görünümü için gerçekten gerekli değildir - alternatif olarak salt okunur olarak kabul edilir bir veritabanınız olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed507-131">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="ed507-132">Buna karşılık, varlık türleri için bir veritabanı nesnesi içinde belirtilen EF Core varsayar `ToTable` yöntemi olarak kabul bir _tablo_, sorgu kaynağı olarak kullanılabilir olduğunu bildirir ancak aynı zamanda update tarafından hedeflenen Sil anlamına gelir ve Ekle işlemler.</span><span class="sxs-lookup"><span data-stu-id="ed507-132">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="ed507-133">Aslında, bir veritabanı görünümü'nde adını belirtebilirsiniz. `ToTable` ve görünümü veritabanında güncelleştirilebilir olarak yapılandırılmış olduğu sürece her şeyin düzgün çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ed507-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="ed507-134">Örnek</span><span class="sxs-lookup"><span data-stu-id="ed507-134">Example</span></span>

<span data-ttu-id="ed507-135">Aşağıdaki örnek bir veritabanı görünümü sorgulamak için sorgu türünü kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ed507-135">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="ed507-136">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="ed507-136">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="ed507-137">İlk olarak, basit bir Blog ve gönderi modeli tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="ed507-137">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="ed507-138">Ardından, bize her blog ile ilişkili gönderi sayısı sorgulamaya izin verecek basit veritabanı görünümü tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="ed507-138">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

<span data-ttu-id="ed507-139">Ardından, veritabanı görünümü sonucu için bir sınıf tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="ed507-139">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="ed507-140">Ardından, sorgu türünde yapılandırıyoruz _OnModelCreating_ kullanarak `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="ed507-140">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="ed507-141">Sorgu türü için eşlemeyi yapılandırmak için standart fluent yapılandırma API'leri kullanın:</span><span class="sxs-lookup"><span data-stu-id="ed507-141">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="ed507-142">Son olarak, biz veritabanı görünümü standart şekilde sorgulayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ed507-142">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="ed507-143">Biz de içerik düzeyi sorgu özelliği (Bu tür sorgu için bir kök olarak görev yapacak DbQuery) tanımladığınız unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ed507-143">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
