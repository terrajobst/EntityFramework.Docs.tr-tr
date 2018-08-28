---
title: EF Core - veritabanı şemalarını yönetme
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994391"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="74042-102">Veritabanı Şemalarını Yönetme</span><span class="sxs-lookup"><span data-stu-id="74042-102">Managing Database Schemas</span></span>
<span data-ttu-id="74042-103">EF Core EF Core model ve veritabanı şemanızı eşitlenmiş durumda tutma iki birincil yöntem sağlar. İki tür arasında seçmek için EF Core modelinizi veya veritabanı şeması gerçeklik kaynağı olup olmadığına karar vermek.</span><span class="sxs-lookup"><span data-stu-id="74042-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="74042-104">EF Core modelinizin gerçeklik kaynağı olmasını istediğiniz kullanırsanız [geçişler][1].</span><span class="sxs-lookup"><span data-stu-id="74042-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="74042-105">EF Core modelinizi yaptığınız gibi EF Core modeliyle uyumlu kalmayacak şekilde bu yaklaşım karşılık gelen şema değişiklikleri veritabanınıza artımlı olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="74042-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="74042-106">Kullanım [ters mühendislik] [ 2] gerçeklik kaynağı olarak veritabanı şemanızı istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="74042-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="74042-107">Bu yaklaşım, veritabanı Şemanızda bir EF Core modeline mühendislik ters tarafından bir DbContext ve varlık türü sınıfları iskelesini olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="74042-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="74042-108">[API oluşturma ve bırakma] [ 3] veritabanı şeması EF Core modelinizden de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="74042-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="74042-109">Ancak, öncelikli olarak sınama, prototip oluşturma ve veritabanı bırakılırken kabul edilebilir olduğu diğer senaryolar için değildirler.</span><span class="sxs-lookup"><span data-stu-id="74042-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
