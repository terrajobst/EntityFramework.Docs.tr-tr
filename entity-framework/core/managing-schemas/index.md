---
title: Veritabanı Şemalarını Yönetme - EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416307"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="b4f4f-102">Veritabanı Şemalarını Yönetme</span><span class="sxs-lookup"><span data-stu-id="b4f4f-102">Managing Database Schemas</span></span>

<span data-ttu-id="b4f4f-103">EF Core, EF Core modelinizi ve veritabanı şemazınızı eşit tutmak için iki temel yol sağlar. İkisi arasında seçim yapmak için, EF Core modelinizin mi yoksa veritabanı şemazınızın gerçeğin kaynağı mı olduğuna karar verin.</span><span class="sxs-lookup"><span data-stu-id="b4f4f-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="b4f4f-104">EF Core modelinizin gerçeğin kaynağı olmasını istiyorsanız, [Geçişler'i][1]kullanın.</span><span class="sxs-lookup"><span data-stu-id="b4f4f-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="b4f4f-105">EF Core modelinizde değişiklik yaptığınızda, bu yaklaşım, EF Core modelinizle uyumlu kalması için veritabanınıza karşılık gelen şema değişikliklerini aşamalı olarak uygular.</span><span class="sxs-lookup"><span data-stu-id="b4f4f-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="b4f4f-106">Veritabanı şemanızın gerçeğin kaynağı olmasını istiyorsanız [Ters Mühendislik'i][2] kullanın.</span><span class="sxs-lookup"><span data-stu-id="b4f4f-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="b4f4f-107">Bu yaklaşım, veritabanı şemanızı bir EF Core modeline dönüştürerek bir DbContext ve varlık türü sınıflarını iskeleye dönüştürmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="b4f4f-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="b4f4f-108">[OLUŞTURMA ve bırakma API'leri,][3] EF Core modelinizden veritabanı şeasını da oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="b4f4f-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="b4f4f-109">Ancak, bunlar öncelikle veritabanını bırakmanın kabul edilebilir olduğu sınama, prototip oluşturma ve diğer senaryolar içindir.</span><span class="sxs-lookup"><span data-stu-id="b4f4f-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
