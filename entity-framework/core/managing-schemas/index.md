---
title: Veritabanı şemalarını yönetme-EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416307"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="ee7f0-102">Veritabanı Şemalarını Yönetme</span><span class="sxs-lookup"><span data-stu-id="ee7f0-102">Managing Database Schemas</span></span>

<span data-ttu-id="ee7f0-103">EF Core, EF Core modelinizi ve veritabanı şemanızı eşitlenmiş halde tutmanın iki temel yolunu sağlar. İkisi arasında seçim yapmak için EF Core modelinizin veya veritabanı şemasının Truth kaynağı olup olmadığına karar verin.</span><span class="sxs-lookup"><span data-stu-id="ee7f0-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="ee7f0-104">EF Core modelinizin Truth kaynağı olmasını istiyorsanız, [geçişleri][1]kullanın.</span><span class="sxs-lookup"><span data-stu-id="ee7f0-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="ee7f0-105">EF Core modelinizde değişiklik yaparken bu yaklaşım, ilgili şema değişikliklerini, EF Core modelinizle uyumlu kalacak şekilde veritabanınıza uygular.</span><span class="sxs-lookup"><span data-stu-id="ee7f0-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="ee7f0-106">Veritabanı şemanızın Truth kaynağı olmasını istiyorsanız [ters mühendislik][2] kullanın.</span><span class="sxs-lookup"><span data-stu-id="ee7f0-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="ee7f0-107">Bu yaklaşım, veritabanı şemanızı bir EF Core modeline ters mühendislik yaparak bir DbContext ve varlık türü sınıflarını dolandırıcılara katmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ee7f0-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="ee7f0-108">[Oluşturma ve bırakma API 'leri][3] , EF Core modelinizdeki veritabanı şemasını da oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="ee7f0-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="ee7f0-109">Bununla birlikte, bunlar birincil olarak test, prototip oluşturma ve veritabanını bırakma için kabul edilebilir olan diğer senaryolar içindir.</span><span class="sxs-lookup"><span data-stu-id="ee7f0-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
