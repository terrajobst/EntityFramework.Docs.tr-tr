---
title: Microsoft SQL Server veritabanı sağlayıcısı - EF çekirdek
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
ms.locfileid: "29678656"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="a2ec9-102">Microsoft SQL Server EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2ec9-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="a2ec9-103">Bu veritabanı sağlayıcısı (SQL Azure dahil olmak üzere) Microsoft SQL Server ile birlikte kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2ec9-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="a2ec9-104">Sağlayıcı bir parçası olarak korunur [Entity Framework Çekirdek proje](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="a2ec9-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="a2ec9-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="a2ec9-105">Install</span></span>

<span data-ttu-id="a2ec9-106">Yükleme [Microsoft.EntityFrameworkCore.SqlServer NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="a2ec9-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="a2ec9-107">Başlarken</span><span class="sxs-lookup"><span data-stu-id="a2ec9-107">Get Started</span></span>

<span data-ttu-id="a2ec9-108">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a2ec9-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="a2ec9-109">.NET Framework (konsol, WinForms, WPF, vb.) kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a2ec9-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="a2ec9-110">ASP.NET Core üzerinde çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a2ec9-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="a2ec9-111">UnicornStore örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="a2ec9-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="a2ec9-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="a2ec9-112">Supported Database Engines</span></span>

* <span data-ttu-id="a2ec9-113">Microsoft SQL Server (2008 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="a2ec9-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a2ec9-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="a2ec9-114">Supported Platforms</span></span>

* <span data-ttu-id="a2ec9-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="a2ec9-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="a2ec9-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2ec9-116">.NET Core</span></span>

* <span data-ttu-id="a2ec9-117">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="a2ec9-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
