---
title: Veritabanı şemalarını - EF çekirdek yönetme
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054741"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="530f3-102">Veritabanı şemalarını yönetme</span><span class="sxs-lookup"><span data-stu-id="530f3-102">Managing Database Schemas</span></span>
<span data-ttu-id="530f3-103">EF çekirdek EF çekirdek modeli ve veritabanı şeması eşitlenmiş şekilde kalmasının iki birincil yolu sağlar. İkisi arasında seçmek için EF çekirdek modelinizi veya veritabanı şeması gerçekte kaynak olup olmadığına karar vermek.</span><span class="sxs-lookup"><span data-stu-id="530f3-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="530f3-104">Gerçekte kaynağı olarak EF çekirdek modelinizi istiyorsanız kullanın [geçişler][1].</span><span class="sxs-lookup"><span data-stu-id="530f3-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="530f3-105">EF çekirdek modelinizi değişiklik gibi EF çekirdek modeliyle uyumlu kalmayacak şekilde bu yaklaşım karşılık gelen şema değişiklikleri veritabanınıza artımlı olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="530f3-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="530f3-106">Kullanım [ters mühendislik] [ 2] gerçekte kaynağı olarak veritabanı şemanızı istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="530f3-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="530f3-107">Bu yaklaşım, bir DbContext ve varlık türü tersine veritabanı şemanızı EF çekirdek modeline mühendislik tarafından iskelesi olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="530f3-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="530f3-108">[Oluşturmak ve API bırakma] [ 3] veritabanı şeması EF çekirdek modelden de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="530f3-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="530f3-109">Ancak, bunlar öncelikli olarak sınama, prototipi oluşturulurken ve veritabanını silmek kabul edilebilir olduğu diğer senaryolar için değildir.</span><span class="sxs-lookup"><span data-stu-id="530f3-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
