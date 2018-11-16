---
title: Sorgu etiketleri - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: 3a4d516cab5836c659e42d825c4f1bf89355d671
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688806"
---
# <a name="query-tags"></a><span data-ttu-id="515b6-102">Sorgu etiketleri</span><span class="sxs-lookup"><span data-stu-id="515b6-102">Query tags</span></span>
> [!NOTE]
> <span data-ttu-id="515b6-103">Bu özellik, EF Core 2.2 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="515b6-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="515b6-104">Bu özellik, kodu, LINQ sorgularında günlüklerde yakalanan oluşturulan SQL sorguları ile ilişkilendirmek yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="515b6-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="515b6-105">Kullanarak yeni bir LINQ Sorgu açıklama `TagWith()` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="515b6-105">You annotate a LINQ query using the new `TagWith()` method:</span></span> 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="515b6-106">Bu LINQ sorgusu için aşağıdaki SQL deyimini çevrilir:</span><span class="sxs-lookup"><span data-stu-id="515b6-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="515b6-107">Çağrılacak mümkündür `TagWith()` birden çok kez aynı sorgu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="515b6-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="515b6-108">Sorgu etiketleri toplu.</span><span class="sxs-lookup"><span data-stu-id="515b6-108">Query tags are cumulative.</span></span>
<span data-ttu-id="515b6-109">Örneğin, aşağıdaki yöntemlerden verilen:</span><span class="sxs-lookup"><span data-stu-id="515b6-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="515b6-110">Aşağıdaki sorgu:</span><span class="sxs-lookup"><span data-stu-id="515b6-110">The following query:</span></span>   

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="515b6-111">İçin çevirir:</span><span class="sxs-lookup"><span data-stu-id="515b6-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="515b6-112">Çok satırlı dize sorgu etiketleri kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="515b6-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="515b6-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="515b6-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="515b6-114">Aşağıdaki SQL üretir:</span><span class="sxs-lookup"><span data-stu-id="515b6-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="515b6-115">Bilinen sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="515b6-115">Known limitations</span></span>
<span data-ttu-id="515b6-116">**Sorgu etiketleri parametrelenebilir değil:** EF Core her zaman sorgu etiketleri LINQ sorgusu olarak değerlendirir oluşturulan SQL dahil edilen dize değişmez değerleri.</span><span class="sxs-lookup"><span data-stu-id="515b6-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="515b6-117">Parametreleri izin verilmeyen olan sorgu etiketleri alın, derlenmiş sorgular.</span><span class="sxs-lookup"><span data-stu-id="515b6-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>