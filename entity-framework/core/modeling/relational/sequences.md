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
# <a name="sequences"></a><span data-ttu-id="a7553-102">Diziler</span><span class="sxs-lookup"><span data-stu-id="a7553-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="a7553-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a7553-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a7553-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="a7553-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a7553-105">Bir dizi veritabanında sıralı bir sayısal değer oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a7553-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="a7553-106">Dizileri, belirli bir tablo ile ilişkili değildir.</span><span class="sxs-lookup"><span data-stu-id="a7553-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="a7553-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="a7553-107">Conventions</span></span>

<span data-ttu-id="a7553-108">Kurala göre dizileri de modele sunulan değil.</span><span class="sxs-lookup"><span data-stu-id="a7553-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a7553-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a7553-109">Data Annotations</span></span>

<span data-ttu-id="a7553-110">Veri ek açıklamaları kullanarak bir dizi yapılandırabilirsiniz değil.</span><span class="sxs-lookup"><span data-stu-id="a7553-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a7553-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="a7553-111">Fluent API</span></span>

<span data-ttu-id="a7553-112">Modelde bir sıra oluşturmak için Fluent API'sini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7553-112">You can use the Fluent API to create a sequence in the model.</span></span>

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

<span data-ttu-id="a7553-113">Şema, başlangıç değeri ve artış gibi dizisi ek boyutu da yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7553-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

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

<span data-ttu-id="a7553-114">Bir dizi sunulan sonra özelliklerinin değerlerini modelinizi oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7553-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="a7553-115">Örneğin, kullanabileceğiniz [varsayılan değerleri](default-values.md) dizisinden sonraki değeri eklemek için.</span><span class="sxs-lookup"><span data-stu-id="a7553-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

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
