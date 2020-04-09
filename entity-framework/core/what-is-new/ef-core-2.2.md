---
title: EF Core 2.2'deki yenilikler - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417449"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="593bd-102">EF Core 2.2'deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="593bd-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="593bd-103">Uzamsal veri desteği</span><span class="sxs-lookup"><span data-stu-id="593bd-103">Spatial data support</span></span>

<span data-ttu-id="593bd-104">Uzamsal veriler nesnelerin fiziksel konumunu ve şeklini temsil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="593bd-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="593bd-105">Birçok veritabanları yerel olarak depolayabilir, dizine ekleyebilir ve uzamsal verileri sorgulayabilir.</span><span class="sxs-lookup"><span data-stu-id="593bd-105">Many databases can natively store, index, and query spatial data.</span></span>
<span data-ttu-id="593bd-106">Yaygın senaryolar, belirli bir mesafedeki nesneler için sorgulama ve çokgenin belirli bir konum içeriyorsa test edilmesidir.</span><span class="sxs-lookup"><span data-stu-id="593bd-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="593bd-107">EF Core 2.2 artık [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) kitaplığından türleri kullanarak çeşitli veritabanlarından gelen uzamsal verilerle çalışmayı desteklemektedir.</span><span class="sxs-lookup"><span data-stu-id="593bd-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="593bd-108">Uzamsal veri desteği sağlayıcıya özgü bir dizi uzantı paketi olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="593bd-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="593bd-109">Bu paketlerin her biri NTS türleri ve yöntemleri ve veritabanındaki ilgili uzamsal türleri ve işlevleri için eşlemeler katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="593bd-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="593bd-110">Bu tür sağlayıcı uzantıları artık [SQL Server,](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/) [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)ve [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) [(Npgsql projesinden)](https://www.npgsql.org/)için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="593bd-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="593bd-111">Uzamsal türler ek uzantılar olmadan doğrudan [EF Core bellek sağlayıcı](xref:core/providers/in-memory/index) ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="593bd-111">Spatial types can be used directly with the [EF Core in-memory provider](xref:core/providers/in-memory/index) without additional extensions.</span></span>

<span data-ttu-id="593bd-112">Sağlayıcı uzantısı yüklendikten sonra, kuruluşlarınıza desteklenen türlerin özelliklerini ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="593bd-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="593bd-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="593bd-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
```

<span data-ttu-id="593bd-114">Daha sonra uzamsal verilerle varlıkları devam ettir:</span><span class="sxs-lookup"><span data-stu-id="593bd-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```

<span data-ttu-id="593bd-115">Ayrıca, uzamsal verilere ve işlemlere dayalı veritabanı sorgularını yürütebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="593bd-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="593bd-116">Bu özellik hakkında daha fazla bilgi [için, uzamsal türleri belgelerine](xref:core/modeling/spatial)bakın.</span><span class="sxs-lookup"><span data-stu-id="593bd-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span>

## <a name="collections-of-owned-entities"></a><span data-ttu-id="593bd-117">Sahip olunan varlıkların koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="593bd-117">Collections of owned entities</span></span>

<span data-ttu-id="593bd-118">EF Core 2.0, bire bir ilişkilendirmelerde sahipliği modelleme olanağını ekledi.</span><span class="sxs-lookup"><span data-stu-id="593bd-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="593bd-119">EF Core 2.2, mülkiyeti bir-çok ilişkilendirmeye ifade etme yeteneğini genişletir.</span><span class="sxs-lookup"><span data-stu-id="593bd-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="593bd-120">Sahiplik, varlıkların nasıl kullanıldığını kısıtlamaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="593bd-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="593bd-121">Örneğin, sahip olunan varlıklar:</span><span class="sxs-lookup"><span data-stu-id="593bd-121">For example, owned entities:</span></span>

- <span data-ttu-id="593bd-122">Yalnızca diğer varlık türlerinin gezinti özelliklerinde görüntülenebilir.</span><span class="sxs-lookup"><span data-stu-id="593bd-122">Can only ever appear on navigation properties of other entity types.</span></span>
- <span data-ttu-id="593bd-123">Otomatik olarak yüklenir ve yalnızca sahibinin yanında bir DbContext tarafından izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="593bd-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="593bd-124">İlişkisel veritabanlarında, sahip olunan koleksiyonlar, normal bire bir ilişki ilişkisi gibi tablolar sahibinden ayırmak üzere eşlenir.</span><span class="sxs-lookup"><span data-stu-id="593bd-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="593bd-125">Ancak belge yönelimli veritabanlarında, sahip olunan varlıkları (sahip olunan koleksiyonlarda veya başvurularda) sahibiyle aynı belge içinde yerleştirmeyi planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="593bd-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="593bd-126">Yeni OwnsMany() API'sini arayarak özelliği kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="593bd-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="593bd-127">Daha fazla bilgi [için, güncelleştirilmiş sahip olunan varlıklar belgelerine](xref:core/modeling/owned-entities#collections-of-owned-types)bakın.</span><span class="sxs-lookup"><span data-stu-id="593bd-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="593bd-128">Sorgu etiketleri</span><span class="sxs-lookup"><span data-stu-id="593bd-128">Query tags</span></span>

<span data-ttu-id="593bd-129">Bu özellik, linq sorgularının koddaki ilişkisini günlüklerde yakalanan oluşturulan SQL sorgularıyla basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="593bd-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="593bd-130">Sorgu etiketlerinden yararlanmak için, yeni TagWith() yöntemini kullanarak bir LINQ sorgusuna açıklama eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="593bd-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="593bd-131">Önceki örnekteki uzamsal sorguyu kullanma:</span><span class="sxs-lookup"><span data-stu-id="593bd-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="593bd-132">Bu LINQ sorgusu aşağıdaki SQL çıktısını üretecektir:</span><span class="sxs-lookup"><span data-stu-id="593bd-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="593bd-133">Daha fazla bilgi için [sorgu etiketleri belgelerine](xref:core/querying/tags)bakın.</span><span class="sxs-lookup"><span data-stu-id="593bd-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span>
