---
title: Ilgili verileri kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 45c7b8e4bfa4ce7967ad76ef4a7d4818b0d3aebf
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197885"
---
# <a name="saving-related-data"></a><span data-ttu-id="6eaac-102">Ilgili verileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="6eaac-102">Saving Related Data</span></span>

<span data-ttu-id="6eaac-103">Yalıtılmış varlıklara ek olarak, modelinizde tanımlanan ilişkileri de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6eaac-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="6eaac-104">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="6eaac-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="6eaac-105">Yeni varlıkların grafiğini ekleme</span><span class="sxs-lookup"><span data-stu-id="6eaac-105">Adding a graph of new entities</span></span>

<span data-ttu-id="6eaac-106">Birden çok yeni varlık oluşturursanız, bunlardan birini bağlamına eklemek başkalarının da eklenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="6eaac-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="6eaac-107">Aşağıdaki örnekte, blogun ve ilgili üç gönderi veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="6eaac-108">Gönderimler, `Blog.Posts` gezinti özelliği aracılığıyla erişilebilir olduklarından bulunur ve eklenir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="6eaac-109">Yalnızca tek bir varlığın durumunu ayarlamak için EntityEntry. State özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6eaac-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="6eaac-110">Örneğin, uygulamasında yönetilen Hyper-V konakları olarak eklemek için aşağıdaki yordamı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6eaac-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="6eaac-111">İlgili varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="6eaac-111">Adding a related entity</span></span>

<span data-ttu-id="6eaac-112">Zaten bağlam tarafından izlenen bir varlığın gezinti özelliğinden yeni bir varlığa başvurdıysanız, varlık keşfedilir ve veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="6eaac-113">Aşağıdaki örnekte, `post` varlık, veritabanından getirilen `blog` varlığın `Posts` özelliğine eklendiği için eklenir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="6eaac-114">İlişkileri değiştirme</span><span class="sxs-lookup"><span data-stu-id="6eaac-114">Changing relationships</span></span>

<span data-ttu-id="6eaac-115">Bir varlığın gezinti özelliğini değiştirirseniz, ilgili değişiklikler veritabanındaki yabancı anahtar sütununa yapılır.</span><span class="sxs-lookup"><span data-stu-id="6eaac-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="6eaac-116">Aşağıdaki örnekte, `post` , `Blog` gezinti özelliği to olarak `blog`ayarlandığı için varlık yeni `blog` varlığa ait olacak şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="6eaac-117">Ayrıca, bağlam (`post`) tarafından zaten izlenmekte olan bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan, veritabanına da ekleneceğini unutmayın. `blog`</span><span class="sxs-lookup"><span data-stu-id="6eaac-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="6eaac-118">İlişkiler kaldırılıyor</span><span class="sxs-lookup"><span data-stu-id="6eaac-118">Removing relationships</span></span>

<span data-ttu-id="6eaac-119">Bir başvuru gezintisi `null`ayarlayarak veya bir koleksiyon gezinmede ilgili varlığı kaldırarak ilişkiyi kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6eaac-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="6eaac-120">İlişkinin kaldırılması, ilişkide yapılandırılan basamaklı silme davranışına göre bağımlı varlık üzerinde yan etkilere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="6eaac-121">Varsayılan olarak, gerekli ilişkiler için, bir basamaklı silme davranışı yapılandırılır ve alt/bağımlı varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="6eaac-122">İsteğe bağlı ilişkiler için, Cascade Delete varsayılan olarak yapılandırılmaz, ancak yabancı anahtar özelliği null olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="6eaac-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="6eaac-123">İlişkilerin gerekli olduğu şekilde nasıl yapılandırılabileceğini öğrenmek için bkz. [gerekli ve Isteğe bağlı ilişkiler](../modeling/relationships.md#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="6eaac-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="6eaac-124">Basamaklı silme davranışlarının nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Cascade Delete](cascade-delete.md) , nasıl açıkça yapılandırılabilecekleri ve kural tarafından nasıl seçildiği hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="6eaac-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="6eaac-125">Aşağıdaki örnekte, ve `Blog` `Post`arasındaki ilişkide bir basamaklı silme yapılandırılır, bu nedenle `post` varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="6eaac-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
