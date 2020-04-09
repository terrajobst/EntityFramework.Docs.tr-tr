---
title: Veri Kaydetme - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417550"
---
# <a name="saving-data"></a><span data-ttu-id="5c052-102">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="5c052-102">Saving Data</span></span>

<span data-ttu-id="5c052-103">Her bağlam örneğinin veritabanına yazılması gereken değişiklikleri izlemekten sorumlu bir `ChangeTracker` örneği vardır.</span><span class="sxs-lookup"><span data-stu-id="5c052-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="5c052-104">Varlık sınıflarınızın örneklerinde değişiklik yaptığınızda, bu değişiklikler veritabanına kaydedilir `ChangeTracker` ve 'yi `SaveChanges`aradiğinizde veritabanına yazılır.</span><span class="sxs-lookup"><span data-stu-id="5c052-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="5c052-105">Veritabanı sağlayıcısı, değişiklikleri veritabanına özgü işlemlere (örneğin, `INSERT` `UPDATE`ve ilişkisel `DELETE` bir veritabanı için komutlar) çevirmekten sorumludur.</span><span class="sxs-lookup"><span data-stu-id="5c052-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
