---
title: Dahil olmak üzere & özellikleri - EF çekirdek dışında
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
ms.technology: entity-framework-core
uid: core/modeling/included-properties
ms.openlocfilehash: a6eaea4319f6a4d30c223265bf75a88731a38443
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054075"
---
# <a name="including--excluding-properties"></a>Dahil olmak üzere & özellikleri dışında

Model bir özelliği de dahil olmak üzere EF bu özellik hakkında meta veriler sahip olduğu anlamına gelir ve okuma ve yazma veritabanı başlangıç/bitiş değerleri dener.

## <a name="conventions"></a>Kurallar

Kurala göre bir alıcı ve ayarlayıcı ortak özelliklerle modele dahil edilir.

## <a name="data-annotations"></a>Veri ek açıklamaları

Modelden bir özelliği dışarıda tutması için veri ek açıklamaları kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API'si

Modelden bir özelliği dışarıda tutması için Fluent API kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
