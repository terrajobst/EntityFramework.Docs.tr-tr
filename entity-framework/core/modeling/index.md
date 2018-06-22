---
title: EF Çekirdek - Model oluşturma
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812443"
---
# <a name="creating-a-model"></a><span data-ttu-id="6c7ce-102">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="6c7ce-102">Creating a Model</span></span>

<span data-ttu-id="6c7ce-103">Entity Framework kuralları kümesi varlık sınıflarınızı şeklin dayalı bir model oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="6c7ce-104">Ek niteliğindedir ve/veya hangi kural tarafından bulunan geçersiz kılmak için ek yapılandırma belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="6c7ce-105">Bu makalede, tüm veri deposu ve olduğu herhangi bir ilişkisel veritabanı hedeflerken uygulanabilir hedefleyen bir model uygulanabilir yapılandırma kapsar.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="6c7ce-106">Sağlayıcıları, belirli veri deposu için özel yapılandırma da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="6c7ce-107">Belirli bir yapılandırma sağlayıcısı belgelerine bakın [veritabanı sağlayıcıları](../providers/index.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="6c7ce-108">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) github'da.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="6c7ce-109">Yapılandırma yöntemleri</span><span class="sxs-lookup"><span data-stu-id="6c7ce-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="6c7ce-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="6c7ce-110">Fluent API</span></span>

<span data-ttu-id="6c7ce-111">Geçersiz kılabilirsiniz `OnModelCreating` türetilmiş bağlamını ve kullanım yönteminde `ModelBuilder API` modelinizi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="6c7ce-112">Bu yapılandırmanın en güçlü bir yöntemdir ve varlık sınıflarınızı değiştirmeden belirtilmesi için yapılandırmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="6c7ce-113">Fluent API yapılandırmasını en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a><span data-ttu-id="6c7ce-114">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="6c7ce-114">Data Annotations</span></span>

<span data-ttu-id="6c7ce-115">Öznitelikleri (veri ek açıklamaları da bilinir) sınıfları ve özellikleri için geçerli olabilir.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="6c7ce-116">Veri ek açıklamaları kuralları geçersiz kılar, ancak tarafından Fluent API yapılandırmanın üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="6c7ce-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
