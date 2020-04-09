---
title: Genel Sorgu Filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417731"
---
# <a name="global-query-filters"></a><span data-ttu-id="2f0a9-102">Genel Sorgu Filtreleri</span><span class="sxs-lookup"><span data-stu-id="2f0a9-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="2f0a9-103">Bu özellik EF Core 2.0 ile tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="2f0a9-104">Genel sorgu filtreleri LINQ sorgu yüklemleridir (genellikle *LINQ'ya* geçirilen bir boolean ifadesi sorgu işleci) meta veri modelinde Varlık Türleri'ne uygulanır (genellikle *OnModelOluşturma'da).*</span><span class="sxs-lookup"><span data-stu-id="2f0a9-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="2f0a9-105">Bu tür filtreler, Dolaylı olarak başvurulan Varlık Türleri de dahil olmak üzere, bu Varlık Türlerini içeren tüm LINQ sorgularına (Örneğin veya doğrudan gezinme özelliği başvuruları nın kullanımı yoluyla) otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="2f0a9-106">Bu özelliğin bazı yaygın uygulamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2f0a9-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="2f0a9-107">**Yumuşak silme** - Varlık Türü *Silinmiş* bir özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="2f0a9-108">**Çoklu kira -** Varlık Türü *Kiracı Kimliği* özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="2f0a9-109">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f0a9-109">Example</span></span>

<span data-ttu-id="2f0a9-110">Aşağıdaki örnek, basit bir bloglama modelinde yumuşak silme ve çoklu kira sorgu davranışlarını uygulamak için Global Sorgu Filtreleri'nin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="2f0a9-111">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="2f0a9-112">İlk olarak, varlıkları tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="2f0a9-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="2f0a9-113">_Blog_ varlığındaki _kiracı Kimliği_ alanının bildirimine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="2f0a9-114">Bu, her blog örneğini belirli bir kiracıyla ilişkilendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="2f0a9-115">Ayrıca, _Post_ varlık türünde bir _Silinmiş_ özelliği tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="2f0a9-116">Bu, _Bir Gönderi_ örneğinin "yumuşak silinip silinmediğini" izlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="2f0a9-117">Diğer bir zamanda, örnek, temel verileri fiziksel olarak kaldırmadan silinmiş olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="2f0a9-118">Ardından, `HasQueryFilter` API'yi kullanarak _OnModelOluşturma'daki_ sorgu filtrelerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="2f0a9-119">_HasQueryFilter_ çağrılarına geçirilen yüklem ifadeleri artık bu türler için linq sorgularına otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="2f0a9-120">Geçerli kiracıyı ayarlamak için `_tenantId` kullanılan DbContext örnek düzeyi alanının kullanımına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="2f0a9-121">Model düzeyindefiltreler doğru bağlam örneğindeki değeri kullanır (diğer bir şekilde, sorguyu yürüten örnek).</span><span class="sxs-lookup"><span data-stu-id="2f0a9-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="2f0a9-122">Şu anda aynı varlık üzerinde birden çok sorgu filtresi tanımlamak mümkün değildir - yalnızca sonuncusu uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="2f0a9-123">Ancak, mantıksal _VE_ işleci[ `&&` (C# )](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)kullanarak birden çok koşula sahip tek bir filtre tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="2f0a9-124">Filtreleri Devre Dışı Bırakma</span><span class="sxs-lookup"><span data-stu-id="2f0a9-124">Disabling Filters</span></span>

<span data-ttu-id="2f0a9-125">`IgnoreQueryFilters()` Filtreler, işleci kullanarak tek tek LINQ sorguları için devre dışı bırakılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="2f0a9-126">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="2f0a9-126">Limitations</span></span>

<span data-ttu-id="2f0a9-127">Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="2f0a9-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="2f0a9-128">Filtreler yalnızca devralma hiyerarşisinin kök Varlık Türü için tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2f0a9-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
