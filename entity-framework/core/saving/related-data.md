---
title: "İlgili verileri - EF Çekirdek kaydetme"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: 078879163002cb66e0f0f439415789963181ec15
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="saving-related-data"></a><span data-ttu-id="13945-102">İlgili verileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="13945-102">Saving Related Data</span></span>

<span data-ttu-id="13945-103">Yalıtılmış varlıklar yanı sıra, aynı zamanda yapabileceğiniz modelinizde tanımlı ilişkilerin kullanın.</span><span class="sxs-lookup"><span data-stu-id="13945-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="13945-104">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) github'da.</span><span class="sxs-lookup"><span data-stu-id="13945-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="13945-105">Bir grafik yeni varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="13945-105">Adding a graph of new entities</span></span>

<span data-ttu-id="13945-106">Birkaç yeni ilgili varlıklar oluşturursanız, bunlardan birini bağlamına eklemek başkalarına çok eklenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="13945-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="13945-107">Aşağıdaki örnekte, blog ve üç ilgili gönderileri tüm veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="13945-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="13945-108">Gönderileri bulundu ve eklenen aracılığıyla erişilebilir olduklarından `Blog.Posts` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="13945-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a><span data-ttu-id="13945-109">İlgili varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="13945-109">Adding a related entity</span></span>

<span data-ttu-id="13945-110">Yeni bir varlık zaten bağlam tarafından izlenen bir varlığın Gezinti özelliğinden başvurursanız, varlık bulunan ve veritabanına eklenen.</span><span class="sxs-lookup"><span data-stu-id="13945-110">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="13945-111">Aşağıdaki örnekte, `post` varlık için eklendiğinden eklenir `Posts` özelliği `blog` veritabanından getirildi varlığı.</span><span class="sxs-lookup"><span data-stu-id="13945-111">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="13945-112">İlişkileri değiştirme</span><span class="sxs-lookup"><span data-stu-id="13945-112">Changing relationships</span></span>

<span data-ttu-id="13945-113">Bir varlığın gezinti özelliği değiştirirseniz, karşılık gelen değişiklikleri için veritabanındaki yabancı anahtar sütunu yapılır.</span><span class="sxs-lookup"><span data-stu-id="13945-113">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="13945-114">Aşağıdaki örnekte, `post` varlık güncelleştirileceğini yeni ait `blog` varlık için kendi `Blog` gezinti özelliği, işaret edecek şekilde ayarlanır `blog`.</span><span class="sxs-lookup"><span data-stu-id="13945-114">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="13945-115">Unutmayın `blog` bağlamdan izlenmekte bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan veritabanına da eklenir (`post`).</span><span class="sxs-lookup"><span data-stu-id="13945-115">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="13945-116">İlişkileri kaldırma</span><span class="sxs-lookup"><span data-stu-id="13945-116">Removing relationships</span></span>

<span data-ttu-id="13945-117">Bir başvuru Gezinti ayarlayarak bir ilişkiyi kaldırmak `null`, veya koleksiyon Gezinti bölmesinden ilgili varlık kaldırma.</span><span class="sxs-lookup"><span data-stu-id="13945-117">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="13945-118">Bir ilişki kaldırma bağımlı varlıkta yan etkisi, yapılandırılan ilişki davranışına göre art arda silme.</span><span class="sxs-lookup"><span data-stu-id="13945-118">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="13945-119">Varsayılan olarak, gerekli ilişkiler için bir cascade delete davranış yapılandırılır ve alt/bağımlı varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="13945-119">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="13945-120">İsteğe bağlı ilişkiler için art arda silme varsayılan olarak yapılandırılmamış ancak yabancı anahtar özelliği ayarlanacak null.</span><span class="sxs-lookup"><span data-stu-id="13945-120">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="13945-121">Bkz: [gerekli ve isteğe bağlı ilişkileri](../modeling/relationships.md#required-and-optional-relationships) nasıl ilişkileri requiredness yapılandırılabilir hakkında bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="13945-121">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="13945-122">Bkz: [art arda silme](cascade-delete.md) nasıl art arda silme davranışları hakkında daha fazla ayrıntı çalışmak için nasıl bunlar açıkça yapılandırılabilir ve kurala göre nasıl seçileceğini.</span><span class="sxs-lookup"><span data-stu-id="13945-122">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="13945-123">Aşağıdaki örnekte, art arda silme arasındaki ilişkiyi yapılandırılan `Blog` ve `Post`, böylece `post` varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="13945-123">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
