---
title: Gerekli ve Isteğe bağlı özellikler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655658"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="e4658-102">Gerekli ve İsteğe Bağlı Özellikler</span><span class="sxs-lookup"><span data-stu-id="e4658-102">Required and Optional Properties</span></span>

<span data-ttu-id="e4658-103">Özelliği, `null`içermesi için geçerliyse, isteğe bağlı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e4658-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="e4658-104">`null` bir özelliğe atanacak geçerli bir değer değilse, gerekli bir özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e4658-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="e4658-105">İlişkisel bir veritabanı şemasına eşleme yaparken, gerekli özellikler null yapılamayan sütunlar olarak oluşturulur ve isteğe bağlı özellikler null yapılabilir sütunlar olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e4658-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="e4658-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="e4658-106">Conventions</span></span>

<span data-ttu-id="e4658-107">Kurala göre, .NET türü null içerebilen bir özellik isteğe bağlı olarak yapılandırılır, ancak .NET türü null değer içermeyen özellikler gerekli olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="e4658-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="e4658-108">Örneğin, .NET değer türleri (`int`, `decimal`, `bool`, vb.) olan tüm özellikler gerekli olarak yapılandırılır ve null yapılabilir .NET değer türleri (`int?`, `decimal?`, `bool?`, vb.) tüm özellikler isteğe bağlı olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="e4658-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="e4658-109">C#8, başvuru türlerinin açıklanmasına izin veren, null [olabilen başvuru](/dotnet/csharp/tutorials/nullable-reference-types)türleri olarak adlandırılan yeni bir özellik sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="e4658-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="e4658-110">Bu özellik varsayılan olarak devre dışıdır ve etkinleştirilirse, EF Core davranışını aşağıdaki şekilde değiştirir:</span><span class="sxs-lookup"><span data-stu-id="e4658-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="e4658-111">Null yapılabilir başvuru türleri devre dışıysa (varsayılan), .NET başvuru türleri olan tüm özellikler, kurala göre isteğe bağlı olarak yapılandırılır (ör. `string`).</span><span class="sxs-lookup"><span data-stu-id="e4658-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="e4658-112">Null yapılabilir başvuru türleri etkinleştirilirse, Özellikler .NET türlerinden C# null değer verilebilirliği temel alınarak yapılandırılır: `string?` isteğe bağlı olarak yapılandırılır, ancak `string` gerekli olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="e4658-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="e4658-113">Aşağıdaki örnek, null olabilen başvuru özelliği devre dışı (varsayılan) ve etkin olarak, gerekli ve isteğe bağlı özelliklerle bir varlık türü gösterir.</span><span class="sxs-lookup"><span data-stu-id="e4658-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="e4658-114">Nullable başvuru türleri olmadan (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="e4658-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="e4658-115">Null yapılabilir başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="e4658-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="e4658-116">Kod içinde C# ifade edilen null değer alabilme durumunu EF Core modeli ve veritabanına akıdığından ve aynı kavramı iki kez ifade etmek Için, akıcı API veya veri ek açıklamalarını obviates, Nullable başvuru türlerini kullanmak önerilir.</span><span class="sxs-lookup"><span data-stu-id="e4658-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="e4658-117">Mevcut bir projede null yapılabilir başvuru türlerini etkinleştirirken dikkatli olun: daha önce isteğe bağlı olarak yapılandırılmış olan başvuru türü özellikleri, açıkça null olarak açıklanmadığı sürece artık gerekli olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="e4658-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="e4658-118">İlişkisel bir veritabanı şemasını yönetirken, bu, veritabanı sütununun null değer alabilme durumu belirleyen geçişlerin oluşturulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e4658-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="e4658-119">Null yapılabilir başvuru türleri hakkında daha fazla bilgi ve bunların EF Core ile nasıl kullanılacağı hakkında daha fazla bilgi için, [Bu özellik için adanmış belgeler sayfasına bakın](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="e4658-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="e4658-120">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e4658-120">Configuration</span></span>

<span data-ttu-id="e4658-121">Kurala göre isteğe bağlı olacak bir özellik, aşağıdaki gibi gerekli olacak şekilde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="e4658-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="e4658-122">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="e4658-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="e4658-123">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="e4658-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
