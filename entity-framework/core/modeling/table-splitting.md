---
title: Tablo bölme-EF Core
description: Entity Framework Core kullanarak tablo bölmeyi yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: de24f8903af79ebd7f68e6b74288257883c1fa8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417400"
---
# <a name="table-splitting"></a><span data-ttu-id="bf523-103">Tablo Bölme</span><span class="sxs-lookup"><span data-stu-id="bf523-103">Table Splitting</span></span>

<span data-ttu-id="bf523-104">EF Core iki veya daha fazla varlığı tek bir satıra eşlemeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="bf523-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="bf523-105">Bu, _tablo bölme_ veya _tablo paylaşma_olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="bf523-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="bf523-106">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf523-106">Configuration</span></span>

<span data-ttu-id="bf523-107">Varlık türlerinin tablo bölmeyi kullanmak için aynı tabloya eşlenmesi gerekir, birincil anahtarların aynı sütunlara ve bir varlık türünün birincil anahtarı ile aynı tabloda diğeri arasında yapılandırılmış en az bir ilişkiye sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bf523-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="bf523-108">Tablo bölme için yaygın bir senaryo, daha fazla performans veya kapsülleme için tablodaki sütunların yalnızca bir alt kümesini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="bf523-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="bf523-109">Bu örnekte `Order` bir `DetailedOrder`alt kümesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="bf523-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="bf523-110">Gerekli yapılandırmaya ek olarak, `DetailedOrder.Status` `Order.Status`ile aynı sütuna eşlemek için `Property(o => o.Status).HasColumnName("Status")` çağırıyoruz.</span><span class="sxs-lookup"><span data-stu-id="bf523-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> <span data-ttu-id="bf523-111">Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="bf523-111">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="bf523-112">Kullanım</span><span class="sxs-lookup"><span data-stu-id="bf523-112">Usage</span></span>

<span data-ttu-id="bf523-113">Tablo bölme kullanarak varlıkları kaydetme ve sorgulama, diğer varlıklarla aynı şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="bf523-113">Saving and querying entities using table splitting is done in the same way as other entities:</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a><span data-ttu-id="bf523-114">İsteğe bağlı bağımlı varlık</span><span class="sxs-lookup"><span data-stu-id="bf523-114">Optional dependent entity</span></span>

> [!NOTE]
> <span data-ttu-id="bf523-115">Bu özellik EF Core 3,0 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="bf523-115">This feature was introduced in EF Core 3.0.</span></span>

<span data-ttu-id="bf523-116">Bağımlı bir varlık tarafından kullanılan tüm sütunlar veritabanında `NULL`, bu durumda sorgulanan bir örnek oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bf523-116">If all of the columns used by a dependent entity are `NULL` in the database, then no instance for it will be created when queried.</span></span> <span data-ttu-id="bf523-117">Bu, Principal 'ın ilişki özelliğinin null olacağı, isteğe bağlı bir bağımlı varlığın modellemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf523-117">This allows modeling an optional dependent entity, where the relationship property on the principal would be null.</span></span> <span data-ttu-id="bf523-118">Bunun aynı zamanda bağımlı olduğu tüm özelliklerin tümünün de olacağını ve `null`olarak ayarlandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="bf523-118">Note that This would also happen all of the dependent's properties are optional and set to `null`, which might not be expected.</span></span>

## <a name="concurrency-tokens"></a><span data-ttu-id="bf523-119">Eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="bf523-119">Concurrency tokens</span></span>

<span data-ttu-id="bf523-120">Bir tabloyu paylaşan varlık türlerinden herhangi birinde eşzamanlılık belirteci varsa, bu, diğer tüm varlık türlerine de dahil olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf523-120">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types as well.</span></span> <span data-ttu-id="bf523-121">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="bf523-121">This is necessary in order to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="bf523-122">Eşzamanlılık belirtecini, tüketim kodunda açığa çıkarmamak için bir [gölge özelliği](xref:core/modeling/shadow-properties)olarak oluştur mümkündür:</span><span class="sxs-lookup"><span data-stu-id="bf523-122">To avoid exposing the concurrency token to the consuming code, it's possible the create one as a [shadow property](xref:core/modeling/shadow-properties):</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
