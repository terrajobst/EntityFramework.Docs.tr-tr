---
title: Model oluşturma ve yapılandırma-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 5b886226b16b5b1a1f01e6040e58d92ae8678d29
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197313"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="ce601-102">Model oluşturma ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ce601-102">Creating and configuring a model</span></span>

<span data-ttu-id="ce601-103">Entity Framework, varlık sınıflarınızın şekline göre bir model oluşturmak için bir kural kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="ce601-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="ce601-104">Kurala göre keşfedildiğini ve/veya geçersiz kılmak için ek yapılandırma belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce601-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="ce601-105">Bu makale, herhangi bir veri deposunu hedefleyen ve herhangi bir ilişkisel veritabanını hedeflerken uygulanabilen bir modele uygulanabilen yapılandırmayı ele alır.</span><span class="sxs-lookup"><span data-stu-id="ce601-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="ce601-106">Sağlayıcılar, belirli bir veri deposuna özgü yapılandırmayı da etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="ce601-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="ce601-107">Sağlayıcıya özgü yapılandırmayla ilgili belgeler için, [veritabanı sağlayıcıları](../providers/index.md) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ce601-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="ce601-108">Bu makalenin [örneğini](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce601-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="ce601-109">Model yapılandırmak için Fluent API kullanma</span><span class="sxs-lookup"><span data-stu-id="ce601-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="ce601-110">Türetilmiş bağlamdaki yöntemi `OnModelCreating`geçersiz kılabilir ve modelinizi yapılandırmakiçinkullanabilirsiniz. `ModelBuilder API`</span><span class="sxs-lookup"><span data-stu-id="ce601-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="ce601-111">Bu, yapılandırmanın en güçlü yöntemidir ve varlık sınıflarınızda değişiklik yapılmadan yapılandırmanın belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="ce601-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="ce601-112">Akıcı API yapılandırması en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ce601-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="ce601-113">Model yapılandırmak için veri ek açıklamalarını kullanma</span><span class="sxs-lookup"><span data-stu-id="ce601-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="ce601-114">Ayrıca, sınıflarınıza ve özelliklerine öznitelikler (veri ek açıklamaları olarak bilinir) uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce601-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="ce601-115">Veri ek açıklamaları kuralları geçersiz kılar, ancak akıcı API yapılandırması tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="ce601-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
