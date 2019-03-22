---
title: Anahtarlar (birincil) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 6272e323b83ccab2ed060a2ebbde1d1e8e353d66
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319172"
---
# <a name="keys-primary"></a>Anahtarlar (birincil)

Bir anahtar, her varlık örneği için birincil benzersiz tanımlayıcısı olarak görev yapar. İlişkisel bir veritabanı kullanılırken bu kavramı, eşleyen bir *birincil anahtar*. Birincil anahtarı olmayan benzersiz bir tanımlayıcı da yapılandırabilirsiniz (bkz [alternatif anahtarlar](alternate-keys.md) daha fazla bilgi için). 

Aşağıdaki yöntemlerden birini Kurulum/birincil bir anahtar oluşturmak için kullanılabilir.

## <a name="conventions"></a>Kurallar

Kural gereği, bir özellik adında `Id` veya `<type name>Id` bir varlığın anahtarı yapılandırılır.

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

Bir varlığın anahtarı olarak tek bir özelliğini yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

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

Fluent API'si, bir varlığın anahtarı olarak tek bir özelliğini yapılandırmak için kullanabilirsiniz.

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

(Bir bileşik anahtarı bilinir) bir varlığın anahtarı birden çok özelliklerini yapılandırmak için Fluent API'si de kullanabilirsiniz. Bileşik anahtarlar yalnızca Fluent API'sini kullanarak yapılandırılabilir - kuralları hiçbir zaman bir bileşik anahtarı Kurulum ve bir yapılandırma için veri ek açıklamaları kullanamazsınız.

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
