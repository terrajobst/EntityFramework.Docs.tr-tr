---
title: Devralma-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197291"
---
# <a name="inheritance"></a><span data-ttu-id="34b4c-102">Devralma</span><span class="sxs-lookup"><span data-stu-id="34b4c-102">Inheritance</span></span>

<span data-ttu-id="34b4c-103">EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34b4c-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="34b4c-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="34b4c-104">Conventions</span></span>

<span data-ttu-id="34b4c-105">Kurala göre, devralmanın veritabanında nasıl temsil edileceğini belirleme veritabanı sağlayıcısına kadar olur.</span><span class="sxs-lookup"><span data-stu-id="34b4c-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="34b4c-106">Bunun ilişkisel veritabanı sağlayıcısıyla nasıl işlendiği hakkında bilgi için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="34b4c-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="34b4c-107">EF yalnızca, modele iki veya daha fazla devralınan tür açık olarak dahil edil, devralma işlemini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="34b4c-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="34b4c-108">EF, başka türlü modele dahil olmayan temel veya türetilmiş türler için tarama olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="34b4c-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="34b4c-109">Devralma hiyerarşisindeki her bir tür için bir *dbset<TEntity>*  sunarak modele türler ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34b4c-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="34b4c-110">Hiyerarşide bir veya daha fazla varlık için bir *dbset<TEntity>*  göstermek istemiyorsanız, bu API 'lerin modele dahil olduklarından emin olmak için akıcı API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34b4c-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="34b4c-111">Kurallara bağlı değilseniz, temel türü açıkça kullanarak `HasBaseType`belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34b4c-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="34b4c-112">Hiyerarşisinden bir varlık `.HasBaseType((Type)null)` türünü kaldırmak için ' i kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34b4c-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="34b4c-113">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="34b4c-113">Data Annotations</span></span>

<span data-ttu-id="34b4c-114">Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="34b4c-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="34b4c-115">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="34b4c-115">Fluent API</span></span>

<span data-ttu-id="34b4c-116">Devralma için akıcı API, kullanmakta olduğunuz veritabanı sağlayıcısına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="34b4c-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="34b4c-117">İlişkisel bir veritabanı sağlayıcısı için gerçekleştirebileceğiniz yapılandırma için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="34b4c-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
