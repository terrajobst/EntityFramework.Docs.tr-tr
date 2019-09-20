---
title: Microsoft SQL Server veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149208"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="52ad2-102">Microsoft SQL Server EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="52ad2-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="52ad2-103">Bu veritabanı sağlayıcısı Entity Framework Core Microsoft SQL Server (SQL Azure dahil) birlikte kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="52ad2-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="52ad2-104">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="52ad2-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="52ad2-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="52ad2-105">Install</span></span>

<span data-ttu-id="52ad2-106">[Microsoft. EntityFrameworkCore. SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükler.</span><span class="sxs-lookup"><span data-stu-id="52ad2-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="52ad2-107">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="52ad2-107">Supported Database Engines</span></span>

* <span data-ttu-id="52ad2-108">Microsoft SQL Server (2012 sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="52ad2-108">Microsoft SQL Server (2012 onwards)</span></span>
