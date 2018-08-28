---
title: SQLite veritabanı sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994008"
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı SQLite ile kullanılacak Entity Framework Core sağlar. Sağlayıcı bir parçası olarak korunur [Entity Framework Core proje](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Yükleme

Yükleme [Microsoft.EntityFrameworkCore.Sqlite NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.
* [UWP üzerinde yerel bir SQLite](../../get-started/uwp/getting-started.md)

* [.NET core uygulaması için yeni bir SQLite veritabanı](../../get-started/netcore/new-db-sqlite.md)

* [Unicorn Clicker örnek uygulaması](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicorn Packer örnek uygulaması](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Veritabanı altyapıları desteklenir

* SQLite (3.7 ve sonraki sürümler)

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

* Mono (4.2.0 ve sonraki sürümler)

* Evrensel Windows Platformu

## <a name="limitations"></a>Sınırlamalar

Bkz: [SQLite sınırlamaları](limitations.md) bazılarında önemli sınırlamalar SQLite sağlayıcısı için.
