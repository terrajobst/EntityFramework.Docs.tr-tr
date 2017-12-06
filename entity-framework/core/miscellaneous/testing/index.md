---
title: "Entity Framework - EF çekirdek kullanarak bileşenleri test"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a><span data-ttu-id="44e99-102">Test etme</span><span class="sxs-lookup"><span data-stu-id="44e99-102">Testing</span></span>

<span data-ttu-id="44e99-103">Asıl veritabanını g/ç işlemleri yükü olmadan gerçek veritabanına bağlanma benzeyen bir şey kullanarak bileşenleri test etmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e99-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="44e99-104">Bunu yapmak için iki ana seçeneğiniz vardır:</span><span class="sxs-lookup"><span data-stu-id="44e99-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="44e99-105">[SQLite bellek içi modu](sqlite.md) ilişkisel veritabanı gibi davranan bir sağlayıcısına verimli testleri yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="44e99-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="44e99-106">[Inmemory sağlayıcısı](in-memory.md) en az bağımlılıklara sahiptir, ancak her zaman bir ilişkisel veritabanı gibi davranır olmayan basit bir sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="44e99-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
