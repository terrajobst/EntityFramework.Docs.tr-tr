---
title: Varlık özellikleri-EF Core
description: Entity Framework Core kullanarak varlık özelliklerini yapılandırma ve eşleme
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417218"
---
# <a name="entity-properties"></a><span data-ttu-id="d9fca-103">Varlık Özellikleri</span><span class="sxs-lookup"><span data-stu-id="d9fca-103">Entity Properties</span></span>

<span data-ttu-id="d9fca-104">Modelinizdeki her varlık türü bir özellikler kümesine sahiptir ve bu EF Core veritabanından okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="d9fca-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="d9fca-105">İlişkisel bir veritabanı kullanıyorsanız, varlık özellikleri tablo sütunlarıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="d9fca-106">Dahil edilen ve dışlanan Özellikler</span><span class="sxs-lookup"><span data-stu-id="d9fca-106">Included and excluded properties</span></span>

<span data-ttu-id="d9fca-107">Kurala göre, bir alıcı ve ayarlayıcı içeren tüm ortak özellikler modele dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="d9fca-108">Belirli özellikler aşağıdaki gibi dışarıda bırakılabilirler:</span><span class="sxs-lookup"><span data-stu-id="d9fca-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="d9fca-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d9fca-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-api"></a>[<span data-ttu-id="d9fca-110">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d9fca-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="d9fca-111">Sütun adları</span><span class="sxs-lookup"><span data-stu-id="d9fca-111">Column names</span></span>

