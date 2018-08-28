---
title: Aynı DbContext tür - EF Core ile birden çok modelleri arasında geçiş yapma
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994992"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Birden çok modeli aynı DbContext türünde arasında geçiş yapma

Yerleşik olarak model `OnModelCreating` özellik bağlamda modelin nasıl oluşturulduğunu değiştirmek için kullanabilirsiniz. Örneğin belirli bir özelliği dışarıda için kullanılabilir:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Ek değişiklik yapmadan yukarıdaki işlemin yapılması denediyseniz herhangi bir değer için yeni içerik oluşturulduğunda ancak aynı modelin elde edebileceğiniz `IgnoreIntProperty`. Bu mekanizma EF kullanan yalnızca çağırarak performansını artırmak için önbelleğe alma modeli kaynaklanır `OnModelCreating` ve sonra model önbelleğe alma.

Varsayılan olarak EF verilen her bağlam için model türü aynı olduğu varsayılır. Varsayılan uygulaması bunu sağlamak için `IModelCacheKeyFactory` yalnızca bağlam türü içeren bir anahtar döndürür. Bunu değiştirmek için gereksinim duyduğunuz değiştirmek için `IModelCacheKeyFactory` hizmeti. Yeni uygulama karşılaştırılabilir nesneyi kullanarak diğer model anahtarları döndürmesi gerekir `Equals` modeli etkileyen tüm değişkenleri dikkate yöntemi:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
