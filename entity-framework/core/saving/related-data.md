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
# <a name="saving-related-data"></a>Ilgili verileri kaydetme

Yalıtılmış varlıklara ek olarak, modelinizde tanımlanan ilişkileri de kullanabilirsiniz.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) GitHub üzerinde.

## <a name="adding-a-graph-of-new-entities"></a>Yeni varlıkların grafiğini ekleme

Birden çok yeni varlık oluşturursanız, bunlardan birini bağlamına eklemek başkalarının da eklenmesine neden olur.

Aşağıdaki örnekte, blogun ve ilgili üç gönderi veritabanına eklenir. Gönderimler, `Blog.Posts` gezinti özelliği aracılığıyla erişilebilir olduklarından bulunur ve eklenir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Yalnızca tek bir varlığın durumunu ayarlamak için EntityEntry. State özelliğini kullanın. Örneğin, uygulamasında yönetilen Hyper-V konakları olarak eklemek için aşağıdaki yordamı kullanabilirsiniz.

## <a name="adding-a-related-entity"></a>İlgili varlık ekleme

Zaten bağlam tarafından izlenen bir varlığın gezinti özelliğinden yeni bir varlığa başvurdıysanız, varlık keşfedilir ve veritabanına eklenir.

Aşağıdaki örnekte, `post` varlık, veritabanından getirilen `blog` varlığın `Posts` özelliğine eklendiği için eklenir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>İlişkileri değiştirme

Bir varlığın gezinti özelliğini değiştirirseniz, ilgili değişiklikler veritabanındaki yabancı anahtar sütununa yapılır.

Aşağıdaki örnekte, `post` , `Blog` gezinti özelliği to olarak `blog`ayarlandığı için varlık yeni `blog` varlığa ait olacak şekilde güncelleştirilir. Ayrıca, bağlam (`post`) tarafından zaten izlenmekte olan bir varlığın gezinti özelliği tarafından başvurulan yeni bir varlık olduğundan, veritabanına da ekleneceğini unutmayın. `blog`

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>İlişkiler kaldırılıyor

Bir başvuru gezintisi `null`ayarlayarak veya bir koleksiyon gezinmede ilgili varlığı kaldırarak ilişkiyi kaldırabilirsiniz.

İlişkinin kaldırılması, ilişkide yapılandırılan basamaklı silme davranışına göre bağımlı varlık üzerinde yan etkilere sahip olabilir.

Varsayılan olarak, gerekli ilişkiler için, bir basamaklı silme davranışı yapılandırılır ve alt/bağımlı varlık veritabanından silinir. İsteğe bağlı ilişkiler için, Cascade Delete varsayılan olarak yapılandırılmaz, ancak yabancı anahtar özelliği null olarak ayarlanır.

İlişkilerin gerekli olduğu şekilde nasıl yapılandırılabileceğini öğrenmek için bkz. [gerekli ve Isteğe bağlı ilişkiler](../modeling/relationships.md#required-and-optional-relationships) .

Basamaklı silme davranışlarının nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Cascade Delete](cascade-delete.md) , nasıl açıkça yapılandırılabilecekleri ve kural tarafından nasıl seçildiği hakkında daha fazla bilgi için.

Aşağıdaki örnekte, ve `Blog` `Post`arasındaki ilişkide bir basamaklı silme yapılandırılır, bu nedenle `post` varlık veritabanından silinir.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
