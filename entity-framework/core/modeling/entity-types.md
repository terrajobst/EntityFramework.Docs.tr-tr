---
title: Varlık türleri-EF Core
description: Entity Framework Core kullanarak varlık türlerini yapılandırma ve eşleme
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417235"
---
# <a name="entity-types"></a><span data-ttu-id="69c73-103">Varlık Türleri</span><span class="sxs-lookup"><span data-stu-id="69c73-103">Entity Types</span></span>

<span data-ttu-id="69c73-104">Bağlam üzerindeki bir DbSet türü de dahil olmak, EF Core modeline dahil olduğu anlamına gelir; genellikle *varlık*olarak böyle bir türe başvurduk.</span><span class="sxs-lookup"><span data-stu-id="69c73-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="69c73-105">EF Core, ve veritabanından varlık örnekleri okuyup yazabilir ve ilişkisel bir veritabanı kullanıyorsanız, geçişler aracılığıyla varlıklarınız için tablolar oluşturabilir EF Core.</span><span class="sxs-lookup"><span data-stu-id="69c73-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="69c73-106">Modeldeki türleri ekleme</span><span class="sxs-lookup"><span data-stu-id="69c73-106">Including types in the model</span></span>

<span data-ttu-id="69c73-107">Kurala göre, bağlamdaki DbSet özelliklerinde sunulan türler, modele varlık olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="69c73-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="69c73-108">`OnModelCreating` yönteminde belirtilen varlık türleri, diğer keşfedilen varlık türlerinin gezinti özelliklerini yinelemeli olarak inceleyerek bulunan her türlü tür olduğu gibi da dahil edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="69c73-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="69c73-109">Aşağıdaki kod örneğinde, tüm türler dahil edilmiştir:</span><span class="sxs-lookup"><span data-stu-id="69c73-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="69c73-110">`Blog`, bağlamdaki bir DbSet özelliğinde kullanıma sunulduğundan dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="69c73-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="69c73-111">`Post`, `Blog.Posts` gezinti özelliği aracılığıyla bulunduğundan dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="69c73-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="69c73-112">`OnModelCreating`belirtildiğinden `AuditEntry`.</span><span class="sxs-lookup"><span data-stu-id="69c73-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="69c73-113">Modeldeki türler dışlanıyor</span><span class="sxs-lookup"><span data-stu-id="69c73-113">Excluding types from the model</span></span>

<span data-ttu-id="69c73-114">Modele bir türün dahil edilmesini istemiyorsanız, bu tür dışında bırakabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="69c73-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="69c73-115">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="69c73-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="69c73-116">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="69c73-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a><span data-ttu-id="69c73-117">Tablo adı</span><span class="sxs-lookup"><span data-stu-id="69c73-117">Table name</span></span>

<span data-ttu-id="69c73-118">Kural gereği, her varlık türü, varlığı sunan DbSet özelliğiyle aynı ada sahip bir veritabanı tablosuna eşlenecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="69c73-118">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="69c73-119">Belirtilen varlık için hiçbir DbSet yoksa, sınıf adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="69c73-119">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="69c73-120">Tablo adını el ile yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="69c73-120">You can manually configure the table name:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="69c73-121">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="69c73-121">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="69c73-122">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="69c73-122">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="69c73-123">Tablo şeması</span><span class="sxs-lookup"><span data-stu-id="69c73-123">Table schema</span></span>

<span data-ttu-id="69c73-124">İlişkisel bir veritabanı kullanılırken, tablolar veritabanınızın varsayılan şemasında oluşturulan kurala göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="69c73-124">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="69c73-125">Örneğin, Microsoft SQL Server `dbo` şemasını kullanır (SQLite şemaları desteklemez).</span><span class="sxs-lookup"><span data-stu-id="69c73-125">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="69c73-126">Belirli bir şemada oluşturulacak tabloları aşağıdaki gibi yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="69c73-126">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="69c73-127">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="69c73-127">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="69c73-128">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="69c73-128">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="69c73-129">Her tablo için şema belirtmek yerine, Fluent API model düzeyinde varsayılan şemayı de tanımlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="69c73-129">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="69c73-130">Varsayılan şemanın ayarlanması, diziler gibi diğer veritabanı nesnelerini de etkileyecek şekilde değişir.</span><span class="sxs-lookup"><span data-stu-id="69c73-130">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>
