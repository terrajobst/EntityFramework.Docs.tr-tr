---
title: İlgili verileri - EF Çekirdek kaydetme
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31006656"
---
# <a name="saving-related-data"></a><span data-ttu-id="9054e-102">İlgili verileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="9054e-102">Saving Related Data</span></span>

<span data-ttu-id="9054e-103">Yalıtılmış varlıklar yanı sıra, aynı zamanda yapabileceğiniz modelinizde tanımlı ilişkilerin kullanın.</span><span class="sxs-lookup"><span data-stu-id="9054e-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="9054e-104">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) github'da.</span><span class="sxs-lookup"><span data-stu-id="9054e-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="9054e-105">Bir grafik yeni varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="9054e-105">Adding a graph of new entities</span></span>

<span data-ttu-id="9054e-106">Birkaç yeni ilgili varlıklar oluşturursanız, bunlardan birini bağlamına eklemek başkalarına çok eklenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="9054e-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="9054e-107">Aşağıdaki örnekte, blog ve üç ilgili gönderileri tüm veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="9054e-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="9054e-108">Gönderileri bulundu ve eklenen aracılığıyla erişilebilir olduklarından `Blog.Posts` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="9054e-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="9054e-109">Yalnızca tek bir varlık durumunu ayarlamak için EntityEntry.State özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="9054e-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="9054e-110">Örneğin, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="9054e-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="9054e-111">İlgili varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="9054e-111">Adding a related entity</span></span>

<span data-ttu-id="9054e-112">Yeni bir varlık zaten bağlam tarafından izlenen bir varlığın Gezinti özelliğinden başvurursanız, varlık bulunan ve veritabanına eklenen.</span><span class="sxs-lookup"><span data-stu-id="9054e-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="9054e-113">Aşağıdaki örnekte, `post` varlık için eklendiğinden eklenir `Posts` özelliği `blog` veritabanından getirildi varlığı.</span><span class="sxs-lookup"><span data-stu-id="9054e-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="9054e-114">İlişkileri değiştirme</span><span class="sxs-lookup"><span data-stu-id="9054e-114">Changing relationships</span></span>

<span data-ttu-id="9054e-115">Bir varlığın gezinti özelliği değiştirirseniz, karşılık gelen değişiklikleri için veritabanındaki yabancı anahtar sütunu yapılır.</span><span class="sxs-lookup"><span data-stu-id="9054e-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="9054e-116">Aşağıdaki örnekte, `post` varlık güncelleştirileceğini yeni ait `blog` varlık için kendi `Blog` gezinti özelliği, işaret edecek şekilde ayarlanır `blog`.</span><span class="sxs-lookup"><span data-stu-id="9054e-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="9054e-117">Unutmayın `blog` bağlamdan izlenmekte bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan veritabanına da eklenir (`post`).</span><span class="sxs-lookup"><span data-stu-id="9054e-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="9054e-118">İlişkileri kaldırma</span><span class="sxs-lookup"><span data-stu-id="9054e-118">Removing relationships</span></span>

<span data-ttu-id="9054e-119">Bir başvuru Gezinti ayarlayarak bir ilişkiyi kaldırmak `null`, veya koleksiyon Gezinti bölmesinden ilgili varlık kaldırma.</span><span class="sxs-lookup"><span data-stu-id="9054e-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="9054e-120">Bir ilişki kaldırma bağımlı varlıkta yan etkisi, yapılandırılan ilişki davranışına göre art arda silme.</span><span class="sxs-lookup"><span data-stu-id="9054e-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="9054e-121">Varsayılan olarak, gerekli ilişkiler için bir cascade delete davranış yapılandırılır ve alt/bağımlı varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="9054e-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="9054e-122">İsteğe bağlı ilişkiler için art arda silme varsayılan olarak yapılandırılmamış ancak yabancı anahtar özelliği ayarlanacak null.</span><span class="sxs-lookup"><span data-stu-id="9054e-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="9054e-123">Bkz: [gerekli ve isteğe bağlı ilişkileri](../modeling/relationships.md#required-and-optional-relationships) nasıl ilişkileri requiredness yapılandırılabilir hakkında bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="9054e-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="9054e-124">Bkz: [art arda silme](cascade-delete.md) nasıl art arda silme davranışları hakkında daha fazla ayrıntı çalışmak için nasıl bunlar açıkça yapılandırılabilir ve kurala göre nasıl seçileceğini.</span><span class="sxs-lookup"><span data-stu-id="9054e-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="9054e-125">Aşağıdaki örnekte, art arda silme arasındaki ilişkiyi yapılandırılan `Blog` ve `Post`, böylece `post` varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="9054e-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
