---
title: EF Core 5,0 ' deki yenilikler
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136251"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="e5cc7-102">EF Core 5,0 ' deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="e5cc7-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="e5cc7-103">EF Core 5,0 şu anda geliştirme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="e5cc7-104">Bu sayfa, her önizlemede sunulan ilginç değişikliklere genel bir bakış içerir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="e5cc7-105">Bu sayfa [EF Core 5,0 planını](plan.md)yinelemez.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-105">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="e5cc7-106">Plan, son yayını teslim etmeden önce dahil ettiğimiz her şey dahil olmak üzere EF Core 5,0 ' a yönelik genel temaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-106">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="e5cc7-107">Yayımlanmakta olan resmi belgelere buradan bağlantılar ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-107">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1"></a><span data-ttu-id="e5cc7-108">Önizleme 1</span><span class="sxs-lookup"><span data-stu-id="e5cc7-108">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="e5cc7-109">Basit günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="e5cc7-109">Simple logging</span></span>

<span data-ttu-id="e5cc7-110">Bu özellik EF6 içinde `Database.Log` benzer işlevler ekler.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-110">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="e5cc7-111">Yani, her türlü harici günlük çerçevesini yapılandırmaya gerek kalmadan EF Core günlükleri almanın basit bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-111">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="e5cc7-112">İlk belgeler, [5 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-112">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="e5cc7-113">Ek belgeler [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-113">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="e5cc7-114">Oluşturulan SQL almanın basit yolu</span><span class="sxs-lookup"><span data-stu-id="e5cc7-114">Simple way to get generated SQL</span></span>

<span data-ttu-id="e5cc7-115">EF Core 5,0, bir LINQ sorgusu yürütürken EF Core üretebileceği SQL döndürecek `ToQueryString` genişletme yöntemini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-115">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="e5cc7-116">Ön belgeler, [9 ocak 2020 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)dahildir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-116">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="e5cc7-117">Ek belgeler [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-117">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="e5cc7-118">Bir varlığın C# anahtara sahip olmadığını göstermek için bir öznitelik kullanın</span><span class="sxs-lookup"><span data-stu-id="e5cc7-118">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="e5cc7-119">Bir varlık türü artık yeni `KeylessAttribute`hiçbir anahtara sahip olmadığı için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-119">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="e5cc7-120">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-120">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="e5cc7-121">Belgeler [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-121">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="e5cc7-122">Bağlantı veya bağlantı dizesi, başlatılmış DbContext üzerinde değiştirilebilir</span><span class="sxs-lookup"><span data-stu-id="e5cc7-122">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="e5cc7-123">Artık herhangi bir bağlantı veya bağlantı dizesi olmadan bir DbContext örneği oluşturulması daha kolay.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-123">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="e5cc7-124">Ayrıca, bağlantı veya bağlantı dizesi artık bağlam örneği üzerinde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-124">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="e5cc7-125">Bu, aynı bağlam örneğinin farklı veritabanlarına dinamik olarak bağlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-125">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="e5cc7-126">Belgeler [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-126">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="e5cc7-127">Değişiklik izleme proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="e5cc7-127">Change-tracking proxies</span></span>

<span data-ttu-id="e5cc7-128">EF Core, artık otomatik olarak [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) ve [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)uygulayan çalışma zamanı proxy 'leri oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-128">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="e5cc7-129">Böylece, varlık özelliklerindeki değer değişiklikleri doğrudan EF Core olarak raporlanarak değişiklik taraması ihtiyacını önler.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-129">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="e5cc7-130">Ancak, proxy 'ler kendi kısıtlama kümesiyle gelir, bu nedenle herkes için değildir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-130">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="e5cc7-131">Belgeler [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-131">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="e5cc7-132">Gelişmiş hata ayıklama görünümleri</span><span class="sxs-lookup"><span data-stu-id="e5cc7-132">Enhanced debug views</span></span>

<span data-ttu-id="e5cc7-133">Hata ayıklama görünümlerinde EF Core iç yapıları göz atmak kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-133">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="e5cc7-134">Modelin bir hata ayıklama görünümü bir süre önce uygulandı.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-134">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="e5cc7-135">EF Core 5,0 için model görünümü ' ne daha kolay okunabilir ve durum yöneticisinde izlenen varlıklar için yeni bir hata ayıklama görünümü ekledik.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-135">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="e5cc7-136">Ön belgeler, [12 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)dahildir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-136">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="e5cc7-137">Ek belgeler [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-137">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="e5cc7-138">Veritabanı null semantiğini gelişmiş işleme</span><span class="sxs-lookup"><span data-stu-id="e5cc7-138">Improved handling of database null semantics</span></span>

<span data-ttu-id="e5cc7-139">İlişkisel veritabanları genellikle NULL değerini bilinmeyen bir değer olarak değerlendirir ve bu nedenle başka bir NULL değere eşit değildir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-139">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="e5cc7-140">C#, diğer taraftan, null değeri, diğer herhangi bir null ile eşit olan tanımlı bir değer olarak davranır.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-140">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="e5cc7-141">EF Core, varsayılan olarak, null semantiğini kullanacak C# şekilde sorguları çevirir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-141">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="e5cc7-142">EF Core 5,0, bu çevirilerin verimliliğini önemli ölçüde geliştirir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-142">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="e5cc7-143">Belgeler [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-143">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="e5cc7-144">Dizin Oluşturucu özellikleri</span><span class="sxs-lookup"><span data-stu-id="e5cc7-144">Indexer properties</span></span>

<span data-ttu-id="e5cc7-145">EF Core 5,0, C# Dizin Oluşturucu özelliklerinin eşlenmesini destekler.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-145">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="e5cc7-146">Bu, varlıkların, sütunların pakette adlandırılmış özelliklerle eşlendikleri özellik paketleri olarak davranmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-146">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="e5cc7-147">Belgeler [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-147">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="e5cc7-148">Enum eşlemeleri için denetim kısıtlamaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5cc7-148">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="e5cc7-149">EF Core 5,0 geçişleri, artık enum özelliği eşlemeleri için DENETIM kısıtlamaları oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-149">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="e5cc7-150">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-150">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="e5cc7-151">Belgeler [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-151">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="e5cc7-152">Isilikisel</span><span class="sxs-lookup"><span data-stu-id="e5cc7-152">IsRelational</span></span>

<span data-ttu-id="e5cc7-153">Mevcut `IsSqlServer`, `IsSqlite`ve `IsInMemory`ek olarak yeni bir `IsRelational` yöntemi eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-153">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="e5cc7-154">Bu, DbContext 'in herhangi bir ilişkisel veritabanı sağlayıcısı kullanıp kullankullanılmadığını test etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-154">This can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="e5cc7-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-155">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="e5cc7-156">Belgeler [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-156">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="e5cc7-157">Cosmos, ETags ile iyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="e5cc7-157">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="e5cc7-158">Azure Cosmos DB veritabanı sağlayıcısı artık ETags kullanarak iyimser eşzamanlılığı desteklemektedir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-158">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="e5cc7-159">ETag yapılandırmak için Onmodelyaratırken model oluşturucuyu kullanın:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-159">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="e5cc7-160">Ardından SaveChanges, yeniden denemeler uygulamak için [işlenebilen](https://docs.microsoft.com/ef/core/saving/concurrency) eşzamanlılık çakışmasıyla ilgili bir `DbUpdateConcurrencyException` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-160">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>


<span data-ttu-id="e5cc7-161">Belgeler [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-161">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="e5cc7-162">Daha fazla TarihSaat yapıları için sorgu çevirileri</span><span class="sxs-lookup"><span data-stu-id="e5cc7-162">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="e5cc7-163">Yeni tarih saat oluşturma içeren sorgular artık çevrilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-163">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="e5cc7-164">Ayrıca, aşağıdaki SQL Server işlevleri artık eşleştirilir:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-164">In addition, the following SQL Server functions are now mapped:</span></span>
* <span data-ttu-id="e5cc7-165">Tarih dağılımı haftası</span><span class="sxs-lookup"><span data-stu-id="e5cc7-165">DateDiffWeek</span></span>
* <span data-ttu-id="e5cc7-166">Datefrompsanat</span><span class="sxs-lookup"><span data-stu-id="e5cc7-166">DateFromParts</span></span>

<span data-ttu-id="e5cc7-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-167">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="e5cc7-168">Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-168">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="e5cc7-169">Daha fazla bayt dizisi yapıları için sorgu çevirileri</span><span class="sxs-lookup"><span data-stu-id="e5cc7-169">Query translations for more byte array constructs</span></span>

<span data-ttu-id="e5cc7-170">Byte [] özellikleri, Contains, length, Sequenceeşittir, vb. kullanan sorgular artık SQL 'e çevrilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-170">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="e5cc7-171">İlk belgeler, [5 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-171">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="e5cc7-172">Ek belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-172">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="e5cc7-173">Ters için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="e5cc7-173">Query translation for Reverse</span></span>

<span data-ttu-id="e5cc7-174">`Reverse` kullanan sorgular artık çevrilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-174">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="e5cc7-175">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-175">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="e5cc7-176">Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-176">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="e5cc7-177">Bit düzeyinde işleçler için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="e5cc7-177">Query translation for bitwise operators</span></span>

<span data-ttu-id="e5cc7-178">Bit düzeyinde işleçler kullanan sorgular artık daha fazla durumda çevrilir:</span><span class="sxs-lookup"><span data-stu-id="e5cc7-178">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="e5cc7-179">Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-179">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="e5cc7-180">Cosmos üzerinde dizeler için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="e5cc7-180">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="e5cc7-181">Dize yöntemlerini kullanan sorgular şunlardır, StartsWith ve EndsWith artık Azure Cosmos DB sağlayıcısı kullanılırken çevrilir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-181">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="e5cc7-182">Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="e5cc7-182">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
