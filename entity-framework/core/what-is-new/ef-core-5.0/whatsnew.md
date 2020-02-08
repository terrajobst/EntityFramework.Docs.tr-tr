---
title: EF Core 5,0 ' deki yenilikler
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052034"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="cafad-102">EF Core 5,0 ' deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="cafad-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="cafad-103">EF Core 5,0 şu anda geliştirme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="cafad-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="cafad-104">Bu sayfa, her önizlemede sunulan ilginç değişikliklere genel bir bakış içerir.</span><span class="sxs-lookup"><span data-stu-id="cafad-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="cafad-105">EF Core 5,0 ' nin ilk önizlemesi, içinde, 2020 ' in ilk çeyreğinde kesin olarak beklenmez.</span><span class="sxs-lookup"><span data-stu-id="cafad-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="cafad-106">Bu sayfa [EF Core 5,0 planını](plan.md)yinelemez.</span><span class="sxs-lookup"><span data-stu-id="cafad-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="cafad-107">Plan, son yayını teslim etmeden önce dahil ettiğimiz her şey dahil olmak üzere EF Core 5,0 ' a yönelik genel temaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="cafad-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="cafad-108">Yayımlanmakta olan resmi belgelere buradan bağlantılar ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="cafad-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="cafad-109">Önizleme 1 (henüz sevk edilmemiş)</span><span class="sxs-lookup"><span data-stu-id="cafad-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="cafad-110">Basit günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="cafad-110">Simple logging</span></span>

