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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="90517-102">Aynı DbContext türü ile birden çok modelleri arasında geçiş yapma</span><span class="sxs-lookup"><span data-stu-id="90517-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="90517-103">Yerleşik modeli `OnModelCreating` bir özellik bağlamda modelin nasıl yapılandırıldığını değiştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90517-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="90517-104">Örneğin belirli bir özelliği dışarıda tutması için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="90517-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="90517-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="90517-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="90517-106">Yukarıdaki ek değişiklik yapmadan yapılması çalıştıysanız herhangi bir değer için her bir yeni içerik oluşturulduğunda ancak aynı modelin elde edebileceğiniz `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="90517-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="90517-107">Bu mekanizma EF kullanan yalnızca çağırarak performansını artırmak için önbelleğe alma modeli kaynaklanır `OnModelCreating` bir kez ve model önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="90517-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="90517-108">Varsayılan olarak, belirtilen her bağlam için modelin türü aynı olacağını EF varsayar.</span><span class="sxs-lookup"><span data-stu-id="90517-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="90517-109">Bu varsayılan uygulaması gerçekleştirmek için `IModelCacheKeyFactory` yalnızca bağlam türü içeren bir anahtar döndürür.</span><span class="sxs-lookup"><span data-stu-id="90517-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="90517-110">Bu gereksinim değiştirmek için değiştirmek için `IModelCacheKeyFactory` hizmet.</span><span class="sxs-lookup"><span data-stu-id="90517-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="90517-111">Yeni uygulama karşılaştırılabilir bir nesne kullanarak diğer model anahtarlar döndürmesi gerekir `Equals` modelini etkileyen tüm değişkenleri dikkate alır yöntemi:</span><span class="sxs-lookup"><span data-stu-id="90517-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
