---
title: "Devralma - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance"></a><span data-ttu-id="fa12f-102">Devralma</span><span class="sxs-lookup"><span data-stu-id="fa12f-102">Inheritance</span></span>

<span data-ttu-id="fa12f-103">Devralma EF modeldeki varlık sınıflarını devralma veritabanında nasıl temsil denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fa12f-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="fa12f-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="fa12f-104">Conventions</span></span>

<span data-ttu-id="fa12f-105">Kurala göre bu veritabanında devralma nasıl gösterileceğini belirlemek için veritabanı sağlayıcısı kadar olur.</span><span class="sxs-lookup"><span data-stu-id="fa12f-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="fa12f-106">Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) için bu bir ilişkisel veritabanı sağlayıcısı ile nasıl işlenir.</span><span class="sxs-lookup"><span data-stu-id="fa12f-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="fa12f-107">İki veya daha fazla devralınan türleri açıkça modelde varsa EF yalnızca devralma ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fa12f-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="fa12f-108">EF modelinde bulunmayan temel veya türetilmiş türler için taramaz.</span><span class="sxs-lookup"><span data-stu-id="fa12f-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="fa12f-109">Göstererek modelde türleri içerebilir bir *DbSet<TEntity>*  her türünün Devralma Hiyerarşisi.</span><span class="sxs-lookup"><span data-stu-id="fa12f-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="fa12f-110">Kullanıma istemiyorsanız, bir *DbSet<TEntity>*  hiyerarşideki bir veya daha fazla varlıklar için Fluent API modele dahil bunlar emin olmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa12f-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="fa12f-111">Ve kurallarına güvenmeyin, açıkça kullanarak temel türünü belirtebilirsiniz `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="fa12f-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="fa12f-112">Kullanabileceğiniz `.HasBaseType((Type)null)` hiyerarşisinden bir varlık türünü kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="fa12f-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="fa12f-113">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="fa12f-113">Data Annotations</span></span>

<span data-ttu-id="fa12f-114">Devralma yapılandırmak için veri ek açıklamaları kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="fa12f-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="fa12f-115">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="fa12f-115">Fluent API</span></span>

<span data-ttu-id="fa12f-116">Devralma Fluent API'si kullandığınız veritabanı sağlayıcısına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fa12f-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="fa12f-117">Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) gerçekleştirmek için bir ilişkisel veritabanı sağlayıcı yapılandırması için.</span><span class="sxs-lookup"><span data-stu-id="fa12f-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
