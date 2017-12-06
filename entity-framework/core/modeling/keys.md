---
title: "Anahtarları (birincil) - EF çekirdek"
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
---
# <a name="keys-primary"></a><span data-ttu-id="0b1f1-102">Anahtarları (birincil)</span><span class="sxs-lookup"><span data-stu-id="0b1f1-102">Keys (primary)</span></span>

<span data-ttu-id="0b1f1-103">Bir anahtar her varlık örneği için birincil benzersiz tanımlayıcı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="0b1f1-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="0b1f1-104">İlişkisel veritabanı kullanılırken bu kavramı, eşleyen bir *birincil anahtar*.</span><span class="sxs-lookup"><span data-stu-id="0b1f1-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="0b1f1-105">Birincil anahtarı olmayan benzersiz bir tanımlayıcı de yapılandırabilirsiniz (bkz [alternatif anahtarları](alternate-keys.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="0b1f1-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="0b1f1-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="0b1f1-106">Conventions</span></span>

<span data-ttu-id="0b1f1-107">Kurala göre bir özellik adlı `Id` veya `<type name>Id` bir varlık anahtarı olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="0b1f1-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="0b1f1-108">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="0b1f1-108">Data Annotations</span></span>

<span data-ttu-id="0b1f1-109">Bir varlığın anahtarı olacak şekilde tek bir özellikte yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0b1f1-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="0b1f1-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="0b1f1-110">Fluent API</span></span>

<span data-ttu-id="0b1f1-111">Bir varlığın anahtarı olacak şekilde tek bir özellikte yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0b1f1-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="0b1f1-112">(Bir bileşik anahtarı da bilinir) bir varlık anahtarı için birden çok özelliklerini yapılandırmak için Fluent API de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0b1f1-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="0b1f1-113">Bileşik anahtarlar yalnızca Fluent API kullanılarak yapılandırılabilir - kuralları bileşik bir anahtar hiçbir zaman Kurulum ve bir yapılandırma için veri ek açıklamaları kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="0b1f1-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
