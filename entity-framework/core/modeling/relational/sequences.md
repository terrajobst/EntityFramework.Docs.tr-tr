---
title: Dizileri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: eb9d9896966af0ad6b778047a1ed6af7358e8eb2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994522"
---
# <a name="sequences"></a><span data-ttu-id="42384-102">Diziler</span><span class="sxs-lookup"><span data-stu-id="42384-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="42384-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="42384-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="42384-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="42384-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="42384-105">Bir sıra veritabanında bir sıralı sayısal değerleri üretir.</span><span class="sxs-lookup"><span data-stu-id="42384-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="42384-106">Dizileri belirli bir tabloyla ilişkili değildir.</span><span class="sxs-lookup"><span data-stu-id="42384-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="42384-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="42384-107">Conventions</span></span>

<span data-ttu-id="42384-108">Kural gereği, dizileri modele de sunulan değil.</span><span class="sxs-lookup"><span data-stu-id="42384-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="42384-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="42384-109">Data Annotations</span></span>

<span data-ttu-id="42384-110">Veri ek açıklamaları kullanarak bir dizi yapılandırabilirsiniz değil.</span><span class="sxs-lookup"><span data-stu-id="42384-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="42384-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="42384-111">Fluent API</span></span>

<span data-ttu-id="42384-112">Fluent API'si modelde bir sıra oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="42384-112">You can use the Fluent API to create a sequence in the model.</span></span>

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

<span data-ttu-id="42384-113">Şema, başlangıç değeri ve artırma gibi dizisi ek yönüyle da yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="42384-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

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

<span data-ttu-id="42384-114">Bir dizi sunulan sonra modelinizde özellik değerleri oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="42384-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="42384-115">Örneğin, kullanabileceğiniz [varsayılan değerleri](default-values.md) dizisinden sonraki değeri eklemek için.</span><span class="sxs-lookup"><span data-stu-id="42384-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

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
