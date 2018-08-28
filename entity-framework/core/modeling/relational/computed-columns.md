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
# <a name="computed-columns"></a>Hesaplanan sütunlar

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Hesaplanmış bir sütun değeri veritabanında hesaplanan bir sütundur. Hesaplanmış bir sütun diğer sütunları tabloda değerini hesaplamak için kullanabilirsiniz.

## <a name="conventions"></a>Kurallar

Kural gereği, hesaplanan sütunlar, modele oluşturulmaz.

## <a name="data-annotations"></a>Veri ek açıklamaları

Hesaplanan sütunlar ile veri ek açıklamaları yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir özellik için hesaplanan bir sütunu eşlemelisiniz belirtmek için kullanabilirsiniz.

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
