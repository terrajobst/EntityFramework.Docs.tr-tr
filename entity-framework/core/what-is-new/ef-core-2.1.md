---
title: EF çekirdek 2.1 - EF çekirdek yenilikler nelerdir?
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 2372a6b2e3f3b7b1d9214a6ea321fe28cea45fff
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754431"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="3df21-102">EF çekirdek 2.1 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="3df21-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="3df21-103">Çok sayıda hata düzeltmeleri ve küçük işlevsel ve performans geliştirmeleri yanı sıra EF çekirdek 2.1 ilgi çekici bazı yeni özellikler içerir:</span><span class="sxs-lookup"><span data-stu-id="3df21-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="3df21-104">yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="3df21-104">Lazy loading</span></span>
<span data-ttu-id="3df21-105">EF çekirdek şimdi herkes, gezinti özellikleri isteğe bağlı olarak yükleyebilir varlık sınıfları yazmak gerekli yapı taşlarını içerir.</span><span class="sxs-lookup"><span data-stu-id="3df21-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="3df21-106">Bu yapı taşları yararlanır Microsoft.EntityFrameworkCore.Proxies, yeni bir paket de oluşturduk yavaş yükleniyor proxy üretmek için en düşük düzeyde temel sınıfları sınıflar (örneğin, sanal Gezinti özellikleri sınıflarıyla) değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="3df21-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="3df21-107">Okuma [geç yükleme bölümünde](xref:core/querying/related-data#lazy-loading) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3df21-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="3df21-108">Varlık oluşturucuları parametrelerinde</span><span class="sxs-lookup"><span data-stu-id="3df21-108">Parameters in entity constructors</span></span>
<span data-ttu-id="3df21-109">Geç yükleme için gerekli yapı taşları biri olarak size, kendi kurucularda parametre almaz varlıkları oluşturma etkin.</span><span class="sxs-lookup"><span data-stu-id="3df21-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="3df21-110">Özellik değerleri, yavaş yükleniyor Temsilciler ve Hizmetleri eklemesine parametrelerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3df21-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="3df21-111">Okuma [varlık Oluşturucusu parametrelerle bölüm](xref:core/modeling/constructors) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3df21-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="3df21-112">Değer dönüşümler</span><span class="sxs-lookup"><span data-stu-id="3df21-112">Value conversions</span></span>
<span data-ttu-id="3df21-113">Şimdiye kadar EF çekirdek yalnızca yerel olarak temel alınan veritabanı sağlayıcısı tarafından desteklenen türlerin özellikleri eşleyin.</span><span class="sxs-lookup"><span data-stu-id="3df21-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="3df21-114">Değerleri, sütunları ve herhangi bir dönüştürme olmadan özellikleri arasında ileri ve geri kopyalandı.</span><span class="sxs-lookup"><span data-stu-id="3df21-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="3df21-115">EF çekirdek 2.1 ile başlayarak, değer dönüşümler özelliklerine ve tersi yönde uygulanmadan önce sütunlarından elde edilen değerleri dönüştürmek için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="3df21-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="3df21-116">Sütunları ve özellikleri arasında özel dönüştürmeler kaydetme imkan tanıyan bir açık yapılandırma API'si yanı sıra, gerekli olarak kural tarafından uygulanan dönüşümleri sayısı sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="3df21-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="3df21-117">Bu özellik uygulama bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="3df21-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="3df21-118">Numaralandırmalar dizeleri depolama</span><span class="sxs-lookup"><span data-stu-id="3df21-118">Storing enums as strings</span></span>
- <span data-ttu-id="3df21-119">Tamsayıları SQL Server ile eşleme imzalanmamış</span><span class="sxs-lookup"><span data-stu-id="3df21-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="3df21-120">Otomatik şifreleme ve şifre çözme özellik değeri</span><span class="sxs-lookup"><span data-stu-id="3df21-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="3df21-121">Okuma [değer dönüşümler bölüm](xref:core/modeling/value-conversions) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3df21-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="3df21-122">LINQ GroupBy çevirisi</span><span class="sxs-lookup"><span data-stu-id="3df21-122">LINQ GroupBy translation</span></span>
<span data-ttu-id="3df21-123">Sürüm 2.1 önce EF çekirdek GroupBy LINQ işleci her zaman bellekte değerlendirilmesi.</span><span class="sxs-lookup"><span data-stu-id="3df21-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="3df21-124">GROUP BY yan tümcesine en yaygın durumlarda çevirme artık destekler.</span><span class="sxs-lookup"><span data-stu-id="3df21-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="3df21-125">Bu örnek bir sorgu ile çeşitli toplama işlevleri hesaplamak için kullanılan GroupBy gösterir:</span><span class="sxs-lookup"><span data-stu-id="3df21-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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

<span data-ttu-id="3df21-126">Karşılık gelen SQL çeviri şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="3df21-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="3df21-127">Dengeli veri</span><span class="sxs-lookup"><span data-stu-id="3df21-127">Data Seeding</span></span>
<span data-ttu-id="3df21-128">Yeni sürümde bir veritabanını doldurmak için ilk veri sağlamak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3df21-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="3df21-129">EF6 içinde verileri dengeli modeli yapılandırmasının bir parçası olarak bir varlık türü için ilişkili benzemez.</span><span class="sxs-lookup"><span data-stu-id="3df21-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="3df21-130">Ardından EF çekirdek geçişler otomatik olarak ne ekleme, güncelleştirme veya silme işlemleri veritabanı modeli yeni bir sürüme yükseltirken uygulanması gerek hesaplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3df21-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="3df21-131">Örnek olarak, bu bir POST çekirdek verileri yapılandırmak için kullanabileceğiniz `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="3df21-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="3df21-132">Okuma [verileri dengeli üzerinde bölüm](xref:core/modeling/data-seeding) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3df21-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="3df21-133">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="3df21-133">Query types</span></span>
<span data-ttu-id="3df21-134">EF çekirdek modeli şimdi sorgu türleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="3df21-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="3df21-135">Varlık türlerinin aksine, sorgu türleri değil sahip üzerlerinde tanımlanmış anahtarları ve eklenen, silinemez veya güncelleştirilmiş (yani, salt okunur), ancak doğrudan sorgular tarafından döndürdü.</span><span class="sxs-lookup"><span data-stu-id="3df21-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="3df21-136">Sorgu türleri için kullanım senaryoları bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="3df21-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="3df21-137">Birincil anahtarlar olmadan görünümlerine eşleme</span><span class="sxs-lookup"><span data-stu-id="3df21-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="3df21-138">Birincil anahtarlar olmadan tablolara eşleme</span><span class="sxs-lookup"><span data-stu-id="3df21-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="3df21-139">Model içinde tanımlanan sorgulara eşleme</span><span class="sxs-lookup"><span data-stu-id="3df21-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="3df21-140">Dönüş türü olarak hizmet veren `FromSql()` sorguları</span><span class="sxs-lookup"><span data-stu-id="3df21-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="3df21-141">Okuma [sorgu türleri bölümüne](xref:core/modeling/query-types) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3df21-141">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="3df21-142">Türetilen türler için</span><span class="sxs-lookup"><span data-stu-id="3df21-142">Include for derived types</span></span>
<span data-ttu-id="3df21-143">Yalnızca tanımlanan Gezinti özelliklerini belirtmek için olası türetilmiş tür ifadeler için yazarken artık olacaktır `Include` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3df21-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="3df21-144">Kesin türü belirtilmiş sürümünü için `Include`, açık bir atama kullanarak destekliyoruz veya `as` işleci.</span><span class="sxs-lookup"><span data-stu-id="3df21-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="3df21-145">Ayrıca artık adları türetilmiş türler dize sürümünde tanımlı Gezinti özelliğinin başvuru destekliyoruz `Include`:</span><span class="sxs-lookup"><span data-stu-id="3df21-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="3df21-146">Okuma [INCLUDE bölüm türetilen türlerle](xref:core/querying/related-data#include-on-derived-types) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3df21-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="3df21-147">System.Transactions desteği</span><span class="sxs-lookup"><span data-stu-id="3df21-147">System.Transactions support</span></span>
<span data-ttu-id="3df21-148">System.Transactions TransactionScope gibi özellikler ile çalışma olanağını ekledik.</span><span class="sxs-lookup"><span data-stu-id="3df21-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="3df21-149">Bu hem .NET Framework hem de .NET Core destekleyen veritabanı sağlayıcıları kullanırken çalışır.</span><span class="sxs-lookup"><span data-stu-id="3df21-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="3df21-150">Okuma [System.Transactions bölüm](xref:core/saving/transactions#using-systemtransactions) Bu konu hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3df21-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="3df21-151">Daha iyi sütun ilk geçişte sıralaması</span><span class="sxs-lookup"><span data-stu-id="3df21-151">Better column ordering in initial migration</span></span>
<span data-ttu-id="3df21-152">Müşteri geri bildirimi doğrultusunda, sınıflar olarak bildirilen özelliklerde gibi tablolar için sütunları aynı sırada başlangıçta oluşturmak için geçişler güncelleştirdiniz.</span><span class="sxs-lookup"><span data-stu-id="3df21-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="3df21-153">İlk tablo oluşturulduktan sonra yeni üye eklediğinizde EF çekirdek sırası değiştirilemiyor unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3df21-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="3df21-154">Bağıntılı alt sorgulara en iyi duruma getirme</span><span class="sxs-lookup"><span data-stu-id="3df21-154">Optimization of correlated subqueries</span></span>
<span data-ttu-id="3df21-155">Biz "N + 1" yürütme önlemek için bizim sorgusu çevirisi iyileştirilmiştir projeksiyon Gezinti özelliğinde kullanımını müşteri adayları verileriyle ilişkili bir alt sorgu kök sorgudan verileri birleştirme için birçok yaygın senaryolar SQL sorguları.</span><span class="sxs-lookup"><span data-stu-id="3df21-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="3df21-156">En iyi duruma getirme form alt sorgu sonuçları arabelleğe alma ve katılımı yeni davranışı için sorguyu değiştirin gerektiren gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3df21-156">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="3df21-157">Örnek olarak, aşağıdaki sorguyu normalde bir sorguyu müşteriler artı (burada "N" döndürülen müşteri sayısını alır) çevrilir sorgular için siparişleri ayırın:</span><span class="sxs-lookup"><span data-stu-id="3df21-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="3df21-158">Ekleyerek `ToList()` doğru yerde arabelleğe alma iyileştirmeyi etkinleştirme siparişleri için uygun olduğunu gösteriyor:</span><span class="sxs-lookup"><span data-stu-id="3df21-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="3df21-159">Bu sorgu için yalnızca iki SQL sorguları çevrilir unutmayın: biri müşteriler ve siparişler bir sonraki.</span><span class="sxs-lookup"><span data-stu-id="3df21-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="3df21-160">[Ait] özniteliği</span><span class="sxs-lookup"><span data-stu-id="3df21-160">[Owned] attribute</span></span>

<span data-ttu-id="3df21-161">Şimdi yapılandırmak olası [varlık türlerine ait](xref:core/modeling/owned-entities) yalnızca türüyle yorumlama tarafından `[Owned]` ve sahibi varlık emin olmak için model eklenir:</span><span class="sxs-lookup"><span data-stu-id="3df21-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="3df21-162">Komut satırı aracı dotnet-ef .NET Core SDK'da bulunan</span><span class="sxs-lookup"><span data-stu-id="3df21-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="3df21-163">_Dotnet ef_ komutlardır şimdi .NET Core SDK parçası, bu nedenle, artık DotNetCliToolReference geçişler kullanın veya var olan bir veritabanından bir DbContext iskele kullanabilmek için projede kullanmak için gerekli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3df21-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="3df21-164">Bölümüne bakarak [araçlarını yükleme](xref:core/miscellaneous/cli/dotnet#installing-the-tools) farklı sürümlerini .NET Core SDK ve EF çekirdek için komut satırı araçları etkinleştirme hakkında daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="3df21-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="3df21-165">Microsoft.EntityFrameworkCore.Abstractions paketi</span><span class="sxs-lookup"><span data-stu-id="3df21-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="3df21-166">Yeni paket öznitelikleri ve projelerinizde bir bütün olarak EF çekirdeği üzerinde bir bağımlılık bırakmadan EF temel özellikleri açık için kullanabileceğiniz arabirimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="3df21-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="3df21-167">Örneğin, [Owned] özniteliği ve ILazyLoader arabirimi burada listelenir.</span><span class="sxs-lookup"><span data-stu-id="3df21-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="3df21-168">Durum değişikliği olayları</span><span class="sxs-lookup"><span data-stu-id="3df21-168">State change events</span></span>

<span data-ttu-id="3df21-169">Yeni `Tracked` ve `StateChanged` olaylarına `ChangeTracker` DbContext girme veya durumlarını değiştirerek varlıklara tepki verdiğini mantığı yazmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3df21-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="3df21-170">Ham SQL parametresi Çözümleyicisi</span><span class="sxs-lookup"><span data-stu-id="3df21-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="3df21-171">Yeni bir kod Çözümleyicisi EF çekirdek ile olmayabilecek kullanımları bizim SQL ham API'leri gibi algılar dahil `FromSql` veya `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="3df21-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="3df21-172">Örneğin</span><span class="sxs-lookup"><span data-stu-id="3df21-172">E.g.</span></span> <span data-ttu-id="3df21-173">Aşağıdaki sorgu için bir uyarı çünkü görürsünüz _minAge_ değil parametreli:</span><span class="sxs-lookup"><span data-stu-id="3df21-173">for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="3df21-174">Veritabanı sağlayıcısı uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="3df21-174">Database provider compatibility</span></span>

<span data-ttu-id="3df21-175">Güncelleştirilmiş veya EF çekirdek 2.1 ile birlikte çalışmak üzere en az test sağlayıcılarla EF çekirdek 2.1 kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="3df21-175">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="3df21-176">Her beklenmeyen bulursanız uyumsuzluk herhangi sorun'deki yeni özelliklerin veya bunlar üzerinde Geribildiriminiz varsa lütfen kullanarak rapor [bizim sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="3df21-176">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
