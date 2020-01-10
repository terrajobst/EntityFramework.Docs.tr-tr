---
title: Aynı DbContext türüne sahip birden çok model arasında değişen-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781137"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="8d567-102">Aynı DbContext türüne sahip birden çok model arasında değişen</span><span class="sxs-lookup"><span data-stu-id="8d567-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="8d567-103">`OnModelCreating` yerleşik modeli, modelin nasıl oluşturulduğunu değiştirmek için bağlamdaki bir özelliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="8d567-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="8d567-104">Örneğin, bir varlığı bir özelliğe göre farklı şekilde yapılandırmak istediğinizi varsayalım:</span><span class="sxs-lookup"><span data-stu-id="8d567-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="8d567-105">Ne yazık ki, EF modeli oluşturduğundan ve `OnModelCreating` yalnızca bir kez çalıştırdığı için bu kod olduğu gibi çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="8d567-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="8d567-106">Bununla birlikte, farklı modeller üreten özelliğin uyumlu olmasını sağlamak için model önbelleğe alma mekanizmasına bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d567-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="8d567-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="8d567-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="8d567-108">EF, modeller için önbellek anahtarları oluşturmak üzere `IModelCacheKeyFactory` kullanır; Varsayılan olarak, AŞV verilen her bir içerik türü için model aynı olacağını varsayar, bu nedenle bu hizmetin varsayılan uygulanması yalnızca bağlam türünü içeren bir anahtar döndürür.</span><span class="sxs-lookup"><span data-stu-id="8d567-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="8d567-109">Aynı bağlam türünden farklı modeller oluşturmak için `IModelCacheKeyFactory` hizmetini doğru uygulamayla değiştirmeniz gerekir; oluşturulan anahtar, modeli etkileyen tüm değişkenleri hesaba ayırarak `Equals` yöntemi kullanılarak diğer model anahtarlarıyla karşılaştırılır:</span><span class="sxs-lookup"><span data-stu-id="8d567-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="8d567-110">Aşağıdaki uygulama bir model önbellek anahtarı üretilirken `IgnoreIntProperty` hesaba alır:</span><span class="sxs-lookup"><span data-stu-id="8d567-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="8d567-111">Son olarak, yeni `IModelCacheKeyFactory` bağlamın `OnConfiguring`kaydedin:</span><span class="sxs-lookup"><span data-stu-id="8d567-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="8d567-112">Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .</span><span class="sxs-lookup"><span data-stu-id="8d567-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
