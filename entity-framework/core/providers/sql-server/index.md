---
title: "Microsoft SQL Server veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: 2ed7c0dd127db03d5e7340fde1ef83cf01b30135
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF çekirdek veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı (SQL Azure dahil olmak üzere) Microsoft SQL Server ile birlikte kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı bir parçası olarak korunur [Entity Framework Çekirdek proje](https://github.com/aspnet/EntityFrameworkCore).

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
