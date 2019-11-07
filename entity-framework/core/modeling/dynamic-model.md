---
title: Aynı DbContext türüne sahip birden çok model arasında değişen-EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655722"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Aynı DbContext türüne sahip birden çok model arasında değişen

`OnModelCreating` yerleşik modeli, modelin nasıl oluşturulduğunu değiştirmek için bağlamdaki bir özelliği kullanabilir. Örneğin, belirli bir özelliği dışlamak için kullanılabilir:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

Ancak, daha fazla değişiklik yapmadan yukarıdaki işlemi gerçekleştirmeye çalıştıysanız, her bir `IgnoreIntProperty`değeri için her yeni bağlam oluşturulduğunda aynı modeli alırsınız. Bu, yalnızca `OnModelCreating` bir kez çağırarak ve modeli önbelleğe alarak performansı geliştirmek için EF tarafından kullanılan model önbelleğe alma mekanizmasından kaynaklanır.

Varsayılan olarak, model, verilen herhangi bir içerik türü için aynı olacak şekilde kabul eder. Bunu gerçekleştirmek için, `IModelCacheKeyFactory` varsayılan uygulamasını yalnızca bağlam türünü içeren bir anahtar döndürür. Bunu değiştirmek için `IModelCacheKeyFactory` hizmetini değiştirmeniz gerekir. Yeni uygulamanın, modeli etkileyen tüm değişkenleri hesaba alan `Equals` yöntemi kullanılarak diğer model anahtarlarıyla karşılaştırılabileceği bir nesne döndürmesi gerekir:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
