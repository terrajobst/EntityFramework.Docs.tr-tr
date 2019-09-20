---
title: SQLite veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149270"
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı, Entity Framework Core SQLite ile kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>Yükleme

[Microsoft. EntityFrameworkCore. SQLite NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)yükler.

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a>Desteklenen veritabanı motorları

* SQLite (3,7 sonraki sürümler)

## <a name="limitations"></a>Sınırlamalar

SQLite sağlayıcının bazı önemli sınırlamaları için bkz. [SQLite sınırlamaları](limitations.md) .
