---
title: Sorgu türleri - EF çekirdek
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163180"
---
# <a name="query-types"></a><span data-ttu-id="95a86-102">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="95a86-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="95a86-103">Bu özellik EF çekirdek 2.1 yenilikler</span><span class="sxs-lookup"><span data-stu-id="95a86-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="95a86-104">EF çekirdek modeli varlık türlerine ek olarak içerebilir _sorgu türü_, varlık türleri eşlenmediği veri veritabanı sorguları yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="95a86-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="95a86-105">Sorgu türleri varlık türleriyle birçok benzerlikler vardır:</span><span class="sxs-lookup"><span data-stu-id="95a86-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="95a86-106">Bunlar ayrıca modeline ya da eklenebilir, `OnModelCreating`, veya türetilmiş bir "Ayarla" özellik aracılığıyla _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="95a86-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="95a86-107">Bunlar aynı eşleme özelliklerini, devralma eşleme, gezinti özellikleri (sınırlamalar aşağıya bakın) gibi ve ilişkisel depoları, hedef veritabanı nesneleri ve sütunları fluent API yöntemlerini veya veri ek açıklamaları aracılığıyla yapılandırma yeteneğini destekler.</span><span class="sxs-lookup"><span data-stu-id="95a86-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="95a86-108">Ancak varlıktan farklı türleri, bunlar:</span><span class="sxs-lookup"><span data-stu-id="95a86-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="95a86-109">Tanımlanmamış bir anahtarı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="95a86-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="95a86-110">Üzerinde değişiklikler için hiçbir zaman izlenir _DbContext_ ve bu nedenle hiçbir zaman eklenir, güncelleştirilmiş veya veritabanında silindi.</span><span class="sxs-lookup"><span data-stu-id="95a86-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="95a86-111">Hiçbir zaman kurala göre bulunur.</span><span class="sxs-lookup"><span data-stu-id="95a86-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="95a86-112">Yalnızca bir alt kümesini Gezinti eşleme özelliklerini - özellikle destekler:</span><span class="sxs-lookup"><span data-stu-id="95a86-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="95a86-113">Bunlar, hiçbir zaman bir ilişkinin asıl ucu çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="95a86-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="95a86-114">Bunlar yalnızca varlıklara işaret eden başvuru Gezinti özellikleri de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="95a86-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="95a86-115">Varlık Gezinti özellikleri için sorgu türleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="95a86-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="95a86-116">Üzerinde ele _ModelBuilder_ kullanarak `Query` yöntemi yerine `Entity` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="95a86-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="95a86-117">Üzerinde eşlenen _DbContext_ türünün özelliklerini aracılığıyla `DbQuery<T>` yerine `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="95a86-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="95a86-118">Kullanarak veritabanı nesneleri eşlenen `ToView` yöntemi yerine `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="95a86-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="95a86-119">Eşlenmiş bir _sorgu tanımlama_ - A olan bir sorgu türü için bir veri kaynağı görevi gören modelinde bildirilen ikincil bir sorgu sorgu tanımlama.</span><span class="sxs-lookup"><span data-stu-id="95a86-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="95a86-120">Sorgu türleri için temel kullanım senaryoları bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="95a86-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="95a86-121">Dönüş türü için geçici hizmet veren `FromSql()` sorgular.</span><span class="sxs-lookup"><span data-stu-id="95a86-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="95a86-122">Veritabanı görünümlerine eşleme.</span><span class="sxs-lookup"><span data-stu-id="95a86-122">Mapping to database views.</span></span>
- <span data-ttu-id="95a86-123">Tanımlı birincil anahtarı olmayan tablolar için eşleme.</span><span class="sxs-lookup"><span data-stu-id="95a86-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="95a86-124">Model içinde tanımlanan sorgulara eşleme.</span><span class="sxs-lookup"><span data-stu-id="95a86-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="95a86-125">Bir veritabanı nesnesi için bir sorgu türü eşleme elde edilir kullanarak `ToView` fluent API.</span><span class="sxs-lookup"><span data-stu-id="95a86-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="95a86-126">EF çekirdek açısından bakıldığında, bu yöntemi, belirtilen veritabanı nesnesidir bir _Görünüm_, yani bir salt okunur sorgu kaynağı olarak kabul edilir ve güncelleştirme hedefi, eklenemiyor veya silme işlemleri.</span><span class="sxs-lookup"><span data-stu-id="95a86-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="95a86-127">Ancak, bu gelmez veritabanı nesne bir veritabanı görünümü olması gerektiğine - alternatif olarak salt okunur olarak kabul edilecek bir veritabanı tablosu olabilir.</span><span class="sxs-lookup"><span data-stu-id="95a86-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="95a86-128">Buna karşılık, varlık türleri için bir veritabanı nesnesi içinde belirtilen EF çekirdek varsayar `ToTable` yöntemi kabul bir _tablo_, bir sorgu kaynağı olarak kullanılabilir ancak aynı zamanda güncelleştirme tarafından hedeflenmiş silme anlamına gelir ve Ekle işlemler.</span><span class="sxs-lookup"><span data-stu-id="95a86-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="95a86-129">Aslında, bir veritabanı görünümünde adını belirtebilirsiniz `ToTable` ve görünüm veritabanında güncelleştirilebilir için yapılandırılmış olduğu sürece her şeyi sorunsuz çalışması gerekir.</span><span class="sxs-lookup"><span data-stu-id="95a86-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="95a86-130">Örnek</span><span class="sxs-lookup"><span data-stu-id="95a86-130">Example</span></span>

<span data-ttu-id="95a86-131">Aşağıdaki örnek sorgu türü bir veritabanı görünümü sorgulamak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="95a86-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="95a86-132">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) github'da.</span><span class="sxs-lookup"><span data-stu-id="95a86-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="95a86-133">İlk olarak, basit bir Blog ve Post modelinin tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="95a86-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="95a86-134">Ardından, bize her blog ile ilişkili gönderileri sayısı sorgulamaya izin veren basit veritabanı görünümü tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="95a86-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="95a86-135">Ardından, veritabanı görünümü sonucundan tutmak için bir sınıf tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="95a86-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="95a86-136">Ardından, biz sorgu türünde yapılandırın _OnModelCreating_ kullanarak `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="95a86-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="95a86-137">Sorgu türü eşlemeyi yapılandırmak için standart fluent yapılandırma API'leri kullanın:</span><span class="sxs-lookup"><span data-stu-id="95a86-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="95a86-138">Son olarak, biz veritabanı görünümü standart şekilde sorgulama yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="95a86-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="95a86-139">Biz de içerik düzeyi sorgu özelliği (DbQuery) bu tür sorguları için bir kök olarak görev yapması için tanımladığınız unutmayın.</span><span class="sxs-lookup"><span data-stu-id="95a86-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
