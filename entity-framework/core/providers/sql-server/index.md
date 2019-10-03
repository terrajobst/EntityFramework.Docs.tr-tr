---
title: Microsoft SQL Server veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: f0aa290e8c5166c278f8c9782c4304de5e91f26b
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813501"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı Entity Framework Core Microsoft SQL Server (SQL Azure dahil) birlikte kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>Yükleme

[Microsoft. EntityFrameworkCore. SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükler.

# <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

## <a name="supported-database-engines"></a>Desteklenen veritabanı motorları

* Microsoft SQL Server (2012 sonraki sürümler)
