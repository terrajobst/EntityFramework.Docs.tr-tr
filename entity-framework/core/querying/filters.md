---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 4afc9fb0338d34845639d57013ac710445321940
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562448"
---
# <a name="global-query-filters"></a><span data-ttu-id="8b77c-102">Genel sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="8b77c-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="8b77c-103">Bu özellik EF Core 2.0 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="8b77c-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="8b77c-104">Genel sorgu filtreleri olan LINQ Sorgu koşullarına (bir Boole ifadesi, LINQ to genellikle geçirilen *burada* sorgu işleci) meta veri modeli varlık türlerine uygulanır (genellikle *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="8b77c-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="8b77c-105">Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8b77c-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="8b77c-106">Bu özelliğin bazı ortak uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="8b77c-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="8b77c-107">**Geçici silme** -bir varlık türü tanımlayan bir *IsDeleted* özelliği.</span><span class="sxs-lookup"><span data-stu-id="8b77c-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="8b77c-108">**Çok kiracılılık** -bir varlık türü tanımlayan bir *Tenantıd* özelliği.</span><span class="sxs-lookup"><span data-stu-id="8b77c-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="8b77c-109">Örnek</span><span class="sxs-lookup"><span data-stu-id="8b77c-109">Example</span></span>

<span data-ttu-id="8b77c-110">Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="8b77c-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="8b77c-111">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8b77c-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="8b77c-112">İlk olarak, varlıklar tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="8b77c-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="8b77c-113">Bir _ bildirimi Not_Tenantıd_ alanını _Blog_ varlık.</span><span class="sxs-lookup"><span data-stu-id="8b77c-113">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="8b77c-114">Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8b77c-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="8b77c-115">Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü.</span><span class="sxs-lookup"><span data-stu-id="8b77c-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="8b77c-116">Bu izlemenin olup için kullanılan bir _Post_ örneği "geçici silinen".</span><span class="sxs-lookup"><span data-stu-id="8b77c-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="8b77c-117">Diğer bir deyişle, örnek, temel alınan verileri fiziksel olarak kaldırmadan silindi olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="8b77c-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="8b77c-118">Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="8b77c-118">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="8b77c-119">Koşul ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak LINQ sorguları bu türleri için.</span><span class="sxs-lookup"><span data-stu-id="8b77c-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="8b77c-120">DbContext örnek düzeyinde bir alanı kullanımına dikkat edin: ```_tenantId``` geçerli Kiracı ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8b77c-120">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="8b77c-121">Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten örnek) doğru bağlam örneğinin değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b77c-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="8b77c-122">Filtreleri devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="8b77c-122">Disabling Filters</span></span>

<span data-ttu-id="8b77c-123">Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak ```IgnoreQueryFilters()``` işleci.</span><span class="sxs-lookup"><span data-stu-id="8b77c-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="8b77c-124">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="8b77c-124">Limitations</span></span>

<span data-ttu-id="8b77c-125">Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="8b77c-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="8b77c-126">Filtreler, gezinti özellikleri başvurular içeremez.</span><span class="sxs-lookup"><span data-stu-id="8b77c-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="8b77c-127">Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="8b77c-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
