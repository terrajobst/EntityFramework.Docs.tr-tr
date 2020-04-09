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
# <a name="query-tags"></a>Sorgu etiketleri

> [!NOTE]
> Bu özellik EF Core 2.2'de yenidir.

Bu özellik, linq sorgularını koddaki SQL sorgularıyla günlüklerde yakalanan sql sorgularıyla ilişkilendirmenize yardımcı olur.
Yeni `TagWith()` yöntemi kullanarak bir LINQ sorgusuna açıklama ekine bilirsiniz:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu LINQ sorgusu aşağıdaki SQL deyimine çevrilir:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Aynı sorguda birçok `TagWith()` kez aramak mümkündür.
Sorgu etiketleri birikmeli.
Örneğin, aşağıdaki yöntemler göz önüne alındığında:

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

Sorgu etiketleri olarak çok satırlı dizeleri kullanmak da mümkündür.
Örneğin:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Aşağıdaki SQL'i üretir:

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

**Sorgu etiketleri parametreyetabi değildir:** EF Core, LINQ sorgusundaki sorgu etiketlerini her zaman oluşturulan SQL'de bulunan dize literal'leri olarak ele alır.
Parametreler olarak sorgu etiketleri alan derlenmiş sorgular izin verilmez.
