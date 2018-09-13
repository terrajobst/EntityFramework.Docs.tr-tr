---
title: EF Core 2.1 - EF Core yenilikleri
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: f67f2e695d269e2dde11d396f9a67fd137600f56
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489409"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="16e0b-102">EF Core 2.1 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="16e0b-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="16e0b-103">EF Core 2.1, çeşitli hata düzeltmeleri ve küçük işlevsel ve performans iyileştirmeleri yanı sıra, bazı ilgi çekici yeni özellikler içerir:</span><span class="sxs-lookup"><span data-stu-id="16e0b-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="16e0b-104">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="16e0b-104">Lazy loading</span></span>
<span data-ttu-id="16e0b-105">EF Core artık herkes Gezinti özelliklerini isteğe bağlı olarak yükleyebilir, varlık sınıfları yazmak gerekli yapı taşlarını içerir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="16e0b-106">Ayrıca yeni bir paket, bu yapı taşlarını yararlanan Microsoft.EntityFrameworkCore.Proxies oluşturduk yavaş yükleniyor proxy oluşturmak için en düşük düzeyde temel sınıfları varlık sınıflarının (örneğin, sanal Gezinti özellikleri ile sınıflar) değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="16e0b-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="16e0b-107">Okuma [yavaş yükleme bölümünde](xref:core/querying/related-data#lazy-loading) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="16e0b-108">Varlık Yapıcılardaki parametreleri</span><span class="sxs-lookup"><span data-stu-id="16e0b-108">Parameters in entity constructors</span></span>
<span data-ttu-id="16e0b-109">Gecikmeli yükleme için gerekli yapı taşlarını biri olarak kendi oluşturucularda parametre alan bir varlık oluşturma etkinleştirdik.</span><span class="sxs-lookup"><span data-stu-id="16e0b-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="16e0b-110">Özellik değerleri, yavaş yükleniyor Temsilciler ve Hizmetleri ekleme için parametreleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16e0b-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="16e0b-111">Okuma [varlık Oluşturucu parametrelere sahip bölüm](xref:core/modeling/constructors) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="16e0b-112">Değer dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="16e0b-112">Value conversions</span></span>
<span data-ttu-id="16e0b-113">Şimdiye kadar EF Core, yalnızca yerel olarak temel alınan veritabanı sağlayıcısı tarafından desteklenen tür özellikleri eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16e0b-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="16e0b-114">Değerleri sütunlar ve herhangi bir dönüştürme olmadan özellikler arasında ileri ve geri kopyalandı.</span><span class="sxs-lookup"><span data-stu-id="16e0b-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="16e0b-115">EF Core 2.1 ile başlayarak, değer dönüştürmeleri özelliklerine ve tam tersi uygulanmadan önce sütunlarından elde edilen değerleri dönüştürmek için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="16e0b-116">Sütunları ve özellikleri arasında özel dönüştürmeler kaydetme izin veren açık yapılandırma API yanı sıra, gerektiğinde kural tarafından uygulanan dönüştürmeler bir dizi sahibiz.</span><span class="sxs-lookup"><span data-stu-id="16e0b-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="16e0b-117">Bu özellik uygulamanın bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="16e0b-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="16e0b-118">Numaralandırmalar dize olarak depolama</span><span class="sxs-lookup"><span data-stu-id="16e0b-118">Storing enums as strings</span></span>
- <span data-ttu-id="16e0b-119">Eşleme işaretsiz tamsayılar SQL Server ile</span><span class="sxs-lookup"><span data-stu-id="16e0b-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="16e0b-120">Otomatik şifreleme ve şifre çözme özellik değerleri</span><span class="sxs-lookup"><span data-stu-id="16e0b-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="16e0b-121">Okuma [değer dönüştürmeleri bölüm](xref:core/modeling/value-conversions) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="16e0b-122">LINQ GroupBy çeviri</span><span class="sxs-lookup"><span data-stu-id="16e0b-122">LINQ GroupBy translation</span></span>
<span data-ttu-id="16e0b-123">Sürüm 2.1 önce EF Core GroupBy LINQ işlecini her zaman bellekte değerlendirilmesi.</span><span class="sxs-lookup"><span data-stu-id="16e0b-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="16e0b-124">GROUP BY yan tümcesine en yaygın durumlarda çevirme artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="16e0b-125">Bu örnek bir sorgu ile çeşitli toplama işlevleri hesaplamak için kullanılan GroupBy gösterir:</span><span class="sxs-lookup"><span data-stu-id="16e0b-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="16e0b-126">Karşılık gelen SQL çeviri şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="16e0b-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="16e0b-127">Veri çekirdeği oluşturma</span><span class="sxs-lookup"><span data-stu-id="16e0b-127">Data Seeding</span></span>
<span data-ttu-id="16e0b-128">Yeni sürümle birlikte bir veritabanını doldurmak için ilk veri sağlamak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="16e0b-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="16e0b-129">Farklı EF6 içinde veri dengeli dağıtım modeli yapılandırmasının bir parçası olarak bir varlık türü için ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="16e0b-130">EF Core geçişleri sonra otomatik olarak ne ekleme, güncelleştirme veya silme işlemleri veritabanı modelin yeni bir sürüme yükseltirken uygulanmasına gerek hesaplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16e0b-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="16e0b-131">Örnek olarak, bu veri kaynağı bir Post yapılandırmak için kullanabileceğiniz `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="16e0b-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="16e0b-132">Okuma [veri üretme bölümü](xref:core/modeling/data-seeding) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="16e0b-133">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="16e0b-133">Query types</span></span>
<span data-ttu-id="16e0b-134">EF Core model artık sorgu türleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="16e0b-135">Varlık türlerinden farklı olarak, sorgu türleri değil üzerinde tanımlanmış tuşu ve eklenmiş, silinmiş veya güncelleştirilmiş olamaz (diğer bir deyişle, bunlar salt okunur), ancak doğrudan sorgular tarafından döndürülebilir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="16e0b-136">Sorgu türleri için kullanım senaryoları bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="16e0b-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="16e0b-137">Birincil anahtarları olmadan görünümlerine eşleme</span><span class="sxs-lookup"><span data-stu-id="16e0b-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="16e0b-138">Tabloların birincil anahtarları olmadan eşleme</span><span class="sxs-lookup"><span data-stu-id="16e0b-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="16e0b-139">Modelde tanımlanan sorguları eşleme</span><span class="sxs-lookup"><span data-stu-id="16e0b-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="16e0b-140">İçin dönüş türü olarak hizmet veren `FromSql()` sorguları</span><span class="sxs-lookup"><span data-stu-id="16e0b-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="16e0b-141">Okuma [sorgu türleri bölümüne](xref:core/modeling/query-types) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-141">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="16e0b-142">Türetilmiş türler</span><span class="sxs-lookup"><span data-stu-id="16e0b-142">Include for derived types</span></span>
<span data-ttu-id="16e0b-143">Türetilmiş türler için ifadeleri yazarken yalnızca tanımlanan Gezinti özelliklerini tanıyabilir artık olacaktır `Include` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="16e0b-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="16e0b-144">Kesin türü belirtilmiş sürümünü için `Include`, açık bir yayın kullanarak destekliyoruz veya `as` işleci.</span><span class="sxs-lookup"><span data-stu-id="16e0b-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="16e0b-145">Ayrıca artık türetilmiş türler dize sürümünde tanımlanan gezinti özelliği adlarını başvuran destekliyoruz `Include`:</span><span class="sxs-lookup"><span data-stu-id="16e0b-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="16e0b-146">Okuma [INCLUDE bölüm türetilen türlerle](xref:core/querying/related-data#include-on-derived-types) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="16e0b-147">System.Transactions desteği</span><span class="sxs-lookup"><span data-stu-id="16e0b-147">System.Transactions support</span></span>
<span data-ttu-id="16e0b-148">TransactionScope gibi System.Transactions özelliklerle çalışma olanağı ekledik.</span><span class="sxs-lookup"><span data-stu-id="16e0b-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="16e0b-149">Bu hem .NET Framework hem de .NET Core destekleyen veritabanı sağlayıcıları kullanırken çalışır.</span><span class="sxs-lookup"><span data-stu-id="16e0b-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="16e0b-150">Okuma [System.Transactions bölüm](xref:core/saving/transactions#using-systemtransactions) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="16e0b-151">İlk geçişinde sıralama daha iyi sütun</span><span class="sxs-lookup"><span data-stu-id="16e0b-151">Better column ordering in initial migration</span></span>
<span data-ttu-id="16e0b-152">Müşteri geri bildirimi doğrultusunda, özellikleri, sınıflarda bildirilen başlangıçta tablolar için aynı sırada vygenerovat sloupce geçişlerini güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="16e0b-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="16e0b-153">EF Core ilk tablo oluşturulduktan sonra yeni üye eklendiğinde sipariş değiştiremeyeceğinizi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="16e0b-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="16e0b-154">Bağıntılı alt sorgularda en iyi duruma getirilmesi</span><span class="sxs-lookup"><span data-stu-id="16e0b-154">Optimization of correlated subqueries</span></span>
<span data-ttu-id="16e0b-155">"N + 1" yürütme önlemek için sunduğumuz sorgu çevirisi geliştirildi SQL sorgularında projeksiyon Gezinti özelliğinde kullanımını müşteri adayları verilerle ilişkili alt sorgu kök sorgudan verileri birleştirme için birçok yaygın senaryo.</span><span class="sxs-lookup"><span data-stu-id="16e0b-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="16e0b-156">Alt sorgu sonuçları arabelleğe alma iyileştirme gerektirir ve yeni davranış katılımı için sorguyu değiştirin gerektirir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="16e0b-157">Örnek olarak, aşağıdaki sorguyu normalde bir sorguya müşterileri, ayrıca (burada döndürülen müşteri sayısı, "N") N çevrilir sorguları siparişlerini ayırın:</span><span class="sxs-lookup"><span data-stu-id="16e0b-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="16e0b-158">Ekleyerek `ToList()` doğru yerde arabelleğe alma iyileştirmeyi etkinleştirme siparişleri için uygun olduğunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="16e0b-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="16e0b-159">Bu sorgu için yalnızca iki SQL sorguları çevrilir unutmayın: biri müşteriler ve siparişler için sonraki komutu.</span><span class="sxs-lookup"><span data-stu-id="16e0b-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="16e0b-160">[Ait] özniteliği</span><span class="sxs-lookup"><span data-stu-id="16e0b-160">[Owned] attribute</span></span>

<span data-ttu-id="16e0b-161">Şimdi yapılandırmak mümkün [varlık türlerine ait](xref:core/modeling/owned-entities) yalnızca türüyle yorumlama tarafından `[Owned]` ve sahibi varlık emin olduktan sonra modele eklenir:</span><span class="sxs-lookup"><span data-stu-id="16e0b-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="16e0b-162">Komut satırı aracı dotnet-ef .NET Core SDK'sına dahil</span><span class="sxs-lookup"><span data-stu-id="16e0b-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="16e0b-163">_Dotnet-ef_ komutları artık .NET Core SDK'sının parçası, bu nedenle, artık DotNetCliToolReference geçişler kullanın veya var olan bir veritabanından bir DbContext iskelesini kullanabilmek için projede kullanmak için gerekli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="16e0b-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="16e0b-164">Bölümüne [araçlarını yükleme](xref:core/miscellaneous/cli/dotnet#installing-the-tools) EF Core ve .NET Core SDK'ün farklı sürümleri için komut satırı araçları'nı etkinleştirme hakkında daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="16e0b-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="16e0b-165">Microsoft.EntityFrameworkCore.Abstractions paket</span><span class="sxs-lookup"><span data-stu-id="16e0b-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="16e0b-166">Yeni paket öznitelikleri ve projelerinizde kullanan bir bütün olarak EF Core üzerinde bir bağımlılık almadan EF Core özellikleri açık arabirimler içerir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="16e0b-167">Örneğin, [ait] özniteliği ve ILazyLoader arabirimi burada yer alır.</span><span class="sxs-lookup"><span data-stu-id="16e0b-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="16e0b-168">Durum değişikliği olayları</span><span class="sxs-lookup"><span data-stu-id="16e0b-168">State change events</span></span>

<span data-ttu-id="16e0b-169">Yeni `Tracked` ve `StateChanged` olaylarına `ChangeTracker` DbContext girme ve durumlarını değiştirerek varlıklara tepki verdiğini mantığı yazmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16e0b-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="16e0b-170">Ham SQL parametresi Çözümleyicisi</span><span class="sxs-lookup"><span data-stu-id="16e0b-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="16e0b-171">Yeni bir kod Çözümleyicisi EF Core ile SQL ham apı'lerimizi güvensiz kullanımı gibi algılar dahil edilen `FromSql` veya `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="16e0b-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="16e0b-172">Örneğin, aşağıdaki sorgu için bir uyarı çünkü göreceğiniz _minAge_ değil parametreli:</span><span class="sxs-lookup"><span data-stu-id="16e0b-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="16e0b-173">Veritabanı sağlayıcısı uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="16e0b-173">Database provider compatibility</span></span>

<span data-ttu-id="16e0b-174">Güncelleştirilen veya EF Core 2.1 ile çalışmak üzere en az test sağlayıcıları ile EF Core 2.1 kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="16e0b-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="16e0b-175">Herhangi bir beklenmeyen bulursanız uyumsuzluk herhangi sorun yeni özellikler veya bunlar üzerinde geri bildirimde bulunmak istiyorsanız lütfen kullanarak rapor [müşterilerimize sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="16e0b-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