<span data-ttu-id="cafad-111">Bu özellik EF6 içinde `Database.Log` benzer işlevler ekler.</span><span class="sxs-lookup"><span data-stu-id="cafad-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="cafad-112">Yani, her türlü harici günlük çerçevesini yapılandırmaya gerek kalmadan EF Core günlükleri almanın basit bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="cafad-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="cafad-113">İlk belgeler, [5 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.</span><span class="sxs-lookup"><span data-stu-id="cafad-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="cafad-114">Ek belgeler [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-114">Additional documentation is tracked by issue [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="cafad-115">Oluşturulan SQL almanın basit yolu</span><span class="sxs-lookup"><span data-stu-id="cafad-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="cafad-116">EF Core 5,0, bir LINQ sorgusu yürütürken EF Core üretebileceği SQL döndürecek `ToQueryString` genişletme yöntemini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="cafad-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="cafad-117">Ön belgeler, [9 ocak 2020 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)dahildir.</span><span class="sxs-lookup"><span data-stu-id="cafad-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="cafad-118">Ek belgeler [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-118">Additional documentation is tracked by issue [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="cafad-119">Gelişmiş hata ayıklama görünümleri</span><span class="sxs-lookup"><span data-stu-id="cafad-119">Enhanced debug views</span></span>

<span data-ttu-id="cafad-120">Hata ayıklama görünümlerinde EF Core iç yapıları göz atmak kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="cafad-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="cafad-121">Modelin bir hata ayıklama görünümü bir süre önce uygulandı.</span><span class="sxs-lookup"><span data-stu-id="cafad-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="cafad-122">EF Core 5,0 için model görünümü ' ne daha kolay okunabilir ve durum yöneticisinde izlenen varlıklar için yeni bir hata ayıklama görünümü ekledik.</span><span class="sxs-lookup"><span data-stu-id="cafad-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="cafad-123">Ön belgeler, [12 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)dahildir.</span><span class="sxs-lookup"><span data-stu-id="cafad-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="cafad-124">Ek belgeler [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-124">Additional documentation is tracked by issue [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="cafad-125">Bağlantı veya bağlantı dizesi, başlatılmış DbContext üzerinde değiştirilebilir</span><span class="sxs-lookup"><span data-stu-id="cafad-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="cafad-126">Artık herhangi bir bağlantı veya bağlantı dizesi olmadan bir DbContext örneği oluşturulması daha kolay.</span><span class="sxs-lookup"><span data-stu-id="cafad-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="cafad-127">Ayrıca, bağlantı veya bağlantı dizesi artık bağlam örneği üzerinde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="cafad-128">Bu, aynı bağlam örneğinin farklı veritabanlarına dinamik olarak bağlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="cafad-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="cafad-129">Belgeler [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-129">Documentation is tracked by issue [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="cafad-130">Değişiklik izleme proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="cafad-130">Change-tracking proxies</span></span>

<span data-ttu-id="cafad-131">EF Core, artık otomatik olarak [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) ve [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)uygulayan çalışma zamanı proxy 'leri oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="cafad-132">Böylece, varlık özelliklerindeki değer değişiklikleri doğrudan EF Core olarak raporlanarak değişiklik taraması ihtiyacını önler.</span><span class="sxs-lookup"><span data-stu-id="cafad-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="cafad-133">Ancak, proxy 'ler kendi kısıtlama kümesiyle gelir, bu nedenle herkes için değildir.</span><span class="sxs-lookup"><span data-stu-id="cafad-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="cafad-134">Belgeler [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-134">Documentation is tracked by issue [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="cafad-135">Veritabanı null semantiğini gelişmiş işleme</span><span class="sxs-lookup"><span data-stu-id="cafad-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="cafad-136">İlişkisel veritabanları genellikle NULL değerini bilinmeyen bir değer olarak değerlendirir ve bu nedenle başka bir NULL değere eşit değildir.</span><span class="sxs-lookup"><span data-stu-id="cafad-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="cafad-137">C#, diğer taraftan, null değeri, diğer herhangi bir null ile eşit olan tanımlı bir değer olarak davranır.</span><span class="sxs-lookup"><span data-stu-id="cafad-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="cafad-138">EF Core, varsayılan olarak, null semantiğini kullanacak C# şekilde sorguları çevirir.</span><span class="sxs-lookup"><span data-stu-id="cafad-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="cafad-139">EF Core 5,0, bu çevirilerin verimliliğini önemli ölçüde geliştirir.</span><span class="sxs-lookup"><span data-stu-id="cafad-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="cafad-140">Belgeler [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-140">Documentation is tracked by issue [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="cafad-141">Dizin Oluşturucu özellikleri</span><span class="sxs-lookup"><span data-stu-id="cafad-141">Indexer properties</span></span>

<span data-ttu-id="cafad-142">EF Core 5,0, C# Dizin Oluşturucu özelliklerinin eşlenmesini destekler.</span><span class="sxs-lookup"><span data-stu-id="cafad-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="cafad-143">Bu, varlıkların, sütunların pakette adlandırılmış özelliklerle eşlendikleri özellik paketleri olarak davranmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="cafad-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="cafad-144">Belgeler [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-144">Documentation is tracked by issue [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="cafad-145">Enum eşlemeleri için denetim kısıtlamaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="cafad-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="cafad-146">EF Core 5,0 geçişleri, artık enum özelliği eşlemeleri için DENETIM kısıtlamaları oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="cafad-147">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cafad-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="cafad-148">Belgeler [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-148">Documentation is tracked by issue [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="cafad-149">Daha fazla TarihSaat yapıları için sorgu çevirileri</span><span class="sxs-lookup"><span data-stu-id="cafad-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="cafad-150">Yeni veri süresi oluşturma içeren sorgular artık çevrilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="cafad-151">Ayrıca, DateDiffWeek SQL Server işlevi artık eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="cafad-152">Belgeler [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-152">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="cafad-153">Daha fazla bayt dizisi yapıları için sorgu çevirileri</span><span class="sxs-lookup"><span data-stu-id="cafad-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="cafad-154">Byte [] özellikleri, Contains, length, Sequenceeşittir, vb. kullanan sorgular artık SQL 'e çevrilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="cafad-155">İlk belgeler, [5 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.</span><span class="sxs-lookup"><span data-stu-id="cafad-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="cafad-156">Ek belgeler [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-156">Additional documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="cafad-157">Ters için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="cafad-157">Query translation for Reverse</span></span>

<span data-ttu-id="cafad-158">`Reverse` kullanan sorgular artık çevrilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="cafad-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cafad-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="cafad-160">Belgeler [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-160">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="cafad-161">Bit düzeyinde işleçler için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="cafad-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="cafad-162">Bit düzeyinde işleçler kullanan sorgular artık daha fazla durumda çevrilir:</span><span class="sxs-lookup"><span data-stu-id="cafad-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="cafad-163">Belgeler [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-163">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="cafad-164">Cosmos üzerinde dizeler için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="cafad-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="cafad-165">Dize yöntemlerini kullanan sorgular şunlardır, StartsWith ve EndsWith artık Azure Cosmos DB sağlayıcısı kullanılırken çevrilir.</span><span class="sxs-lookup"><span data-stu-id="cafad-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="cafad-166">Belgeler [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="cafad-166">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>
