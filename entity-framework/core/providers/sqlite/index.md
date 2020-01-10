---
title: SQLite veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502259"
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı, Entity Framework Core SQLite ile kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>yükleme

[Microsoft. EntityFrameworkCore. SQLite NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)yükler.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Desteklenen veritabanı motorları

* SQLite (3,7 sonraki sürümler)

## <a name="limitations"></a>Sınırlamalar

SQLite sağlayıcının bazı önemli sınırlamaları için bkz. [SQLite sınırlamaları](limitations.md) .
