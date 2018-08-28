---
title: EF Core - Model oluşturma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 9f702d5833b88e6eb77c0afefdae0ed3bc162ec8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993939"
---
# <a name="creating-a-model"></a><span data-ttu-id="a44df-102">Model Oluşturma</span><span class="sxs-lookup"><span data-stu-id="a44df-102">Creating a Model</span></span>

<span data-ttu-id="a44df-103">Entity Framework, varlık sınıfları şeklinizde dayalı bir model oluşturmak için kuralları kümesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="a44df-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="a44df-104">Ek ve/veya ne kuralı tarafından bulunan geçersiz kılmak için ek yapılandırma belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a44df-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="a44df-105">Bu makale, herhangi bir veri deposu ve herhangi bir ilişkisel veritabanı hedeflenirken uygulanabilen, hedefleyen bir model için uygulanabilir yapılandırmayı kapsar.</span><span class="sxs-lookup"><span data-stu-id="a44df-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="a44df-106">Sağlayıcıları, belirli veri deposuna özgü yapılandırma da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="a44df-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="a44df-107">Belirli yapılandırma sağlayıcısı hakkında bilgi için bkz. [veritabanı sağlayıcıları](../providers/index.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a44df-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="a44df-108">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="a44df-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="a44df-109">Yapılandırma yöntemleri</span><span class="sxs-lookup"><span data-stu-id="a44df-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="a44df-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="a44df-110">Fluent API</span></span>

<span data-ttu-id="a44df-111">Geçersiz kılabilirsiniz `OnModelCreating` yöntemi türetilmiş bağlam ve kullanım `ModelBuilder API` modelinizi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="a44df-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="a44df-112">Bu yapılandırmanın en güçlü bir yöntemdir ve yapılandırması, varlık sınıfları değiştirmeden belirtilmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a44df-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="a44df-113">Fluent API configuration en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="a44df-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

### <a name="data-annotations"></a><span data-ttu-id="a44df-114">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a44df-114">Data Annotations</span></span>

<span data-ttu-id="a44df-115">Ayrıca, sınıfları ve özellikleri de (veri ek açıklamaları da bilinir) öznitelikleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a44df-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="a44df-116">Veri ek açıklamaları kuralları geçersiz kılar, ancak tarafından Fluent API'si yapılandırmanın üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="a44df-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
