---
title: "Genel sorgu filtreleri - EF çekirdek"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="1fbb2-102">Genel sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="1fbb2-102">Global Query Filters</span></span>

<span data-ttu-id="1fbb2-103">Genel sorgu filtreleri şunlardır LINQ Sorgu koşulları (Boole ifadesi genellikle LINQ to geçirilen *nerede* sorgu işleci) meta veri modeli varlık türlerine uygulanan (genellikle *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="1fbb2-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="1fbb2-104">Bu tür filtreleri, varlık türleri INCLUDE kullanımıyla dolaylı olarak gibi başvurulan veya doğrudan gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ Sorgu otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="1fbb2-105">Bu özellik, bazı ortak uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1fbb2-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="1fbb2-106">**Yumuşak silme** -bir varlık türünü tanımlayan bir *IsDeleted* özelliği.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="1fbb2-107">**Çoklu kiracı** -bir varlık türünü tanımlayan bir *Tenantıd* özelliği.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="1fbb2-108">Örnek</span><span class="sxs-lookup"><span data-stu-id="1fbb2-108">Example</span></span>

<span data-ttu-id="1fbb2-109">Aşağıdaki örnek genel sorgu filtreleri basit blog modelinde yumuşak silebilir ve çoklu kiracı sorgu davranışlarını uygulamak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="1fbb2-110">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) github'da.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="1fbb2-111">İlk olarak, varlıkları tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="1fbb2-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="1fbb2-112">Bir _ bildirimi Not_Tenantıd_ alanını _Blog_ varlık.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="1fbb2-113">Bu, her bir Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="1fbb2-114">Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="1fbb2-115">Bu kullanılır olup izlenmesi için bir _Post_ örneği "geçici olarak silinen" açıldı.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="1fbb2-116">Yani Örnek temel alınan verileri fiziksel olarak kaldırma silinen withouth işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="1fbb2-117">Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="1fbb2-118">Karşılaştırma ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak bu türleri için herhangi bir LINQ Sorgu.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="1fbb2-119">DbContext örnek düzeyi alanı kullanımına dikkat edin: ```_tenantId``` geçerli Kiracı ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="1fbb2-120">Model düzeyindeki filtreleri doğru bağlamı örneğinden değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="1fbb2-121">Yani Sorgu yürütülürken örneği.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="1fbb2-122">Filtreleri devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="1fbb2-122">Disabling Filters</span></span>

<span data-ttu-id="1fbb2-123">Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak ```IgnoreQueryFilters()``` işleci.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="1fbb2-124">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="1fbb2-124">Limitations</span></span>

<span data-ttu-id="1fbb2-125">Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1fbb2-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="1fbb2-126">Filtreler, gezinti özellikleri için başvuru içeremez.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="1fbb2-127">Filtreler yalnızca varlık türü bir devralma hiyerarşisini kök için tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
