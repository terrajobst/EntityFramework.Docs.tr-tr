---
title: EF Core - veri kaydetme
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949401"
---
# <a name="saving-data"></a><span data-ttu-id="8cc9c-102">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="8cc9c-102">Saving Data</span></span>

<span data-ttu-id="8cc9c-103">Bağlam örneğinin her bir `ChangeTracker` veritabanına yazılması gereken değişiklikleri izlemek için sorumlu.</span><span class="sxs-lookup"><span data-stu-id="8cc9c-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="8cc9c-104">Bu değişiklikler kaydedilir varlık sınıfları örneklerine değişiklik olarak `ChangeTracker` ve çağırdığınızda ardından veritabanına yazılan `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="8cc9c-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="8cc9c-105">Değişiklikleri veritabanına özel işlemler çevirmek için veritabanı sağlayıcısı sorumludur (örneğin, `INSERT`, `UPDATE`, ve `DELETE` komutları ilişkisel bir veritabanı için).</span><span class="sxs-lookup"><span data-stu-id="8cc9c-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
