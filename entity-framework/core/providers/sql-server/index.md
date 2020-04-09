---
title: Microsoft SQL Server Veritabanı Sağlayıcısı - EF Core
description: Entity Framework Core'un Microsoft SQL Server ile kullanılmasına izin veren veritabanı sağlayıcısı için belgeler
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417796"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="3fe46-103">Microsoft SQL Server EF Çekirdek Veritabanı Sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="3fe46-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="3fe46-104">Bu veritabanı sağlayıcısı, Entity Framework Core'un Microsoft SQL Server (Azure SQL Veritabanı dahil) ile kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="3fe46-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="3fe46-105">Sağlayıcı, Varlık Çerçeve Çekirdek [Projesi'nin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="3fe46-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="3fe46-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="3fe46-106">Install</span></span>

<span data-ttu-id="3fe46-107">[Microsoft.EntityFrameworkCore.SqlServer NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3fe46-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="3fe46-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3fe46-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="3fe46-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3fe46-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="3fe46-110">Sürüm 3.0.0'dan bu yana sağlayıcı Microsoft.Data.SqlClient'a başvurur (önceki sürümler System.Data.SqlClient'a bağlıdır).</span><span class="sxs-lookup"><span data-stu-id="3fe46-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="3fe46-111">Projeniz SqlClient'a doğrudan bağımlı hale alıyorsa, Microsoft.Data.SqlClient paketine başvuruldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="3fe46-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="3fe46-112">Desteklenen Veritabanı Motorları</span><span class="sxs-lookup"><span data-stu-id="3fe46-112">Supported Database Engines</span></span>

* <span data-ttu-id="3fe46-113">Microsoft SQL Server (2012'den itibaren)</span><span class="sxs-lookup"><span data-stu-id="3fe46-113">Microsoft SQL Server (2012 onwards)</span></span>
