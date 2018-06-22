---
title: Aynı DbContext türü - EF çekirdek ile birden çok modelleri arasında geçiş yapma
author: AndriySvyryd
ms.author: divega
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/dynamic-model
ms.openlocfilehash: 57136802001124ebf9ae7682e33f8dc7826fc2b0
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678728"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Aynı DbContext türü ile birden çok modelleri arasında geçiş yapma

Yerleşik modeli `OnModelCreating` bir özellik bağlamda modelin nasıl yapılandırıldığını değiştirmek için kullanabilirsiniz. Örneğin belirli bir özelliği dışarıda tutması için kullanılabilir:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Yukarıdaki ek değişiklik yapmadan yapılması çalıştıysanız herhangi bir değer için her bir yeni içerik oluşturulduğunda ancak aynı modelin elde edebileceğiniz `IgnoreIntProperty`. Bu mekanizma EF kullanan yalnızca çağırarak performansını artırmak için önbelleğe alma modeli kaynaklanır `OnModelCreating` bir kez ve model önbelleğe alma.

Varsayılan olarak, belirtilen her bağlam için modelin türü aynı olacağını EF varsayar. Bu varsayılan uygulaması gerçekleştirmek için `IModelCacheKeyFactory` yalnızca bağlam türü içeren bir anahtar döndürür. Bu gereksinim değiştirmek için değiştirmek için `IModelCacheKeyFactory` hizmet. Yeni uygulama karşılaştırılabilir bir nesne kullanarak diğer model anahtarlar döndürmesi gerekir `Equals` modelini etkileyen tüm değişkenleri dikkate alır yöntemi:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
