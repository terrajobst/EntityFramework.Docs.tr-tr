---
title: "PostgreSQL - EF çekirdek için Npgsql veritabanı sağlayıcısı"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="e9173-102">PostgreSQL için Npgsql EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="e9173-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="e9173-103">Bu veritabanı sağlayıcısı ile PostgreSQL kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e9173-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="e9173-104">Sağlayıcı bir parçası olarak korunur [Npgsql proje](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="e9173-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="e9173-105">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="e9173-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="e9173-106">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="e9173-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="e9173-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="e9173-107">Install</span></span>

<span data-ttu-id="e9173-108">Yükleme [Npgsql.EntityFrameworkCore.PostgreSQL NuGet paketi](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="e9173-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="e9173-109">Başlarken</span><span class="sxs-lookup"><span data-stu-id="e9173-109">Get Started</span></span>

<span data-ttu-id="e9173-110">Bkz: [Npgsql belgelerine](http://www.npgsql.org/efcore/index.html) başlamak için.</span><span class="sxs-lookup"><span data-stu-id="e9173-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="e9173-111">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="e9173-111">Supported Database Engines</span></span>

* <span data-ttu-id="e9173-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e9173-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e9173-113">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="e9173-113">Supported Platforms</span></span>

* <span data-ttu-id="e9173-114">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="e9173-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="e9173-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9173-115">.NET Core</span></span>

* <span data-ttu-id="e9173-116">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="e9173-116">Mono (4.2.0 onwards)</span></span>
