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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Aynı DbContext türüne sahip birden çok model arasında değişen

`OnModelCreating` yerleşik modeli, modelin nasıl oluşturulduğunu değiştirmek için bağlamdaki bir özelliği kullanabilir. Örneğin, bir varlığı bir özelliğe göre farklı şekilde yapılandırmak istediğinizi varsayalım:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Ne yazık ki, EF modeli oluşturduğundan ve `OnModelCreating` yalnızca bir kez çalıştırdığı için bu kod olduğu gibi çalışmaz. Bununla birlikte, farklı modeller üreten özelliğin uyumlu olmasını sağlamak için model önbelleğe alma mekanizmasına bağlayabilirsiniz.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

EF, modeller için önbellek anahtarları oluşturmak üzere `IModelCacheKeyFactory` kullanır; Varsayılan olarak, AŞV verilen her bir içerik türü için model aynı olacağını varsayar, bu nedenle bu hizmetin varsayılan uygulanması yalnızca bağlam türünü içeren bir anahtar döndürür. Aynı bağlam türünden farklı modeller oluşturmak için `IModelCacheKeyFactory` hizmetini doğru uygulamayla değiştirmeniz gerekir; oluşturulan anahtar, modeli etkileyen tüm değişkenleri hesaba ayırarak `Equals` yöntemi kullanılarak diğer model anahtarlarıyla karşılaştırılır:

Aşağıdaki uygulama bir model önbellek anahtarı üretilirken `IgnoreIntProperty` hesaba alır:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Son olarak, yeni `IModelCacheKeyFactory` bağlamın `OnConfiguring`kaydedin:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .
