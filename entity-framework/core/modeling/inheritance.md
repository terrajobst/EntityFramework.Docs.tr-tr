---
title: Devralma-EF Core
description: Entity Framework Core kullanarak varlık türü devralmayı yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417297"
---
# <a name="inheritance"></a><span data-ttu-id="4626c-103">Devralma</span><span class="sxs-lookup"><span data-stu-id="4626c-103">Inheritance</span></span>

<span data-ttu-id="4626c-104">EF bir .NET tür hiyerarşisini bir veritabanıyla eşleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4626c-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="4626c-105">Bu, temel ve türetilmiş türleri kullanarak .NET varlıklarınızı kodda her zamanki gibi yazmanızı sağlar ve uygun veritabanı şemasını, sorun sorgularını vb. kullanarak sorunsuz bir şekilde oluşturabilirsiniz. Bir tür hiyerarşisinin nasıl eşlendiğine ilişkin gerçek Ayrıntılar sağlayıcıya bağımlıdır; Bu sayfa, bir ilişkisel veritabanı bağlamında devralma desteğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="4626c-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="4626c-106">Şu anda EF Core yalnızca hiyerarşi başına tablo (TPH) modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="4626c-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="4626c-107">TPH, hiyerarşideki tüm türlerin verilerini depolamak için tek bir tablo kullanır ve her bir satırın hangi türü temsil ettiğini belirlemek için bir Ayrıştırıcı sütunu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4626c-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="4626c-108">EF6 tarafından desteklenen tablo/tür (TPT) ve tablo-somut türü (TPC) EF Core tarafından henüz desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="4626c-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="4626c-109">TPT EF Core 5,0 için planlanmış bir ana özelliktir.</span><span class="sxs-lookup"><span data-stu-id="4626c-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="4626c-110">Varlık türü hiyerarşisi eşleme</span><span class="sxs-lookup"><span data-stu-id="4626c-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="4626c-111">Kurala göre, yalnızca modele iki veya daha fazla devralınmış tür açık olarak dahil ediliyorsa, EF yalnızca devralmayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4626c-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="4626c-112">EF, modele dahil olmayan temel veya türetilmiş türler için otomatik olarak taramayacaktır.</span><span class="sxs-lookup"><span data-stu-id="4626c-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="4626c-113">Devralma hiyerarşisindeki her bir tür için bir DbSet sunarak modele türler ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4626c-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="4626c-114">Bu model aşağıdaki veritabanı şemasına eşlendi (her satırda hangi *Blog* türünün depolandığını belirleyen örtük olarak oluşturulmuş *ayrıştırıcı* sütununu aklınızda bulunur):</span><span class="sxs-lookup"><span data-stu-id="4626c-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="4626c-116">Veritabanı sütunları, TPH eşlemesi kullanılırken gerektiğinde otomatik olarak null yapılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="4626c-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="4626c-117">Örneğin, normal *Blog* örnekleri bu özelliğe sahip olmadığından *rssurl* sütunu null yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="4626c-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="4626c-118">Hiyerarşide bir veya daha fazla varlık için bir DbSet göstermek istemiyorsanız, bu API 'lerin modele dahil olduklarından emin olmak için akıcı API 'yi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4626c-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="4626c-119">Kurallara dayanmıyorsanız, `HasBaseType`kullanarak temel türü açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4626c-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="4626c-120">Bir varlık türünü hiyerarşiden kaldırmak için `.HasBaseType((Type)null)` de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4626c-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="4626c-121">Ayrıştırıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4626c-121">Discriminator configuration</span></span>

<span data-ttu-id="4626c-122">Ayrıştırıcı sütununun adını ve türünü ve hiyerarşideki her türü tanımlamak için kullanılan değerleri yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4626c-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="4626c-123">Yukarıdaki örneklerde EF, Ayrıştırıcıyı hiyerarşinin temel varlığına örtük bir şekilde bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak eklemiştir.</span><span class="sxs-lookup"><span data-stu-id="4626c-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="4626c-124">Bu özellik, herhangi bir şekilde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="4626c-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="4626c-125">Son olarak, ayrıştırıcı, varlığınızda düzenli bir .NET özelliğine de eşleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="4626c-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="4626c-126">Paylaşılan sütunlar</span><span class="sxs-lookup"><span data-stu-id="4626c-126">Shared columns</span></span>

<span data-ttu-id="4626c-127">Varsayılan olarak, hiyerarşideki iki eşdüzey varlık türünün aynı ada sahip bir özelliği varsa, bunlar iki ayrı sütuna eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4626c-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="4626c-128">Ancak, türleri aynıysa aynı veritabanı sütunuyla eşleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="4626c-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
