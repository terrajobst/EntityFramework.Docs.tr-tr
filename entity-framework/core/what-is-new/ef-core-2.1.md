---
title: "EF çekirdek 2.1 - EF çekirdek yenilikler nelerdir?"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 1e5e9839bae1e5da4082d90c02d098bb3b2b43bd
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="fbe76-102">EF çekirdek 2.1 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="fbe76-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="fbe76-103">Bu sürüm hala önizlemede değil.</span><span class="sxs-lookup"><span data-stu-id="fbe76-103">This release is still in preview.</span></span>

<span data-ttu-id="fbe76-104">Çok sayıda küçük geliştirmeleri ve birden fazla yüzlerce ürün hata düzeltmeleri yanı sıra, EF çekirdek 2.1 çeşitli yeni özellikler içerir:</span><span class="sxs-lookup"><span data-stu-id="fbe76-104">Besides numerous small improvements and more than a hundred product bug fixes, EF Core 2.1 includes several new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="fbe76-105">yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="fbe76-105">Lazy loading</span></span>
<span data-ttu-id="fbe76-106">EF çekirdek şimdi herkes, gezinti özellikleri isteğe bağlı olarak yükleyebilir varlık sınıfları yazmak gerekli yapı taşlarını içerir.</span><span class="sxs-lookup"><span data-stu-id="fbe76-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="fbe76-107">Bu yapı taşları yararlanır Microsoft.EntityFrameworkCore.Proxies, yeni bir paket de oluşturduk yavaş yükleniyor proxy üretmek için en düşük düzeyde temel sınıfları sınıflar (örneğin sanal Gezinti özellikleri sınıflarıyla) değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="fbe76-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="fbe76-108">Okuma [geç yükleme bölümünde](xref:core/querying/related-data#lazy-loading) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fbe76-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="fbe76-109">Varlık oluşturucuları parametrelerinde</span><span class="sxs-lookup"><span data-stu-id="fbe76-109">Parameters in entity constructors</span></span>
<span data-ttu-id="fbe76-110">Geç yükleme için gerekli yapı taşları biri olarak size, kendi kurucularda parametre almaz varlıkları oluşturma etkin.</span><span class="sxs-lookup"><span data-stu-id="fbe76-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="fbe76-111">Özellik değerleri, yavaş yükleniyor Temsilciler ve Hizmetleri eklemesine parametrelerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fbe76-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="fbe76-112">Okuma [varlık Oluşturucusu parametrelerle bölüm](xref:core/modeling/constructors) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fbe76-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="fbe76-113">Değer dönüşümler</span><span class="sxs-lookup"><span data-stu-id="fbe76-113">Value conversions</span></span>
<span data-ttu-id="fbe76-114">Şimdiye kadar EF çekirdek yalnızca yerel olarak temel alınan veritabanı sağlayıcısı tarafından desteklenen türlerin özellikleri eşleyin.</span><span class="sxs-lookup"><span data-stu-id="fbe76-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="fbe76-115">Değerleri, sütunları ve herhangi bir dönüştürme olmadan özellikleri arasında ileri ve geri kopyalandı.</span><span class="sxs-lookup"><span data-stu-id="fbe76-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="fbe76-116">EF çekirdek 2.1 ile başlayarak, değer dönüşümler özelliklerine ve tersi yönde uygulanmadan önce sütunlarından elde edilen değerleri dönüştürmek için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fbe76-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="fbe76-117">Sütunları ve özellikleri arasında özel dönüştürmeler kaydetme imkan tanıyan bir açık yapılandırma API'si yanı sıra, gerekli olarak kural tarafından uygulanan dönüşümleri sayısı sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="fbe76-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="fbe76-118">Bu özellik uygulama bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="fbe76-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="fbe76-119">Numaralandırmalar dizeleri depolama</span><span class="sxs-lookup"><span data-stu-id="fbe76-119">Storing enums as strings</span></span>
- <span data-ttu-id="fbe76-120">Tamsayıları SQL Server ile eşleme imzalanmamış</span><span class="sxs-lookup"><span data-stu-id="fbe76-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="fbe76-121">Otomatik şifreleme ve şifre çözme özellik değeri</span><span class="sxs-lookup"><span data-stu-id="fbe76-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="fbe76-122">Okuma [değer dönüşümler bölüm](xref:core/modeling/value-conversions) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fbe76-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="fbe76-123">LINQ GroupBy çevirisi</span><span class="sxs-lookup"><span data-stu-id="fbe76-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="fbe76-124">GroupBy LINQ işleci her zaman olduğu EF çekirdek 2.1 sürümünde bellekte değerlendirilmesi önce.</span><span class="sxs-lookup"><span data-stu-id="fbe76-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="fbe76-125">GROUP BY yan tümcesine en yaygın durumlarda çevirme artık destekler.</span><span class="sxs-lookup"><span data-stu-id="fbe76-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="fbe76-126">Bu örnek bir sorgu ile çeşitli toplama işlevleri hesaplamak için kullanılan GroupBy gösterir:</span><span class="sxs-lookup"><span data-stu-id="fbe76-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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

<span data-ttu-id="fbe76-127">Karşılık gelen SQL çeviri şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="fbe76-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="fbe76-128">Dengeli veri</span><span class="sxs-lookup"><span data-stu-id="fbe76-128">Data Seeding</span></span>
<span data-ttu-id="fbe76-129">Yeni sürümde bir veritabanını doldurmak için ilk veri sağlamak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fbe76-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="fbe76-130">EF6 içinde verileri dengeli modeli yapılandırmasının bir parçası olarak bir varlık türü için ilişkili benzemez.</span><span class="sxs-lookup"><span data-stu-id="fbe76-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="fbe76-131">Ardından EF çekirdek geçişler otomatik olarak ne ekleme, güncelleştirme veya silme işlemleri veritabanı modeli yeni bir sürüme yükseltirken uygulanması gerek hesaplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fbe76-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="fbe76-132">Örnek olarak, bu bir POST çekirdek verileri yapılandırmak için kullanabileceğiniz `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="fbe76-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="fbe76-133">Okuma [verileri dengeli üzerinde bölüm](xref:core/modeling/data-seeding) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fbe76-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="fbe76-134">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="fbe76-134">Query types</span></span>
<span data-ttu-id="fbe76-135">EF çekirdek modeli şimdi sorgu türleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="fbe76-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="fbe76-136">Varlık türlerinin aksine, sorgu türleri değil sahip üzerlerinde tanımlanmış anahtarları ve eklenen, silinemez veya güncelleştirilmiş (yani, salt okunur), ancak doğrudan sorgular tarafından döndürdü.</span><span class="sxs-lookup"><span data-stu-id="fbe76-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="fbe76-137">Sorgu türleri için kullanım senaryoları bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="fbe76-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="fbe76-138">Birincil anahtarlar olmadan görünümlerine eşleme</span><span class="sxs-lookup"><span data-stu-id="fbe76-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="fbe76-139">Birincil anahtarlar olmadan tablolara eşleme</span><span class="sxs-lookup"><span data-stu-id="fbe76-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="fbe76-140">Model içinde tanımlanan sorgulara eşleme</span><span class="sxs-lookup"><span data-stu-id="fbe76-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="fbe76-141">Dönüş türü olarak hizmet veren `FromSql()` sorguları</span><span class="sxs-lookup"><span data-stu-id="fbe76-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="fbe76-142">Okuma [sorgu türleri bölümüne](xref:core/modeling/query-types) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fbe76-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="fbe76-143">Türetilen türler için</span><span class="sxs-lookup"><span data-stu-id="fbe76-143">Include for derived types</span></span>
<span data-ttu-id="fbe76-144">Yalnızca tanımlanan Gezinti özelliklerini belirtmek için olası türetilmiş tür ifadeler için yazarken artık olacaktır `Include` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fbe76-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="fbe76-145">Kesin türü belirtilmiş sürümünü için `Include`, açık bir atama kullanarak destekliyoruz veya `as` işleci.</span><span class="sxs-lookup"><span data-stu-id="fbe76-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="fbe76-146">Ayrıca artık adları türetilmiş türler dize sürümünde tanımlı Gezinti özelliğinin başvuru destekliyoruz `Include`:</span><span class="sxs-lookup"><span data-stu-id="fbe76-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="fbe76-147">Okuma [INCLUDE bölüm türetilen türlerle](xref:core/querying/related-data#include-on-derived-types) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fbe76-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="fbe76-148">System.Transactions desteği</span><span class="sxs-lookup"><span data-stu-id="fbe76-148">System.Transactions support</span></span>
<span data-ttu-id="fbe76-149">System.Transactions TransactionScope gibi özellikler ile çalışma olanağını ekledik.</span><span class="sxs-lookup"><span data-stu-id="fbe76-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="fbe76-150">Bu hem .NET Framework hem de .NET Core destekleyen veritabanı sağlayıcıları kullanırken çalışır.</span><span class="sxs-lookup"><span data-stu-id="fbe76-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="fbe76-151">Okuma [System.Transactions bölüm](xref:core/saving/transactions#using-systemtransactions) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fbe76-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="fbe76-152">Daha iyi sütun ilk geçişte sıralaması</span><span class="sxs-lookup"><span data-stu-id="fbe76-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="fbe76-153">Müşteri geri bildirimi doğrultusunda, sınıflar olarak bildirilen özelliklerde gibi tablolar için sütunları aynı sırada başlangıçta oluşturmak için geçişler güncelleştirdiniz.</span><span class="sxs-lookup"><span data-stu-id="fbe76-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="fbe76-154">İlk tablo oluşturulduktan sonra yeni üye eklediğinizde EF çekirdek sırası değiştirilemiyor unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fbe76-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="fbe76-155">Bağıntılı alt sorgulara en iyi duruma getirme</span><span class="sxs-lookup"><span data-stu-id="fbe76-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="fbe76-156">Biz "N + 1" yürütme önlemek için bizim sorgusu çevirisi iyileştirilmiştir projeksiyon Gezinti özelliğinde kullanımını müşteri adayları verileriyle ilişkili bir alt sorgu kök sorgudan verileri birleştirme için birçok yaygın senaryolar SQL sorguları.</span><span class="sxs-lookup"><span data-stu-id="fbe76-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="fbe76-157">En iyi duruma getirme form alt sorgu sonuçları arabelleğe alma ve katılımı yeni davranışı için sorguyu değiştirin gerektiren gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fbe76-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="fbe76-158">Örnek olarak, aşağıdaki sorguyu normalde bir sorguyu müşteriler artı (burada "N" döndürülen müşteri sayısını alır) çevrilir sorgular için siparişleri ayırın:</span><span class="sxs-lookup"><span data-stu-id="fbe76-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="fbe76-159">Ekleyerek `ToList()` doğru yerde arabelleğe alma iyileştirmeyi etkinleştirme siparişleri için uygun olduğunu gösteriyor:</span><span class="sxs-lookup"><span data-stu-id="fbe76-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="fbe76-160">Bu sorgu için yalnızca iki SQL sorguları çevrilir unutmayın: biri müşteriler ve siparişler bir sonraki.</span><span class="sxs-lookup"><span data-stu-id="fbe76-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

### <a name="ownedattribute"></a><span data-ttu-id="fbe76-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="fbe76-161">OwnedAttribute</span></span>

<span data-ttu-id="fbe76-162">Şimdi yapılandırmak olası [varlık türlerine ait](xref:core/modeling/owned-entities) yalnızca türüyle yorumlama tarafından `[Owned]` ve sahibi varlık emin olmak için model eklenir:</span><span class="sxs-lookup"><span data-stu-id="fbe76-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="database-provider-compatibility"></a><span data-ttu-id="fbe76-163">Veritabanı sağlayıcısı uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="fbe76-163">Database provider compatibility</span></span>

<span data-ttu-id="fbe76-164">EF çekirdek 2.1 EF çekirdek 2.0 için oluşturulan veritabanı sağlayıcıları ile uyumlu olacak şekilde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="fbe76-164">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0.</span></span> <span data-ttu-id="fbe76-165">(Örn. değer dönüşümler) açıklanan özelliklerden bazıları güncellenmiş bir sağlayıcı gerektirirken başkalarının (örneğin yavaş yükleniyor) mevcut sağlayıcılarıyla yanar.</span><span class="sxs-lookup"><span data-stu-id="fbe76-165">While some of the features described above (e.g. value conversions) require an updated provider others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="fbe76-166">Her beklenmeyen bulursanız uyumsuzluk herhangi sorun'deki yeni özelliklerin veya bunlar üzerinde Geribildiriminiz varsa lütfen kullanarak rapor [bizim sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="fbe76-166">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
