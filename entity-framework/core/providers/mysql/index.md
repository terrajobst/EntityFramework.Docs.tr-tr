---
title: "MySQL veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: c151845c8b08ef6a668b352f15545752156b0a9d
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="29062-102">MySQL EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="29062-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="29062-103">Bu veritabanı sağlayıcısı MySQL ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="29062-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="29062-104">Sağlayıcı bir parçası olarak korunur [MySQL proje](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="29062-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="29062-105">Yayın öncesi sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="29062-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="29062-106">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="29062-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="29062-107">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="29062-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="29062-108">Yükleme</span><span class="sxs-lookup"><span data-stu-id="29062-108">Install</span></span>

<span data-ttu-id="29062-109">Yükleme [MySql.Data.EntityFrameworkCore NuGet paketi](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="29062-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="29062-110">Başlarken</span><span class="sxs-lookup"><span data-stu-id="29062-110">Get Started</span></span>

<span data-ttu-id="29062-111">Bkz: [MySQL EF çekirdek sağlayıcısı ve Connector/Net 7.0.4 başlatarak](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span><span class="sxs-lookup"><span data-stu-id="29062-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="29062-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="29062-112">Supported Database Engines</span></span>

* <span data-ttu-id="29062-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="29062-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="29062-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="29062-114">Supported Platforms</span></span>

* <span data-ttu-id="29062-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="29062-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="29062-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="29062-116">.NET Core</span></span>
