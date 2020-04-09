---
title: EF Core 2.1'deki yenilikler - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417484"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="329f1-102">EF Core 2.1'deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="329f1-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="329f1-103">Çok sayıda hata düzeltmeleri ve küçük fonksiyonel ve performans geliştirmeleri yanı sıra, EF Core 2.1 bazı zorlayıcı yeni özellikler içerir:</span><span class="sxs-lookup"><span data-stu-id="329f1-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="329f1-104">Tembel yükleme</span><span class="sxs-lookup"><span data-stu-id="329f1-104">Lazy loading</span></span>

<span data-ttu-id="329f1-105">EF Core artık, navigasyon özelliklerini isteğe bağlı olarak yükleyebilen varlık sınıflarını yazar herkes için gerekli yapı taşlarını içerir.</span><span class="sxs-lookup"><span data-stu-id="329f1-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="329f1-106">Ayrıca, en az modifiye edilmiş varlık sınıflarına (örneğin, sanal gezinme özelliklerine sahip sınıflar) dayalı tembel yükleme proxy sınıfları üretmek için bu yapı taşlarından yararlanan microsoft.entityframeworkcore.Proxies( microsoft.entityframeworkcore.Proxies) olarak da yeni bir paket oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="329f1-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="329f1-107">Bu konu hakkında daha fazla bilgi için [tembel yükleme bölümünü](xref:core/querying/related-data#lazy-loading) okuyun.</span><span class="sxs-lookup"><span data-stu-id="329f1-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="329f1-108">Varlık yapıcılarında parametreler</span><span class="sxs-lookup"><span data-stu-id="329f1-108">Parameters in entity constructors</span></span>

<span data-ttu-id="329f1-109">Tembel yükleme için gerekli yapı taşlarından biri olarak, kendi yapıcıları parametreleri almak varlıkların oluşturulmasını sağladı.</span><span class="sxs-lookup"><span data-stu-id="329f1-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="329f1-110">Özellik değerlerini, tembel yükleme temsilcilerini ve hizmetleri enjekte etmek için parametreleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="329f1-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="329f1-111">Bu konu hakkında daha fazla bilgi için [parametreleri ile varlık oluşturucu bölümü](xref:core/modeling/constructors) okuyun.</span><span class="sxs-lookup"><span data-stu-id="329f1-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="329f1-112">Değer dönüşümleri</span><span class="sxs-lookup"><span data-stu-id="329f1-112">Value conversions</span></span>

<span data-ttu-id="329f1-113">Şimdiye kadar, EF Core yalnızca temel veritabanı sağlayıcısı tarafından desteklenen türlerin özelliklerini eşlenebiliyordu.</span><span class="sxs-lookup"><span data-stu-id="329f1-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="329f1-114">Değerler, sütunlar ve özellikler arasında herhangi bir dönüşüm olmaksızın ileri geri kopyalandı.</span><span class="sxs-lookup"><span data-stu-id="329f1-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="329f1-115">EF Core 2.1 ile başlayarak, sütunlardan elde edilen değerleri özelliklere uygulanmadan önce dönüştürmek için değer dönüştürmeleri uygulanabilir ve bunun tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="329f1-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="329f1-116">Gerektiğinde kuralla uygulanabilen bir dizi dönüşüme ve sütunlar ve özellikler arasında özel dönüşümlerin kaydedilmesine olanak tanıyan açık bir yapılandırma API'miz vardır.</span><span class="sxs-lookup"><span data-stu-id="329f1-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="329f1-117">Bu özelliğin bazı uygulamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="329f1-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="329f1-118">Enumları dizeler olarak depolama</span><span class="sxs-lookup"><span data-stu-id="329f1-118">Storing enums as strings</span></span>
- <span data-ttu-id="329f1-119">SQL Server ile imzasız tümsavarları eşleme</span><span class="sxs-lookup"><span data-stu-id="329f1-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="329f1-120">Özellik değerlerinin otomatik şifrelemesi ve şifreçözmesi</span><span class="sxs-lookup"><span data-stu-id="329f1-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="329f1-121">Bu konu hakkında daha fazla bilgi için [değer dönüşümleri bölümünü](xref:core/modeling/value-conversions) okuyun.</span><span class="sxs-lookup"><span data-stu-id="329f1-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="329f1-122">LINQ GroupBy çevirisi</span><span class="sxs-lookup"><span data-stu-id="329f1-122">LINQ GroupBy translation</span></span>

<span data-ttu-id="329f1-123">Sürüm 2.1'den önce, EF Core'da GroupBy LINQ işleci her zaman bellekte değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="329f1-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="329f1-124">Artık çoğu durumda SQL GROUP BY yan tümcesine çevrilmesini destekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="329f1-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="329f1-125">Bu örnek, çeşitli toplu işlevleri hesaplamak için groupby ile kullanılan bir sorgu gösterir:</span><span class="sxs-lookup"><span data-stu-id="329f1-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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

<span data-ttu-id="329f1-126">İlgili SQL çevirisi aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="329f1-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="329f1-127">Veri Çekirdeği Oluşturma</span><span class="sxs-lookup"><span data-stu-id="329f1-127">Data Seeding</span></span>

<span data-ttu-id="329f1-128">Yeni sürümle birlikte, bir veritabanıdoldurmak için ilk verileri sağlamak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="329f1-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="329f1-129">EF6'dan farklı olarak, tohumlama verileri model yapılandırmasının bir parçası olarak bir varlık türüyle ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="329f1-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="329f1-130">Daha sonra EF Core geçişleri, veritabanını modelin yeni bir sürümüne yükseltirken hangi ekleme, güncelleştirme veya silme işlemlerinin uygulanması gerektiğini otomatik olarak hesaplayabilir.</span><span class="sxs-lookup"><span data-stu-id="329f1-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="329f1-131">Örnek olarak, bunu bir Gönderi için tohum verilerini `OnModelCreating`yapılandırmak için kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="329f1-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="329f1-132">Bu konu hakkında daha fazla bilgi için [veri tohumlama bölümünü](xref:core/modeling/data-seeding) okuyun.</span><span class="sxs-lookup"><span data-stu-id="329f1-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="329f1-133">Sorgu türleri</span><span class="sxs-lookup"><span data-stu-id="329f1-133">Query types</span></span>

<span data-ttu-id="329f1-134">BIR EF Core modeli artık sorgu türlerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="329f1-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="329f1-135">Varlık türlerinin aksine, sorgu türlerinde anahtarlar tanımlanmaz ve eklenemez, silinemez veya güncelleştirilemez (diğer bir deyişle, bunlar salt okunur) ancak doğrudan sorgularla döndürülebilir.</span><span class="sxs-lookup"><span data-stu-id="329f1-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="329f1-136">Sorgu türleri için kullanım senaryolarından bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="329f1-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="329f1-137">Birincil anahtarlar olmadan görünümlere eşleme</span><span class="sxs-lookup"><span data-stu-id="329f1-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="329f1-138">Birincil anahtarlar olmadan tablolara eşleme</span><span class="sxs-lookup"><span data-stu-id="329f1-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="329f1-139">Modelde tanımlanan sorgularla eşleme</span><span class="sxs-lookup"><span data-stu-id="329f1-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="329f1-140">Sorgular için `FromSql()` iade türü olarak hizmet</span><span class="sxs-lookup"><span data-stu-id="329f1-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="329f1-141">Bu konu hakkında daha fazla bilgi için [sorgu türleri hakkındaki bölümü](xref:core/modeling/keyless-entity-types) okuyun.</span><span class="sxs-lookup"><span data-stu-id="329f1-141">Read the [section on query types](xref:core/modeling/keyless-entity-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="329f1-142">Türemiş türler için ekleme</span><span class="sxs-lookup"><span data-stu-id="329f1-142">Include for derived types</span></span>

<span data-ttu-id="329f1-143">Yöntem için `Include` ifadeler yazarken yalnızca türetilmiş türlerde tanımlanan gezinti özelliklerini belirtmek artık mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="329f1-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="329f1-144">Güçlü bir şekilde yazılan `Include`sürümü için, açık bir döküm `as` veya işleç kullanarak destek.</span><span class="sxs-lookup"><span data-stu-id="329f1-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="329f1-145">Ayrıca şimdi dize sürümünde türemiş türleri üzerinde tanımlanan gezinti `Include`özelliği adları başvurudestek:</span><span class="sxs-lookup"><span data-stu-id="329f1-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="329f1-146">Bu konu hakkında daha fazla bilgi için [türemiş türleri ile Ekle bölümünü](xref:core/querying/related-data#include-on-derived-types) okuyun.</span><span class="sxs-lookup"><span data-stu-id="329f1-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="329f1-147">System.Transactions desteği</span><span class="sxs-lookup"><span data-stu-id="329f1-147">System.Transactions support</span></span>

<span data-ttu-id="329f1-148">TransactionScope gibi System.Transactions özellikleriyle çalışma özelliğini ekledik.</span><span class="sxs-lookup"><span data-stu-id="329f1-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="329f1-149">Bu, destekleyen veritabanı sağlayıcılarını kullanırken hem .NET Framework hem de .NET Core üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="329f1-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="329f1-150">Bu konu hakkında daha fazla bilgi için [System.Transactions bölümünü](xref:core/saving/transactions#using-systemtransactions) okuyun.</span><span class="sxs-lookup"><span data-stu-id="329f1-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="329f1-151">İlk geçişte daha iyi sütun sırası</span><span class="sxs-lookup"><span data-stu-id="329f1-151">Better column ordering in initial migration</span></span>

<span data-ttu-id="329f1-152">Müşteri geri bildirimlerine dayanarak, geçişleri, sınıflarda özellikler beyan edildiği sırada tablolar için sütunoluşturmak üzere güncelledik.</span><span class="sxs-lookup"><span data-stu-id="329f1-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="329f1-153">Ef Core'un ilk tablo oluşturulduktan sonra yeni üyeler eklendiğinde düzeni değiştiremeyeceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="329f1-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="329f1-154">İlişkili alt sorguların optimizasyonu</span><span class="sxs-lookup"><span data-stu-id="329f1-154">Optimization of correlated subqueries</span></span>

<span data-ttu-id="329f1-155">Projeksiyonda bir gezinti özelliğinin kullanımının, ilişkili bir alt sorgudaki verilerle kök sorgusundan veri birletmeye yol açtığı birçok yaygın senaryoda "N + 1" SQL sorgularının yürütülmesini önlemek için sorgu çevirimizi geliştirdik.</span><span class="sxs-lookup"><span data-stu-id="329f1-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="329f1-156">En iyi duruma getirilmesi alt sorgusonuçları arabelleğe gerektirir ve biz yeni davranışı devre dışı bırakmak için sorgu değiştirmenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="329f1-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="329f1-157">Örnek olarak, aşağıdaki sorgu normalde Müşteriler için bir sorguya, artı N ("N" döndürülen müşteri sayısı) Siparişler için ayrı sorgular olarak çevrilmiş olur:</span><span class="sxs-lookup"><span data-stu-id="329f1-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="329f1-158">Doğru yere `ToList()` ekleyerek, arabelleğe almanın Siparişler için uygun olduğunu ve bu sayede optimizasyonu sağlarsınız:</span><span class="sxs-lookup"><span data-stu-id="329f1-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="329f1-159">Bu sorgunun yalnızca iki SQL sorgusuna çevrileceğini unutmayın: Biri Müşteriler için, diğeri siparişler için.</span><span class="sxs-lookup"><span data-stu-id="329f1-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="329f1-160">[Sahipolunan] öznitelik</span><span class="sxs-lookup"><span data-stu-id="329f1-160">[Owned] attribute</span></span>

<span data-ttu-id="329f1-161">Artık yalnızca türü açıklamaek `[Owned]` ve sonra sahibi varlığın modele eklenmesini sağlayarak sahip [olunan varlık türlerini](xref:core/modeling/owned-entities) yapılandırmak mümkündür:</span><span class="sxs-lookup"><span data-stu-id="329f1-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="329f1-162">.NET Core SDK'da yer alan komut satırı aracı dotnet-ef</span><span class="sxs-lookup"><span data-stu-id="329f1-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="329f1-163">_Dotnet-ef_ komutları artık .NET Core SDK'nın bir parçasıdır, bu nedenle artık geçişleri kullanabilmek veya varolan bir veritabanından Bir DbContext'ı iskelelemek için projede DotNetCliToolReference'ı kullanmak gerekli olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="329f1-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="329f1-164">.NET Core SDK ve EF Core'un farklı sürümleri için komut satırı araçlarının nasıl etkinleştirilen hakkında daha fazla bilgi için [araçları yükleme](xref:core/miscellaneous/cli/dotnet#installing-the-tools) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="329f1-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="329f1-165">Microsoft.EntityFrameworkCore.Abstractions paketi</span><span class="sxs-lookup"><span data-stu-id="329f1-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>

<span data-ttu-id="329f1-166">Yeni paket, EF Core'a bir bütün olarak bağımlılık yapmadan EF Core özelliklerini aydınlatmak için projelerinizde kullanabileceğiniz öznitelikler ve arabirimler içerir.</span><span class="sxs-lookup"><span data-stu-id="329f1-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="329f1-167">Örneğin, [Owned] özniteliği ve ILazyLoader arabirimi burada bulunur.</span><span class="sxs-lookup"><span data-stu-id="329f1-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="329f1-168">Durum değişikliği olayları</span><span class="sxs-lookup"><span data-stu-id="329f1-168">State change events</span></span>

<span data-ttu-id="329f1-169">Yeni `Tracked` `StateChanged` Ve `ChangeTracker` olaylar DbContext giren veya durum larını değiştiren varlıklar tepki mantık yazmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="329f1-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="329f1-170">Ham SQL parametre çözümleyicisi</span><span class="sxs-lookup"><span data-stu-id="329f1-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="329f1-171">EF Core'a yeni bir kod çözümleyicisi dahildir ve raw-SQL API'lerimizin `FromSql` `ExecuteSqlCommand`güvenli olmayan kullanımlarını algılar.</span><span class="sxs-lookup"><span data-stu-id="329f1-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="329f1-172">Örneğin, aşağıdaki sorgu için, _minAge_ parametreli olmadığından bir uyarı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="329f1-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="329f1-173">Veritabanı sağlayıcısı uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="329f1-173">Database provider compatibility</span></span>

<span data-ttu-id="329f1-174">EF Core 2.1'i güncelleştirilmiş veya en azından EF Core 2.1 ile çalışmak üzere test edilmiş sağlayıcılarla kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="329f1-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="329f1-175">Yeni özelliklerde beklenmeyen bir uyumsuzluk veya herhangi bir sorun bulursanız veya bunlar hakkında geri bildirimde bulunduysanız, lütfen [sorun izleyicimizi](https://github.com/aspnet/EntityFrameworkCore/issues/new)kullanarak bildirin.</span><span class="sxs-lookup"><span data-stu-id="329f1-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
