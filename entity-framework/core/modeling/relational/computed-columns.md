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
# <a name="computed-columns"></a>Hesaplanan Sütunlar

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Hesaplanan sütun, değeri veritabanında hesaplanan bir sütundur. Hesaplanan bir sütun, değerini hesaplamak için tablodaki diğer sütunları kullanabilir.

## <a name="conventions"></a>Kurallar

Kurala göre, hesaplanan sütunlar modelde oluşturulmaz.

## <a name="data-annotations"></a>Veri Açıklamaları

Hesaplanan sütunlar, veri açıklamaları ile yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Bir özelliğin hesaplanan bir sütuna eşlenmesi gerektiğini belirtmek için Floent API 'sini kullanabilirsiniz.

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
