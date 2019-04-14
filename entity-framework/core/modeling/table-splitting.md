---
title: Tablo bölme - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562602"
---
# <a name="table-splitting"></a><span data-ttu-id="ed9aa-102">Tablo bölme</span><span class="sxs-lookup"><span data-stu-id="ed9aa-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="ed9aa-103">Bu özellik, EF Core 2.0 sürümünde yenidir.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="ed9aa-104">EF Core tek bir satır için iki veya daha fazla varlık eşlemeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="ed9aa-105">Bu adlandırılır _tablo bölme_ veya _tablo paylaşımı_.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="ed9aa-106">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed9aa-106">Configuration</span></span>

<span data-ttu-id="ed9aa-107">Tablo varlık türleri aynı tabloya eşlenmesi gerekir bölme kullanmak için aynı sütunlarıyla eşlenen birincil anahtarlar ve bir varlık türünün birincil anahtarı ile aynı tablodaki başka bir arasındaki yapılandırılmış en az bir ilişki var.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="ed9aa-108">Tablo bölmek için yaygın bir senaryo büyük performans veya saklama için tabloda yalnızca bir sütun alt kümesi kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="ed9aa-109">Bu örnekte `Order` bir alt kümesini temsil eden `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="ed9aa-110">Ek gerekli yapılandırma diyoruz `HasBaseType((string)null)` eşleme önlemek için `DetailedOrder` aynı hiyerarşide `Order`.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="ed9aa-111">Bkz: [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) daha fazla bağlam için.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="ed9aa-112">Kullanım</span><span class="sxs-lookup"><span data-stu-id="ed9aa-112">Usage</span></span>

<span data-ttu-id="ed9aa-113">Kaydetme ve tablo bölmeyi kullanarak varlıkları sorgulama gibi diğer varlıklar aynı şekilde yapılır, tek fark bir satır paylaşma tüm varlıklar için INSERT izlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="ed9aa-114">Eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="ed9aa-114">Concurrency tokens</span></span>

<span data-ttu-id="ed9aa-115">Ardından herhangi bir tablo paylaşımı varlık türleri varsa eşzamanlı bir simge eski eşzamanlılık belirteç değeri aynı tabloya eşlenen varlıkları yalnızca biri güncelleştirildiğinde önlemek için diğer tüm varlık türleri eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="ed9aa-116">Kullanan kodu için kullanılmasını önlemek için mümkündür oluşturma bir gölge durumu.</span><span class="sxs-lookup"><span data-stu-id="ed9aa-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]