---
title: EF Core 2.2 - EF Core yenilikleri
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688807"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="de56a-102">EF Core 2.2 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="de56a-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="de56a-103">Uzamsal veri desteği</span><span class="sxs-lookup"><span data-stu-id="de56a-103">Spatial data support</span></span>

<span data-ttu-id="de56a-104">Uzamsal veriler fiziksel konuma ve nesnelerin şeklini temsil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="de56a-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="de56a-105">Yerel olarak çok sayıda veritabanında depolamak, dizin ve uzamsal verileri sorgulama.</span><span class="sxs-lookup"><span data-stu-id="de56a-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="de56a-106">Yaygın senaryolar şunlardır: belirli bir uzaklık içindeki nesneler için sorgulama ve belirli bir konuma bir Çokgen içeriyorsa, test etme.</span><span class="sxs-lookup"><span data-stu-id="de56a-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="de56a-107">EF Core 2.2 destekler türlerinden kullanarak çeşitli veritabanları uzamsal verilerle çalışma [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="de56a-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="de56a-108">Uzamsal veri desteği, sağlayıcıya özgü uzantı paketleri bir dizi olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="de56a-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="de56a-109">Bu paketlerin her NTS türleri ve yöntemleri ve karşılık gelen uzamsal türleri ve işlevleri veritabanında eşlemelerini katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="de56a-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="de56a-110">Bu tür sağlayıcısı uzantılar için kullanıma sunulmuştur [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), ve [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (gelen [Npgsql proje](http://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="de56a-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](http://www.npgsql.org/)).</span></span>
<span data-ttu-id="de56a-111">Uzamsal türleri, doğrudan kullanılabilir [EF Core bellek içi sağlayıcısı](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) ek uzantıları olmadan.</span><span class="sxs-lookup"><span data-stu-id="de56a-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="de56a-112">Sağlayıcı uzantısı yüklendikten sonra desteklenen tür özellikleri için varlıklarınızı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de56a-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="de56a-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de56a-113">For example:</span></span>

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

<span data-ttu-id="de56a-114">Ardından, uzamsal verileri içeren varlıklar devam edebilir:</span><span class="sxs-lookup"><span data-stu-id="de56a-114">You can then persist entities with spatial data:</span></span>

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
<span data-ttu-id="de56a-115">Ve uzamsal verileri ve işlemleri temel alan veritabanı sorgularını yürütebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="de56a-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="de56a-116">Bu özellik hakkında daha fazla bilgi için bkz. [uzamsal türler belgeleri](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="de56a-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="de56a-117">Sahip olunan varlık koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="de56a-117">Collections of owned entities</span></span>

<span data-ttu-id="de56a-118">EF Core 2.0 için bire bir ilişkileri modeli sahip olma özelliği eklendi.</span><span class="sxs-lookup"><span data-stu-id="de56a-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="de56a-119">EF Core 2.2-çok ilişkileri sahipliği express olanağı genişletir.</span><span class="sxs-lookup"><span data-stu-id="de56a-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="de56a-120">Sahipliği varlıkları nasıl kullanıldığını sınırlamak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="de56a-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="de56a-121">Örneğin, sahip olunan varlık:</span><span class="sxs-lookup"><span data-stu-id="de56a-121">For example, owned entities:</span></span>
- <span data-ttu-id="de56a-122">Gezinti özellikleri diğer varlık türleri yalnızca görünür.</span><span class="sxs-lookup"><span data-stu-id="de56a-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="de56a-123">Otomatik olarak yüklenir ve yalnızca bir DbContext birlikte bunların sahibi tarafından izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="de56a-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="de56a-124">İlişkisel veritabanları, sahibi, normal bir-çok ilişkileri gibi tablolar ayırmak için sahip olduğu koleksiyonları eşlenir.</span><span class="sxs-lookup"><span data-stu-id="de56a-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="de56a-125">Ancak, belge yönelimli veritabanlarında, sahip olunan varlıklar (sahibi koleksiyonları veya başvuruları) aynı belge sahibi olarak içinde iç içe planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="de56a-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="de56a-126">Yeni OwnsMany() API'sini çağırarak özelliğini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="de56a-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="de56a-127">Daha fazla bilgi için [sahip olunan varlık belgeleri güncelleştirildi](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="de56a-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="de56a-128">Sorgu etiketleri</span><span class="sxs-lookup"><span data-stu-id="de56a-128">Query tags</span></span>

<span data-ttu-id="de56a-129">Bu özellik, kodu, LINQ sorgularında günlüklerde yakalanan oluşturulan SQL sorgularının bağıntısını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="de56a-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="de56a-130">Sorgu etiketleri yararlanmak için yeni TagWith() yöntemini kullanarak bir LINQ Sorgu açıklama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="de56a-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="de56a-131">Önceki örnekte uzamsal sorgu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="de56a-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="de56a-132">Bu LINQ sorgusu SQL aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="de56a-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="de56a-133">Daha fazla bilgi için [sorgu etiketleri belgeleri](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="de56a-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 