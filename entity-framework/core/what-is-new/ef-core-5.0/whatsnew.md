---
title: EF Core 5.0'da Yenilikler
description: EF Core 5.0'daki yeni özelliklere genel bakış
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634274"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="fba31-103">EF Core 5.0'da Yenilikler</span><span class="sxs-lookup"><span data-stu-id="fba31-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="fba31-104">EF Core 5.0 şu anda geliştirilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fba31-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="fba31-105">Bu sayfa, her önizlemede tanıtılan ilginç değişikliklerin genel bir özetini içerir.</span><span class="sxs-lookup"><span data-stu-id="fba31-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="fba31-106">Bu [sayfa, EF Core 5.0 için planı](plan.md)çoğaltmaz.</span><span class="sxs-lookup"><span data-stu-id="fba31-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="fba31-107">Planda, son sürümü göndermeden önce eklemeyi planladığımız her şey de dahil olmak üzere EF Core 5.0'ın genel temaları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fba31-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="fba31-108">Yayınlandığı resmi belgelere buradan linkler ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="fba31-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-2"></a><span data-ttu-id="fba31-109">Önizleme 2</span><span class="sxs-lookup"><span data-stu-id="fba31-109">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="fba31-110">Özellik destek alanı belirtmek için C# özniteliği kullanma</span><span class="sxs-lookup"><span data-stu-id="fba31-110">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="fba31-111">C# özniteliği artık bir özelliğin destek alanını belirtmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fba31-111">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="fba31-112">Bu öznitelik, EF Core'un destek alanı otomatik olarak bulunamasa bile normalde olduğu gibi destek alanına yazmaya ve okumasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="fba31-112">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="fba31-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fba31-113">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="fba31-114">Dokümantasyon, sorun [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-114">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="fba31-115">Tam ayırıcı eşleme</span><span class="sxs-lookup"><span data-stu-id="fba31-115">Complete discriminator mapping</span></span>

<span data-ttu-id="fba31-116">EF Core, [bir kalıtım hiyerarşisinin TPH eşlemi](/ef/core/modeling/inheritance)için bir ayırıcı sütun kullanır.</span><span class="sxs-lookup"><span data-stu-id="fba31-116">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="fba31-117">EF Core ayırıcı için tüm olası değerleri bildiği sürece bazı performans geliştirmeleri mümkündür.</span><span class="sxs-lookup"><span data-stu-id="fba31-117">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="fba31-118">EF Core 5.0 şimdi bu geliştirmeleri uygular.</span><span class="sxs-lookup"><span data-stu-id="fba31-118">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="fba31-119">Örneğin, EF Core'un önceki sürümleri, hiyerarşideki tüm türleri döndüren bir sorgu için her zaman bu SQL'i oluşturur:</span><span class="sxs-lookup"><span data-stu-id="fba31-119">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="fba31-120">EF Core 5.0 şimdi tam bir ayrımcı eşleme yapılandırıldığınızda aşağıdakileri oluşturacaktır:</span><span class="sxs-lookup"><span data-stu-id="fba31-120">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="fba31-121">Önizleme 3 ile başlayan varsayılan davranış olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fba31-121">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="fba31-122">Microsoft.Data.Sqlite'de performans iyileştirmeleri</span><span class="sxs-lookup"><span data-stu-id="fba31-122">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="fba31-123">SQLIte için iki performans iyileştirmesi yaptık:</span><span class="sxs-lookup"><span data-stu-id="fba31-123">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="fba31-124">GetBytes, GetChars ve GetTextReader ile ikili ve dize verilerini alma artık SqliteBlob ve akışlardan yararlanarak daha verimli hale geldi.</span><span class="sxs-lookup"><span data-stu-id="fba31-124">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="fba31-125">SqliteConnection'ın başlatılması artık tembel.</span><span class="sxs-lookup"><span data-stu-id="fba31-125">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="fba31-126">Bu geliştirmeler Microsoft.Data.Sqlite sağlayıcısı ADO.NET ve dolayısıyla DA EF Core dışında performansı artırmak bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fba31-126">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="fba31-127">Önizleme 1</span><span class="sxs-lookup"><span data-stu-id="fba31-127">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="fba31-128">Basit günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="fba31-128">Simple logging</span></span>

<span data-ttu-id="fba31-129">Bu özellik, EF6'dakine `Database.Log` benzer işlevsellik ekler.</span><span class="sxs-lookup"><span data-stu-id="fba31-129">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="fba31-130">Diğer bir zamanda, her türlü dış günlük çerçevesini yapılandırmaya gerek kalmadan EF Core'dan günlük almanın basit bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="fba31-130">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="fba31-131">Ön belgeler [5 Aralık 2019 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.</span><span class="sxs-lookup"><span data-stu-id="fba31-131">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="fba31-132">Ek belgeler [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-132">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="fba31-133">Oluşturulan SQL almak için basit bir yol</span><span class="sxs-lookup"><span data-stu-id="fba31-133">Simple way to get generated SQL</span></span>

<span data-ttu-id="fba31-134">EF Core 5.0, `ToQueryString` BIR LINQ sorgusu yürükarırken EF Core'un oluşturacağı SQL'i döndürecek uzantı yöntemini sunar.</span><span class="sxs-lookup"><span data-stu-id="fba31-134">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="fba31-135">Ön belgeler [9 Ocak 2020 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)dahildir.</span><span class="sxs-lookup"><span data-stu-id="fba31-135">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="fba31-136">Ek belgeler [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-136">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="fba31-137">Bir varlığın anahtarı olmadığını belirtmek için C# özniteliği kullanın</span><span class="sxs-lookup"><span data-stu-id="fba31-137">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="fba31-138">Bir varlık türü artık yeni `KeylessAttribute`yi kullanarak anahtara sahip olmayacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fba31-138">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="fba31-139">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fba31-139">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="fba31-140">Dokümantasyon, sorun [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-140">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="fba31-141">Başlatmalı DbContext'da bağlantı veya bağlantı dizesi değiştirilebilir</span><span class="sxs-lookup"><span data-stu-id="fba31-141">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="fba31-142">Herhangi bir bağlantı veya bağlantı dizesi olmadan bir DbContext örneği oluşturmak artık daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="fba31-142">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="fba31-143">Ayrıca, bağlantı veya bağlantı dizesi artık bağlam örneğinde mutasyona uğrayabilir.</span><span class="sxs-lookup"><span data-stu-id="fba31-143">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="fba31-144">Bu özellik, aynı bağlam örneğinin farklı veritabanlarına dinamik olarak bağlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="fba31-144">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="fba31-145">Dokümantasyon, sorun [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-145">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="fba31-146">Değiştirme izleme vekilleri</span><span class="sxs-lookup"><span data-stu-id="fba31-146">Change-tracking proxies</span></span>

<span data-ttu-id="fba31-147">EF Core artık otomatik olarak [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) ve [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)uygulayan çalışma zamanı vekilleri oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="fba31-147">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="fba31-148">Bunlar daha sonra varlık özelliklerindeki değer değişikliklerini doğrudan EF Core'a raporlayarak değişiklikleri taramaya ihtiyaç duymayı önler.</span><span class="sxs-lookup"><span data-stu-id="fba31-148">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="fba31-149">Ancak, vekiller sınırlamalar kendi kümesi ile gelir, bu yüzden herkes için değildir.</span><span class="sxs-lookup"><span data-stu-id="fba31-149">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="fba31-150">Dokümantasyon, #2076 [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-150">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="fba31-151">Gelişmiş hata ayıklama görünümleri</span><span class="sxs-lookup"><span data-stu-id="fba31-151">Enhanced debug views</span></span>

<span data-ttu-id="fba31-152">Hata ayıklama görünümleri, hata ayıklama sorunları sırasında EF Core'un iç etkilerine bakmanın kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="fba31-152">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="fba31-153">Model için hata ayıklama görünümü bir süre önce uygulandı.</span><span class="sxs-lookup"><span data-stu-id="fba31-153">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="fba31-154">EF Core 5.0 için, model görünümünün okunmasını kolaylaştırdık ve devlet yöneticisindeki izlenen varlıklar için yeni bir hata ayıklama görünümü ekledik.</span><span class="sxs-lookup"><span data-stu-id="fba31-154">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="fba31-155">Ön belgeler [12 Aralık 2019 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)dahildir.</span><span class="sxs-lookup"><span data-stu-id="fba31-155">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="fba31-156">Ek belgeler sorun [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-156">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="fba31-157">Veritabanı null semantik geliştirilmiş işleme</span><span class="sxs-lookup"><span data-stu-id="fba31-157">Improved handling of database null semantics</span></span>

<span data-ttu-id="fba31-158">İlişkisel veritabanları genellikle null'u bilinmeyen bir değer olarak ele alacaktır ve bu nedenle diğer NULL'lara eşit değildir.</span><span class="sxs-lookup"><span data-stu-id="fba31-158">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="fba31-159">C# null'u diğer null'lara eşit olan tanımlı bir değer olarak değerlendirirken.</span><span class="sxs-lookup"><span data-stu-id="fba31-159">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="fba31-160">EF Core varsayılan olarak sorguları c# null semantiklerini kullanarak çevirir.</span><span class="sxs-lookup"><span data-stu-id="fba31-160">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="fba31-161">EF Core 5.0 bu çevirilerin verimliliğini büyük ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="fba31-161">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="fba31-162">Dokümantasyon, sorun [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-162">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="fba31-163">Dizinleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="fba31-163">Indexer properties</span></span>

<span data-ttu-id="fba31-164">EF Core 5.0, C# dizinleyici özelliklerinin eşlenemesini destekler.</span><span class="sxs-lookup"><span data-stu-id="fba31-164">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="fba31-165">Bu özellikler, varlıkların, sütunların çantadaki adlandırılmış özelliklere eşlendiği mülk torbası olarak hareket etmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="fba31-165">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="fba31-166">Dokümantasyon, #2018 [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-166">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="fba31-167">Enum eşlemeleri için denetim kısıtlamaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="fba31-167">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="fba31-168">EF Core 5.0 Geçişler artık enum özellik eşlemeleri için ÇEK kısıtlamaları oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="fba31-168">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="fba31-169">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fba31-169">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="fba31-170">Dokümantasyon, sorun [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-170">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="fba31-171">İlişkisel</span><span class="sxs-lookup"><span data-stu-id="fba31-171">IsRelational</span></span>

<span data-ttu-id="fba31-172">Yeni `IsRelational` bir yöntem varolan `IsSqlServer`ek olarak `IsSqlite`eklendi `IsInMemory`, , ve .</span><span class="sxs-lookup"><span data-stu-id="fba31-172">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="fba31-173">Bu yöntem, DbContext'ın herhangi bir ilişkisel veritabanı sağlayıcısı kullanıp kullanmadığını sınamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fba31-173">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="fba31-174">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fba31-174">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="fba31-175">Dokümantasyon, sorun [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-175">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="fba31-176">ETags ile Cosmos iyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="fba31-176">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="fba31-177">Azure Cosmos DB veritabanı sağlayıcısı artık ETags kullanarak iyimser eşzamanlılığı destekliyor.</span><span class="sxs-lookup"><span data-stu-id="fba31-177">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="fba31-178">Bir ETag yapılandırmak için OnModelCreating modeli oluşturucu kullanın:</span><span class="sxs-lookup"><span data-stu-id="fba31-178">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="fba31-179">SaveChanges daha sonra `DbUpdateConcurrencyException` yeniden denemeler, vb uygulamak için [ele alınabilir](https://docs.microsoft.com/ef/core/saving/concurrency) bir eşzamanlılık çakışması, bir atar.</span><span class="sxs-lookup"><span data-stu-id="fba31-179">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="fba31-180">Dokümantasyon, #2099 [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-180">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="fba31-181">Daha fazla DateTime yapıları için sorgu çevirileri</span><span class="sxs-lookup"><span data-stu-id="fba31-181">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="fba31-182">Yeni DateTime yapısı içeren sorgular artık çevrilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fba31-182">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="fba31-183">Buna ek olarak, aşağıdaki SQL Server işlevleri şimdi eşlenir:</span><span class="sxs-lookup"><span data-stu-id="fba31-183">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="fba31-184">TarihDiffWeek</span><span class="sxs-lookup"><span data-stu-id="fba31-184">DateDiffWeek</span></span>
* <span data-ttu-id="fba31-185">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="fba31-185">DateFromParts</span></span>

<span data-ttu-id="fba31-186">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fba31-186">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="fba31-187">Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-187">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="fba31-188">Daha fazla bayt dizi yapıları için sorgu çevirileri</span><span class="sxs-lookup"><span data-stu-id="fba31-188">Query translations for more byte array constructs</span></span>

<span data-ttu-id="fba31-189">Bayt[] özelliklerindeki İçer, Uzunluk, DiziEşit vb. sorgular artık SQL'e çevrilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fba31-189">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="fba31-190">Ön belgeler [5 Aralık 2019 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.</span><span class="sxs-lookup"><span data-stu-id="fba31-190">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="fba31-191">Ek belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-191">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="fba31-192">Ters için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="fba31-192">Query translation for Reverse</span></span>

<span data-ttu-id="fba31-193">Kullanan `Reverse` sorgular artık çevrilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fba31-193">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="fba31-194">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fba31-194">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="fba31-195">Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-195">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="fba31-196">Bitwise işleçleri için sorgu çevirisi</span><span class="sxs-lookup"><span data-stu-id="fba31-196">Query translation for bitwise operators</span></span>

<span data-ttu-id="fba31-197">Bitwise işleçleri kullanan sorgular artık daha fazla durumda tercüme edilir Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fba31-197">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="fba31-198">Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-198">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="fba31-199">Cosmos'taki dizeleri sorgula çeviri</span><span class="sxs-lookup"><span data-stu-id="fba31-199">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="fba31-200">İçeren, StartsWith ve EndsWith dize yöntemlerini kullanan sorgular artık Azure Cosmos DB sağlayıcısını kullanırken çevrilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fba31-200">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="fba31-201">Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="fba31-201">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
