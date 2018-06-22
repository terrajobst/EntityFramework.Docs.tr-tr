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
# <a name="saving-related-data"></a>İlgili verileri kaydetme

Yalıtılmış varlıklar yanı sıra, aynı zamanda yapabileceğiniz modelinizde tanımlı ilişkilerin kullanın.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) github'da.

## <a name="adding-a-graph-of-new-entities"></a>Bir grafik yeni varlık ekleme

Birkaç yeni ilgili varlıklar oluşturursanız, bunlardan birini bağlamına eklemek başkalarına çok eklenmesine neden olur.

Aşağıdaki örnekte, blog ve üç ilgili gönderileri tüm veritabanına eklenir. Gönderileri bulundu ve eklenen aracılığıyla erişilebilir olduklarından `Blog.Posts` gezinti özelliği.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Yalnızca tek bir varlık durumunu ayarlamak için EntityEntry.State özelliğini kullanın. Örneğin, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>İlgili varlık ekleme

Yeni bir varlık zaten bağlam tarafından izlenen bir varlığın Gezinti özelliğinden başvurursanız, varlık bulunan ve veritabanına eklenen.

Aşağıdaki örnekte, `post` varlık için eklendiğinden eklenir `Posts` özelliği `blog` veritabanından getirildi varlığı.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>İlişkileri değiştirme

Bir varlığın gezinti özelliği değiştirirseniz, karşılık gelen değişiklikleri için veritabanındaki yabancı anahtar sütunu yapılır.

Aşağıdaki örnekte, `post` varlık güncelleştirileceğini yeni ait `blog` varlık için kendi `Blog` gezinti özelliği, işaret edecek şekilde ayarlanır `blog`. Unutmayın `blog` bağlamdan izlenmekte bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan veritabanına da eklenir (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>İlişkileri kaldırma

Bir başvuru Gezinti ayarlayarak bir ilişkiyi kaldırmak `null`, veya koleksiyon Gezinti bölmesinden ilgili varlık kaldırma.

Bir ilişki kaldırma bağımlı varlıkta yan etkisi, yapılandırılan ilişki davranışına göre art arda silme.

Varsayılan olarak, gerekli ilişkiler için bir cascade delete davranış yapılandırılır ve alt/bağımlı varlık veritabanından silinir. İsteğe bağlı ilişkiler için art arda silme varsayılan olarak yapılandırılmamış ancak yabancı anahtar özelliği ayarlanacak null.

Bkz: [gerekli ve isteğe bağlı ilişkileri](../modeling/relationships.md#required-and-optional-relationships) nasıl ilişkileri requiredness yapılandırılabilir hakkında bilgi edinmek için.

Bkz: [art arda silme](cascade-delete.md) nasıl art arda silme davranışları hakkında daha fazla ayrıntı çalışmak için nasıl bunlar açıkça yapılandırılabilir ve kurala göre nasıl seçileceğini.

Aşağıdaki örnekte, art arda silme arasındaki ilişkiyi yapılandırılan `Blog` ve `Post`, böylece `post` varlık veritabanından silinir.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
