---
title: Devralma - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: f6b5c8f5a398ac1e28e29bc17f0674c5b76d7b20
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319133"
---
# <a name="inheritance"></a><span data-ttu-id="7d00b-102">Devralma</span><span class="sxs-lookup"><span data-stu-id="7d00b-102">Inheritance</span></span>

<span data-ttu-id="7d00b-103">Devralma EF modeli, varlık sınıflarının Devralmada veritabanında nasıl temsil edildiğini denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7d00b-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7d00b-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="7d00b-104">Conventions</span></span>

<span data-ttu-id="7d00b-105">Kural olarak, bu devralma veritabanında nasıl gösterileceğini belirlemek için veritabanı sağlayıcısı kadar olur.</span><span class="sxs-lookup"><span data-stu-id="7d00b-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="7d00b-106">Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) için bu bir ilişkisel veritabanı sağlayıcısı ile nasıl işlenir.</span><span class="sxs-lookup"><span data-stu-id="7d00b-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="7d00b-107">İki veya daha fazla devralınan türlerin açıkça modelde varsa EF yalnızca devralma ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7d00b-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="7d00b-108">EF modeli bulunmayan temel veya türetilmiş türleri için taramaz.</span><span class="sxs-lookup"><span data-stu-id="7d00b-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="7d00b-109">Göstererek modelde türleri içerebilir bir *olan DB<TEntity>*  devralma hiyerarşisinde her türü için.</span><span class="sxs-lookup"><span data-stu-id="7d00b-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="7d00b-110">Kullanıma sunmak istemiyorsanız bir *olan DB<TEntity>*  hiyerarşideki bir veya daha fazla varlık için modele dahil oldukları emin olmak için Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d00b-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="7d00b-111">Ve kurallarını güvenmeyin, açıkça kullanarak temel tür belirtebilirsiniz `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="7d00b-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="7d00b-112">Kullanabileceğiniz `.HasBaseType((Type)null)` bu hiyerarşisinden bir varlık türü kaldırılamadı.</span><span class="sxs-lookup"><span data-stu-id="7d00b-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7d00b-113">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="7d00b-113">Data Annotations</span></span>

<span data-ttu-id="7d00b-114">Veri ek açıklamaları, devralmayı yapılandırmak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="7d00b-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7d00b-115">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="7d00b-115">Fluent API</span></span>

<span data-ttu-id="7d00b-116">Devralma Fluent API'si, kullanmakta olduğunuz veritabanı sağlayıcısına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="7d00b-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="7d00b-117">Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) gerçekleştirmek için ilişkisel veritabanı sağlayıcısı yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="7d00b-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
