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
# <a name="saving-related-data"></a>İlgili Verileri Kaydetme

Yalıtılmış varlıklara ek olarak, modelinizde tanımlanan ilişkilerden de yararlanabilirsiniz.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) GitHub'da görüntüleyebilirsiniz.

## <a name="adding-a-graph-of-new-entities"></a>Yeni varlıkların grafiği ekleme

Birkaç yeni ilgili varlık oluşturursanız, bunlardan birini bağlama eklemek diğerlerinin de eklenmesine neden olur.

Aşağıdaki örnekte, blog ve ilgili üç gönderi veritabanına eklenir. Gönderiler bulunur ve eklenir, çünkü `Blog.Posts` gezinti özelliği üzerinden erişilebilir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> EntityEntry.State özelliğini kullanarak tek bir varlığın durumunu ayarlayın. Örneğin, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>İlgili bir varlık ekleme

Zaten bağlam tarafından izlenen bir varlığın gezinti özelliğinden yeni bir varlık başvurursanız, varlık keşfedilir ve veritabanına eklenir.

Aşağıdaki örnekte, `post` veritabanından getirilen `Posts` `blog` varlığın özelliğine eklendiği için varlık eklenir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>İlişkileri değiştirme

Bir varlığın gezinti özelliğini değiştirirseniz, karşılık gelen değişiklikler veritabanındaki yabancı anahtar sütununa yapılır.

Aşağıdaki örnekte, `post` `Blog` gezinti özelliği `blog`' ye `blog` işaret etmek üzere ayarlandığından, varlık yeni varlığa ait olacak şekilde güncelleştirilir `blog` Zaten bağlam tarafından izlenen bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan veritabanına da eklenecektir`post`unutmayın ( ).

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>İlişkileri kaldırma

Bir başvuru navigasyonuna bir başvuru `null`gezintisi ayarlayarak veya ilgili varlığı bir koleksiyon gezintisinden kaldırarak ilişkiyi kaldırabilirsiniz.

İlişkide yapılandırılan basamaklı silme davranışına göre, bir ilişkinin kaldırılması bağımlı varlık üzerinde yan etkilere sahip olabilir.

Varsayılan olarak, gerekli ilişkiler için basamaklı silme davranışı yapılandırılır ve alt/bağımlı varlık veritabanından silinir. İsteğe bağlı ilişkiler için basamaklı silme varsayılan olarak yapılandırılamaz, ancak yabancı anahtar özelliği null olarak ayarlanır.

İlişkilerin gerekliliğinin nasıl yapılandırılabildiğini öğrenmek için [Gerekli ve İsteğe Bağlı İlişkiler'e](../modeling/relationships.md#required-and-optional-relationships) bakın.

Basamaklı silme davranışlarının nasıl çalıştığı, bunların nasıl açıkça yapılandırılabildiği ve kuralkuralı tarafından nasıl seçildikleri hakkında daha fazla bilgi için [Basamaklı Silme'ye](cascade-delete.md) bakın.

Aşağıdaki örnekte, bir basamaklı silme arasındaki `Blog` ilişki `Post`üzerinde yapılandırılır ve , böylece `post` varlık veritabanından silinir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
