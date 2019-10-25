---
title: Microsoft SQL Server veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812095"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="777da-102">Microsoft SQL Server EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="777da-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="777da-103">Bu veritabanı sağlayıcısı Entity Framework Core Microsoft SQL Server (SQL Azure dahil) birlikte kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="777da-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="777da-104">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="777da-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="777da-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="777da-105">Install</span></span>

<span data-ttu-id="777da-106">[Microsoft. EntityFrameworkCore. SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükler.</span><span class="sxs-lookup"><span data-stu-id="777da-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="777da-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="777da-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="777da-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="777da-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="777da-109">Sürüm 3.0.0 sürümünden itibaren, sağlayıcı Microsoft. Data. SqlClient 'e başvuruyor (önceki sürümler System. Data. SqlClient 'a bağımlı).</span><span class="sxs-lookup"><span data-stu-id="777da-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="777da-110">Projeniz SqlClient doğrudan bağımlılığı alırsa, doğru pakete başvurduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="777da-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="777da-111">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="777da-111">Supported Database Engines</span></span>

* <span data-ttu-id="777da-112">Microsoft SQL Server (2012 sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="777da-112">Microsoft SQL Server (2012 onwards)</span></span>
