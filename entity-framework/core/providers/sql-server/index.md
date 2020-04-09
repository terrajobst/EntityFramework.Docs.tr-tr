---
title: Microsoft SQL Server Veritabanı Sağlayıcısı - EF Core
description: Entity Framework Core'un Microsoft SQL Server ile kullanılmasına izin veren veritabanı sağlayıcısı için belgeler
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417796"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Çekirdek Veritabanı Sağlayıcısı

Bu veritabanı sağlayıcısı, Entity Framework Core'un Microsoft SQL Server (Azure SQL Veritabanı dahil) ile kullanılmasına izin verir. Sağlayıcı, Varlık Çerçeve Çekirdek [Projesi'nin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.

## <a name="install"></a>Yükleme

[Microsoft.EntityFrameworkCore.SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükleyin.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Sürüm 3.0.0'dan bu yana sağlayıcı Microsoft.Data.SqlClient'a başvurur (önceki sürümler System.Data.SqlClient'a bağlıdır). Projeniz SqlClient'a doğrudan bağımlı hale alıyorsa, Microsoft.Data.SqlClient paketine başvuruldığından emin olun.

## <a name="supported-database-engines"></a>Desteklenen Veritabanı Motorları

* Microsoft SQL Server (2012'den itibaren)
