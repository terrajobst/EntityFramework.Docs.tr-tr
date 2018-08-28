---
title: İlgili verileri - EF Core kaydetme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 7349c57c0dccd3c911178641d3b34a478a4f6194
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994750"
---
# <a name="saving-related-data"></a>İlgili verileri kaydetme

Yalıtılmış varlıklara ek olarak, aynı zamanda yapabilirsiniz modelinizde tanımlı ilişkiler kullanın.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) GitHub üzerinde.

## <a name="adding-a-graph-of-new-entities"></a>Yeni varlıklar bir grafik ekleme

Birkaç yeni ilgili varlıkları oluşturun, bunlardan biri için içerik ekleme çok eklenecek diğer neden olur.

Aşağıdaki örnekte, blog ve üç ilgili postaların tümü veritabanına eklenir. Postalar bulundu ve eklenen aracılığıyla erişilebilir olduklarından `Blog.Posts` gezinme özelliği.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Yalnızca tek bir varlık durumunu ayarlamak için EntityEntry.State özelliğini kullanın. Örneğin, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>İlgili varlık ekleme

Yeni bir varlık zaten bağlam tarafından izlenen bir varlığın Gezinti özelliğinin başvuru, varlık bulunan ve veritabanına eklenir.

Aşağıdaki örnekte, `post` varlığı için eklendiğinden eklendiğinde `Posts` özelliği `blog` veritabanından getirildi varlığı.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>İlişkilerini değiştirme

Bir varlığın gezinti özelliği değiştirirseniz, karşılık gelen değişiklikleri veritabanında yabancı anahtar sütunu yapılmaz.

Aşağıdaki örnekte, `post` varlık yeni ait güncelleştirilir `blog` varlık olduğundan, `Blog` gezinti özelliği, işaret edecek şekilde ayarlanır `blog`. Unutmayın `blog` bağlam tarafından zaten izlenen bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan veritabanı'na da eklenir (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>İlişkiler

Bir başvuru Gezinti ayarlayarak bir ilişkiyi kaldırmak `null`, veya ilgili varlık koleksiyon Gezinti bölmesinden kaldırma.

Bir ilişki kaldırma bağımlı varlıkta yan etkileri olduğu, ilişkide yapılandırılan davranışına göre cascade silin.

Varsayılan olarak gerekli ilişkiler için art arda silme davranışını yapılandırılır ve alt/bağımlı varlık veritabanından silinecek. İsteğe bağlı ilişkiler için art arda silme, varsayılan olarak yapılandırılmamış ancak yabancı anahtar özelliği null.

Bkz: [gerekli ve isteğe bağlı ilişkileri](../modeling/relationships.md#required-and-optional-relationships) nasıl ilişki requiredness yapılandırılabilir hakkında bilgi edinmek için.

Bkz: [art arda silme](cascade-delete.md) için nasıl art arda silme davranışları hakkında daha fazla ayrıntı çalışma, nasıl bunlar açıkça yapılandırılabilir ve nasıl kurala göre seçilir.

Aşağıdaki örnekte, art arda silme arasındaki ilişkiyi yapılandırılmış `Blog` ve `Post`, bu nedenle `post` varlık veritabanından silinir.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
