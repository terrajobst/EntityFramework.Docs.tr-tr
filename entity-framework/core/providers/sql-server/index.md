---
title: "Microsoft SQL Server veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF çekirdek veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı (SQL Azure dahil olmak üzere) Microsoft SQL Server ile birlikte kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı bir parçası olarak korunur [EntityFramework GitHub proje](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Yükleme

Yükleme [Microsoft.EntityFrameworkCore.SqlServer NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.
* [.NET Framework (konsol, WinForms, WPF, vb.) kullanmaya başlama](../../get-started/full-dotnet/index.md)

* [ASP.NET Core üzerinde çalışmaya başlama](../../get-started/aspnetcore/index.md)

* [UnicornStore örnek uygulama](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* Microsoft SQL Server (2008 veya sonraki sürümleri)

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

* Mono (veya sonraki sürümleri 4.2.0)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
