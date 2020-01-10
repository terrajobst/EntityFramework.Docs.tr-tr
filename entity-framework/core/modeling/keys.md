---
title: Anahtarlar-EF Core
description: Entity Framework Core kullanırken varlık türleri için anahtarlar yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502012"
---
# <a name="keys"></a><span data-ttu-id="92f63-103">Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="92f63-103">Keys</span></span>

<span data-ttu-id="92f63-104">Bir anahtar, her varlık örneği için benzersiz bir tanımlayıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="92f63-104">A key serves as a unique identifier for each entity instance.</span></span> <span data-ttu-id="92f63-105">EF 'teki varlıkların çoğu, ilişkisel veritabanlarında *birincil anahtar* kavramıyla eşleşen tek bir anahtara sahiptir (anahtar içermeyen varlıklar için bkz. [Keyless varlıkları](xref:core/modeling/keyless-entity-types)).</span><span class="sxs-lookup"><span data-stu-id="92f63-105">Most entities in EF have a single key, which maps to the concept of a *primary key* in relational databases (for entities without keys, see [Keyless entities](xref:core/modeling/keyless-entity-types)).</span></span> <span data-ttu-id="92f63-106">Varlıklar birincil anahtarın ötesinde ek anahtarlara sahip olabilir (daha fazla bilgi için bkz. [Alternatif anahtarlar](#alternate-keys) ).</span><span class="sxs-lookup"><span data-stu-id="92f63-106">Entities can have additional keys beyond the primary key (see [Alternate Keys](#alternate-keys) for more information).</span></span>

<span data-ttu-id="92f63-107">Kurala göre, `Id` veya `<type name>Id` adlı bir özellik, bir varlığın birincil anahtarı olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="92f63-107">By convention, a property named `Id` or `<type name>Id` will be configured as the primary key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> <span data-ttu-id="92f63-108">[Sahip olan varlık türleri](xref:core/modeling/owned-entities) anahtarları tanımlamak için farklı kurallar kullanır.</span><span class="sxs-lookup"><span data-stu-id="92f63-108">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

<span data-ttu-id="92f63-109">Tek bir özelliği, bir varlığın birincil anahtarı olacak şekilde aşağıdaki gibi yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="92f63-109">You can configure a single property to be the primary key of an entity as follows:</span></span>

## <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="92f63-110">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="92f63-110">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="92f63-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="92f63-111">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

<span data-ttu-id="92f63-112">Ayrıca, birden çok özelliği bir varlığın anahtarı olacak şekilde yapılandırabilirsiniz. Bu bileşik anahtar olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="92f63-112">You can also configure multiple properties to be the key of an entity - this is known as a composite key.</span></span> <span data-ttu-id="92f63-113">Bileşik anahtarlar yalnızca Floent API 'SI kullanılarak yapılandırılabilir; kurallar hiçbir şekilde bir bileşik anahtar kurmayacaktır ve veri açıklamalarını yapılandırmak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="92f63-113">Composite keys can only be configured using the Fluent API; conventions will never setup a composite key, and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a><span data-ttu-id="92f63-114">Birincil anahtar adı</span><span class="sxs-lookup"><span data-stu-id="92f63-114">Primary key name</span></span>

<span data-ttu-id="92f63-115">Kurala göre, ilişkisel veritabanlarında birincil anahtarlar `PK_<type name>`adıyla oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="92f63-115">By convention, on relational databases primary keys are created with the name `PK_<type name>`.</span></span> <span data-ttu-id="92f63-116">Birincil anahtar kısıtlamasının adını şu şekilde yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="92f63-116">You can configure the name of the primary key constraint as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a><span data-ttu-id="92f63-117">Anahtar türleri ve değerleri</span><span class="sxs-lookup"><span data-stu-id="92f63-117">Key types and values</span></span>

