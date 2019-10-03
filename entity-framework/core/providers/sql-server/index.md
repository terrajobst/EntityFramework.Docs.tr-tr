---
title: Microsoft SQL Server veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: f0aa290e8c5166c278f8c9782c4304de5e91f26b
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813501"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="7d66d-102">Microsoft SQL Server EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="7d66d-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="7d66d-103">Bu veritabanı sağlayıcısı Entity Framework Core Microsoft SQL Server (SQL Azure dahil) birlikte kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="7d66d-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="7d66d-104">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="7d66d-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="7d66d-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="7d66d-105">Install</span></span>

<span data-ttu-id="7d66d-106">[Microsoft. EntityFrameworkCore. SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükler.</span><span class="sxs-lookup"><span data-stu-id="7d66d-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="7d66d-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d66d-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="7d66d-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d66d-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="7d66d-109">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="7d66d-109">Supported Database Engines</span></span>

* <span data-ttu-id="7d66d-110">Microsoft SQL Server (2012 sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="7d66d-110">Microsoft SQL Server (2012 onwards)</span></span>
