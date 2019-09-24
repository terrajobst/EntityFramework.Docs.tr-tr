---
title: Diziler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: ce02b9840e58102a60c1d8eacf6810365104d7d7
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196915"
---
# <a name="sequences"></a><span data-ttu-id="c0a57-102">Diziler</span><span class="sxs-lookup"><span data-stu-id="c0a57-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="c0a57-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c0a57-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c0a57-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="c0a57-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c0a57-105">Dizi, veritabanında ardışık bir sayısal değer oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c0a57-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="c0a57-106">Diziler belirli bir tabloyla ilişkilendirilmemiş.</span><span class="sxs-lookup"><span data-stu-id="c0a57-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="c0a57-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="c0a57-107">Conventions</span></span>

<span data-ttu-id="c0a57-108">Kurala göre, diziler ' de modele tanıtılmaz.</span><span class="sxs-lookup"><span data-stu-id="c0a57-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c0a57-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="c0a57-109">Data Annotations</span></span>

<span data-ttu-id="c0a57-110">Veri ek açıklamalarını kullanarak bir sıra yapılandıramazsınız.</span><span class="sxs-lookup"><span data-stu-id="c0a57-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c0a57-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="c0a57-111">Fluent API</span></span>

<span data-ttu-id="c0a57-112">Modelde bir sıra oluşturmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0a57-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/Sequence.cs?highlight=7)] -->
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

<span data-ttu-id="c0a57-113">Ayrıca, dizinin şeması, başlangıç değeri ve artışı gibi ek yönlerini de yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0a57-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
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

<span data-ttu-id="c0a57-114">Bir sıra alındıktan sonra, modelinizdeki özelliklerin değerlerini oluşturmak için bunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0a57-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="c0a57-115">Örneğin, sıradaki değeri sırayla eklemek için [varsayılan değerleri](default-values.md) kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0a57-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
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
