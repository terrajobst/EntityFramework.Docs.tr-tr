---
title: Devralma (Ilişkisel veritabanı)-EF Core
description: Entity Framework Core kullanarak ilişkisel bir veritabanında varlık türü devralmayı yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824750"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="d1bb4-103">Devralma (İlişkisel Veritabanı)</span><span class="sxs-lookup"><span data-stu-id="d1bb4-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="d1bb4-104">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d1bb4-105">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d1bb4-106">EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="d1bb4-107">Şu anda EF Core ' de yalnızca hiyerarşi başına tablo (TPH) düzeni uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="d1bb4-108">Tablo/tür (TPT) ve tablo başına somut-tür (TPC) gibi diğer yaygın desenler henüz kullanılamamaktadır.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="d1bb4-109">Kurallar</span><span class="sxs-lookup"><span data-stu-id="d1bb4-109">Conventions</span></span>

<span data-ttu-id="d1bb4-110">Devralma varsayılan olarak, hiyerarşi başına tablo (TPH) düzeni kullanılarak eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="d1bb4-111">TPH, hiyerarşideki tüm türlerin verilerini depolamak için tek bir tablo kullanır.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="d1bb4-112">Bir Ayrıştırıcı sütunu, her bir satırın hangi tür temsil ettiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="d1bb4-113">EF Core, yalnızca modele açıkça iki veya daha fazla devralınmış tür varsa devralma ayarı yapılır (daha fazla ayrıntı için bkz. [Devralma](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="d1bb4-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="d1bb4-114">Aşağıda, bir basit devralma senaryosunu ve TPH deseninin kullanıldığı bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="d1bb4-115">*Ayrıştırıcı* sütunu, her satırda hangi *Blog* türünün depolandığını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![görüntü](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="d1bb4-117">Veritabanı sütunları, TPH eşlemesi kullanılırken gerektiğinde otomatik olarak null yapılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d1bb4-118">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d1bb4-118">Data Annotations</span></span>

<span data-ttu-id="d1bb4-119">Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d1bb4-120">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d1bb4-120">Fluent API</span></span>

<span data-ttu-id="d1bb4-121">Ayrıştırıcı sütununun adını ve türünü ve hiyerarşideki her türü tanımlamak için kullanılan değerleri yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="d1bb4-122">Ayrıştırıcı özelliğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d1bb4-122">Configuring the discriminator property</span></span>

<span data-ttu-id="d1bb4-123">Yukarıdaki örneklerde, ayrıştırıcı hiyerarşinin temel varlığında bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="d1bb4-124">Modeldeki bir özellik olduğu için, diğer özellikler gibi yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="d1bb4-125">Örneğin, varsayılan, kural olarak ayrıştırıcı kullanılıyorsa en fazla uzunluğu ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="d1bb4-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="d1bb4-126">Ayrıştırıcı, varlığınızda bir .NET özelliğine de eşleştirilebilir ve bunu yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="d1bb4-127">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d1bb4-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="d1bb4-128">Paylaşılan sütunlar</span><span class="sxs-lookup"><span data-stu-id="d1bb4-128">Shared columns</span></span>

<span data-ttu-id="d1bb4-129">İki eşdüzey varlık türünün aynı ada sahip bir özelliği olduğunda, varsayılan olarak iki ayrı sütuna eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d1bb4-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="d1bb4-130">Ancak uyumluysa, aynı sütunla eşleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="d1bb4-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]