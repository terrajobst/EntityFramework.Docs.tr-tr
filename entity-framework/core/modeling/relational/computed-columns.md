---
title: Hesaplanan sütunlar - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993959"
---
# <a name="computed-columns"></a><span data-ttu-id="eb4d3-102">Hesaplanan sütunlar</span><span class="sxs-lookup"><span data-stu-id="eb4d3-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="eb4d3-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="eb4d3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="eb4d3-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="eb4d3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="eb4d3-105">Hesaplanmış bir sütun değeri veritabanında hesaplanan bir sütundur.</span><span class="sxs-lookup"><span data-stu-id="eb4d3-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="eb4d3-106">Hesaplanmış bir sütun diğer sütunları tabloda değerini hesaplamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb4d3-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="eb4d3-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="eb4d3-107">Conventions</span></span>

<span data-ttu-id="eb4d3-108">Kural gereği, hesaplanan sütunlar, modele oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="eb4d3-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="eb4d3-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="eb4d3-109">Data Annotations</span></span>

<span data-ttu-id="eb4d3-110">Hesaplanan sütunlar ile veri ek açıklamaları yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="eb4d3-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="eb4d3-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="eb4d3-111">Fluent API</span></span>

<span data-ttu-id="eb4d3-112">Fluent API'si, bir özellik için hesaplanan bir sütunu eşlemelisiniz belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eb4d3-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
