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
# <a name="saving-related-data"></a>Ilgili verileri kaydetme

Yalıtılmış varlıklara ek olarak, modelinizde tanımlanan ilişkileri de kullanabilirsiniz.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) GitHub ' da görebilirsiniz.

## <a name="adding-a-graph-of-new-entities"></a>Yeni varlıkların grafiğini ekleme

Birden çok yeni varlık oluşturursanız, bunlardan birini bağlamına eklemek başkalarının da eklenmesine neden olur.

Aşağıdaki örnekte, blogun ve ilgili üç gönderi veritabanına eklenir. Gönderimler, `Blog.Posts` gezinti özelliği aracılığıyla erişilebilir olduklarından bulunur ve eklenir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Yalnızca tek bir varlığın durumunu ayarlamak için EntityEntry. State özelliğini kullanın. Örneğin, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>İlgili varlık ekleme

Zaten bağlam tarafından izlenen bir varlığın gezinti özelliğinden yeni bir varlığa başvurdıysanız, varlık keşfedilir ve veritabanına eklenir.

Aşağıdaki örnekte, `post` varlığı, veritabanından getirilen `blog` varlığının `Posts` özelliğine eklendiğinden eklenir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>İlişkileri değiştirme

Bir varlığın gezinti özelliğini değiştirirseniz, ilgili değişiklikler veritabanındaki yabancı anahtar sütununa yapılır.

Aşağıdaki örnekte, `post` varlık yeni `blog` varlığına ait olacak şekilde güncelleştirilir çünkü `Blog` gezinti özelliği `blog`işaret etmek üzere ayarlanmıştır. `blog`, zaten bağlam tarafından izlenen bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan (`post`) veritabanına da ekleneceğini unutmayın.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>İlişkiler kaldırılıyor

`null`bir başvuru gezintisi ayarlayarak veya bir koleksiyon gezintisinde ilgili varlığı kaldırarak ilişkiyi kaldırabilirsiniz.

İlişkinin kaldırılması, ilişkide yapılandırılan basamaklı silme davranışına göre bağımlı varlık üzerinde yan etkilere sahip olabilir.

Varsayılan olarak, gerekli ilişkiler için, bir basamaklı silme davranışı yapılandırılır ve alt/bağımlı varlık veritabanından silinir. İsteğe bağlı ilişkiler için, Cascade Delete varsayılan olarak yapılandırılmaz, ancak yabancı anahtar özelliği null olarak ayarlanır.

İlişkilerin gerekli olduğu şekilde nasıl yapılandırılabileceğini öğrenmek için bkz. [gerekli ve Isteğe bağlı ilişkiler](../modeling/relationships.md#required-and-optional-relationships) .

Basamaklı silme davranışlarının nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Cascade Delete](cascade-delete.md) , nasıl açıkça yapılandırılabilecekleri ve kural tarafından nasıl seçildiği hakkında daha fazla bilgi için.

Aşağıdaki örnekte, `Blog` ve `Post`arasındaki ilişkide bir basamaklı silme yapılandırılır, bu nedenle `post` varlığı veritabanından silinir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
