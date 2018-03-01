---
title: "Microsoft SQL Server Compact Edition veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Microsoft SQL Server Compact Edition EF çekirdek veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı, SQL Server Compact Edition ile kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı bir parçası olarak korunur [ErikEJ/EntityFramework.SqlServerCompact GitHub proje](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

> [!NOTE]  
> Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz. Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.

## <a name="install"></a>Yükleme

SQL Server Compact Edition 4.0 ile çalışmak için yükleme [EntityFrameworkCore.SqlServerCompact40 NuGet paketi](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

SQL Server Compact Edition 3.5 ile çalışmak için yükleme [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Başlarken

Bkz: [Başlarken belgeleri proje sitesinde](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)
