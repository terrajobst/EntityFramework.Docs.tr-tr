---
title: "MySQL veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="5e7c1-102">MySQL EF çekirdek veritabanı sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="5e7c1-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="5e7c1-103">Bu veritabanı sağlayıcısı MySQL ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e7c1-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="5e7c1-104">Sağlayıcı bir parçası olarak korunur [MySQL proje](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="5e7c1-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="5e7c1-105">Yayın öncesi sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="5e7c1-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="5e7c1-106">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="5e7c1-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="5e7c1-107">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="5e7c1-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="5e7c1-108">Yükleme</span><span class="sxs-lookup"><span data-stu-id="5e7c1-108">Install</span></span>

<span data-ttu-id="5e7c1-109">Yükleme [MySql.Data.EntityFrameworkCore NuGet paketi](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="5e7c1-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="5e7c1-110">Başlarken</span><span class="sxs-lookup"><span data-stu-id="5e7c1-110">Get Started</span></span>

<span data-ttu-id="5e7c1-111">Bkz: [MySQL EF çekirdek sağlayıcısı ve Connector/Net 7.0.4 başlatarak](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span><span class="sxs-lookup"><span data-stu-id="5e7c1-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="5e7c1-112">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="5e7c1-112">Supported Database Engines</span></span>

* <span data-ttu-id="5e7c1-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="5e7c1-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="5e7c1-114">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="5e7c1-114">Supported Platforms</span></span>

* <span data-ttu-id="5e7c1-115">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="5e7c1-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="5e7c1-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e7c1-116">.NET Core</span></span>

<span data-ttu-id="5e7c1-117">MySQL belgeleri sürüm uyumluluk bilgilerini gözden geçirmeyi unutmayın [burada](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) ve [burada](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span><span class="sxs-lookup"><span data-stu-id="5e7c1-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
