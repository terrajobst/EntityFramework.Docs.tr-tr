---
title: EF Core 2,1 ' deki yenilikler-EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 5f97015f0228387574e3a19fb20cae1bdb403410
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149174"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="d6ee4-102">EF Core 2,1 ' deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="d6ee4-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="d6ee4-103">Çok sayıda hata düzeltmesi ve küçük işlevsellik ve performans geliştirmelerinin yanı sıra EF Core 2,1 bazı etkileyici yeni özellikler içerir:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="d6ee4-104">geç yükleme</span><span class="sxs-lookup"><span data-stu-id="d6ee4-104">Lazy loading</span></span>
<span data-ttu-id="d6ee4-105">EF Core artık, herhangi bir kişinin gezinti özelliklerini isteğe bağlı olarak yükleyebilen varlık sınıfları yazmak için gerekli yapı taşlarını içerir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="d6ee4-106">Ayrıca, en düşük düzeyde değiştirilen varlık sınıflarına (örneğin, sanal gezinti özelliklerine sahip sınıflar) göre yavaş yükleme proxy sınıfları oluşturmak için bu yapı taşlarından yararlanan yeni bir Microsoft. EntityFrameworkCore. proxy paketi oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="d6ee4-107">Bu konu hakkında daha fazla bilgi için, [yavaş yükleme bölümündeki bölümü](xref:core/querying/related-data#lazy-loading) okuyun.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="d6ee4-108">Varlık oluşturuculardaki parametreler</span><span class="sxs-lookup"><span data-stu-id="d6ee4-108">Parameters in entity constructors</span></span>
<span data-ttu-id="d6ee4-109">Yavaş yükleme için gereken yapı taşlarından biri olarak, kurucularında parametre alan varlıkların oluşturulmasını etkinleştirdik.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="d6ee4-110">Özellik değerlerini eklemek, yavaş yükleme temsilcileri ve hizmetleri eklemek için parametreleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="d6ee4-111">Bu konu hakkında daha fazla bilgi için, [parametrelere sahip varlık Oluşturucu bölümünü](xref:core/modeling/constructors) okuyun.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="d6ee4-112">Değer dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="d6ee4-112">Value conversions</span></span>
<span data-ttu-id="d6ee4-113">Artık EF Core, yalnızca temel alınan veritabanı sağlayıcısı tarafından desteklenen türlerin özelliklerini eşleyebilir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="d6ee4-114">Değerler, herhangi bir dönüştürme olmadan sütunlar ve özellikler arasında geri ve ileri kopyalandı.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="d6ee4-115">EF Core 2,1 ' den başlayarak, değerlere uygulanmadan önce sütunlardan elde edilen değerleri dönüştürmek için değer dönüştürmeleri uygulanabilir ve tam tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="d6ee4-116">Kural ve özellikler arasında özel dönüştürmeleri kaydetmeye izin veren bir açık yapılandırma API 'SI de olmak üzere kurala göre uygulanabilen birkaç dönüştürmemiz vardır.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="d6ee4-117">Bu özelliğin bazı uygulamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="d6ee4-118">Numaralandırmaların dizeler olarak depolanması</span><span class="sxs-lookup"><span data-stu-id="d6ee4-118">Storing enums as strings</span></span>
- <span data-ttu-id="d6ee4-119">İşaretsiz tamsayılar SQL Server ile eşleme</span><span class="sxs-lookup"><span data-stu-id="d6ee4-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="d6ee4-120">Özellik değerlerinin otomatik şifrelenmesi ve şifresinin çözülmesi</span><span class="sxs-lookup"><span data-stu-id="d6ee4-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="d6ee4-121">Bu konu hakkında daha fazla bilgi için [değer dönüştürmeleriyle ilgili bölümü](xref:core/modeling/value-conversions) okuyun.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="d6ee4-122">LINQ GroupBy çevirisi</span><span class="sxs-lookup"><span data-stu-id="d6ee4-122">LINQ GroupBy translation</span></span>
<span data-ttu-id="d6ee4-123">Sürüm 2,1 ' den önce, EF Core GroupBy LINQ operatörü her zaman bellekte değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="d6ee4-124">Artık en yaygın durumlarda bunu SQL GROUP BY yan tümcesine çevirmeyi destekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="d6ee4-125">Bu örnek, çeşitli toplama işlevlerini hesaplamak için kullanılan GroupBy ile bir sorgu gösterir:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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
          Avg = g.Average(o => o.Amount)
        });
