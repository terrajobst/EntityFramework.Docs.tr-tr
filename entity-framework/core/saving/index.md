---
title: EF çekirdek verileri - kaydetme
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="saving-data"></a><span data-ttu-id="b741c-102">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="b741c-102">Saving Data</span></span>

<span data-ttu-id="b741c-103">Her bağlam örneğine sahip bir `ChangeTracker` veritabanına yazılması gereken değişiklikleri izlemek için sorumlu.</span><span class="sxs-lookup"><span data-stu-id="b741c-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="b741c-104">Varlık sınıflarınızı örneklerine değişiklik gibi bu değişiklikler kaydedilir `ChangeTracker` ve çağırdığınızda veritabanına yazılan `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b741c-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="b741c-105">Veritabanı sağlayıcısı değişiklikleri veritabanına özgü işlemlere çevirmek için sorumlu olduğu (örneğin `INSERT`, `UPDATE`, ve `DELETE` komutları bir ilişkisel veritabanı için).</span><span class="sxs-lookup"><span data-stu-id="b741c-105">The database provider is responsible for translating the changes into database-specific operations (e.g. `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