<span data-ttu-id="92f63-118">EF Core, `string`, `Guid`, `byte[]` ve diğerleri dahil olmak üzere herhangi bir ilkel türün özelliklerinin kullanılmasını destekler, çünkü tüm veritabanları anahtar olarak tüm türleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="92f63-118">While EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others, not all databases support all types as keys.</span></span> <span data-ttu-id="92f63-119">Bazı durumlarda, anahtar değerleri otomatik olarak desteklenen bir türe dönüştürülebilir, aksi takdirde dönüştürmenin [el ile belirtilmesi](xref:core/modeling/value-conversions)gerekir.</span><span class="sxs-lookup"><span data-stu-id="92f63-119">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="92f63-120">Bağlam için yeni bir varlık eklenirken anahtar özellikler her zaman varsayılan olmayan bir değere sahip olmalıdır, ancak bazı türler [veritabanı tarafından oluşturulacaktır](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="92f63-120">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="92f63-121">Bu durumda, varlık izleme amacıyla eklendiğinde EF geçici bir değer oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="92f63-121">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="92f63-122">[SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) çağrıldıktan sonra, geçici değer veritabanı tarafından oluşturulan değerle değiştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="92f63-122">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="92f63-123">Bir anahtar özelliğin değeri veritabanı tarafından oluşturulduysa ve bir varlık eklendiğinde varsayılan olmayan bir değer belirtilmişse EF, varlığın veritabanında zaten var olduğunu varsayacaktır ve yenisini eklemek yerine, güncelleştirmeyi deneyebilir.</span><span class="sxs-lookup"><span data-stu-id="92f63-123">If a key property has its value generated by the database and a non-default value is specified when an entity is added, then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="92f63-124">Bu değer oluşturmayı devre dışı bırakmak veya [üretilen özellikler için nasıl açık değerler belirteceğinize](../saving/explicit-values-generated-properties.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="92f63-124">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>

## <a name="alternate-keys"></a><span data-ttu-id="92f63-125">Alternatif Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="92f63-125">Alternate Keys</span></span>

<span data-ttu-id="92f63-126">Alternatif bir anahtar, birincil anahtara ek olarak her bir varlık örneği için alternatif benzersiz tanımlayıcı işlevi görür; ilişki hedefi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="92f63-126">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key; it can be used as the target of a relationship.</span></span> <span data-ttu-id="92f63-127">İlişkisel bir veritabanı kullanırken, bu, diğer anahtar sütunları üzerinde benzersiz bir dizin/kısıtlama kavramına eşlenir ve sütunlara (ler) başvuran bir veya daha fazla yabancı anahtar kısıtlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="92f63-127">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]
> <span data-ttu-id="92f63-128">Yalnızca bir sütunda benzersizlik zorlamak istiyorsanız, alternatif bir anahtar yerine benzersiz bir dizin tanımlayın (bkz. [dizinler](indexes.md)).</span><span class="sxs-lookup"><span data-stu-id="92f63-128">If you just want to enforce uniqueness on a column, define a unique index rather than an alternate key (see [Indexes](indexes.md)).</span></span> <span data-ttu-id="92f63-129">EF 'de, alternatif anahtarlar salt okunurdur ve benzersiz dizinler üzerinde ek semantik sağlar çünkü bir yabancı anahtarın hedefi olarak kullanılabilirler.</span><span class="sxs-lookup"><span data-stu-id="92f63-129">In EF, alternate keys are read-only and provide additional semantics over unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="92f63-130">Diğer anahtarlar genellikle gerektiğinde sizin için sunulmuştur ve bunları el ile yapılandırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="92f63-130">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="92f63-131">Kural gereği, bir ilişkinin hedefi olarak birincil anahtar olmayan bir özelliği tanımlarken sizin için alternatif bir anahtar sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="92f63-131">By convention, an alternate key is introduced for you when you identify a property which isn't the primary key as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

<span data-ttu-id="92f63-132">Ayrıca, tek bir özelliği alternatif bir anahtar olacak şekilde yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="92f63-132">You can also configure a single property to be an alternate key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

<span data-ttu-id="92f63-133">Ayrıca, birden çok özelliği alternatif bir anahtar (bileşik alternatif anahtar olarak bilinir) olarak yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="92f63-133">You can also configure multiple properties to be an alternate key (known as a composite alternate key):</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

<span data-ttu-id="92f63-134">Son olarak, kurala göre, alternatif bir anahtar için tanıtılan dizin ve kısıtlama `AK_<type name>_<property name>` olarak adlandırılır (`<property name>` bileşik alternatif anahtarlar için, özellik adlarının alt çizgiyle ayrılmış bir listesi olur).</span><span class="sxs-lookup"><span data-stu-id="92f63-134">Finally, by convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>` (for composite alternate keys `<property name>` becomes an underscore separated list of property names).</span></span> <span data-ttu-id="92f63-135">Alternatif anahtarın dizin ve benzersiz kısıtlamasının adını yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="92f63-135">You can configure the name of the alternate key's index and unique constraint:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
