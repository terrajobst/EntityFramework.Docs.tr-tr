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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="fdf2d-102">Birden çok modeli aynı DbContext türünde arasında geçiş yapma</span><span class="sxs-lookup"><span data-stu-id="fdf2d-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="fdf2d-103">Yerleşik olarak model `OnModelCreating` özellik bağlamda modelin nasıl oluşturulduğunu değiştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fdf2d-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="fdf2d-104">Örneğin belirli bir özelliği dışarıda için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fdf2d-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="fdf2d-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="fdf2d-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="fdf2d-106">Ek değişiklik yapmadan yukarıdaki işlemin yapılması denediyseniz herhangi bir değer için yeni içerik oluşturulduğunda ancak aynı modelin elde edebileceğiniz `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="fdf2d-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="fdf2d-107">Bu mekanizma EF kullanan yalnızca çağırarak performansını artırmak için önbelleğe alma modeli kaynaklanır `OnModelCreating` ve sonra model önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="fdf2d-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="fdf2d-108">Varsayılan olarak EF verilen her bağlam için model türü aynı olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="fdf2d-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="fdf2d-109">Varsayılan uygulaması bunu sağlamak için `IModelCacheKeyFactory` yalnızca bağlam türü içeren bir anahtar döndürür.</span><span class="sxs-lookup"><span data-stu-id="fdf2d-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="fdf2d-110">Bunu değiştirmek için gereksinim duyduğunuz değiştirmek için `IModelCacheKeyFactory` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="fdf2d-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="fdf2d-111">Yeni uygulama karşılaştırılabilir nesneyi kullanarak diğer model anahtarları döndürmesi gerekir `Equals` modeli etkileyen tüm değişkenleri dikkate yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fdf2d-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
