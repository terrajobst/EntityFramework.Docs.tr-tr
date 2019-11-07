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
# <a name="query-tags"></a>Sorgu etiketleri

> [!NOTE]
> Bu özellik EF Core 2,2 ' de yenidir.

Bu özellik, günlüklerde yakalanan oluşturulmuş SQL sorgularıyla kodda LINQ sorgularının bağıntılı olarak yardımcı olur.
Yeni `TagWith()` yöntemini kullanarak bir LINQ sorgusuna açıklama ekleyebilirsiniz:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu LINQ sorgusu şu SQL ifadesine çevrilir:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Aynı sorgu üzerinde `TagWith()` çok sayıda çağırmak mümkündür.
Sorgu etiketleri birikimlidir.
Örneğin, aşağıdaki yöntemler verildiğinde:

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

Aşağıdaki sorgu:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

Çevirir:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Çok satırlı dizeleri sorgu etiketleri olarak kullanmak da mümkündür.
Örneğin:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Aşağıdaki SQL 'i üretir:

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a>Bilinen sınırlamalar

**Sorgu etiketlerine parametreleştirilebilir:** EF Core, LINQ sorgusundaki sorgu etiketlerine her zaman oluşturulan SQL 'e dahil edilen dize sabit değerleri olarak davranır.
Sorgu etiketleri parametre olarak alan derlenmiş sorgulara izin verilmez.
