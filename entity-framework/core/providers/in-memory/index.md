---
title: InMemory veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 4b35e8c4b29a951449d4a26c6e274eb3015069bc
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656019"
---
# <a name="ef-core-in-memory-database-provider"></a>Bellek Içi veritabanı sağlayıcısını EF Core

Bu veritabanı sağlayıcısı, Entity Framework Core bellek içi veritabanıyla birlikte kullanılmasına izin verir. Bu, sınama için faydalı olabilir, ancak bellek içi moddaki SQLite sağlayıcı, ilişkisel veritabanları için daha uygun bir test değişikliği olabilir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>Yükleme

[Microsoft. EntityFrameworkCore. InMemory NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)yükler.

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar, bu sağlayıcıyı kullanmaya başlamanıza yardımcı olur.

* [InMemory ile test etme](../../miscellaneous/testing/in-memory.md)
* [UnicornStore örnek uygulama testleri](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Desteklenen veritabanı motorları

İşlem içi bellek veritabanı (yalnızca test amaçlı olarak tasarlanmıştır)
