---
title: Veri kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417550"
---
# <a name="saving-data"></a><span data-ttu-id="d2585-102">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d2585-102">Saving Data</span></span>

<span data-ttu-id="d2585-103">Her bağlam örneği, veritabanına yazılması gereken değişiklikleri saklamaktan sorumlu bir `ChangeTracker` sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d2585-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="d2585-104">Varlık sınıflarınızın örneklerine değişiklik yaparken, bu değişiklikler `ChangeTracker` kaydedilir ve `SaveChanges`çağırdığınızda veritabanına yazılır.</span><span class="sxs-lookup"><span data-stu-id="d2585-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="d2585-105">Veritabanı sağlayıcısı, değişiklikleri veritabanına özgü işlemlere çevirmekten sorumludur (örneğin, bir ilişkisel veritabanı için `INSERT`, `UPDATE`ve `DELETE` komutları).</span><span class="sxs-lookup"><span data-stu-id="d2585-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
