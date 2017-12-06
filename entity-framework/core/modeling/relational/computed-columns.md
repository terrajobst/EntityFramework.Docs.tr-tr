---
title: "Hesaplanan sütunlar - EF çekirdek"
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
---
# <a name="computed-columns"></a>Hesaplanan sütunlar

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Hesaplanmış bir sütun değeri veritabanında hesaplanan bir sütundur. Hesaplanmış bir sütun diğer sütunları tabloda değerini hesaplamak için kullanabilirsiniz.

## <a name="conventions"></a>Kurallar

Kurala göre model içinde hesaplanan sütunlar oluşturulmaz.

## <a name="data-annotations"></a>Veri ek açıklamaları

Hesaplanan sütunlar veri ek açıklamaları ile yapılandırılabilir değildir.

## <a name="fluent-api"></a>Fluent API'si

Bir özellik için hesaplanan sütunu eşlemelisiniz belirtmek için Fluent API kullanabilirsiniz.

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
