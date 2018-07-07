---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 0c7858d660665b4f17aedea2101452048f9aff25
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900297"
---
# <a name="global-query-filters"></a><span data-ttu-id="db53e-102">Genel sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="db53e-102">Global Query Filters</span></span>

<span data-ttu-id="db53e-103">Genel sorgu filtreleri olan LINQ Sorgu koşullarına (bir Boole ifadesi, LINQ to genellikle geçirilen *burada* sorgu işleci) meta veri modeli varlık türlerine uygulanır (genellikle *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="db53e-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="db53e-104">Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="db53e-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="db53e-105">Bu özelliğin bazı ortak uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="db53e-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="db53e-106">**Geçici silme** -bir varlık türü tanımlayan bir *IsDeleted* özelliği.</span><span class="sxs-lookup"><span data-stu-id="db53e-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="db53e-107">**Çok kiracılılık** -bir varlık türü tanımlayan bir *Tenantıd* özelliği.</span><span class="sxs-lookup"><span data-stu-id="db53e-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="db53e-108">Örnek</span><span class="sxs-lookup"><span data-stu-id="db53e-108">Example</span></span>

<span data-ttu-id="db53e-109">Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="db53e-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="db53e-110">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="db53e-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="db53e-111">İlk olarak, varlıklar tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="db53e-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="db53e-112">Bir _ bildirimi Not_Tenantıd_ alanını _Blog_ varlık.</span><span class="sxs-lookup"><span data-stu-id="db53e-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="db53e-113">Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db53e-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="db53e-114">Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü.</span><span class="sxs-lookup"><span data-stu-id="db53e-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="db53e-115">Bu kullanılır olup izlenmesi için bir _Post_ örneği "geçici silinen".</span><span class="sxs-lookup"><span data-stu-id="db53e-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="db53e-116">Yani Örnek, temel alınan verileri fiziksel olarak kaldırma silinen withouth işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="db53e-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="db53e-117">Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="db53e-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="db53e-118">Koşul ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak LINQ sorguları bu türleri için.</span><span class="sxs-lookup"><span data-stu-id="db53e-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="db53e-119">DbContext örnek düzeyinde bir alanı kullanımına dikkat edin: ```_tenantId``` geçerli Kiracı ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db53e-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="db53e-120">Model düzeyinde filtreler doğru bağlam örneğinin değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="db53e-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="db53e-121">Yani Sorgu yürütülürken örneği.</span><span class="sxs-lookup"><span data-stu-id="db53e-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="db53e-122">Filtreleri devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="db53e-122">Disabling Filters</span></span>

<span data-ttu-id="db53e-123">Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak ```IgnoreQueryFilters()``` işleci.</span><span class="sxs-lookup"><span data-stu-id="db53e-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="db53e-124">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="db53e-124">Limitations</span></span>

<span data-ttu-id="db53e-125">Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="db53e-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="db53e-126">Filtreler, gezinti özellikleri başvurular içeremez.</span><span class="sxs-lookup"><span data-stu-id="db53e-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="db53e-127">Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="db53e-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
