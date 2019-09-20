---
title: SQLite veritabanı sağlayıcısı-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149270"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="d508b-102">SQLite EF Core veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d508b-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="d508b-103">Bu veritabanı sağlayıcısı, Entity Framework Core SQLite ile kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="d508b-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="d508b-104">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="d508b-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="d508b-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="d508b-105">Install</span></span>

<span data-ttu-id="d508b-106">[Microsoft. EntityFrameworkCore. SQLite NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)yükler.</span><span class="sxs-lookup"><span data-stu-id="d508b-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a><span data-ttu-id="d508b-107">Desteklenen veritabanı motorları</span><span class="sxs-lookup"><span data-stu-id="d508b-107">Supported Database Engines</span></span>

* <span data-ttu-id="d508b-108">SQLite (3,7 sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="d508b-108">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="d508b-109">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="d508b-109">Limitations</span></span>

<span data-ttu-id="d508b-110">SQLite sağlayıcının bazı önemli sınırlamaları için bkz. [SQLite sınırlamaları](limitations.md) .</span><span class="sxs-lookup"><span data-stu-id="d508b-110">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
