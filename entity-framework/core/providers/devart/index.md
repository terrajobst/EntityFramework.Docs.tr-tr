---
title: "Devart veritabanı sağlayıcıları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aad70a75-d04d-4d63-a489-7f9062a3c73d
ms.technology: entity-framework-core
uid: core/providers/devart/index
ms.openlocfilehash: 04de917b3327a6f544292781bca42a1170c199ad
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="devart-ef-core-database-providers"></a><span data-ttu-id="0e987-102">Devart EF çekirdek veritabanı sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="0e987-102">Devart EF Core Database Providers</span></span>

<span data-ttu-id="0e987-103">Devart çok çeşitli veritabanları için veritabanı sağlayıcısı sunan bir üçüncü taraf sağlayıcı yazıcı ' dir.</span><span class="sxs-lookup"><span data-stu-id="0e987-103">Devart is a third party provider writer that offers database providers for a wide range of databases.</span></span> <span data-ttu-id="0e987-104">Hakkında daha fazla bilgi [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span><span class="sxs-lookup"><span data-stu-id="0e987-104">Find out more at [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span></span>

> [!NOTE]  
> <span data-ttu-id="0e987-105">Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="0e987-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="0e987-106">Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="0e987-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="paid-versions-only"></a><span data-ttu-id="0e987-107">Yalnızca ücretli sürümleri</span><span class="sxs-lookup"><span data-stu-id="0e987-107">Paid Versions Only</span></span>

<span data-ttu-id="0e987-108">Devart dotConnect ticari üçüncü taraf sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="0e987-108">Devart dotConnect is a commercial third party provider.</span></span> <span data-ttu-id="0e987-109">Entity Framework desteği yalnızca ücretli dotConnect sürümlerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0e987-109">Entity Framework support is only available in paid versions of dotConnect.</span></span>

## <a name="install"></a><span data-ttu-id="0e987-110">Yükleme</span><span class="sxs-lookup"><span data-stu-id="0e987-110">Install</span></span>

<span data-ttu-id="0e987-111">Bkz: [Devart dotConnect belgelerine](https://www.devart.com/dotconnect/) yükleme yönergeleri için.</span><span class="sxs-lookup"><span data-stu-id="0e987-111">See the [Devart dotConnect documentation](https://www.devart.com/dotconnect/) for installation instructions.</span></span>

## <a name="get-started"></a><span data-ttu-id="0e987-112">Başlarken</span><span class="sxs-lookup"><span data-stu-id="0e987-112">Get Started</span></span>

<span data-ttu-id="0e987-113">Bkz: [Devart dotConnect Entity Framework belgelerine](https://www.devart.com/dotconnect/entityframework.html) ve [blog makale Entity Framework Çekirdek 1 desteği hakkında](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html) başlamak için.</span><span class="sxs-lookup"><span data-stu-id="0e987-113">See the [Devart dotConnect Entity Framework documentation](https://www.devart.com/dotconnect/entityframework.html) and [blog article about Entity Framework Core 1 Support](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="0e987-114">Veritabanı motoru desteklenen</span><span class="sxs-lookup"><span data-stu-id="0e987-114">Supported Database Engines</span></span>

* <span data-ttu-id="0e987-115">MySQL</span><span class="sxs-lookup"><span data-stu-id="0e987-115">MySQL</span></span>

* <span data-ttu-id="0e987-116">Oracle</span><span class="sxs-lookup"><span data-stu-id="0e987-116">Oracle</span></span>

* <span data-ttu-id="0e987-117">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0e987-117">PostgreSQL</span></span>

* <span data-ttu-id="0e987-118">SQLite</span><span class="sxs-lookup"><span data-stu-id="0e987-118">SQLite</span></span>

* <span data-ttu-id="0e987-119">DB2</span><span class="sxs-lookup"><span data-stu-id="0e987-119">DB2</span></span>

* [<span data-ttu-id="0e987-120">Bulut uygulamaları</span><span class="sxs-lookup"><span data-stu-id="0e987-120">Cloud apps</span></span>](https://www.devart.com/dotconnect/#cloud)

## <a name="supported-platforms"></a><span data-ttu-id="0e987-121">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="0e987-121">Supported Platforms</span></span>

* <span data-ttu-id="0e987-122">.NET framework (4.5.1 veya sonraki sürümleri)</span><span class="sxs-lookup"><span data-stu-id="0e987-122">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="0e987-123">.NET core 1.0 ve daha yüksek (Oracle, MySQL, PostgreSQL, SQLite sağlayıcıları)</span><span class="sxs-lookup"><span data-stu-id="0e987-123">.NET Core 1.0 and higher (providers for Oracle, MySQL, PostgreSQL, SQLite)</span></span>
