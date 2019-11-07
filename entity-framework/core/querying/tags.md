---
title: Sorgu etiketleri-EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654828"
---
# <a name="query-tags"></a><span data-ttu-id="4364c-102">Sorgu etiketleri</span><span class="sxs-lookup"><span data-stu-id="4364c-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="4364c-103">Bu özellik EF Core 2,2 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="4364c-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="4364c-104">Bu özellik, günlüklerde yakalanan oluşturulmuş SQL sorgularıyla kodda LINQ sorgularının bağıntılı olarak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4364c-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="4364c-105">Yeni `TagWith()` yöntemini kullanarak bir LINQ sorgusuna açıklama ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4364c-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="4364c-106">Bu LINQ sorgusu şu SQL ifadesine çevrilir:</span><span class="sxs-lookup"><span data-stu-id="4364c-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="4364c-107">Aynı sorgu üzerinde `TagWith()` çok sayıda çağırmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4364c-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="4364c-108">Sorgu etiketleri birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="4364c-108">Query tags are cumulative.</span></span>
<span data-ttu-id="4364c-109">Örneğin, aşağıdaki yöntemler verildiğinde:</span><span class="sxs-lookup"><span data-stu-id="4364c-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="4364c-110">Aşağıdaki sorgu:</span><span class="sxs-lookup"><span data-stu-id="4364c-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="4364c-111">Çevirir:</span><span class="sxs-lookup"><span data-stu-id="4364c-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="4364c-112">Çok satırlı dizeleri sorgu etiketleri olarak kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4364c-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="4364c-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4364c-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="4364c-114">Aşağıdaki SQL 'i üretir:</span><span class="sxs-lookup"><span data-stu-id="4364c-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="4364c-115">Bilinen sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="4364c-115">Known limitations</span></span>

<span data-ttu-id="4364c-116">**Sorgu etiketlerine parametreleştirilebilir:** EF Core, LINQ sorgusundaki sorgu etiketlerine her zaman oluşturulan SQL 'e dahil edilen dize sabit değerleri olarak davranır.</span><span class="sxs-lookup"><span data-stu-id="4364c-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="4364c-117">Sorgu etiketleri parametre olarak alan derlenmiş sorgulara izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4364c-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
