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
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="ee879-102">Microsoft SQL Server EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="ee879-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="ee879-103">Bu veritabanı sağlayıcısı (SQL Azure dahil olmak üzere) Microsoft SQL Server ile birlikte kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="ee879-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="ee879-104">Sağlayıcı bir parçası olarak korunur [EntityFramework GitHub proje](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="ee879-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="ee879-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="ee879-105">Install</span></span>

<span data-ttu-id="ee879-106">Yükleme [Microsoft.EntityFrameworkCore.SqlServer NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="ee879-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="ee879-107">Başlarken</span><span class="sxs-lookup"><span data-stu-id="ee879-107">Get Started</span></span>

<span data-ttu-id="ee879-108">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="ee879-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="ee879-109">.NET Framework (konsol, WinForms, WPF, vb.) kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ee879-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="ee879-110">ASP.NET Core üzerinde çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ee879-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="ee879-111">UnicornStore örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="ee879-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="ee879-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="ee879-112">Supported Database Engines</span></span>

* <span data-ttu-id="ee879-113">Microsoft SQL Server (2008 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="ee879-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ee879-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="ee879-114">Supported Platforms</span></span>

* <span data-ttu-id="ee879-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="ee879-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="ee879-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee879-116">.NET Core</span></span>

* <span data-ttu-id="ee879-117">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="ee879-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
