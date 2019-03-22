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
# <a name="keys-primary"></a><span data-ttu-id="51cf2-102">Anahtarlar (birincil)</span><span class="sxs-lookup"><span data-stu-id="51cf2-102">Keys (primary)</span></span>

<span data-ttu-id="51cf2-103">Bir anahtar, her varlık örneği için birincil benzersiz tanımlayıcısı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="51cf2-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="51cf2-104">İlişkisel bir veritabanı kullanılırken bu kavramı, eşleyen bir *birincil anahtar*.</span><span class="sxs-lookup"><span data-stu-id="51cf2-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="51cf2-105">Birincil anahtarı olmayan benzersiz bir tanımlayıcı da yapılandırabilirsiniz (bkz [alternatif anahtarlar](alternate-keys.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="51cf2-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="51cf2-106">Aşağıdaki yöntemlerden birini Kurulum/birincil bir anahtar oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="51cf2-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="51cf2-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="51cf2-107">Conventions</span></span>

<span data-ttu-id="51cf2-108">Kural gereği, bir özellik adında `Id` veya `<type name>Id` bir varlığın anahtarı yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="51cf2-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="51cf2-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="51cf2-109">Data Annotations</span></span>

<span data-ttu-id="51cf2-110">Bir varlığın anahtarı olarak tek bir özelliğini yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51cf2-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="51cf2-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="51cf2-111">Fluent API</span></span>

<span data-ttu-id="51cf2-112">Fluent API'si, bir varlığın anahtarı olarak tek bir özelliğini yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51cf2-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="51cf2-113">(Bir bileşik anahtarı bilinir) bir varlığın anahtarı birden çok özelliklerini yapılandırmak için Fluent API'si de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51cf2-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="51cf2-114">Bileşik anahtarlar yalnızca Fluent API'sini kullanarak yapılandırılabilir - kuralları hiçbir zaman bir bileşik anahtarı Kurulum ve bir yapılandırma için veri ek açıklamaları kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="51cf2-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
