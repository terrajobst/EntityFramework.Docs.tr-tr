---
title: "Pomelo MySQL veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="51a74-102">MySQL için pomelo EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="51a74-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="51a74-103">Bu veritabanı sağlayıcısı MySQL ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="51a74-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="51a74-104">Sağlayıcı bir parçası olarak korunur [Pomelo Foundation proje](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="51a74-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="51a74-105">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="51a74-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="51a74-106">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="51a74-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="51a74-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="51a74-107">Install</span></span>

<span data-ttu-id="51a74-108">Yükleme [Pomelo.EntityFrameworkCore.MySql NuGet paketi](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="51a74-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="51a74-109">Başlarken</span><span class="sxs-lookup"><span data-stu-id="51a74-109">Get Started</span></span>

<span data-ttu-id="51a74-110">Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="51a74-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="51a74-111">Başlatılan belgeleri alma</span><span class="sxs-lookup"><span data-stu-id="51a74-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="51a74-112">Yuuko Blog örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="51a74-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="51a74-113">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="51a74-113">Supported Database Engines</span></span>

* <span data-ttu-id="51a74-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="51a74-114">MySQL</span></span>
* <span data-ttu-id="51a74-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="51a74-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="51a74-116">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="51a74-116">Supported Platforms</span></span>

* <span data-ttu-id="51a74-117">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="51a74-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="51a74-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="51a74-118">.NET Core</span></span>

* <span data-ttu-id="51a74-119">Mono (veya sonraki sürümleri 4.2.0)</span><span class="sxs-lookup"><span data-stu-id="51a74-119">Mono (4.2.0 onwards)</span></span>
