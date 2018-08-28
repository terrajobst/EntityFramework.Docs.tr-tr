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
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="e719e-102">Microsoft SQL Server EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="e719e-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="e719e-103">Bu veritabanı sağlayıcısı (SQL Azure dahil olmak üzere) Microsoft SQL Server ile kullanılmak üzere Entity Framework Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="e719e-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="e719e-104">Sağlayıcı bir parçası olarak korunur [Entity Framework Core projesi](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="e719e-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="e719e-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="e719e-105">Install</span></span>

<span data-ttu-id="e719e-106">Yükleme [Microsoft.EntityFrameworkCore.SqlServer NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="e719e-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="e719e-107">Başlarken</span><span class="sxs-lookup"><span data-stu-id="e719e-107">Get Started</span></span>

<span data-ttu-id="e719e-108">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="e719e-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="e719e-109">.NET Framework (konsol, WinForms, WPF, vb.) kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e719e-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="e719e-110">Üzerinde ASP.NET Core kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e719e-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="e719e-111">UnicornStore örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="e719e-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="e719e-112">Veritabanı altyapıları desteklenir</span><span class="sxs-lookup"><span data-stu-id="e719e-112">Supported Database Engines</span></span>

* <span data-ttu-id="e719e-113">Microsoft SQL Server (2008 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="e719e-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e719e-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="e719e-114">Supported Platforms</span></span>

* <span data-ttu-id="e719e-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="e719e-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="e719e-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="e719e-116">.NET Core</span></span>

* <span data-ttu-id="e719e-117">Mono (4.2.0 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="e719e-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
