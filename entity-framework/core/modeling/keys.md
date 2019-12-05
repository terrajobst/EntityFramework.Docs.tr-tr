---
title: Anahtarlar (birincil)-EF Core
description: Entity Framework Core kullanırken varlık türleri için anahtarlar yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824614"
---
# <a name="keys-primary"></a><span data-ttu-id="aec61-103">Anahtarlar (birincil)</span><span class="sxs-lookup"><span data-stu-id="aec61-103">Keys (primary)</span></span>

<span data-ttu-id="aec61-104">Bir anahtar, her varlık örneği için birincil benzersiz tanımlayıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="aec61-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="aec61-105">İlişkisel bir veritabanı kullanılırken bu, *birincil anahtar*kavramıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="aec61-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="aec61-106">Birincil anahtar olmayan benzersiz bir tanımlayıcı da yapılandırabilirsiniz (daha fazla bilgi için bkz. [Alternatif anahtarlar](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="aec61-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="aec61-107">Aşağıdaki yöntemlerden biri, bir birincil anahtar ayarlamak/oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aec61-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="aec61-108">Kurallar</span><span class="sxs-lookup"><span data-stu-id="aec61-108">Conventions</span></span>

<span data-ttu-id="aec61-109">Varsayılan olarak, `Id` veya `<type name>Id` adlı bir özellik bir varlığın anahtarı olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="aec61-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="aec61-110">[Sahip olan varlık türleri](xref:core/modeling/owned-entities) anahtarları tanımlamak için farklı kurallar kullanır.</span><span class="sxs-lookup"><span data-stu-id="aec61-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="aec61-111">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="aec61-111">Data Annotations</span></span>

<span data-ttu-id="aec61-112">Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="aec61-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="aec61-113">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="aec61-113">Fluent API</span></span>

<span data-ttu-id="aec61-114">Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="aec61-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="aec61-115">Ayrıca, birden çok özelliği bir varlığın anahtarı olacak şekilde (bileşik anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="aec61-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="aec61-116">Bileşik anahtarlar yalnızca akıcı API kullanılarak yapılandırılabilir-kurallar hiçbir şekilde bir bileşik anahtar kurmayacaktır ve veri açıklamalarını yapılandırmak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="aec61-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="aec61-117">Anahtar türleri ve değerleri</span><span class="sxs-lookup"><span data-stu-id="aec61-117">Key types and values</span></span>

<span data-ttu-id="aec61-118">EF Core, `string`, `Guid`, `byte[]` ve diğerleri dahil olmak üzere herhangi bir ilkel türün özelliklerinin, birincil anahtar olarak kullanılmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="aec61-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="aec61-119">Ancak tüm veritabanları bunları desteklemez.</span><span class="sxs-lookup"><span data-stu-id="aec61-119">But not all databases support them.</span></span> <span data-ttu-id="aec61-120">Bazı durumlarda, anahtar değerleri otomatik olarak desteklenen bir türe dönüştürülebilir, aksi takdirde dönüştürmenin [el ile belirtilmesi](xref:core/modeling/value-conversions)gerekir.</span><span class="sxs-lookup"><span data-stu-id="aec61-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="aec61-121">Bağlam için yeni bir varlık eklenirken anahtar özellikler her zaman varsayılan olmayan bir değere sahip olmalıdır, ancak bazı türler [veritabanı tarafından oluşturulacaktır](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="aec61-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="aec61-122">Bu durumda, varlık izleme amacıyla eklendiğinde EF geçici bir değer oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="aec61-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="aec61-123">[SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) çağrıldıktan sonra, geçici değer veritabanı tarafından oluşturulan değerle değiştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="aec61-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="aec61-124">Bir anahtar özelliğin veritabanı tarafından oluşturulmuş bir değeri varsa ve bir varlık eklendiğinde varsayılan olmayan bir değer belirtilmişse EF, varlığın veritabanında zaten var olduğunu varsayar ve yeni bir tane eklemek yerine bu alanı güncelleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="aec61-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="aec61-125">Bu değer oluşturmayı devre dışı bırakmak veya [üretilen özellikler için nasıl açık değerler belirteceğinize](../saving/explicit-values-generated-properties.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="aec61-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>