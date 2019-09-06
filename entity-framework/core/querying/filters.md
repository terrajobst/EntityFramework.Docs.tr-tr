---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: c9bbb8a5889834ea078ddb7e432863b3d0cf2ffe
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271452"
---
# <a name="global-query-filters"></a><span data-ttu-id="f4a08-102">Genel sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="f4a08-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="f4a08-103">Bu özellik EF Core 2,0 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="f4a08-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="f4a08-104">Genel sorgu filtreleri olan LINQ Sorgu koşullarına (bir Boole ifadesi, LINQ to genellikle geçirilen *burada* sorgu işleci) meta veri modeli varlık türlerine uygulanır (genellikle *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="f4a08-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="f4a08-105">Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f4a08-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="f4a08-106">Bu özelliğin bazı ortak uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f4a08-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="f4a08-107">**Geçici silme** -bir varlık türü tanımlayan bir *IsDeleted* özelliği.</span><span class="sxs-lookup"><span data-stu-id="f4a08-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="f4a08-108">**Çok kiracılılık** -bir varlık türü tanımlayan bir *Tenantıd* özelliği.</span><span class="sxs-lookup"><span data-stu-id="f4a08-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="f4a08-109">Örnek</span><span class="sxs-lookup"><span data-stu-id="f4a08-109">Example</span></span>

<span data-ttu-id="f4a08-110">Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="f4a08-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="f4a08-111">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="f4a08-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="f4a08-112">İlk olarak, varlıklar tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="f4a08-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="f4a08-113">_Blog_ varlığındaki bir _tenantıd_ alanının bildirimine göz önünde varın.</span><span class="sxs-lookup"><span data-stu-id="f4a08-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="f4a08-114">Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f4a08-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="f4a08-115">Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü.</span><span class="sxs-lookup"><span data-stu-id="f4a08-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="f4a08-116">Bu izlemenin olup için kullanılan bir _Post_ örneği "geçici silinen".</span><span class="sxs-lookup"><span data-stu-id="f4a08-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="f4a08-117">Diğer bir deyişle, örnek, temel alınan verileri fiziksel olarak kaldırmadan silindi olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="f4a08-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="f4a08-118">Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak `HasQueryFilter` API.</span><span class="sxs-lookup"><span data-stu-id="f4a08-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="f4a08-119">Koşul ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak LINQ sorguları bu türleri için.</span><span class="sxs-lookup"><span data-stu-id="f4a08-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="f4a08-120">DbContext örnek düzeyinde bir alanı kullanımına dikkat edin: `_tenantId` geçerli Kiracı ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f4a08-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="f4a08-121">Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten örnek) doğru bağlam örneğinin değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="f4a08-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="f4a08-122">Aynı varlık üzerinde birden çok sorgu filtresi tanımlamak mümkün değildir; yalnızca en son bir değer geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="f4a08-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="f4a08-123">Ancak, mantıksal _ve_ işlecini kullanarak birden çok koşuldan oluşan tek bir filtre tanımlayabilirsiniz ([ `&&` içinde C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span><span class="sxs-lookup"><span data-stu-id="f4a08-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="f4a08-124">Filtreleri devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="f4a08-124">Disabling Filters</span></span>

<span data-ttu-id="f4a08-125">Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak `IgnoreQueryFilters()` işleci.</span><span class="sxs-lookup"><span data-stu-id="f4a08-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="f4a08-126">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="f4a08-126">Limitations</span></span>

<span data-ttu-id="f4a08-127">Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="f4a08-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="f4a08-128">Filtreler, gezinti özellikleri başvurular içeremez.</span><span class="sxs-lookup"><span data-stu-id="f4a08-128">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="f4a08-129">Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f4a08-129">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
