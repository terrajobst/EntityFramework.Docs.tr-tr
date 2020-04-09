---
title: İlgili Verileri Kaydetme - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417547"
---
# <a name="saving-related-data"></a><span data-ttu-id="3389a-102">İlgili Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="3389a-102">Saving Related Data</span></span>

<span data-ttu-id="3389a-103">Yalıtılmış varlıklara ek olarak, modelinizde tanımlanan ilişkilerden de yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3389a-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="3389a-104">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3389a-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="3389a-105">Yeni varlıkların grafiği ekleme</span><span class="sxs-lookup"><span data-stu-id="3389a-105">Adding a graph of new entities</span></span>

<span data-ttu-id="3389a-106">Birkaç yeni ilgili varlık oluşturursanız, bunlardan birini bağlama eklemek diğerlerinin de eklenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="3389a-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="3389a-107">Aşağıdaki örnekte, blog ve ilgili üç gönderi veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="3389a-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="3389a-108">Gönderiler bulunur ve eklenir, çünkü `Blog.Posts` gezinti özelliği üzerinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3389a-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="3389a-109">EntityEntry.State özelliğini kullanarak tek bir varlığın durumunu ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3389a-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="3389a-110">Örneğin, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="3389a-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="3389a-111">İlgili bir varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="3389a-111">Adding a related entity</span></span>

<span data-ttu-id="3389a-112">Zaten bağlam tarafından izlenen bir varlığın gezinti özelliğinden yeni bir varlık başvurursanız, varlık keşfedilir ve veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="3389a-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="3389a-113">Aşağıdaki örnekte, `post` veritabanından getirilen `Posts` `blog` varlığın özelliğine eklendiği için varlık eklenir.</span><span class="sxs-lookup"><span data-stu-id="3389a-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="3389a-114">İlişkileri değiştirme</span><span class="sxs-lookup"><span data-stu-id="3389a-114">Changing relationships</span></span>

<span data-ttu-id="3389a-115">Bir varlığın gezinti özelliğini değiştirirseniz, karşılık gelen değişiklikler veritabanındaki yabancı anahtar sütununa yapılır.</span><span class="sxs-lookup"><span data-stu-id="3389a-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="3389a-116">Aşağıdaki örnekte, `post` `Blog` gezinti özelliği `blog`' ye `blog` işaret etmek üzere ayarlandığından, varlık yeni varlığa ait olacak şekilde güncelleştirilir</span><span class="sxs-lookup"><span data-stu-id="3389a-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="3389a-117">`blog` Zaten bağlam tarafından izlenen bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan veritabanına da eklenecektir`post`unutmayın ( ).</span><span class="sxs-lookup"><span data-stu-id="3389a-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="3389a-118">İlişkileri kaldırma</span><span class="sxs-lookup"><span data-stu-id="3389a-118">Removing relationships</span></span>

<span data-ttu-id="3389a-119">Bir başvuru navigasyonuna bir başvuru `null`gezintisi ayarlayarak veya ilgili varlığı bir koleksiyon gezintisinden kaldırarak ilişkiyi kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3389a-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="3389a-120">İlişkide yapılandırılan basamaklı silme davranışına göre, bir ilişkinin kaldırılması bağımlı varlık üzerinde yan etkilere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="3389a-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="3389a-121">Varsayılan olarak, gerekli ilişkiler için basamaklı silme davranışı yapılandırılır ve alt/bağımlı varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="3389a-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="3389a-122">İsteğe bağlı ilişkiler için basamaklı silme varsayılan olarak yapılandırılamaz, ancak yabancı anahtar özelliği null olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="3389a-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="3389a-123">İlişkilerin gerekliliğinin nasıl yapılandırılabildiğini öğrenmek için [Gerekli ve İsteğe Bağlı İlişkiler'e](../modeling/relationships.md#required-and-optional-relationships) bakın.</span><span class="sxs-lookup"><span data-stu-id="3389a-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="3389a-124">Basamaklı silme davranışlarının nasıl çalıştığı, bunların nasıl açıkça yapılandırılabildiği ve kuralkuralı tarafından nasıl seçildikleri hakkında daha fazla bilgi için [Basamaklı Silme'ye](cascade-delete.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="3389a-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="3389a-125">Aşağıdaki örnekte, bir basamaklı silme arasındaki `Blog` ilişki `Post`üzerinde yapılandırılır ve , böylece `post` varlık veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="3389a-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
