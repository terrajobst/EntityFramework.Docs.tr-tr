---
title: Hesaplanan sütunlar - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054072"
---
# <a name="computed-columns"></a><span data-ttu-id="0765e-102">Hesaplanan sütunlar</span><span class="sxs-lookup"><span data-stu-id="0765e-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="0765e-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0765e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0765e-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="0765e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0765e-105">Hesaplanmış bir sütun değeri veritabanında hesaplanan bir sütundur.</span><span class="sxs-lookup"><span data-stu-id="0765e-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="0765e-106">Hesaplanmış bir sütun diğer sütunları tabloda değerini hesaplamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0765e-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="0765e-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="0765e-107">Conventions</span></span>

<span data-ttu-id="0765e-108">Kurala göre model içinde hesaplanan sütunlar oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="0765e-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0765e-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="0765e-109">Data Annotations</span></span>

<span data-ttu-id="0765e-110">Hesaplanan sütunlar veri ek açıklamaları ile yapılandırılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="0765e-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0765e-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="0765e-111">Fluent API</span></span>

<span data-ttu-id="0765e-112">Bir özellik için hesaplanan sütunu eşlemelisiniz belirtmek için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0765e-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
