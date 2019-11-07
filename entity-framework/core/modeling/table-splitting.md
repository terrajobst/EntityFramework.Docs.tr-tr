---
title: Tablo bölme-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: a3a2e5842a6c6b4b490084d205a0d44bb46c17ee
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656036"
---
# <a name="table-splitting"></a><span data-ttu-id="2d89d-102">Tablo Bölme</span><span class="sxs-lookup"><span data-stu-id="2d89d-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="2d89d-103">Bu özellik EF Core 2,0 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="2d89d-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="2d89d-104">EF Core iki veya daha fazla varlığı tek bir satıra eşlemeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="2d89d-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="2d89d-105">Bu, _tablo bölme_ veya _tablo paylaşma_olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2d89d-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="2d89d-106">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2d89d-106">Configuration</span></span>

<span data-ttu-id="2d89d-107">Varlık türlerinin tablo bölmeyi kullanmak için aynı tabloya eşlenmesi gerekir, birincil anahtarların aynı sütunlara ve bir varlık türünün birincil anahtarı ile aynı tabloda diğeri arasında yapılandırılmış en az bir ilişkiye sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2d89d-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="2d89d-108">Tablo bölme için yaygın bir senaryo, daha fazla performans veya kapsülleme için tablodaki sütunların yalnızca bir alt kümesini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="2d89d-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="2d89d-109">Bu örnekte `Order` bir `DetailedOrder`alt kümesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2d89d-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="2d89d-110">Gerekli yapılandırmaya ek olarak, `DetailedOrder.Status` `Order.Status`ile aynı sütuna eşlemek için `Property(o => o.Status).HasColumnName("Status")` çağırıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2d89d-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="2d89d-111">Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="2d89d-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="2d89d-112">Kullanım</span><span class="sxs-lookup"><span data-stu-id="2d89d-112">Usage</span></span>

<span data-ttu-id="2d89d-113">Tablo bölme kullanarak varlıkları kaydetme ve sorgulama, diğer varlıklarla aynı şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="2d89d-113">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="2d89d-114">EF Core 3,0 ile başlayarak, bağımlı varlık başvurusu `null`olabilir.</span><span class="sxs-lookup"><span data-stu-id="2d89d-114">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="2d89d-115">Bağımlı varlık tarafından kullanılan tüm sütunlar veritabanı `NULL`, sorgulandığında hiçbir örnek oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="2d89d-115">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="2d89d-116">Ayrıca, tüm özellikler isteğe bağlıdır ve `null`olarak ayarlanır ve bu da beklenmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="2d89d-116">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="2d89d-117">Eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="2d89d-117">Concurrency tokens</span></span>

<span data-ttu-id="2d89d-118">Bir tabloyu paylaşan varlık türlerinden herhangi birinde bir eşzamanlılık belirteci varsa, aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için diğer tüm varlık türlerine dahil olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2d89d-118">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="2d89d-119">Bunu, tüketim kodunda ortaya çıkarmamak için gölge durumundaki bir oluşturma olasılığı vardır.</span><span class="sxs-lookup"><span data-stu-id="2d89d-119">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
