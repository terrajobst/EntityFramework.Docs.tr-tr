---
title: "Dizileri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="sequences"></a>Diziler

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Bir dizi veritabanında sıralı bir sayısal değer oluşturur. Dizileri, belirli bir tablo ile ilişkili değildir.

## <a name="conventions"></a>Kurallar

Kurala göre dizileri de modele sunulan değil.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamaları kullanarak bir dizi yapılandırabilirsiniz değil.

## <a name="fluent-api"></a>Fluent API'si

Modelde bir sıra oluşturmak için Fluent API'sini kullanın.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

Şema, başlangıç değeri ve artış gibi dizisi ek boyutu da yapılandırabilirsiniz.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

Bir dizi sunulan sonra özelliklerinin değerlerini modelinizi oluşturmak için kullanabilirsiniz. Örneğin, kullanabileceğiniz [varsayılan değerleri](default-values.md) dizisinden sonraki değeri eklemek için.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
