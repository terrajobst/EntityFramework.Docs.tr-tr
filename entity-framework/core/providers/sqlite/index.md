---
title: SQLite veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f8b0044e11ba665b610ac583bf3a79ed7174630
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813473"
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı, Entity Framework Core SQLite ile kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>Yükleme

[Microsoft. EntityFrameworkCore. SQLite NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)yükler.

# <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Desteklenen veritabanı motorları

* SQLite (3,7 sonraki sürümler)

## <a name="limitations"></a>Sınırlamalar

SQLite sağlayıcının bazı önemli sınırlamaları için bkz. [SQLite sınırlamaları](limitations.md) .
