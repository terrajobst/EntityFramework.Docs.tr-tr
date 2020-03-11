---
title: Ilgili verileri kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417547"
---
# <a name="saving-related-data"></a><span data-ttu-id="520bf-102">Ilgili verileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="520bf-102">Saving Related Data</span></span>

<span data-ttu-id="520bf-103">Yalıtılmış varlıklara ek olarak, modelinizde tanımlanan ilişkileri de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="520bf-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="520bf-104">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="520bf-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="520bf-105">Yeni varlıkların grafiğini ekleme</span><span class="sxs-lookup"><span data-stu-id="520bf-105">Adding a graph of new entities</span></span>

<span data-ttu-id="520bf-106">Birden çok yeni varlık oluşturursanız, bunlardan birini bağlamına eklemek başkalarının da eklenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="520bf-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="520bf-107">Aşağıdaki örnekte, blogun ve ilgili üç gönderi veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="520bf-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="520bf-108">Gönderimler, `Blog.Posts` gezinti özelliği aracılığıyla erişilebilir olduklarından bulunur ve eklenir.</span><span class="sxs-lookup"><span data-stu-id="520bf-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="520bf-109">Yalnızca tek bir varlığın durumunu ayarlamak için EntityEntry. State özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="520bf-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="520bf-110">Örneğin, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="520bf-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="520bf-111">İlgili varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="520bf-111">Adding a related entity</span></span>

<span data-ttu-id="520bf-112">Zaten bağlam tarafından izlenen bir varlığın gezinti özelliğinden yeni bir varlığa başvurdıysanız, varlık keşfedilir ve veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="520bf-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="520bf-113">Aşağıdaki örnekte, `post` varlığı, veritabanından getirilen `blog` varlığının `Posts` özelliğine eklendiğinden eklenir.</span><span class="sxs-lookup"><span data-stu-id="520bf-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="520bf-114">İlişkileri değiştirme</span><span class="sxs-lookup"><span data-stu-id="520bf-114">Changing relationships</span></span>

<span data-ttu-id="520bf-115">Bir varlığın gezinti özelliğini değiştirirseniz, ilgili değişiklikler veritabanındaki yabancı anahtar sütununa yapılır.</span><span class="sxs-lookup"><span data-stu-id="520bf-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="520bf-116">Aşağıdaki örnekte, `post` varlık yeni `blog` varlığına ait olacak şekilde güncelleştirilir çünkü `Blog` gezinti özelliği `blog`işaret etmek üzere ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="520bf-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="520bf-117">`blog`, zaten bağlam tarafından izlenen bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan (`post`) veritabanına da ekleneceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="520bf-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="520bf-118">İlişkiler kaldırılıyor</span><span class="sxs-lookup"><span data-stu-id="520bf-118">Removing relationships</span></span>

<span data-ttu-id="520bf-119">`null`bir başvuru gezintisi ayarlayarak veya bir koleksiyon gezintisinde ilgili varlığı kaldırarak ilişkiyi kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="520bf-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="520bf-120">İlişkinin kaldırılması, ilişkide yapılandırılan basamaklı silme davranışına göre bağımlı varlık üzerinde yan etkilere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="520bf-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="520bf-121">Varsayılan olarak, gerekli ilişkiler için, bir basamaklı silme davranışı yapılandırılır ve alt/bağımlı varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="520bf-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="520bf-122">İsteğe bağlı ilişkiler için, Cascade Delete varsayılan olarak yapılandırılmaz, ancak yabancı anahtar özelliği null olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="520bf-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="520bf-123">İlişkilerin gerekli olduğu şekilde nasıl yapılandırılabileceğini öğrenmek için bkz. [gerekli ve Isteğe bağlı ilişkiler](../modeling/relationships.md#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="520bf-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="520bf-124">Basamaklı silme davranışlarının nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Cascade Delete](cascade-delete.md) , nasıl açıkça yapılandırılabilecekleri ve kural tarafından nasıl seçildiği hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="520bf-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="520bf-125">Aşağıdaki örnekte, `Blog` ve `Post`arasındaki ilişkide bir basamaklı silme yapılandırılır, bu nedenle `post` varlığı veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="520bf-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
