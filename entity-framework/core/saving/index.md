---
title: EF Core - veri kaydetme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998186"
---
# <a name="saving-data"></a><span data-ttu-id="3d7f9-102">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="3d7f9-102">Saving Data</span></span>

<span data-ttu-id="3d7f9-103">Bağlam örneğinin her bir `ChangeTracker` veritabanına yazılması gereken değişiklikleri izlemek için sorumlu.</span><span class="sxs-lookup"><span data-stu-id="3d7f9-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="3d7f9-104">Bu değişiklikler kaydedilir varlık sınıfları örneklerine değişiklik olarak `ChangeTracker` ve çağırdığınızda ardından veritabanına yazılan `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3d7f9-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="3d7f9-105">Değişiklikleri veritabanına özel işlemler çevirmek için veritabanı sağlayıcısı sorumludur (örneğin, `INSERT`, `UPDATE`, ve `DELETE` komutları ilişkisel bir veritabanı için).</span><span class="sxs-lookup"><span data-stu-id="3d7f9-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
