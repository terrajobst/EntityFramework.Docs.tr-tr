---
title: SQLite Veritabanı Sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417779"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="0ae4d-102">SQLite EF Çekirdek Veritabanı Sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="0ae4d-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="0ae4d-103">Bu veritabanı sağlayıcısı Entity Framework Core'un SQLite ile kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="0ae4d-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="0ae4d-104">Sağlayıcı, Entity Framework Core [projesinin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="0ae4d-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="0ae4d-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="0ae4d-105">Install</span></span>

<span data-ttu-id="0ae4d-106">[Microsoft.EntityFrameworkCore.Sqlite NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)yükleyin.</span><span class="sxs-lookup"><span data-stu-id="0ae4d-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="0ae4d-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0ae4d-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="0ae4d-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ae4d-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="0ae4d-109">Desteklenen Veritabanı Motorları</span><span class="sxs-lookup"><span data-stu-id="0ae4d-109">Supported Database Engines</span></span>

* <span data-ttu-id="0ae4d-110">SQLite (3.7'den itibaren)</span><span class="sxs-lookup"><span data-stu-id="0ae4d-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="0ae4d-111">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="0ae4d-111">Limitations</span></span>

<span data-ttu-id="0ae4d-112">SQLite sağlayıcısının bazı önemli sınırlamaları için [SQLite Sınırlamalarına](limitations.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="0ae4d-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
