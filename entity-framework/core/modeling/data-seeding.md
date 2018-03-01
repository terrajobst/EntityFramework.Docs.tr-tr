---
title: "Verileri dengeli - EF çekirdek"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a><span data-ttu-id="c7ba4-102">Dengeli veri</span><span class="sxs-lookup"><span data-stu-id="c7ba4-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="c7ba4-103">Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="c7ba4-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c7ba4-104">Bir veritabanını doldurmak için ilk veri sağlamak için verileri dengeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="c7ba4-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="c7ba4-105">Farklı olarak EF çekirdek içinde EF6 içinde verileri dengeli bir varlık türü, model yapılandırmasının bir parçası olarak ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="c7ba4-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="c7ba4-106">Ardından EF çekirdek geçişler otomatik olarak ne ekleme, güncelleştirme veya silme işlemleri veritabanı modeli yeni bir sürüme yükseltirken uygulanması gerek hesaplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7ba4-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="c7ba4-107">Örnek olarak, bu çekirdek verileri için yapılandırmak için kullanabileceğiniz bir `Blog` içinde `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="c7ba4-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="c7ba4-108">Bir ilişki yabancı anahtar değerlere sahip varlıklar ekleyebilir belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c7ba4-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="c7ba4-109">Sık anonim bir sınıf değerlerini ayarlamak için kullanılması gereken şekilde gölge durumunda, yabancı anahtar özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c7ba4-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
