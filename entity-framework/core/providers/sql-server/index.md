---
title: Microsoft SQL Server veritabanı sağlayıcısı-EF Core
description: Entity Framework Core Microsoft SQL Server birlikte kullanılmasına izin veren veritabanı sağlayıcısına yönelik belgeler
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 18a69789ff4ae013c1d60bb6d34ca5c27ee285c2
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824776"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı, Entity Framework Core Microsoft SQL Server (Azure SQL veritabanı dahil) birlikte kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

## <a name="install"></a>yükleme

[Microsoft. EntityFrameworkCore. SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükler.

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Sürüm 3.0.0 sürümünden itibaren, sağlayıcı Microsoft. Data. SqlClient 'e başvuruyor (önceki sürümler System. Data. SqlClient 'a bağımlı). Projeniz SqlClient doğrudan bağımlılığı alırsa, Microsoft. Data. SqlClient paketine başvurduğundan emin olun.

## <a name="supported-database-engines"></a>Desteklenen veritabanı motorları

* Microsoft SQL Server (2012 sonraki sürümler)
