---
title: Hesaplanan sütunlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: da106c94698a202744d7cd465aa84d0d72802833
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197237"
---
# <a name="computed-columns"></a><span data-ttu-id="5aef3-102">Hesaplanan Sütunlar</span><span class="sxs-lookup"><span data-stu-id="5aef3-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="5aef3-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5aef3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5aef3-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="5aef3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5aef3-105">Hesaplanan sütun, değeri veritabanında hesaplanan bir sütundur.</span><span class="sxs-lookup"><span data-stu-id="5aef3-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="5aef3-106">Hesaplanan bir sütun, değerini hesaplamak için tablodaki diğer sütunları kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="5aef3-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="5aef3-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="5aef3-107">Conventions</span></span>

<span data-ttu-id="5aef3-108">Kurala göre, hesaplanan sütunlar modelde oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5aef3-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5aef3-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5aef3-109">Data Annotations</span></span>

<span data-ttu-id="5aef3-110">Hesaplanan sütunlar, veri açıklamaları ile yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="5aef3-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5aef3-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="5aef3-111">Fluent API</span></span>

<span data-ttu-id="5aef3-112">Bir özelliğin hesaplanan bir sütuna eşlenmesi gerektiğini belirtmek için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5aef3-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/ComputedColumn.cs?highlight=9)] -->
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
