---
title: Microsoft SQL Server veritabanı sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995675"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Core veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı (SQL Azure dahil olmak üzere) Microsoft SQL Server ile kullanılmak üzere Entity Framework Core sağlar. Sağlayıcı bir parçası olarak korunur [Entity Framework Core projesi](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Yükleme

Yükleme [Microsoft.EntityFrameworkCore.SqlServer NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.
* [.NET Framework (konsol, WinForms, WPF, vb.) kullanmaya başlama](../../get-started/full-dotnet/index.md)

* [Üzerinde ASP.NET Core kullanmaya başlama](../../get-started/aspnetcore/index.md)

* [UnicornStore örnek uygulaması](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Veritabanı altyapıları desteklenir

* Microsoft SQL Server (2008 ve sonraki sürümler)

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

* Mono (4.2.0 ve sonraki sürümler)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
