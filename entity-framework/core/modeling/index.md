---
title: "EF Çekirdek - Model oluşturma"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a>Model oluşturma

Entity Framework kuralları kümesi varlık sınıflarınızı şeklin dayalı bir model oluşturmak için kullanır. Ek niteliğindedir ve/veya hangi kural tarafından bulunan geçersiz kılmak için ek yapılandırma belirtebilirsiniz.

Bu makalede, tüm veri deposu ve olduğu herhangi bir ilişkisel veritabanı hedeflerken uygulanabilir hedefleyen bir model uygulanabilir yapılandırma kapsar. Sağlayıcıları, belirli veri deposu için özel yapılandırma da sağlayabilir. Belirli bir yapılandırma sağlayıcısı belgelerine bakın [veritabanı sağlayıcıları](../providers/index.md) bölümü.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) github'da.

## <a name="methods-of-configuration"></a>Yapılandırma yöntemleri

### <a name="fluent-api"></a>Fluent API'si

Geçersiz kılabilirsiniz `OnModelCreating` türetilmiş bağlamını ve kullanım yönteminde `ModelBuilder API` modelinizi yapılandırmak için. Bu yapılandırmanın en güçlü bir yöntemdir ve varlık sınıflarınızı değiştirmeden belirtilmesi için yapılandırmasını sağlar. Fluent API yapılandırmasını en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamaları geçersiz kılar.

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

### <a name="data-annotations"></a>Veri ek açıklamaları

Öznitelikleri (veri ek açıklamaları da bilinir) sınıfları ve özellikleri için geçerli olabilir. Veri ek açıklamaları kuralları geçersiz kılar, ancak tarafından Fluent API yapılandırmanın üzerine yazılır.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
