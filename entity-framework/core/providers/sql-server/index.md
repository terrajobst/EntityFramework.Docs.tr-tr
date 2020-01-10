---
title: Microsoft SQL Server veritabanı sağlayıcısı-EF Core
description: Entity Framework Core Microsoft SQL Server birlikte kullanılmasına izin veren veritabanı sağlayıcısına yönelik belgeler
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502272"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="b2895-103">Microsoft SQL Server EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b2895-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="b2895-104">Bu veritabanı sağlayıcısı, Entity Framework Core Microsoft SQL Server (Azure SQL veritabanı dahil) birlikte kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b2895-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="b2895-105">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="b2895-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="b2895-106">yükleme</span><span class="sxs-lookup"><span data-stu-id="b2895-106">Install</span></span>

<span data-ttu-id="b2895-107">[Microsoft. EntityFrameworkCore. SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükler.</span><span class="sxs-lookup"><span data-stu-id="b2895-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="b2895-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2895-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="b2895-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2895-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="b2895-110">Sürüm 3.0.0 sürümünden itibaren, sağlayıcı Microsoft. Data. SqlClient 'e başvuruyor (önceki sürümler System. Data. SqlClient 'a bağımlı).</span><span class="sxs-lookup"><span data-stu-id="b2895-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="b2895-111">Projeniz SqlClient doğrudan bağımlılığı alırsa, Microsoft. Data. SqlClient paketine başvurduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="b2895-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="b2895-112">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="b2895-112">Supported Database Engines</span></span>

* <span data-ttu-id="b2895-113">Microsoft SQL Server (2012 sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="b2895-113">Microsoft SQL Server (2012 onwards)</span></span>