```

<span data-ttu-id="d6ee4-126">Karşılık gelen SQL çevirisi şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="d6ee4-127">Veri dengeli dağıtımı</span><span class="sxs-lookup"><span data-stu-id="d6ee4-127">Data Seeding</span></span>
<span data-ttu-id="d6ee4-128">Yeni sürüm ile bir veritabanını doldurmak için ilk verileri sağlamak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="d6ee4-129">EF6 'in aksine, dengeli dağıtım verileri model yapılandırmasının bir parçası olarak bir varlık türü ile ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="d6ee4-130">Daha sonra EF Core geçişler, veritabanının yeni bir sürümüne yükseltilirken ekleme, güncelleştirme veya silme işlemlerinin uygulanması gerektiğini otomatik olarak hesaplar.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="d6ee4-131">Örnek olarak bunu, içinde `OnModelCreating`bir gönderi için çekirdek verileri yapılandırmak üzere kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="d6ee4-132">Bu konu hakkında daha fazla bilgi için [veri dengeli dağıtım bölümünü](xref:core/modeling/data-seeding) okuyun.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="d6ee4-133">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="d6ee4-133">Query types</span></span>
<span data-ttu-id="d6ee4-134">EF Core modeli artık sorgu türlerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="d6ee4-135">Varlık türlerinden farklı olarak, sorgu türlerinde tanımlanmış anahtarlar yoktur ve eklenemez, silinemez veya güncelleştirilemez (yani, salt okunurdur), ancak sorgular tarafından doğrudan döndürülebilecek.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="d6ee4-136">Sorgu türleri için kullanım senaryolarından bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="d6ee4-137">Birincil anahtarlar olmadan görünümlere eşleme</span><span class="sxs-lookup"><span data-stu-id="d6ee4-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="d6ee4-138">Birincil anahtarlar olmadan tablolarla eşleme</span><span class="sxs-lookup"><span data-stu-id="d6ee4-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="d6ee4-139">Modelde tanımlanan sorgularla eşleme</span><span class="sxs-lookup"><span data-stu-id="d6ee4-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="d6ee4-140">Sorgular için `FromSql()` dönüş türü olarak sunma</span><span class="sxs-lookup"><span data-stu-id="d6ee4-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="d6ee4-141">Bu konu hakkında daha fazla bilgi için [sorgu türleri bölümünü](xref:core/modeling/keyless-entity-types) okuyun.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-141">Read the [section on query types](xref:core/modeling/keyless-entity-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="d6ee4-142">Türetilmiş türler için bulundur</span><span class="sxs-lookup"><span data-stu-id="d6ee4-142">Include for derived types</span></span>
<span data-ttu-id="d6ee4-143">Artık `Include` Yöntem için ifadeler yazılırken türetilmiş türlerde tanımlanmış olan gezinti özelliklerini belirtmek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="d6ee4-144">Türü kesin belirlenmiş olan `Include`sürümü için, açık bir dönüştürme `as` veya işleç kullanmayı destekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="d6ee4-145">Ayrıca, öğesinin `Include`dize sürümündeki türetilmiş türlerde tanımlanan gezinti özelliği adlarına başvurmayı de destekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="d6ee4-146">Bu konu hakkında daha fazla bilgi için [türetilmiş türlerle birlikte ekle bölümündeki bölümü](xref:core/querying/related-data#include-on-derived-types) okuyun.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="d6ee4-147">System. Transactions desteği</span><span class="sxs-lookup"><span data-stu-id="d6ee4-147">System.Transactions support</span></span>
<span data-ttu-id="d6ee4-148">TransactionScope gibi System. Transactions özellikleriyle çalışma yeteneği ekledik.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="d6ee4-149">Bu, bunu destekleyen veritabanı sağlayıcıları kullanılırken hem .NET Framework hem de .NET Core üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="d6ee4-150">Bu konu hakkında daha fazla bilgi için [System. Transactions bölümündeki bölümü](xref:core/saving/transactions#using-systemtransactions) okuyun.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="d6ee4-151">İlk geçişte daha iyi sütun sıralaması</span><span class="sxs-lookup"><span data-stu-id="d6ee4-151">Better column ordering in initial migration</span></span>
<span data-ttu-id="d6ee4-152">Müşteri geri bildirimlerine bağlı olarak, Özellikler sınıflarda bildirildiği sırada tablolar için ilk olarak sütunlar oluşturmak üzere geçişleri güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="d6ee4-153">İlk tablo oluşturulduktan sonra yeni üyeler eklendiğinde EF Core sırayı değiştiremediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="d6ee4-154">Bağıntılı alt sorgular iyileştirmesi</span><span class="sxs-lookup"><span data-stu-id="d6ee4-154">Optimization of correlated subqueries</span></span>
<span data-ttu-id="d6ee4-155">"N + 1" SQL sorgularını, projeksiyondaki bir gezinti özelliğinin kullanımının, kök sorgudan bağıntılı bir alt sorgudan verilerle katılmasını sağlayan çok sayıda yaygın senaryoda yürütmemek için geliştirdik.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="d6ee4-156">İyileştirme, sorgunun sonuçlarını arabelleğe almayı gerektirir ve yeni davranışı kabul etmek için sorguyu değiştirmenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="d6ee4-157">Örnek olarak, aşağıdaki sorgu normal olarak müşteriler için tek bir sorguya çevrilir, ek olarak N ("N" döndürülen müşterilerin sayısı) siparişlerin ayrı sorgularını alır:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="d6ee4-158">Doğru yere `ToList()` dahil ederek, arabelleğe alma işleminin, en iyi duruma getirmeyi sağlayan siparişlere uygun olduğunu belirtirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="d6ee4-159">Bu sorgunun yalnızca iki SQL sorgusuna çevrildiğine unutmayın: Biri müşteriler ve bir sonraki siparişler için.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="d6ee4-160">[Sahipli] özniteliği</span><span class="sxs-lookup"><span data-stu-id="d6ee4-160">[Owned] attribute</span></span>

<span data-ttu-id="d6ee4-161">Artık türe `[Owned]` açıklama ekleyerek ve sonra sahip varlığın modele eklendiğinden emin olarak sahip olan [varlık türlerini](xref:core/modeling/owned-entities) yapılandırmak mümkündür:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="d6ee4-162">Komut satırı aracı DotNet-.NET Core SDK içinde yer alan EF</span><span class="sxs-lookup"><span data-stu-id="d6ee4-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="d6ee4-163">_DotNet-EF_ komutları artık .NET Core SDK bir parçasıdır. bu nedenle, geçişleri kullanabilmeniz veya var olan bir veritabanından bir DbContext 'i kullanmak için artık projede Dotnetclientoolreference kullanılması gerekli olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="d6ee4-164">Farklı .NET Core SDK ve EF Core sürümleri için komut satırı araçlarının nasıl etkinleştirileceği hakkında daha fazla bilgi için [araçları yükleme](xref:core/miscellaneous/cli/dotnet#installing-the-tools) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="d6ee4-165">Microsoft. EntityFrameworkCore. soyutlamalar paketi</span><span class="sxs-lookup"><span data-stu-id="d6ee4-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="d6ee4-166">Yeni paket, bir bütün olarak EF Core bağımlılığı almadan EF Core özellikleri açmak için projelerinizde kullanabileceğiniz öznitelikleri ve arabirimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="d6ee4-167">Örneğin, [sahip] özniteliği ve ılazyloader arabirimi burada bulunur.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="d6ee4-168">Durum değişikliği olayları</span><span class="sxs-lookup"><span data-stu-id="d6ee4-168">State change events</span></span>

<span data-ttu-id="d6ee4-169">' Deki `StateChanged` `Tracked` Yeni`ChangeTracker` ve olaylar, DbContext ' i girerek veya durumlarını değiştirerek varlıklara yeniden davranan mantığı yazmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="d6ee4-170">Ham SQL parametre Çözümleyicisi</span><span class="sxs-lookup"><span data-stu-id="d6ee4-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="d6ee4-171">Yeni bir kod Çözümleyicisi, veya `FromSql` `ExecuteSqlCommand`gibi ham SQL API 'lerimizin güvensiz kullanımlarını algılayan EF Core dahildir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="d6ee4-172">Örneğin, aşağıdaki sorgu için bir uyarı görürsünüz çünkü _Minage_ parametreli değil:</span><span class="sxs-lookup"><span data-stu-id="d6ee4-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="d6ee4-173">Veritabanı sağlayıcısı uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="d6ee4-173">Database provider compatibility</span></span>

<span data-ttu-id="d6ee4-174">EF Core 2,1 ' i güncelleştirilmiş veya en az test edilmiş EF Core 2,1 ile çalışacak şekilde kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="d6ee4-175">Yeni özelliklerde beklenmeyen bir uyumsuzluk veya herhangi bir sorun bulursanız veya bunlarla ilgili geri bildiriminiz varsa, lütfen [sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues/new)'ni kullanarak bildirin.</span><span class="sxs-lookup"><span data-stu-id="d6ee4-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
