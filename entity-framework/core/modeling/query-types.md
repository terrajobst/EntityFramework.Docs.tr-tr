---
title: "Sorgu türleri - EF çekirdek"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 19a371c65da33e8209cc1ab3423a67c34ddae61e
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="query-types"></a><span data-ttu-id="d4412-102">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="d4412-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="d4412-103">Bu özellik EF çekirdek 2.1 yenilikler</span><span class="sxs-lookup"><span data-stu-id="d4412-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="d4412-104">Sorgu, EF çekirdek modeli eklenebilir salt okunur sorgu sonuç türleri türleridir.</span><span class="sxs-lookup"><span data-stu-id="d4412-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="d4412-105">Sorgu türleri geçici (anonim türleri gibi) sorgulama yapmayı etkinleştirmek, ancak belirtilen eşleme yapılandırması olduğundan daha esnektir.</span><span class="sxs-lookup"><span data-stu-id="d4412-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="d4412-106">Bunlar, varlık türleri için kavramsal olarak benzerdir:</span><span class="sxs-lookup"><span data-stu-id="d4412-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="d4412-107">Model ya da eklenen POCO C# türleri oldukları ```OnModelCreating``` kullanarak ```ModelBuilder.Query``` yöntemi, veya bir DbContext "Ayarla" özelliği aracılığıyla (olarak sorgu böyle bir özellik türleri için yazılan ```DbQuery<T>``` yerine, ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="d4412-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather that ```DbSet<T>```).</span></span>
- <span data-ttu-id="d4412-108">Bunlar normal varlık türü olarak aynı eşleme özelliklerinin çoğunu destekler.</span><span class="sxs-lookup"><span data-stu-id="d4412-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="d4412-109">Örneğin, devralma eşleme, gezintilerini (limitiations aşağıya bakın) ve ilişkisel depoları, hedef veritabanı şema nesnelerindeki aracılığıyla yapılandırma yeteneğini üzerinde ```ToTable```, ```HasColumn``` fluent API yöntemlerini (veya veri ek açıklamaları).</span><span class="sxs-lookup"><span data-stu-id="d4412-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="d4412-110">Sorgu türleri varlıktan farklı türleri, bunlar:</span><span class="sxs-lookup"><span data-stu-id="d4412-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="d4412-111">Tanımlanmamış bir anahtarı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="d4412-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="d4412-112">Hiçbir zaman değişikliği İzleyicisi tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="d4412-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="d4412-113">Hiçbir zaman kurala göre bulunur.</span><span class="sxs-lookup"><span data-stu-id="d4412-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="d4412-114">Yalnızca bir alt kümesini Gezinti eşleme özelliklerini - özellikle destek, hiçbir zaman bir ilişkinin asıl ucu çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="d4412-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="d4412-115">Eşlenmiş bir _sorgu tanımlama_ -A tanımlama sorgudur sorgu türü için bir veri kaynağı görevi gören bir ikincil sorgu.</span><span class="sxs-lookup"><span data-stu-id="d4412-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="d4412-116">Sorgu türleri için temel kullanım senaryoları bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d4412-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="d4412-117">Veritabanı görünümlerine eşleme.</span><span class="sxs-lookup"><span data-stu-id="d4412-117">Mapping to database views.</span></span>
- <span data-ttu-id="d4412-118">Tanımlı birincil anahtarı olmayan tablolar için eşleme.</span><span class="sxs-lookup"><span data-stu-id="d4412-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="d4412-119">Dönüş türü için geçici hizmet veren ```FromSql()``` sorgular.</span><span class="sxs-lookup"><span data-stu-id="d4412-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="d4412-120">Model içinde tanımlanan sorgulara eşleme.</span><span class="sxs-lookup"><span data-stu-id="d4412-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="d4412-121">Bir veritabanı görünümü için bir sorgu türü eşleme elde edilir kullanarak ```ToTable``` fluent API.</span><span class="sxs-lookup"><span data-stu-id="d4412-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="d4412-122">Örnek</span><span class="sxs-lookup"><span data-stu-id="d4412-122">Example</span></span>

<span data-ttu-id="d4412-123">Aşağıdaki örnek sorgu türü bir veritabanı görünümü sorgulamak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d4412-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="d4412-124">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) github'da.</span><span class="sxs-lookup"><span data-stu-id="d4412-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="d4412-125">İlk olarak, basit bir Blog ve Post modelinin tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="d4412-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="d4412-126">Ardından, bize her blog ile ilişkili gönderileri sayısı sorgulamaya izin veren basit veritabanı görünümü tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="d4412-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="d4412-127">Ardından, veritabanı görünümü sonucundan tutmak için bir sınıf tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="d4412-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="d4412-128">Ardından, biz sorgu türünde yapılandırın _OnModelCreating_ kullanarak ```modelBuilder.Query<T>``` API.</span><span class="sxs-lookup"><span data-stu-id="d4412-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="d4412-129">Sorgu türü eşlemeyi yapılandırmak için standart fluent yapılandırma API'leri kullanın:</span><span class="sxs-lookup"><span data-stu-id="d4412-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="d4412-130">Son olarak, biz veritabanı görünümü standart şekilde sorgulama yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d4412-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="d4412-131">Biz de içerik düzeyi sorgu özelliği (DbQuery) bu tür sorguları için bir kök olarak görev yapması için tanımladığınız unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d4412-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
