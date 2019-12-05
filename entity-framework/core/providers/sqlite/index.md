---
title: SQLite veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 433dedbe1e0e11bf2fd83e259e321ece32b2c188
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824799"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="b74c3-102">SQLite EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b74c3-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="b74c3-103">Bu veritabanı sağlayıcısı, Entity Framework Core SQLite ile kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b74c3-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="b74c3-104">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="b74c3-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="b74c3-105">yükleme</span><span class="sxs-lookup"><span data-stu-id="b74c3-105">Install</span></span>

<span data-ttu-id="b74c3-106">[Microsoft. EntityFrameworkCore. SQLite NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)yükler.</span><span class="sxs-lookup"><span data-stu-id="b74c3-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="b74c3-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b74c3-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="b74c3-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b74c3-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="b74c3-109">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="b74c3-109">Supported Database Engines</span></span>

* <span data-ttu-id="b74c3-110">SQLite (3,7 sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="b74c3-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="b74c3-111">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="b74c3-111">Limitations</span></span>

<span data-ttu-id="b74c3-112">SQLite sağlayıcının bazı önemli sınırlamaları için bkz. [SQLite sınırlamaları](limitations.md) .</span><span class="sxs-lookup"><span data-stu-id="b74c3-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
