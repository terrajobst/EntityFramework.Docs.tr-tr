---
title: EF Core 2,2 ' deki yenilikler-EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 5fcf7c6dfb4d8cb7928ef974af6deb52df7c63eb
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181370"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="0bbaa-102">EF Core 2,2 ' deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="0bbaa-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="0bbaa-103">Uzamsal veri desteği</span><span class="sxs-lookup"><span data-stu-id="0bbaa-103">Spatial data support</span></span>

<span data-ttu-id="0bbaa-104">Uzamsal veriler, nesnelerin fiziksel konumunu ve şeklini temsil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="0bbaa-105">Birçok veritabanı, uzamsal verileri yerel olarak saklayabilir, dizine alabilir ve sorgulayabilir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="0bbaa-106">Yaygın senaryolar belirli bir mesafe içindeki nesnelerin sorgulanmasını ve bir çokgenin belirli bir konumu içerip içermediği test edilmesini içerir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="0bbaa-107">EF Core 2,2 artık [Nettopologyısuite](https://github.com/NetTopologySuite/NetTopologySuite) (,) kitaplığındaki türler kullanılarak çeşitli veritabanlarındaki uzamsal verilerle çalışmayı desteklemektedir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="0bbaa-108">Uzamsal veri desteği, bir dizi sağlayıcıya özgü uzantı paketleri olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="0bbaa-109">Bu paketlerin her biri, türler ve yöntemler için eşlemeler ve veritabanındaki karşılık gelen uzamsal türleri ve işlevleri katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="0bbaa-110">Bu tür sağlayıcı uzantıları artık [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)ve [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) ( [npgsql projesinden](https://www.npgsql.org/)) için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="0bbaa-111">Uzamsal türler, ek Uzantılar olmadan doğrudan [EF Core bellek içi sağlayıcıyla](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="0bbaa-112">Sağlayıcı uzantısı yüklendikten sonra, varlıklarınızda desteklenen türlerin özelliklerini ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="0bbaa-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0bbaa-113">For example:</span></span>

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

<span data-ttu-id="0bbaa-114">Daha sonra varlıkları uzamsal verilerle kalıcı hale getirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0bbaa-114">You can then persist entities with spatial data:</span></span>

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
<span data-ttu-id="0bbaa-115">Ve uzamsal verilere ve işlemlere göre veritabanı sorguları yürütebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0bbaa-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="0bbaa-116">Bu özellik hakkında daha fazla bilgi için bkz. [uzamsal türler belgeleri](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="0bbaa-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="0bbaa-117">Sahip olunan varlıkların koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="0bbaa-117">Collections of owned entities</span></span>

<span data-ttu-id="0bbaa-118">EF Core 2,0, bire bir İlişkilendirmelerde sahiplik modelleyebilme özelliği ekledi.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="0bbaa-119">EF Core 2,2, sahipliğin bire çok ilişkilerine kapsamını ifade etme yeteneğini uzatır.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="0bbaa-120">Sahiplik, varlıkların nasıl kullanıldığını sınırlandırmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="0bbaa-121">Örneğin, sahip olunan varlıklar:</span><span class="sxs-lookup"><span data-stu-id="0bbaa-121">For example, owned entities:</span></span>
- <span data-ttu-id="0bbaa-122">Yalnızca diğer varlık türlerinin gezinti özelliklerinde görünebilirler.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="0bbaa-123">Otomatik olarak yüklenir ve yalnızca sahibinden sonra bir DbContext ile izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="0bbaa-124">İlişkisel veritabanlarında, sahip olunan koleksiyonlar, benzer bir şekilde, normal bire çok İlişkilendirmelerde olduğu gibi, sahip tarafından ayrı tablolara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="0bbaa-125">Ancak, belge odaklı veritabanlarında, sahip olduğu aynı belge içinde sahip olunan varlıkları (sahip olan koleksiyonlar veya başvurular) iç içe bir şekilde planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="0bbaa-126">Yeni OwnsMany () API 'sini çağırarak özelliği kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0bbaa-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="0bbaa-127">Daha fazla bilgi için bkz. [sahip olunan varlıklar belgeleri](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="0bbaa-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="0bbaa-128">Sorgu etiketleri</span><span class="sxs-lookup"><span data-stu-id="0bbaa-128">Query tags</span></span>

<span data-ttu-id="0bbaa-129">Bu özellik, günlüklerde yakalanan oluşturulmuş SQL sorguları ile koddaki LINQ sorgularının bağıntısını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="0bbaa-130">Sorgu etiketlerinin avantajlarından yararlanmak için New TagWith () yöntemini kullanarak bir LINQ sorgusuna açıklama ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0bbaa-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="0bbaa-131">Önceki bir örnekteki uzamsal sorguyu kullanma:</span><span class="sxs-lookup"><span data-stu-id="0bbaa-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="0bbaa-132">Bu LINQ sorgusu aşağıdaki SQL çıktısını oluşturacak:</span><span class="sxs-lookup"><span data-stu-id="0bbaa-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="0bbaa-133">Daha fazla bilgi için bkz. [sorgu etiketleri belgeleri](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="0bbaa-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 