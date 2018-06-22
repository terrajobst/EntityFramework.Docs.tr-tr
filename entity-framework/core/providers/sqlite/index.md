---
title: SQLite veritabanı sağlayıcısı - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678754"
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF çekirdek veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı ile SQLite kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı bir parçası olarak korunur [Entity Framework Çekirdek proje](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Yükleme

Yükleme [Microsoft.EntityFrameworkCore.Sqlite NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.
* [UWP üzerinde yerel SQLite](../../get-started/uwp/getting-started.md)

* [.NET core uygulamasına yeni SQLite veritabanı](../../get-started/netcore/new-db-sqlite.md)

* [Unicorn Clicker örnek uygulama](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicorn Packer örnek uygulama](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* SQLite (3.7 veya sonraki sürümleri)

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

* Mono (veya sonraki sürümleri 4.2.0)

* Evrensel Windows Platformu

## <a name="limitations"></a>Sınırlamalar

Bkz: [SQLite sınırlamalar](limitations.md) bazı önemli sınırlamalar SQLite sağlayıcısı için.