<span data-ttu-id="d9fca-112">Kurala göre, ilişkisel bir veritabanı kullanılırken varlık özellikleri, özelliği ile aynı ada sahip tablo sütunlarına eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="d9fca-113">Sütunlarınızı farklı adlarla yapılandırmayı tercih ediyorsanız, bunu aşağıdaki şekilde yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d9fca-113">If you prefer to configure your columns with different names, you can do so as following:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="d9fca-114">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d9fca-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-api"></a>[<span data-ttu-id="d9fca-115">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d9fca-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="d9fca-116">Sütun veri türleri</span><span class="sxs-lookup"><span data-stu-id="d9fca-116">Column data types</span></span>

<span data-ttu-id="d9fca-117">İlişkisel bir veritabanı kullanılırken, veritabanı sağlayıcısı özelliğin .NET türüne göre bir veri türü seçer.</span><span class="sxs-lookup"><span data-stu-id="d9fca-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="d9fca-118">Ayrıca, yapılandırılmış [Maksimum uzunluk](#maximum-length), özelliğin birincil bir anahtarın parçası olup olmadığı gibi diğer meta veriler de hesaba girer.</span><span class="sxs-lookup"><span data-stu-id="d9fca-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="d9fca-119">Örneğin, SQL Server `DateTime` özelliklerini `datetime2(7)` sütunlara ve `string` özelliklerini `nvarchar(max)` sütunlara (veya anahtar olarak kullanılan özellikler için `nvarchar(450)`) eşler.</span><span class="sxs-lookup"><span data-stu-id="d9fca-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="d9fca-120">Sütunları, sütun için tam bir veri türü belirtmek üzere de yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9fca-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="d9fca-121">Örneğin, aşağıdaki kod, `Url` en fazla `200` olan Unicode olmayan bir dize olarak ve `2``5` ve ölçeği ölçeklendirerek ondalık olarak `Rating`:</span><span class="sxs-lookup"><span data-stu-id="d9fca-121">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="d9fca-122">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d9fca-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-api"></a>[<span data-ttu-id="d9fca-123">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d9fca-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="d9fca-124">Maksimum uzunluk</span><span class="sxs-lookup"><span data-stu-id="d9fca-124">Maximum length</span></span>

<span data-ttu-id="d9fca-125">En büyük uzunluk yapılandırması, belirli bir özellik için seçim yapmak üzere uygun sütun veri türü hakkında veritabanı sağlayıcısına bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="d9fca-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="d9fca-126">Maksimum uzunluk yalnızca `string` ve `byte[]`gibi dizi veri türleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="d9fca-127">Entity Framework, verileri sağlayıcıya geçirmeden önce en fazla uzunluk doğrulaması yapmaz.</span><span class="sxs-lookup"><span data-stu-id="d9fca-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="d9fca-128">Uygun olup olmadığını doğrulamak için sağlayıcıya veya veri deposuna kadar olur.</span><span class="sxs-lookup"><span data-stu-id="d9fca-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="d9fca-129">Örneğin, SQL Server hedeflenirken, en fazla uzunluğu aşarak temel alınan sütunun veri türü fazla verilerin depolanmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="d9fca-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="d9fca-130">Aşağıdaki örnekte, 500 uzunluk üst sınırını yapılandırmak SQL Server üzerinde `nvarchar(500)` türünde bir sütunun oluşturulmasına neden olur:</span><span class="sxs-lookup"><span data-stu-id="d9fca-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="d9fca-131">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d9fca-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="d9fca-132">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d9fca-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="d9fca-133">Gerekli ve isteğe bağlı özellikler</span><span class="sxs-lookup"><span data-stu-id="d9fca-133">Required and optional properties</span></span>

<span data-ttu-id="d9fca-134">Özelliği, `null`içermesi için geçerliyse, isteğe bağlı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-134">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="d9fca-135">`null` bir özelliğe atanacak geçerli bir değer değilse, gerekli bir özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-135">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="d9fca-136">İlişkisel bir veritabanı şemasına eşleme yaparken, gerekli özellikler null yapılamayan sütunlar olarak oluşturulur ve isteğe bağlı özellikler null yapılabilir sütunlar olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d9fca-136">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="d9fca-137">Kurallar</span><span class="sxs-lookup"><span data-stu-id="d9fca-137">Conventions</span></span>

<span data-ttu-id="d9fca-138">Kurala göre, .NET türü null içerebilen bir özellik isteğe bağlı olarak yapılandırılır, ancak .NET türü null değer içermeyen özellikler gerekli olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d9fca-138">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="d9fca-139">Örneğin, .NET değer türleri (`int`, `decimal`, `bool`, vb.) olan tüm özellikler gerekli olarak yapılandırılır ve null yapılabilir .NET değer türleri (`int?`, `decimal?`, `bool?`, vb.) tüm özellikler isteğe bağlı olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d9fca-139">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="d9fca-140">C#8, başvuru türlerinin açıklanmasına izin veren, null [olabilen başvuru](/dotnet/csharp/tutorials/nullable-reference-types)türleri olarak adlandırılan yeni bir özellik sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="d9fca-140">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="d9fca-141">Bu özellik varsayılan olarak devre dışıdır ve etkinleştirilirse, EF Core davranışını aşağıdaki şekilde değiştirir:</span><span class="sxs-lookup"><span data-stu-id="d9fca-141">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="d9fca-142">Null yapılabilir başvuru türleri devre dışıysa (varsayılan), .NET başvuru türleri olan tüm özellikler, kurala göre isteğe bağlı olarak yapılandırılır (ör. `string`).</span><span class="sxs-lookup"><span data-stu-id="d9fca-142">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="d9fca-143">Null yapılabilir başvuru türleri etkinleştirilirse, Özellikler .NET türlerinden C# null değer verilebilirliği temel alınarak yapılandırılır: `string?` isteğe bağlı olarak yapılandırılır, ancak `string` gerekli olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d9fca-143">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="d9fca-144">Aşağıdaki örnek, null olabilen başvuru özelliği devre dışı (varsayılan) ve etkin olarak, gerekli ve isteğe bağlı özelliklerle bir varlık türü gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-144">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-default"></a>[<span data-ttu-id="d9fca-145">Nullable başvuru türleri olmadan (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="d9fca-145">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-types"></a>[<span data-ttu-id="d9fca-146">Null yapılabilir başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="d9fca-146">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="d9fca-147">Kod içinde C# ifade edilen null değer alabilme durumunu EF Core modeli ve veritabanına akıdığından ve aynı kavramı iki kez ifade etmek Için, akıcı API veya veri ek açıklamalarını obviates, Nullable başvuru türlerini kullanmak önerilir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-147">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="d9fca-148">Mevcut bir projede null yapılabilir başvuru türlerini etkinleştirirken dikkatli olun: daha önce isteğe bağlı olarak yapılandırılmış olan başvuru türü özellikleri, açıkça null olarak açıklanmadığı sürece artık gerekli olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d9fca-148">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="d9fca-149">İlişkisel bir veritabanı şemasını yönetirken, bu, veritabanı sütununun null değer alabilme durumu belirleyen geçişlerin oluşturulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="d9fca-149">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="d9fca-150">Null yapılabilir başvuru türleri hakkında daha fazla bilgi ve bunların EF Core ile nasıl kullanılacağı hakkında daha fazla bilgi için, [Bu özellik için adanmış belgeler sayfasına bakın](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="d9fca-150">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="d9fca-151">Açık yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d9fca-151">Explicit configuration</span></span>

<span data-ttu-id="d9fca-152">Kurala göre isteğe bağlı olacak bir özellik, aşağıdaki gibi gerekli olacak şekilde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="d9fca-152">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="d9fca-153">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d9fca-153">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="d9fca-154">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d9fca-154">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
