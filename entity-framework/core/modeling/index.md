---
title: Model oluşturma ve yapılandırma - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416417"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="e7f7f-102">Model oluşturma ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e7f7f-102">Creating and configuring a model</span></span>

<span data-ttu-id="e7f7f-103">Varlık Çerçevesi, varlık sınıflarınızın şekline dayalı bir model oluşturmak için bir dizi kural kullanır.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="e7f7f-104">Sözleşme tarafından keşfedilenleri tamamlamak ve/veya geçersiz kılmak için ek yapılandırma belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="e7f7f-105">Bu makalede, herhangi bir veri deposu hedefleyen bir modele uygulanabilecek ve herhangi bir ilişkisel veritabanıhedeflediğinde uygulanabilecek yapılandırma yı kapsar.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="e7f7f-106">Sağlayıcılar, belirli bir veri deposuna özgü yapılandırmayı da etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="e7f7f-107">Sağlayıcıya özgü yapılandırmayla ilgili belgeler için [Veritabanı Sağlayıcıları](../providers/index.md) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="e7f7f-108">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="e7f7f-109">Bir modeli yapılandırmak için akıcı API kullanın</span><span class="sxs-lookup"><span data-stu-id="e7f7f-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="e7f7f-110">Türemiş `OnModelCreating` bağlamınızdaki yöntemi geçersiz kılabilir ve modelinizi yapılandırmak için yöntemk `ModelBuilder API` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="e7f7f-111">Bu yapılandırmanın en güçlü yöntemidir ve varlık sınıflarınızı değiştirmeden yapılandırmanın belirtilmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="e7f7f-112">Akıcı API yapılandırması en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="e7f7f-113">Bir modeli yapılandırmak için veri ek açıklamalarını kullanma</span><span class="sxs-lookup"><span data-stu-id="e7f7f-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="e7f7f-114">Sınıflarınıza ve özelliklerinize öznitelikleri (Veri Ek Açıklamaları olarak da bilinir) de uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="e7f7f-115">Veri ek açıklamaları kuralları geçersiz kılar, ancak Akıcı API yapılandırması tarafından geçersiz kılınacaktır.</span><span class="sxs-lookup"><span data-stu-id="e7f7f-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
