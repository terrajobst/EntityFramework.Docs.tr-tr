---
title: Anahtarları (birincil) - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054111"
---
# <a name="keys-primary"></a>Anahtarları (birincil)

Bir anahtar her varlık örneği için birincil benzersiz tanımlayıcı olarak görev yapar. İlişkisel veritabanı kullanılırken bu kavramı, eşleyen bir *birincil anahtar*. Birincil anahtarı olmayan benzersiz bir tanımlayıcı de yapılandırabilirsiniz (bkz [alternatif anahtarları](alternate-keys.md) daha fazla bilgi için).

## <a name="conventions"></a>Kurallar

Kurala göre bir özellik adlı `Id` veya `<type name>Id` bir varlık anahtarı olarak yapılandırılır.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir varlığın anahtarı olacak şekilde tek bir özellikte yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API'si

Bir varlığın anahtarı olacak şekilde tek bir özellikte yapılandırmak için Fluent API kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

(Bir bileşik anahtarı da bilinir) bir varlık anahtarı için birden çok özelliklerini yapılandırmak için Fluent API de kullanabilirsiniz. Bileşik anahtarlar yalnızca Fluent API kullanılarak yapılandırılabilir - kuralları bileşik bir anahtar hiçbir zaman Kurulum ve bir yapılandırma için veri ek açıklamaları kullanamazsınız.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
