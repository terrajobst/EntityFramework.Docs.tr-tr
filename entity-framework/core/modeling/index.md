---
title: EF Core - Model oluşturma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 78a8ffd2393a914edf737104f14e41f8a9074ad5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929904"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="38f15-102">Oluşturma ve yapılandırma modeli</span><span class="sxs-lookup"><span data-stu-id="38f15-102">Creating and configuring a Model</span></span>

<span data-ttu-id="38f15-103">Entity Framework, varlık sınıfları şeklinizde dayalı bir model oluşturmak için kuralları kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="38f15-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="38f15-104">Ek ve/veya ne kuralı tarafından bulunan geçersiz kılmak için ek yapılandırma belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38f15-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="38f15-105">Bu makale, herhangi bir veri deposu ve herhangi bir ilişkisel veritabanı hedeflenirken uygulanabilen, hedefleyen bir model için uygulanabilir yapılandırmayı kapsar.</span><span class="sxs-lookup"><span data-stu-id="38f15-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="38f15-106">Sağlayıcıları, belirli veri deposuna özgü yapılandırma da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="38f15-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="38f15-107">Belirli yapılandırma sağlayıcısı hakkında bilgi için bkz. [veritabanı sağlayıcıları](../providers/index.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="38f15-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="38f15-108">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="38f15-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="38f15-109">Bir modeli yapılandırmak için Fluent API'sini kullanın</span><span class="sxs-lookup"><span data-stu-id="38f15-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="38f15-110">Geçersiz kılabilirsiniz `OnModelCreating` yöntemi türetilmiş bağlam ve kullanım `ModelBuilder API` modelinizi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="38f15-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="38f15-111">Bu yapılandırmanın en güçlü bir yöntemdir ve yapılandırması, varlık sınıfları değiştirmeden belirtilmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="38f15-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="38f15-112">Fluent API configuration en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="38f15-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="38f15-113">Bir modeli yapılandırmak için veri ek açıklamalarını kullanma</span><span class="sxs-lookup"><span data-stu-id="38f15-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="38f15-114">Ayrıca, sınıfları ve özellikleri de (veri ek açıklamaları da bilinir) öznitelikleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38f15-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="38f15-115">Veri ek açıklamaları kuralları geçersiz kılar, ancak Fluent API'si yapılandırması tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="38f15-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]
