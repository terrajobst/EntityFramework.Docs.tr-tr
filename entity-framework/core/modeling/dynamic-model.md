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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="3f043-102">Aynı DbContext türüne sahip birden çok model arasında değişen</span><span class="sxs-lookup"><span data-stu-id="3f043-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="3f043-103">`OnModelCreating` yerleşik modeli, modelin nasıl oluşturulduğunu değiştirmek için bağlamdaki bir özelliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="3f043-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="3f043-104">Örneğin, belirli bir özelliği dışlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="3f043-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="3f043-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="3f043-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="3f043-106">Ancak, daha fazla değişiklik yapmadan yukarıdaki işlemi gerçekleştirmeye çalıştıysanız, her bir `IgnoreIntProperty`değeri için her yeni bağlam oluşturulduğunda aynı modeli alırsınız.</span><span class="sxs-lookup"><span data-stu-id="3f043-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="3f043-107">Bu, yalnızca `OnModelCreating` bir kez çağırarak ve modeli önbelleğe alarak performansı geliştirmek için EF tarafından kullanılan model önbelleğe alma mekanizmasından kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="3f043-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="3f043-108">Varsayılan olarak, model, verilen herhangi bir içerik türü için aynı olacak şekilde kabul eder.</span><span class="sxs-lookup"><span data-stu-id="3f043-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="3f043-109">Bunu gerçekleştirmek için, `IModelCacheKeyFactory` varsayılan uygulamasını yalnızca bağlam türünü içeren bir anahtar döndürür.</span><span class="sxs-lookup"><span data-stu-id="3f043-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="3f043-110">Bunu değiştirmek için `IModelCacheKeyFactory` hizmetini değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3f043-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="3f043-111">Yeni uygulamanın, modeli etkileyen tüm değişkenleri hesaba alan `Equals` yöntemi kullanılarak diğer model anahtarlarıyla karşılaştırılabileceği bir nesne döndürmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="3f043-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
