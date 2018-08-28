---
title: Entity Framework - EF Core kullanarak bileşenleri test
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997843"
---
# <a name="testing"></a><span data-ttu-id="ca363-102">Sınama</span><span class="sxs-lookup"><span data-stu-id="ca363-102">Testing</span></span>

<span data-ttu-id="ca363-103">Asıl veritabanı g/ç işlemleri yükü olmadan gerçek veritabanına bağlanıyor benzeyen bir şey kullanarak bileşenleri test isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca363-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="ca363-104">Bunu yapmak için iki ana seçeneğiniz vardır:</span><span class="sxs-lookup"><span data-stu-id="ca363-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="ca363-105">[SQLite bellek içi modda](sqlite.md) , ilişkisel bir veritabanı gibi davranan bir sağlayıcısına verimli testleri yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ca363-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="ca363-106">[Inmemory sağlayıcı](in-memory.md) en az bir bağımlılığı yoktur, ancak her zaman bir ilişkisel veritabanı gibi davranmaz basit bir sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="ca363-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
