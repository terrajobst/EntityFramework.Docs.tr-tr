---
title: SQLite Veritabanı Sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417779"
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF Çekirdek Veritabanı Sağlayıcısı

Bu veritabanı sağlayıcısı Entity Framework Core'un SQLite ile kullanılmasına izin verir. Sağlayıcı, Entity Framework Core [projesinin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.

## <a name="install"></a>Yükleme

[Microsoft.EntityFrameworkCore.Sqlite NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)yükleyin.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Desteklenen Veritabanı Motorları

* SQLite (3.7'den itibaren)

## <a name="limitations"></a>Sınırlamalar

SQLite sağlayıcısının bazı önemli sınırlamaları için [SQLite Sınırlamalarına](limitations.md) bakın.
