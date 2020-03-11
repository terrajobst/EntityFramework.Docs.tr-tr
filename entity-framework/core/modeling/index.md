---
title: Model oluşturma ve yapılandırma-EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416417"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="c6097-102">Model oluşturma ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c6097-102">Creating and configuring a model</span></span>

<span data-ttu-id="c6097-103">Entity Framework, varlık sınıflarınızın şekline göre bir model oluşturmak için bir kural kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6097-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="c6097-104">Kurala göre keşfedildiğini ve/veya geçersiz kılmak için ek yapılandırma belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6097-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="c6097-105">Bu makale, herhangi bir veri deposunu hedefleyen ve herhangi bir ilişkisel veritabanını hedeflerken uygulanabilen bir modele uygulanabilen yapılandırmayı ele alır.</span><span class="sxs-lookup"><span data-stu-id="c6097-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="c6097-106">Sağlayıcılar, belirli bir veri deposuna özgü yapılandırmayı da etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="c6097-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="c6097-107">Sağlayıcıya özgü yapılandırmayla ilgili belgelerde, [veritabanı sağlayıcıları](../providers/index.md) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="c6097-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="c6097-108">Bu makalenin [örnek](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) GitHub ' da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6097-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="c6097-109">Model yapılandırmak için Fluent API kullanma</span><span class="sxs-lookup"><span data-stu-id="c6097-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="c6097-110">Türetilmiş içerikinizdeki `OnModelCreating` yöntemini geçersiz kılabilir ve modelinizi yapılandırmak için `ModelBuilder API` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6097-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="c6097-111">Bu, yapılandırmanın en güçlü yöntemidir ve varlık sınıflarınızda değişiklik yapılmadan yapılandırmanın belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6097-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="c6097-112">Akıcı API yapılandırması en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="c6097-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="c6097-113">Model yapılandırmak için veri ek açıklamalarını kullanma</span><span class="sxs-lookup"><span data-stu-id="c6097-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="c6097-114">Ayrıca, sınıflarınıza ve özelliklerine öznitelikler (veri ek açıklamaları olarak bilinir) uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6097-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="c6097-115">Veri ek açıklamaları kuralları geçersiz kılar, ancak akıcı API yapılandırması tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="c6097-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
