---
title: Microsoft SQL Server veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: dd352b81da05fa8ea8970495f20947bd109edf65
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655888"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı Entity Framework Core Microsoft SQL Server (SQL Azure dahil) birlikte kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>Yükleme

[Microsoft. EntityFrameworkCore. SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükler.

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Sürüm 3.0.0 sürümünden itibaren, sağlayıcı Microsoft. Data. SqlClient 'e başvuruyor (önceki sürümler System. Data. SqlClient 'a bağımlı). Projeniz SqlClient doğrudan bağımlılığı alırsa, doğru pakete başvurduğundan emin olun.

## <a name="supported-database-engines"></a>Desteklenen veritabanı motorları

* Microsoft SQL Server (2012 sonraki sürümler)
