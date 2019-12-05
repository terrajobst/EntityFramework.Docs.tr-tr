---
title: Devralma-EF Core
description: Entity Framework Core kullanarak varlık türü devralmayı yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824683"
---
# <a name="inheritance"></a><span data-ttu-id="d5d91-103">Devralma</span><span class="sxs-lookup"><span data-stu-id="d5d91-103">Inheritance</span></span>

<span data-ttu-id="d5d91-104">EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5d91-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d5d91-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="d5d91-105">Conventions</span></span>

<span data-ttu-id="d5d91-106">Varsayılan olarak, devralmanın veritabanında nasıl temsil edileceğini belirleme veritabanı sağlayıcısına ait olur.</span><span class="sxs-lookup"><span data-stu-id="d5d91-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="d5d91-107">Bunun ilişkisel veritabanı sağlayıcısıyla nasıl işlendiği hakkında bilgi için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="d5d91-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="d5d91-108">EF yalnızca, modele iki veya daha fazla devralınan tür açık olarak dahil edil, devralma işlemini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d5d91-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="d5d91-109">EF, başka türlü modele dahil olmayan temel veya türetilmiş türler için tarama olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d5d91-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="d5d91-110">Devralma hiyerarşisindeki her tür için bir *Dbset\<TEntity >* sunarak modele türler ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d5d91-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="d5d91-111">Hiyerarşide bir veya daha fazla varlık için bir *Dbset\<TEntity >* göstermek istemiyorsanız, bu API 'lerin modele dahil olduklarından emin olmak IÇIN akıcı API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d5d91-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="d5d91-112">Kurallara dayanmıyorsanız, `HasBaseType`kullanarak temel türü açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d5d91-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="d5d91-113">Hiyerarşisinden bir varlık türünü kaldırmak için `.HasBaseType((Type)null)` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d5d91-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d5d91-114">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d5d91-114">Data Annotations</span></span>

<span data-ttu-id="d5d91-115">Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="d5d91-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d5d91-116">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d5d91-116">Fluent API</span></span>

<span data-ttu-id="d5d91-117">Devralma için akıcı API, kullanmakta olduğunuz veritabanı sağlayıcısına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="d5d91-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="d5d91-118">İlişkisel bir veritabanı sağlayıcısı için gerçekleştirebileceğiniz yapılandırma için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="d5d91-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
