---
title: EF Core - Model oluşturma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: e4eed480178ce43cbc5ece8db8e584032da7b2b9
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250354"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="e5210-102">Oluşturma ve yapılandırma modeli</span><span class="sxs-lookup"><span data-stu-id="e5210-102">Creating and configuring a Model</span></span>

<span data-ttu-id="e5210-103">Entity Framework, varlık sınıfları şeklinizde dayalı bir model oluşturmak için kuralları kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5210-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="e5210-104">Ek ve/veya ne kuralı tarafından bulunan geçersiz kılmak için ek yapılandırma belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5210-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="e5210-105">Bu makale, herhangi bir veri deposu ve herhangi bir ilişkisel veritabanı hedeflenirken uygulanabilen, hedefleyen bir model için uygulanabilir yapılandırmayı kapsar.</span><span class="sxs-lookup"><span data-stu-id="e5210-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="e5210-106">Sağlayıcıları, belirli veri deposuna özgü yapılandırma da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="e5210-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="e5210-107">Belirli yapılandırma sağlayıcısı hakkında bilgi için bkz. [veritabanı sağlayıcıları](../providers/index.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="e5210-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="e5210-108">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="e5210-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="e5210-109">Bir modeli yapılandırmak için Fluent API'sini kullanın</span><span class="sxs-lookup"><span data-stu-id="e5210-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="e5210-110">Geçersiz kılabilirsiniz `OnModelCreating` yöntemi türetilmiş bağlam ve kullanım `ModelBuilder API` modelinizi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="e5210-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="e5210-111">Bu yapılandırmanın en güçlü bir yöntemdir ve yapılandırması, varlık sınıfları değiştirmeden belirtilmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5210-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="e5210-112">Fluent API configuration en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="e5210-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="e5210-113">Bir modeli yapılandırmak için veri ek açıklamalarını kullanma</span><span class="sxs-lookup"><span data-stu-id="e5210-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="e5210-114">Ayrıca, sınıfları ve özellikleri de (veri ek açıklamaları da bilinir) öznitelikleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5210-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="e5210-115">Veri ek açıklamaları kuralları geçersiz kılar, ancak tarafından Fluent API'si yapılandırmanın üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="e5210-115">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
