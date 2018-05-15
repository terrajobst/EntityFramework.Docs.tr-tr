---
title: Verileri dengeli - EF çekirdek
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="data-seeding"></a><span data-ttu-id="5de39-102">Dengeli veri</span><span class="sxs-lookup"><span data-stu-id="5de39-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="5de39-103">Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="5de39-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="5de39-104">Bir veritabanını doldurmak için ilk veri sağlamak için verileri dengeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="5de39-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="5de39-105">Farklı olarak EF çekirdek içinde EF6 içinde verileri dengeli bir varlık türü, model yapılandırmasının bir parçası olarak ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="5de39-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="5de39-106">Ardından EF çekirdek [geçişler](xref:core/managing-schemas/migrations/index) ne ekleme, güncelleştirme veya silme işlemleri veritabanı modeli yeni bir sürüme yükseltirken uygulanması gerek işlem otomatik olarak.</span><span class="sxs-lookup"><span data-stu-id="5de39-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="5de39-107">Örnek olarak, bu çekirdek verileri için yapılandırmak için kullanabileceğiniz bir `Blog` içinde `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="5de39-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="5de39-108">Bir ilişki yabancı anahtar değerlere sahip varlıklar ekleyebilir belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5de39-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="5de39-109">Sık anonim bir sınıf değerlerini ayarlamak için kullanılması gereken şekilde gölge durumunda, yabancı anahtar özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5de39-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="5de39-110">Varlıkları eklendikten sonra kullanmak için önerilir [geçişler](xref:core/managing-schemas/migrations/index) değişiklikleri uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="5de39-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="5de39-111">Alternatif olarak, kullanabileceğiniz `context.Database.EnsureCreated()` çekirdek verileri, örneğin, bir test veritabanı için veya bellek içi sağlayıcı kullanırken içeren yeni bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="5de39-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="5de39-112">Unutmayın veritabanı zaten var., `EnsureCreated()` hiçbiri şemayı ya da veritabanındaki çekirdek verileri güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="5de39-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
