---
title: Sorgu Etiketleri - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417892"
---
# <a name="query-tags"></a><span data-ttu-id="f8df4-102">Sorgu etiketleri</span><span class="sxs-lookup"><span data-stu-id="f8df4-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="f8df4-103">Bu özellik EF Core 2.2'de yenidir.</span><span class="sxs-lookup"><span data-stu-id="f8df4-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="f8df4-104">Bu özellik, linq sorgularını koddaki SQL sorgularıyla günlüklerde yakalanan sql sorgularıyla ilişkilendirmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="f8df4-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="f8df4-105">Yeni `TagWith()` yöntemi kullanarak bir LINQ sorgusuna açıklama ekine bilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="f8df4-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="f8df4-106">Bu LINQ sorgusu aşağıdaki SQL deyimine çevrilir:</span><span class="sxs-lookup"><span data-stu-id="f8df4-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="f8df4-107">Aynı sorguda birçok `TagWith()` kez aramak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f8df4-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="f8df4-108">Sorgu etiketleri birikmeli.</span><span class="sxs-lookup"><span data-stu-id="f8df4-108">Query tags are cumulative.</span></span>
<span data-ttu-id="f8df4-109">Örneğin, aşağıdaki yöntemler göz önüne alındığında:</span><span class="sxs-lookup"><span data-stu-id="f8df4-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="f8df4-110">Aşağıdaki sorgu:</span><span class="sxs-lookup"><span data-stu-id="f8df4-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="f8df4-111">Çevirir:</span><span class="sxs-lookup"><span data-stu-id="f8df4-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="f8df4-112">Sorgu etiketleri olarak çok satırlı dizeleri kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f8df4-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="f8df4-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f8df4-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="f8df4-114">Aşağıdaki SQL'i üretir:</span><span class="sxs-lookup"><span data-stu-id="f8df4-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="f8df4-115">Bilinen sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="f8df4-115">Known limitations</span></span>

<span data-ttu-id="f8df4-116">**Sorgu etiketleri parametreyetabi değildir:** EF Core, LINQ sorgusundaki sorgu etiketlerini her zaman oluşturulan SQL'de bulunan dize literal'leri olarak ele alır.</span><span class="sxs-lookup"><span data-stu-id="f8df4-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="f8df4-117">Parametreler olarak sorgu etiketleri alan derlenmiş sorgular izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="f8df4-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
