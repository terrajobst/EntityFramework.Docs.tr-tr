---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417731"
---
# <a name="global-query-filters"></a><span data-ttu-id="52437-102">Genel Sorgu Filtreleri</span><span class="sxs-lookup"><span data-stu-id="52437-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="52437-103">Bu özellik EF Core 2,0 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="52437-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="52437-104">Genel sorgu filtreleri LINQ sorgu koşullarına (genellikle LINQ *WHERE* sorgu işlecine geçirilen bir Boole ifadesi) meta veri modelindeki varlık türlerine (genellikle *onmodeloluþturma*içinde) uygulanan.</span><span class="sxs-lookup"><span data-stu-id="52437-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="52437-105">Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="52437-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="52437-106">Bu özelliğin bazı ortak uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="52437-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="52437-107">**Geçici silme** -bir varlık türü, *IsDeleted* özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="52437-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="52437-108">**Çok kiracılı** -varlık türü bir *tenantıd* özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="52437-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="52437-109">Örnek</span><span class="sxs-lookup"><span data-stu-id="52437-109">Example</span></span>

<span data-ttu-id="52437-110">Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="52437-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="52437-111">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="52437-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="52437-112">İlk olarak, varlıklar tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="52437-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="52437-113">_Blog_ varlığındaki bir _tenantıd_ alanının bildirimine göz önünde varın.</span><span class="sxs-lookup"><span data-stu-id="52437-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="52437-114">Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="52437-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="52437-115">Ayrıca, _Post_ varlık türünde bir _IsDeleted_ özelliği de tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="52437-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="52437-116">Bu, bir _Post_ örneğinin "geçici olarak silinmiş" olup olmadığını izlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="52437-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="52437-117">Diğer bir deyişle, örnek, temel alınan verileri fiziksel olarak kaldırmadan silindi olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="52437-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="52437-118">Sonra, `HasQueryFilter` API 'sini kullanarak _Onmodelyaratırken_ sorgu filtrelerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="52437-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="52437-119">_Hasqueryfilter_ çağrılarına geçirilen koşul ifadeleri artık bu türler için HERHANGI bir LINQ sorgusuna otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="52437-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="52437-120">DbContext örnek düzeyi alanının kullanımını, geçerli kiracıyı ayarlamak için kullanılan `_tenantId`.</span><span class="sxs-lookup"><span data-stu-id="52437-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="52437-121">Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten örnek) doğru bağlam örneğinin değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="52437-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="52437-122">Aynı varlık üzerinde birden çok sorgu filtresi tanımlamak mümkün değildir; yalnızca en son bir değer geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="52437-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="52437-123">Ancak, mantıksal _ve_ işlecini ([`&&` içinde C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) kullanarak birden çok koşuldan oluşan tek bir filtre tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="52437-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="52437-124">Filtreleri devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="52437-124">Disabling Filters</span></span>

<span data-ttu-id="52437-125">Filtreler, `IgnoreQueryFilters()` işleci kullanılarak tekil LINQ sorguları için devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="52437-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="52437-126">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="52437-126">Limitations</span></span>

<span data-ttu-id="52437-127">Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="52437-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="52437-128">Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="52437-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
