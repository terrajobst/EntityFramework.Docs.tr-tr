---
title: EF Core - Model oluşturma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 67012d0f52cc77ce872fc428fccc20526f3fefad
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489277"
---
# <a name="creating-and-configuring-a-model"></a>Oluşturma ve yapılandırma modeli

Entity Framework, varlık sınıfları şeklinizde dayalı bir model oluşturmak için kuralları kümesi kullanır. Ek ve/veya ne kuralı tarafından bulunan geçersiz kılmak için ek yapılandırma belirtebilirsiniz.

Bu makale, herhangi bir veri deposu ve herhangi bir ilişkisel veritabanı hedeflenirken uygulanabilen, hedefleyen bir model için uygulanabilir yapılandırmayı kapsar. Sağlayıcıları, belirli veri deposuna özgü yapılandırma da sağlayabilir. Belirli yapılandırma sağlayıcısı hakkında bilgi için bkz. [veritabanı sağlayıcıları](../providers/index.md) bölümü.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) GitHub üzerinde.

## <a name="use-fluent-api-to-configure-a-model"></a>Bir modeli yapılandırmak için Fluent API'sini kullanın

Geçersiz kılabilirsiniz `OnModelCreating` yöntemi türetilmiş bağlam ve kullanım `ModelBuilder API` modelinizi yapılandırmak için. Bu yapılandırmanın en güçlü bir yöntemdir ve yapılandırması, varlık sınıfları değiştirmeden belirtilmesine olanak sağlar. Fluent API configuration en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.

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

## <a name="use-data-annotations-to-configure-a-model"></a>Bir modeli yapılandırmak için veri ek açıklamalarını kullanma

Ayrıca, sınıfları ve özellikleri de (veri ek açıklamaları da bilinir) öznitelikleri uygulayabilirsiniz. Veri ek açıklamaları kuralları geçersiz kılar, ancak Fluent API'si yapılandırması tarafından geçersiz kılınır.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
