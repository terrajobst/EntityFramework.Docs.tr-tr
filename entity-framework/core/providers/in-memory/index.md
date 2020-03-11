---
title: InMemory veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417221"
---
# <a name="ef-core-in-memory-database-provider"></a>Bellek Içi veritabanı sağlayıcısını EF Core

Bu veritabanı sağlayıcısı, Entity Framework Core bellek içi veritabanıyla birlikte kullanılmasına izin verir. Bu, sınama için faydalı olabilir, ancak bellek içi moddaki SQLite sağlayıcı, ilişkisel veritabanları için daha uygun bir test değişikliği olabilir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>Yükleme

[Microsoft. EntityFrameworkCore. InMemory NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)yükler.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

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
