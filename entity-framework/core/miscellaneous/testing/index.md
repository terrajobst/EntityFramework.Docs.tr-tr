---
title: EF Core kullanarak bileşenleri test etme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181303"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="60802-102">EF Core kullanarak bileşenleri test etme</span><span class="sxs-lookup"><span data-stu-id="60802-102">Testing components using EF Core</span></span>

<span data-ttu-id="60802-103">Gerçek veritabanı g/ç işlemlerinin ek yükü olmadan, gerçek veritabanına bağlanan bir şeyi kullanarak bileşenleri test etmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="60802-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="60802-104">Bunu yapmanın iki ana seçeneği vardır:</span><span class="sxs-lookup"><span data-stu-id="60802-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="60802-105">[SQLite bellek içi modu](sqlite.md) , ilişkisel bir veritabanı gibi davranan bir sağlayıcıya karşı etkili testler yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="60802-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="60802-106">[InMemory sağlayıcısı](in-memory.md) , en az bağımlılığı olan basit bir sağlayıcıdır, ancak her zaman ilişkisel bir veritabanı gibi davranmaz.</span><span class="sxs-lookup"><span data-stu-id="60802-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
