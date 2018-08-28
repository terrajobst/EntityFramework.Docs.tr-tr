---
title: Veri üretme - EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994485"
---
# <a name="data-seeding"></a><span data-ttu-id="97584-102">Veri çekirdeği oluşturma</span><span class="sxs-lookup"><span data-stu-id="97584-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="97584-103">Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="97584-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="97584-104">Bir veritabanını doldurmak için ilk veri sağlayacak veri üretme sağlar.</span><span class="sxs-lookup"><span data-stu-id="97584-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="97584-105">EF Core içinde EF6 içinde veri dengeli dağıtım modeli yapılandırmasının bir parçası olarak bir varlık türü ile ilişkili benzemez.</span><span class="sxs-lookup"><span data-stu-id="97584-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="97584-106">Ardından EF Core [geçişler](xref:core/managing-schemas/migrations/index) ne ekleme, güncelleştirme veya silme işlemleri veritabanı modelin yeni bir sürüme yükseltirken uygulanmasına gerek işlem otomatik olarak.</span><span class="sxs-lookup"><span data-stu-id="97584-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="97584-107">Örneğin, bu için çekirdek veri yapılandırmak için kullanabileceğiniz bir `Blog` içinde `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="97584-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="97584-108">Bir ilişki, yabancı anahtar değerlerine sahip varlıklar eklemek için belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="97584-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="97584-109">Sık anonim bir sınıf değerleri ayarlayabilmek için kullanılması gereken şekilde gölge durumunda, yabancı anahtar özellikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="97584-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="97584-110">Varlıkları ekledikten sonra kullanmak için önerilir [geçişler](xref:core/managing-schemas/migrations/index) değişiklikleri uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="97584-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="97584-111">Alternatif olarak, `context.Database.EnsureCreated()` bellek içi sağlayıcısı kullanırken veya örneğin, bir test veritabanı için çekirdek veri içeren yeni bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="97584-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="97584-112">Unutmayın veritabanı zaten var, `EnsureCreated()` ne ya da veritabanında çekirdek veri şemasını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="97584-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
