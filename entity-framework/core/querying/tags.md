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
# <a name="query-tags"></a>Sorgu etiketleri
> [!NOTE]
> Bu özellik, EF Core 2.2 içinde yeni bir özelliktir.

Bu özellik, kodu, LINQ sorgularında günlüklerde yakalanan oluşturulan SQL sorguları ile ilişkilendirmek yardımcı olur.
Kullanarak yeni bir LINQ Sorgu açıklama `TagWith()` yöntemi: 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu LINQ sorgusu için aşağıdaki SQL deyimini çevrilir:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Çağrılacak mümkündür `TagWith()` birden çok kez aynı sorgu üzerinde.
Sorgu etiketleri toplu.
Örneğin, aşağıdaki yöntemlerden verilen:

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

İçin çevirir:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Çok satırlı dize sorgu etiketleri kullanmak da mümkündür.
Örneğin:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Aşağıdaki SQL üretir:

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
**Sorgu etiketleri parametrelenebilir değil:** EF Core her zaman sorgu etiketleri LINQ sorgusu olarak değerlendirir oluşturulan SQL dahil edilen dize değişmez değerleri.
Parametreleri izin verilmeyen olan sorgu etiketleri alın, derlenmiş sorgular.