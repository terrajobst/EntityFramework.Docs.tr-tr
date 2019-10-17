---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 690c7828cfe5019f4e7ae904c92430fab4726cb9
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446022"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="076d7-102">EF Core 3,0 ' de yer alan son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="076d7-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="076d7-103">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken mevcut uygulamaları bozmak için olası bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="076d7-104">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="076d7-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="076d7-105">Özet</span><span class="sxs-lookup"><span data-stu-id="076d7-105">Summary</span></span>

| <span data-ttu-id="076d7-106">**Son değişiklik**</span><span class="sxs-lookup"><span data-stu-id="076d7-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="076d7-107">**ACT**</span><span class="sxs-lookup"><span data-stu-id="076d7-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="076d7-108">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="076d7-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="076d7-109">Yüksek</span><span class="sxs-lookup"><span data-stu-id="076d7-109">High</span></span>       |
| [<span data-ttu-id="076d7-110">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="076d7-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="076d7-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="076d7-111">High</span></span>      |
| [<span data-ttu-id="076d7-112">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="076d7-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="076d7-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="076d7-113">High</span></span>      |
| [<span data-ttu-id="076d7-114">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="076d7-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="076d7-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="076d7-115">High</span></span>      |
| [<span data-ttu-id="076d7-116">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="076d7-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="076d7-117">High</span></span>      |
| [<span data-ttu-id="076d7-118">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="076d7-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="076d7-119">Yüksek</span><span class="sxs-lookup"><span data-stu-id="076d7-119">High</span></span>      |
| [<span data-ttu-id="076d7-120">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="076d7-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="076d7-121">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-121">Medium</span></span>      |
| [<span data-ttu-id="076d7-122">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="076d7-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="076d7-123">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-123">Medium</span></span>      |
| [<span data-ttu-id="076d7-124">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="076d7-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="076d7-125">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-125">Medium</span></span>      |
| [<span data-ttu-id="076d7-126">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="076d7-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="076d7-127">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-127">Medium</span></span>      |
| [<span data-ttu-id="076d7-128">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="076d7-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="076d7-129">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-129">Medium</span></span>      |
| [<span data-ttu-id="076d7-130">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="076d7-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="076d7-131">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-131">Medium</span></span>      |
| [<span data-ttu-id="076d7-132">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="076d7-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="076d7-133">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-133">Medium</span></span>      |
| [<span data-ttu-id="076d7-134">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="076d7-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="076d7-135">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-135">Medium</span></span>      |
| [<span data-ttu-id="076d7-136">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="076d7-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="076d7-137">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-137">Medium</span></span>      |
| [<span data-ttu-id="076d7-138">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="076d7-139">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-139">Medium</span></span>      |
| [<span data-ttu-id="076d7-140">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="076d7-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="076d7-141">Orta</span><span class="sxs-lookup"><span data-stu-id="076d7-141">Medium</span></span>      |
| [<span data-ttu-id="076d7-142">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="076d7-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="076d7-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-143">Low</span></span>      |
| [<span data-ttu-id="076d7-144">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="076d7-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="076d7-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-145">Low</span></span>      |
| [<span data-ttu-id="076d7-146">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="076d7-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="076d7-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-147">Low</span></span>      |
| [<span data-ttu-id="076d7-148">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="076d7-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="076d7-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-149">Low</span></span>      |
| [<span data-ttu-id="076d7-150">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="076d7-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="076d7-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-151">Low</span></span>      |
| [<span data-ttu-id="076d7-152">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="076d7-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="076d7-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-153">Low</span></span>      |
| [<span data-ttu-id="076d7-154">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="076d7-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="076d7-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-155">Low</span></span>      |
| [<span data-ttu-id="076d7-156">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="076d7-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="076d7-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-157">Low</span></span>      |
| [<span data-ttu-id="076d7-158">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="076d7-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="076d7-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-159">Low</span></span>      |
| [<span data-ttu-id="076d7-160">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="076d7-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="076d7-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-161">Low</span></span>      |
| [<span data-ttu-id="076d7-162">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="076d7-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="076d7-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-163">Low</span></span>      |
| [<span data-ttu-id="076d7-164">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="076d7-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="076d7-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-165">Low</span></span>      |
| [<span data-ttu-id="076d7-166">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="076d7-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="076d7-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-167">Low</span></span>      |
| [<span data-ttu-id="076d7-168">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="076d7-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="076d7-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-169">Low</span></span>      |
| [<span data-ttu-id="076d7-170">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="076d7-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="076d7-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-171">Low</span></span>      |
| [<span data-ttu-id="076d7-172">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="076d7-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="076d7-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-173">Low</span></span>      |
| [<span data-ttu-id="076d7-174">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="076d7-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="076d7-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-175">Low</span></span>      |
| [<span data-ttu-id="076d7-176">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="076d7-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="076d7-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-177">Low</span></span>      |
| [<span data-ttu-id="076d7-178">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="076d7-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="076d7-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-179">Low</span></span>      |
| [<span data-ttu-id="076d7-180">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="076d7-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="076d7-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-181">Low</span></span>      |
| [<span data-ttu-id="076d7-182">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="076d7-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="076d7-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-183">Low</span></span>      |
| [<span data-ttu-id="076d7-184">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="076d7-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="076d7-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-185">Low</span></span>      |
| [<span data-ttu-id="076d7-186">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="076d7-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="076d7-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-187">Low</span></span>      |
| [<span data-ttu-id="076d7-188">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="076d7-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="076d7-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-189">Low</span></span>      |
| [<span data-ttu-id="076d7-190">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="076d7-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="076d7-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-191">Low</span></span>      |
| [<span data-ttu-id="076d7-192">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="076d7-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="076d7-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-193">Low</span></span>      |
| [<span data-ttu-id="076d7-194">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="076d7-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-195">Low</span></span>      |
| [<span data-ttu-id="076d7-196">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="076d7-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-197">Low</span></span>      |
| [<span data-ttu-id="076d7-198">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="076d7-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="076d7-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-199">Low</span></span>      |
| [<span data-ttu-id="076d7-200">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="076d7-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-201">Low</span></span>      |
| [<span data-ttu-id="076d7-202">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="076d7-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="076d7-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-203">Low</span></span>      |
| [<span data-ttu-id="076d7-204">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="076d7-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="076d7-205">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-205">Low</span></span>      |
| [<span data-ttu-id="076d7-206">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="076d7-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="076d7-207">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-207">Low</span></span>      |
| [<span data-ttu-id="076d7-208">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="076d7-208">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="076d7-209">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-209">Low</span></span>      |
| [<span data-ttu-id="076d7-210">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="076d7-210">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="076d7-211">Düşük</span><span class="sxs-lookup"><span data-stu-id="076d7-211">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="076d7-212">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="076d7-212">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="076d7-213">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[ayrıca bkz. #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="076d7-213">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="076d7-214">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-214">**Old behavior**</span></span>

<span data-ttu-id="076d7-215">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="076d7-215">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="076d7-216">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="076d7-216">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="076d7-217">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-217">**New behavior**</span></span>

<span data-ttu-id="076d7-218">3,0 ile başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrısı) istemci üzerinde değerlendirilmek için yalnızca ifadeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="076d7-218">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="076d7-219">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="076d7-219">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="076d7-220">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-220">**Why**</span></span>

<span data-ttu-id="076d7-221">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="076d7-221">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="076d7-222">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-222">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="076d7-223">Örneğin, çevrilemeyen `Where()` çağrısındaki bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-223">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="076d7-224">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-224">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="076d7-225">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="076d7-225">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="076d7-226">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-226">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="076d7-227">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-227">**Mitigations**</span></span>

<span data-ttu-id="076d7-228">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın veya `AsEnumerable()`, `ToList()` ' i kullanabilir ya da daha sonra verileri LINQ-Objects kullanılarak daha sonra işlenebileceği istemciye açık bir şekilde geri getirmek için benzer.</span><span class="sxs-lookup"><span data-stu-id="076d7-228">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="076d7-229">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="076d7-229">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="076d7-230">Sorun izleniyor #15498</span><span class="sxs-lookup"><span data-stu-id="076d7-230">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="076d7-231">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-231">**Old behavior**</span></span>

<span data-ttu-id="076d7-232">3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-232">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="076d7-233">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-233">**New behavior**</span></span>

<span data-ttu-id="076d7-234">3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="076d7-234">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="076d7-235">Bu, .NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="076d7-235">This does not include .NET Framework.</span></span>

<span data-ttu-id="076d7-236">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-236">**Why**</span></span>

<span data-ttu-id="076d7-237">Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-237">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="076d7-238">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-238">**Mitigations**</span></span>

<span data-ttu-id="076d7-239">Modern bir .NET platformuna geçmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="076d7-239">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="076d7-240">Bu mümkün değilse, her ikisi de .NET Framework EF Core 2,1 veya EF Core 2,2 ' i kullanmaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="076d7-240">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="076d7-241">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="076d7-241">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="076d7-242">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="076d7-242">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="076d7-243">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-243">**Old behavior**</span></span>

<span data-ttu-id="076d7-244">ASP.NET Core 3,0 önce, `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All` ' e bir paket başvurusu eklediğinizde, EF Core ve EF Core sağlayıcı gibi SQL Server veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="076d7-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="076d7-245">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-245">**New behavior**</span></span>

<span data-ttu-id="076d7-246">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="076d7-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="076d7-247">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-247">**Why**</span></span>

<span data-ttu-id="076d7-248">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="076d7-249">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="076d7-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="076d7-250">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="076d7-251">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="076d7-252">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-252">**Mitigations**</span></span>

<span data-ttu-id="076d7-253">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="076d7-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="076d7-254">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="076d7-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="076d7-255">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="076d7-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="076d7-256">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-256">**Old behavior**</span></span>

<span data-ttu-id="076d7-257">3,0 tarihinden önce, `dotnet ef` aracı .NET Core SDK eklenmiştir ve ek adımlara gerek duymadan herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="076d7-258">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-258">**New behavior**</span></span>

<span data-ttu-id="076d7-259">3,0 ' den başlayarak, .NET SDK `dotnet ef` aracını içermez, bu nedenle kullanabilmeniz için onu yerel veya genel bir araç olarak açıkça yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="076d7-260">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-260">**Why**</span></span>

<span data-ttu-id="076d7-261">Bu değişiklik, EF Core 3,0 'nin her zaman bir NuGet paketi olarak dağıtılması açısından, NuGet üzerinde düzenli bir .NET CLı aracı olarak `dotnet ef` ' ı dağıtmamızı ve güncelleştirmemizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="076d7-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="076d7-262">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-262">**Mitigations**</span></span>

<span data-ttu-id="076d7-263">@No__t-0 ' ı yönetmek için, `dotnet-ef` ' i bir genel araç olarak yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="076d7-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="076d7-264">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="076d7-265">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="076d7-266">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="076d7-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="076d7-267">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-267">**Old behavior**</span></span>

<span data-ttu-id="076d7-268">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="076d7-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="076d7-269">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-269">**New behavior**</span></span>

<span data-ttu-id="076d7-270">EF Core 3,0 ' den başlayarak, parametrelerin sorgu dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlRaw`, `ExecuteSqlRaw` ve `ExecuteSqlRawAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="076d7-271">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="076d7-272">Parametrelerin, enterpolasyonlu bir sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlInterpolated`, `ExecuteSqlInterpolated` ve `ExecuteSqlInterpolatedAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="076d7-273">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="076d7-274">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="076d7-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="076d7-275">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-275">**Why**</span></span>

<span data-ttu-id="076d7-276">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="076d7-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="076d7-277">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="076d7-278">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-278">**Mitigations**</span></span>

<span data-ttu-id="076d7-279">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="076d7-279">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="076d7-280">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="076d7-280">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="076d7-281">Sorun izleniyor #15392</span><span class="sxs-lookup"><span data-stu-id="076d7-281">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="076d7-282">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-282">**Old behavior**</span></span>

<span data-ttu-id="076d7-283">3,0 EF Core önce, FromSql yöntemi, geçirilen SQL 'in üzerine oluşmayabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-283">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="076d7-284">SQL, saklı yordam gibi birleştirilmemiş bir işlem olduğu zaman istemci değerlendirmesi gerçekleştirdi.</span><span class="sxs-lookup"><span data-stu-id="076d7-284">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="076d7-285">Aşağıdaki sorgu, sunucuda saklı yordam çalıştırılarak ve istemci tarafında FirstOrDefault ' i gerçekleştirerek çalıştı.</span><span class="sxs-lookup"><span data-stu-id="076d7-285">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="076d7-286">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-286">**New behavior**</span></span>

<span data-ttu-id="076d7-287">EF Core 3,0 ' den başlayarak, EF Core SQL ayrıştırmaya çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="076d7-287">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="076d7-288">Bu nedenle, FromSqlRaw/Fromsqlenterpolasyonından sonra oluşturuyorsanız, EF Core alt sorguya neden olarak SQL 'i oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="076d7-288">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="076d7-289">Bu nedenle, bileşim ile saklı yordam kullanıyorsanız, geçersiz SQL söz dizimi için bir özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="076d7-289">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="076d7-290">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-290">**Why**</span></span>

<span data-ttu-id="076d7-291">EF Core 3,0, [burada](#linq-queries-are-no-longer-evaluated-on-the-client)açıklandığı gibi bir hata olduğundan, otomatik istemci değerlendirmesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="076d7-291">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="076d7-292">**Mayı**</span><span class="sxs-lookup"><span data-stu-id="076d7-292">**Mitigation**</span></span>

<span data-ttu-id="076d7-293">FromSqlRaw/Fromsqlenterpolasyonnda saklı bir yordam kullanıyorsanız, bunun üzerine oluşmadığını bilirsiniz; böylece sunucu tarafında herhangi bir kompozisyonu önlemek için FromSql yöntem çağrısından sonra __asslanabilir/AsAsyncEnumerable__ ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-293">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="076d7-294">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="076d7-294">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="076d7-295">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="076d7-295">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="076d7-296">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-296">**Old behavior**</span></span>

<span data-ttu-id="076d7-297">EF Core 3,0 önce, `FromSql` yöntemi sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-297">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="076d7-298">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-298">**New behavior**</span></span>

<span data-ttu-id="076d7-299">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (`FromSql` ' yi değiştirme) yalnızca sorgu köklerinin üzerinde belirtilebilir, yani doğrudan `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="076d7-299">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="076d7-300">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="076d7-300">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="076d7-301">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-301">**Why**</span></span>

<span data-ttu-id="076d7-302">@No__t-0 ' ı bir `DbSet` ' in dışında bir yerde belirtmek, hiçbir anlam veya eklenen değeri belirtmediyse, belirli senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-302">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="076d7-303">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-303">**Mitigations**</span></span>

<span data-ttu-id="076d7-304">`FromSql` çağırmaları, uygulandıkları `DbSet` ' e doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-304">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="076d7-305">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="076d7-305">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="076d7-306">Sorun izleniyor #13518</span><span class="sxs-lookup"><span data-stu-id="076d7-306">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="076d7-307">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-307">**Old behavior**</span></span>

<span data-ttu-id="076d7-308">EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-308">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="076d7-309">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="076d7-309">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="076d7-310">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="076d7-310">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="076d7-311">, belirtilen kategori ile ilişkili her bir `Product` için aynı `Category` örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="076d7-311">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="076d7-312">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-312">**New behavior**</span></span>

<span data-ttu-id="076d7-313">EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-313">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="076d7-314">Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde bile her `Product` için yeni bir `Category` örneği döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="076d7-314">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="076d7-315">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-315">**Why**</span></span>

<span data-ttu-id="076d7-316">Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="076d7-316">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="076d7-317">Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="076d7-317">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="076d7-318">Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="076d7-318">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="076d7-319">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-319">**Mitigations**</span></span>

<span data-ttu-id="076d7-320">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-320">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="076d7-321">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="076d7-321">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="076d7-322">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="076d7-322">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="076d7-323">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="076d7-323">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="076d7-324">Örneğin, SQL 'in günlüğe kaydedilmesini `Debug` ' a geçmek için `OnConfiguring` veya `AddDbContext` ' deki düzeyi açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="076d7-324">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="076d7-325">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="076d7-325">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="076d7-326">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="076d7-326">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="076d7-327">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-327">**Old behavior**</span></span>

<span data-ttu-id="076d7-328">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="076d7-328">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="076d7-329">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="076d7-329">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="076d7-330">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-330">**New behavior**</span></span>

<span data-ttu-id="076d7-331">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="076d7-331">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="076d7-332">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-332">**Why**</span></span>

<span data-ttu-id="076d7-333">Daha önce bazı `DbContext` örnekleri tarafından izlenen bir varlık farklı bir `DbContext` örneğine taşındığında, geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-333">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="076d7-334">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-334">**Mitigations**</span></span>

<span data-ttu-id="076d7-335">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlara birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup `Added` durumundaki varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-335">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="076d7-336">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="076d7-336">This can be avoided by:</span></span>
* <span data-ttu-id="076d7-337">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="076d7-337">Not using store-generated keys.</span></span>
* <span data-ttu-id="076d7-338">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="076d7-338">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="076d7-339">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="076d7-339">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="076d7-340">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` `blog.Id` kendisi ayarlanmamışsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="076d7-340">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="076d7-341">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="076d7-341">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="076d7-342">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="076d7-342">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="076d7-343">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-343">**Old behavior**</span></span>

<span data-ttu-id="076d7-344">EF Core 3,0 ' dan önce, `DetectChanges` tarafından bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve `SaveChanges` çağrıldığında yeni bir satır olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="076d7-344">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="076d7-345">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-345">**New behavior**</span></span>

<span data-ttu-id="076d7-346">EF Core 3,0 ' den başlayarak, bir varlık oluşturulan anahtar değerlerini kullanıyorsa ve bazı anahtar değer ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="076d7-346">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="076d7-347">Bu, varlık için bir satırın var olduğu varsayılır ve `SaveChanges` çağrıldığında güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-347">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="076d7-348">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="076d7-348">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="076d7-349">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-349">**Why**</span></span>

<span data-ttu-id="076d7-350">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-350">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="076d7-351">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-351">**Mitigations**</span></span>

<span data-ttu-id="076d7-352">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-352">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="076d7-353">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-353">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="076d7-354">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="076d7-354">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="076d7-355">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="076d7-355">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="076d7-356">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="076d7-356">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="076d7-357">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="076d7-357">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="076d7-358">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-358">**Old behavior**</span></span>

<span data-ttu-id="076d7-359">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="076d7-359">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="076d7-360">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-360">**New behavior**</span></span>

<span data-ttu-id="076d7-361">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="076d7-361">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="076d7-362">Örneğin, bir asıl varlığı silmek için `context.Remove()` ' ı çağırmak, izlenen ilişkili ilgili tüm bağımlıların da hemen `Deleted` olarak ayarlanmasının oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="076d7-362">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="076d7-363">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-363">**Why**</span></span>

<span data-ttu-id="076d7-364">Bu değişiklik, `SaveChanges` çağrılmadan _önce_ hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-364">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="076d7-365">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-365">**Mitigations**</span></span>

<span data-ttu-id="076d7-366">Önceki davranış `context.ChangedTracker` ayarları aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-366">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="076d7-367">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-367">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="076d7-368">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="076d7-368">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="076d7-369">Sorun izleniyor #18022</span><span class="sxs-lookup"><span data-stu-id="076d7-369">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="076d7-370">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-370">**Old behavior**</span></span>

<span data-ttu-id="076d7-371">3,0 öncesinde, `Include` işleçleri aracılığıyla koleksiyon gezginlerini yüklemek, her ilgili varlık türü için bir tane olmak üzere ilişkisel veritabanında birden çok sorgunun oluşturulmasına neden oldu.</span><span class="sxs-lookup"><span data-stu-id="076d7-371">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="076d7-372">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-372">**New behavior**</span></span>

<span data-ttu-id="076d7-373">3,0 ile başlayarak, EF Core ilişkisel veritabanlarında birleştirmelere sahip tek bir sorgu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="076d7-373">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="076d7-374">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-374">**Why**</span></span>

<span data-ttu-id="076d7-375">Tek bir LINQ sorgusu uygulamak için birden çok sorgu verilmesi, birden çok veritabanı yuvarlaklığına gerek olduğu için negatif performans dahil olmak üzere çok sayıda soruna neden olmuş ve her sorgu veritabanının farklı bir durumunu gözlemleyebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-375">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="076d7-376">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-376">**Mitigations**</span></span>

<span data-ttu-id="076d7-377">Teknik olarak bu bir değişiklik olmadığı sürece, tek bir sorgu koleksiyon gezginlerde çok sayıda `Include` işleci içerdiğinde uygulama performansının önemli bir etkisi olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-377">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="076d7-378">Daha fazla bilgi edinmek ve sorguları daha verimli bir şekilde yeniden yazmak için [Bu açıklamaya bakın](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="076d7-378">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="076d7-379">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="076d7-379">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="076d7-380">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="076d7-380">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="076d7-381">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-381">**Old behavior**</span></span>

<span data-ttu-id="076d7-382">3,0 öncesinde, `DeleteBehavior.Restrict`, veritabanında `Restrict` semantiklerle yabancı anahtarlar oluşturdu, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="076d7-382">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="076d7-383">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-383">**New behavior**</span></span>

<span data-ttu-id="076d7-384">3,0 ile başlayarak, `DeleteBehavior.Restrict`, yabancı anahtarların `Restrict` semantiğinin oluşturulmasını sağlar; Yani bu, basamaksız; sınır iç düzeltmesini etkilemeden kısıtlama ihlalinde throw.</span><span class="sxs-lookup"><span data-stu-id="076d7-384">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="076d7-385">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-385">**Why**</span></span>

<span data-ttu-id="076d7-386">Bu değişiklik, beklenmeyen yan etkileri olmadan `DeleteBehavior` ' ı sezgisel bir şekilde kullanma deneyimini geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-386">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="076d7-387">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-387">**Mitigations**</span></span>

<span data-ttu-id="076d7-388">Önceki davranış `DeleteBehavior.ClientNoAction` kullanılarak geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-388">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="076d7-389">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="076d7-389">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="076d7-390">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="076d7-390">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="076d7-391">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-391">**Old behavior**</span></span>

<span data-ttu-id="076d7-392">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/keyless-entity-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="076d7-392">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="076d7-393">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="076d7-393">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="076d7-394">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-394">**New behavior**</span></span>

<span data-ttu-id="076d7-395">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="076d7-395">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="076d7-396">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="076d7-396">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="076d7-397">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-397">**Why**</span></span>

<span data-ttu-id="076d7-398">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-398">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="076d7-399">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-399">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="076d7-400">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="076d7-400">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="076d7-401">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-401">**Mitigations**</span></span>

<span data-ttu-id="076d7-402">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="076d7-402">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="076d7-403">**`ModelBuilder.Query<>()`** -`ModelBuilder.Entity<>().HasNoKey()` ' nin bir varlık türünü anahtar olmadan işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-403">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="076d7-404">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-404">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="076d7-405">**`DbQuery<>`** -`DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-405">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="076d7-406">**`DbContext.Query<>()`** -`DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-406">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="076d7-407">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="076d7-407">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="076d7-408">[Sorun izleme #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
 izleme sorunu[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[izleme sorunu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="076d7-408">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="076d7-409">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-409">**Old behavior**</span></span>

<span data-ttu-id="076d7-410">3,0 EF Core önce, sahip olan ilişkinin yapılandırması `OwnsOne` veya `OwnsMany` çağrısından sonra doğrudan gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="076d7-410">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="076d7-411">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-411">**New behavior**</span></span>

<span data-ttu-id="076d7-412">EF Core 3,0 ' den başlayarak, artık `WithOwner()` kullanarak sahip için bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="076d7-412">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="076d7-413">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-413">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="076d7-414">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin yapılandırılmasına benzer şekilde `WithOwner()` ' dan sonra zinciredilmelidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-414">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="076d7-415">Sahip olduğu için yapılandırma `OwnsOne()/OwnsMany()` ' dan sonra hala zincirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-415">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="076d7-416">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-416">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="076d7-417">Ayrıca, sahip bir tür hedefi ile `Entity()`, `HasOne()` veya `Set()` çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="076d7-417">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="076d7-418">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-418">**Why**</span></span>

<span data-ttu-id="076d7-419">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-419">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="076d7-420">Bu, ' de `HasForeignKey` gibi yöntemler etrafında belirsizlik ve karışıklık ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="076d7-420">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="076d7-421">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-421">**Mitigations**</span></span>

<span data-ttu-id="076d7-422">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="076d7-422">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="076d7-423">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="076d7-423">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="076d7-424">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="076d7-424">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="076d7-425">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-425">**Old behavior**</span></span>

<span data-ttu-id="076d7-426">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="076d7-426">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="076d7-427">EF Core 3,0 ' dan önce, `OrderDetails` `Order` ' e aitse veya aynı tabloya açık olarak eşlenmişse, yeni bir @no__t 3 eklendiğinde her zaman bir `OrderDetails` örneği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-427">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="076d7-428">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-428">**New behavior**</span></span>

<span data-ttu-id="076d7-429">EF Core 3,0 ' den başlayarak, bir `OrderDetails` olmadan `Order` ekleme ve birincil anahtar null yapılabilir sütunlara hariç tüm `OrderDetails` özelliklerini eşleme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="076d7-429">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="076d7-430">EF Core kümelerini sorgularken, gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar ve tüm Özellikler ' in yanı sıra gerekli özellikleri yoksa, `null` ' ye `null` @no__t.</span><span class="sxs-lookup"><span data-stu-id="076d7-430">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="076d7-431">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-431">**Mitigations**</span></span>

<span data-ttu-id="076d7-432">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinmenin `null` olması beklenmiyorsa, gezinti `null` olduğunda, uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-432">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="076d7-433">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir veya en az bir özelliğe atanmış @no__t 0 olmayan bir değer olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-433">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="076d7-434">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="076d7-434">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="076d7-435">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="076d7-435">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="076d7-436">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-436">**Old behavior**</span></span>

<span data-ttu-id="076d7-437">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="076d7-437">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="076d7-438">EF Core 3,0 ' dan önce, `OrderDetails` `Order` ' e aitse veya aynı tabloya açık olarak eşlenmişse, yalnızca `OrderDetails` ' yi güncelleştirmek istemcide `Version` değerini güncelleştirmeyecektir ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="076d7-438">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="076d7-439">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-439">**New behavior**</span></span>

<span data-ttu-id="076d7-440">3,0 ' dan başlayarak, EF Core yeni `Version` değerini @no__t 2 ' ye sahip `Order` ' e yayar.</span><span class="sxs-lookup"><span data-stu-id="076d7-440">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="076d7-441">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="076d7-441">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="076d7-442">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-442">**Why**</span></span>

<span data-ttu-id="076d7-443">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-443">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="076d7-444">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-444">**Mitigations**</span></span>

<span data-ttu-id="076d7-445">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-445">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="076d7-446">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="076d7-446">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="076d7-447">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="076d7-447">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="076d7-448">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="076d7-448">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="076d7-449">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-449">**Old behavior**</span></span>

<span data-ttu-id="076d7-450">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="076d7-450">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="076d7-451">EF Core 3,0 ' dan önce, `ShippingAddress` özelliği varsayılan olarak `BulkOrder` ve `Order` sütunları için ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="076d7-451">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="076d7-452">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-452">**New behavior**</span></span>

<span data-ttu-id="076d7-453">3,0 ile başlayarak, EF Core `ShippingAddress` için yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="076d7-453">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="076d7-454">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-454">**Why**</span></span>

<span data-ttu-id="076d7-455">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="076d7-455">The old behavoir was unexpected.</span></span>

<span data-ttu-id="076d7-456">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-456">**Mitigations**</span></span>

<span data-ttu-id="076d7-457">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="076d7-457">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="076d7-458">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="076d7-458">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="076d7-459">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="076d7-459">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="076d7-460">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-460">**Old behavior**</span></span>

<span data-ttu-id="076d7-461">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="076d7-461">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="076d7-462">EF Core 3,0 ' dan önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-462">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="076d7-463">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtar @no__t ve genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="076d7-463">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="076d7-464">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-464">**New behavior**</span></span>

<span data-ttu-id="076d7-465">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="076d7-465">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="076d7-466">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-466">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="076d7-467">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-467">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="076d7-468">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-468">**Why**</span></span>

<span data-ttu-id="076d7-469">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-469">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="076d7-470">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-470">**Mitigations**</span></span>

<span data-ttu-id="076d7-471">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="076d7-471">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="076d7-472">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="076d7-472">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="076d7-473">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="076d7-473">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="076d7-474">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-474">**Old behavior**</span></span>

<span data-ttu-id="076d7-475">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope` içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="076d7-475">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="076d7-476">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-476">**New behavior**</span></span>

<span data-ttu-id="076d7-477">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="076d7-477">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="076d7-478">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-478">**Why**</span></span>

<span data-ttu-id="076d7-479">Bu değişiklik, aynı `TransactionScope` ' da birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="076d7-479">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="076d7-480">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="076d7-480">The new behavior also matches EF6.</span></span>

<span data-ttu-id="076d7-481">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-481">**Mitigations**</span></span>

<span data-ttu-id="076d7-482">Bağlantının `OpenConnection()` ' a açık açık çağrı kalması gerekiyorsa, EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="076d7-482">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="076d7-483">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="076d7-483">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="076d7-484">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="076d7-484">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="076d7-485">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-485">**Old behavior**</span></span>

<span data-ttu-id="076d7-486">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-486">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="076d7-487">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-487">**New behavior**</span></span>

<span data-ttu-id="076d7-488">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="076d7-488">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="076d7-489">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="076d7-489">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="076d7-490">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-490">**Why**</span></span>

<span data-ttu-id="076d7-491">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-491">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="076d7-492">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-492">**Mitigations**</span></span>

<span data-ttu-id="076d7-493">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-493">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="076d7-494">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="076d7-494">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="076d7-495">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="076d7-495">Backing fields are used by default</span></span>

[<span data-ttu-id="076d7-496">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="076d7-496">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="076d7-497">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-497">**Old behavior**</span></span>

<span data-ttu-id="076d7-498">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="076d7-498">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="076d7-499">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="076d7-499">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="076d7-500">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-500">**New behavior**</span></span>

<span data-ttu-id="076d7-501">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="076d7-501">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="076d7-502">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-502">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="076d7-503">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-503">**Why**</span></span>

<span data-ttu-id="076d7-504">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-504">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="076d7-505">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-505">**Mitigations**</span></span>

<span data-ttu-id="076d7-506">3,0 öncesi davranış `ModelBuilder` ' da özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-506">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="076d7-507">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-507">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="076d7-508">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="076d7-508">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="076d7-509">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="076d7-509">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="076d7-510">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-510">**Old behavior**</span></span>

<span data-ttu-id="076d7-511">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-511">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="076d7-512">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-512">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="076d7-513">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-513">**New behavior**</span></span>

<span data-ttu-id="076d7-514">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="076d7-514">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="076d7-515">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-515">**Why**</span></span>

<span data-ttu-id="076d7-516">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-516">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="076d7-517">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-517">**Mitigations**</span></span>

<span data-ttu-id="076d7-518">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-518">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="076d7-519">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="076d7-519">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="076d7-520">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="076d7-520">Field-only property names should match the field name</span></span>

<span data-ttu-id="076d7-521">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-521">**Old behavior**</span></span>

<span data-ttu-id="076d7-522">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="076d7-522">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="076d7-523">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-523">**New behavior**</span></span>

<span data-ttu-id="076d7-524">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-524">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="076d7-525">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-525">**Why**</span></span>

<span data-ttu-id="076d7-526">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="076d7-526">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="076d7-527">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-527">**Mitigations**</span></span>

<span data-ttu-id="076d7-528">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-528">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="076d7-529">3,0 sonrasında EF Core gelecek bir sürümünde, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="076d7-529">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="076d7-530">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="076d7-530">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="076d7-531">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="076d7-531">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="076d7-532">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-532">**Old behavior**</span></span>

<span data-ttu-id="076d7-533">EF Core 3,0 ' dan önce, `AddDbContext` veya `AddDbContextPool` ' i çağırmak, günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini D. I ile birlikte [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrıları aracılığıyla da kaydeder.</span><span class="sxs-lookup"><span data-stu-id="076d7-533">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="076d7-534">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-534">**New behavior**</span></span>

<span data-ttu-id="076d7-535">EF Core 3,0 ' den başlayarak, `AddDbContext` ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmez.</span><span class="sxs-lookup"><span data-stu-id="076d7-535">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="076d7-536">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-536">**Why**</span></span>

<span data-ttu-id="076d7-537">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="076d7-537">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="076d7-538">Ancak, `ILoggerFactory`, uygulamanın DI kapsayıcısında kayıtlıysa, EF Core tarafından kullanılmaya devam edilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-538">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="076d7-539">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-539">**Mitigations**</span></span>

<span data-ttu-id="076d7-540">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="076d7-540">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="076d7-541">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="076d7-541">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="076d7-542">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="076d7-542">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="076d7-543">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-543">**Old behavior**</span></span>

<span data-ttu-id="076d7-544">EF Core 3,0 ' dan önce, `DbContext.Entry` ' ı çağırmak izlenen tüm varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="076d7-544">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="076d7-545">Bu, `EntityEntry` ' da kullanıma sunulan durumun güncel olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="076d7-545">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="076d7-546">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-546">**New behavior**</span></span>

<span data-ttu-id="076d7-547">EF Core 3,0 ' den başlayarak, `DbContext.Entry` ' ı çağırmak artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="076d7-547">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="076d7-548">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="076d7-548">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="076d7-549">@No__t-0 ' ı `false` olarak ayarlanırsa, bu yerel değişiklik algılama devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-549">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="076d7-550">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` ve `SaveChanges`--tüm izlenen varlıkların tam @no__t 2 ' ye neden olur.</span><span class="sxs-lookup"><span data-stu-id="076d7-550">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="076d7-551">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-551">**Why**</span></span>

<span data-ttu-id="076d7-552">Bu değişiklik, `context.Entry` kullanmanın varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-552">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="076d7-553">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-553">**Mitigations**</span></span>

<span data-ttu-id="076d7-554">3,0 öncesi davranışı sağlamak için `Entry` çağrılmadan önce `ChgangeTracker.DetectChanges()` ' ı çağırın.</span><span class="sxs-lookup"><span data-stu-id="076d7-554">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="076d7-555">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="076d7-555">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="076d7-556">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="076d7-556">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="076d7-557">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-557">**Old behavior**</span></span>

<span data-ttu-id="076d7-558">EF Core 3,0 önce, `string` ve `byte[]` anahtar özellikleri açıkça null olmayan bir değer Ayarlamasız şekilde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-558">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="076d7-559">Böyle bir durumda, anahtar değeri istemci üzerinde `byte[]` için bayt olarak seri hale getirilen bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="076d7-559">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="076d7-560">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-560">**New behavior**</span></span>

<span data-ttu-id="076d7-561">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-561">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="076d7-562">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-562">**Why**</span></span>

<span data-ttu-id="076d7-563">Bu değişiklik, istemci tarafından oluşturulan `string` @ no__t-1 @ no__t-2 değerleri genellikle faydalı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-563">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="076d7-564">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-564">**Mitigations**</span></span>

<span data-ttu-id="076d7-565">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-565">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="076d7-566">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="076d7-566">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="076d7-567">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="076d7-567">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="076d7-568">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="076d7-568">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="076d7-569">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="076d7-569">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="076d7-570">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-570">**Old behavior**</span></span>

<span data-ttu-id="076d7-571">EF Core 3,0 ' dan önce, `ILoggerFactory` tek bir hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="076d7-571">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="076d7-572">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-572">**New behavior**</span></span>

<span data-ttu-id="076d7-573">EF Core 3,0 ' den başlayarak, `ILoggerFactory` artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-573">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="076d7-574">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-574">**Why**</span></span>

<span data-ttu-id="076d7-575">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi Pathik davranışlarının bazı örneklerini kaldıran `DbContext` örneğiyle bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-575">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="076d7-576">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-576">**Mitigations**</span></span>

<span data-ttu-id="076d7-577">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-577">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="076d7-578">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="076d7-578">This isn't common.</span></span>
<span data-ttu-id="076d7-579">Bu durumlarda, çoğu şey çalışmaya devam eder, ancak `ILoggerFactory` ' a bağlı olan herhangi bir tek hizmetin, `ILoggerFactory` ' i farklı bir şekilde alması için değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-579">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="076d7-580">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) ' nde bir sorun @no__t bildirin.</span><span class="sxs-lookup"><span data-stu-id="076d7-580">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="076d7-581">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="076d7-581">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="076d7-582">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="076d7-582">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="076d7-583">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-583">**Old behavior**</span></span>

<span data-ttu-id="076d7-584">EF Core 3,0 ' dan önce, bir `DbContext` ' ı bir kez atıldıktan sonra, söz konusu bağlamdan alınan bir varlıkta verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="076d7-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="076d7-585">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="076d7-586">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="076d7-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="076d7-587">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-587">**New behavior**</span></span>

<span data-ttu-id="076d7-588">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="076d7-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="076d7-589">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="076d7-590">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="076d7-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="076d7-591">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="076d7-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="076d7-592">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-592">**Why**</span></span>

<span data-ttu-id="076d7-593">Bu değişiklik, atılmış bir `DbContext` örneğinde yavaş yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="076d7-594">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-594">**Mitigations**</span></span>

<span data-ttu-id="076d7-595">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="076d7-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="076d7-596">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="076d7-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="076d7-597">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="076d7-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="076d7-598">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-598">**Old behavior**</span></span>

<span data-ttu-id="076d7-599">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="076d7-600">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-600">**New behavior**</span></span>

<span data-ttu-id="076d7-601">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="076d7-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="076d7-602">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-602">**Why**</span></span>

<span data-ttu-id="076d7-603">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="076d7-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="076d7-604">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-604">**Mitigations**</span></span>

<span data-ttu-id="076d7-605">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="076d7-606">Ancak, hata, `DbContextOptionsBuilder` ' da yapılandırma yoluyla bir uyarıya (veya yoksayıldı) geri dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="076d7-607">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="076d7-608">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="076d7-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="076d7-609">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="076d7-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="076d7-610">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-610">**Old behavior**</span></span>

<span data-ttu-id="076d7-611">EF Core 3,0 öncesinde, tek bir dizeyle `HasOne` veya `HasMany` çağıran kod kafa karıştırıcı bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="076d7-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="076d7-612">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="076d7-613">Kod, özel olabilecek `Entrance` gezinti özelliğini kullanarak `Samurai` ' ı diğer bir varlık türü ile ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="076d7-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="076d7-614">Gerçekte, bu kod, gezinti özelliği olmayan `Entrance` adlı bir varlık türüne ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="076d7-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="076d7-615">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-615">**New behavior**</span></span>

<span data-ttu-id="076d7-616">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="076d7-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="076d7-617">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-617">**Why**</span></span>

<span data-ttu-id="076d7-618">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="076d7-619">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-619">**Mitigations**</span></span>

<span data-ttu-id="076d7-620">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="076d7-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="076d7-621">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="076d7-621">This is not common.</span></span>
<span data-ttu-id="076d7-622">Önceki davranış, gezinti özelliği adı için `null` ' ı açıkça geçirerek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="076d7-623">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="076d7-624">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="076d7-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="076d7-625">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="076d7-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="076d7-626">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-626">**Old behavior**</span></span>

<span data-ttu-id="076d7-627">Aşağıdaki zaman uyumsuz yöntemler daha önce bir @no__t döndürdü-0:</span><span class="sxs-lookup"><span data-stu-id="076d7-627">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="076d7-628">`ValueGenerator.NextValueAsync()` (ve türetilen sınıflar)</span><span class="sxs-lookup"><span data-stu-id="076d7-628">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="076d7-629">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-629">**New behavior**</span></span>

<span data-ttu-id="076d7-630">Yukarıda bahsedilen yöntemler bundan sonra aynı `T` üzerinden `ValueTask<T>` döndürür.</span><span class="sxs-lookup"><span data-stu-id="076d7-630">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="076d7-631">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-631">**Why**</span></span>

<span data-ttu-id="076d7-632">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="076d7-632">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="076d7-633">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-633">**Mitigations**</span></span>

<span data-ttu-id="076d7-634">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="076d7-634">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="076d7-635">Daha karmaşık bir kullanım (örn. döndürülen `Task` ' A `Task.WhenAny()` ' e geçirilmesi) genellikle `AsTask()` ' @no__t çağırarak `Task<T>` ' e dönüştürülmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="076d7-635">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="076d7-636">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="076d7-636">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="076d7-637">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="076d7-637">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="076d7-638">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="076d7-638">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="076d7-639">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-639">**Old behavior**</span></span>

<span data-ttu-id="076d7-640">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="076d7-640">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="076d7-641">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-641">**New behavior**</span></span>

<span data-ttu-id="076d7-642">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="076d7-642">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="076d7-643">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-643">**Why**</span></span>

<span data-ttu-id="076d7-644">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-644">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="076d7-645">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-645">**Mitigations**</span></span>

<span data-ttu-id="076d7-646">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-646">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="076d7-647">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-647">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="076d7-648">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="076d7-648">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="076d7-649">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="076d7-649">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="076d7-650">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-650">**Old behavior**</span></span>

<span data-ttu-id="076d7-651">EF Core 3,0 öncesinde, türetilmiş bir tür üzerinde çağrılan `ToTable()`, yalnızca devralma eşleme stratejisi bu geçersiz olduğu için yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-651">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="076d7-652">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-652">**New behavior**</span></span>

<span data-ttu-id="076d7-653">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, türetilmiş bir tür üzerinde çağrılan `ToTable()`, bundan sonra beklenmeyen bir eşleme değişikliğini önlemek için bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="076d7-653">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="076d7-654">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-654">**Why**</span></span>

<span data-ttu-id="076d7-655">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="076d7-655">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="076d7-656">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="076d7-656">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="076d7-657">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-657">**Mitigations**</span></span>

<span data-ttu-id="076d7-658">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="076d7-658">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="076d7-659">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="076d7-659">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="076d7-660">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="076d7-660">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="076d7-661">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-661">**Old behavior**</span></span>

<span data-ttu-id="076d7-662">EF Core 3,0 ' dan önce, `ForSqlServerHasIndex().ForSqlServerInclude()` `INCLUDE` ile kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="076d7-662">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="076d7-663">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-663">**New behavior**</span></span>

<span data-ttu-id="076d7-664">EF Core 3,0 ' den itibaren, bir dizinde `Include` kullanılması artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="076d7-664">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="076d7-665">@No__t-0 kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-665">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="076d7-666">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-666">**Why**</span></span>

<span data-ttu-id="076d7-667">Bu değişiklik, tüm veritabanı sağlayıcıları için `Include` olan dizinler için API 'yi tek bir yerde birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-667">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="076d7-668">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-668">**Mitigations**</span></span>

<span data-ttu-id="076d7-669">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-669">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="076d7-670">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="076d7-670">Metadata API changes</span></span>

[<span data-ttu-id="076d7-671">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="076d7-671">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="076d7-672">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-672">**New behavior**</span></span>

<span data-ttu-id="076d7-673">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="076d7-673">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="076d7-674">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-674">**Why**</span></span>

<span data-ttu-id="076d7-675">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="076d7-675">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="076d7-676">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-676">**Mitigations**</span></span>

<span data-ttu-id="076d7-677">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-677">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="076d7-678">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="076d7-678">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="076d7-679">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="076d7-679">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="076d7-680">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-680">**New behavior**</span></span>

<span data-ttu-id="076d7-681">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="076d7-681">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="076d7-682">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-682">**Why**</span></span>

<span data-ttu-id="076d7-683">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="076d7-683">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="076d7-684">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-684">**Mitigations**</span></span>

<span data-ttu-id="076d7-685">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-685">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="076d7-686">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="076d7-686">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="076d7-687">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="076d7-687">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="076d7-688">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-688">**Old behavior**</span></span>

<span data-ttu-id="076d7-689">EF Core 3,0 ' dan önce, bir SQLite bağlantısı açıldığında EF Core `PRAGMA foreign_keys = 1` gönderir.</span><span class="sxs-lookup"><span data-stu-id="076d7-689">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="076d7-690">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-690">**New behavior**</span></span>

<span data-ttu-id="076d7-691">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı açıldığında EF Core artık `PRAGMA foreign_keys = 1` göndermez.</span><span class="sxs-lookup"><span data-stu-id="076d7-691">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="076d7-692">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-692">**Why**</span></span>

<span data-ttu-id="076d7-693">Bu değişiklik, EF Core varsayılan olarak `SQLitePCLRaw.bundle_e_sqlite3` ' ı kullandığından, FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="076d7-693">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="076d7-694">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-694">**Mitigations**</span></span>

<span data-ttu-id="076d7-695">Yabancı anahtarlar varsayılan olarak, EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 içinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="076d7-695">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="076d7-696">Diğer durumlar için, bağlantı dizeniz içinde `Foreign Keys=True` belirterek yabancı anahtarlar etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-696">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="076d7-697">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="076d7-697">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="076d7-698">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-698">**Old behavior**</span></span>

<span data-ttu-id="076d7-699">3,0 EF Core önce, EF Core `SQLitePCLRaw.bundle_green` ' ı kullandı.</span><span class="sxs-lookup"><span data-stu-id="076d7-699">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="076d7-700">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-700">**New behavior**</span></span>

<span data-ttu-id="076d7-701">EF Core 3,0 ' den başlayarak EF Core `SQLitePCLRaw.bundle_e_sqlite3` ' ı kullanır.</span><span class="sxs-lookup"><span data-stu-id="076d7-701">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="076d7-702">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-702">**Why**</span></span>

<span data-ttu-id="076d7-703">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-703">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="076d7-704">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-704">**Mitigations**</span></span>

<span data-ttu-id="076d7-705">İOS 'ta yerel SQLite sürümünü kullanmak için `Microsoft.Data.Sqlite` ' ı farklı bir `SQLitePCLRaw` paketi kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="076d7-705">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="076d7-706">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="076d7-706">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="076d7-707">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="076d7-707">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="076d7-708">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-708">**Old behavior**</span></span>

<span data-ttu-id="076d7-709">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="076d7-709">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="076d7-710">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-710">**New behavior**</span></span>

<span data-ttu-id="076d7-711">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="076d7-711">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="076d7-712">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-712">**Why**</span></span>

<span data-ttu-id="076d7-713">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="076d7-713">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="076d7-714">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="076d7-714">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="076d7-715">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-715">**Mitigations**</span></span>

<span data-ttu-id="076d7-716">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-716">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="076d7-717">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-717">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="076d7-718">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-718">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="076d7-719">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="076d7-719">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="076d7-720">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="076d7-720">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="076d7-721">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-721">**Old behavior**</span></span>

<span data-ttu-id="076d7-722">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="076d7-722">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="076d7-723">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="076d7-723">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="076d7-724">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-724">**New behavior**</span></span>

<span data-ttu-id="076d7-725">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="076d7-725">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="076d7-726">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-726">**Why**</span></span>

<span data-ttu-id="076d7-727">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="076d7-727">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="076d7-728">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-728">**Mitigations**</span></span>

<span data-ttu-id="076d7-729">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-729">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="076d7-730">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-730">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="076d7-731">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-731">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="076d7-732">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="076d7-732">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="076d7-733">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="076d7-733">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="076d7-734">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-734">**Old behavior**</span></span>

<span data-ttu-id="076d7-735">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="076d7-735">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="076d7-736">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-736">**New behavior**</span></span>

<span data-ttu-id="076d7-737">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="076d7-737">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="076d7-738">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-738">**Why**</span></span>

<span data-ttu-id="076d7-739">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-739">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="076d7-740">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="076d7-740">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="076d7-741">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-741">**Mitigations**</span></span>

<span data-ttu-id="076d7-742">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="076d7-742">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="076d7-743">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-743">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="076d7-744">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-744">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="076d7-745">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-745">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="076d7-746">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-746">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="076d7-747">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="076d7-747">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="076d7-748">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-748">**Old behavior**</span></span>

<span data-ttu-id="076d7-749">EF Core 3,0 ' dan önce, `UseRowNumberForPaging` SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-749">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="076d7-750">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-750">**New behavior**</span></span>

<span data-ttu-id="076d7-751">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="076d7-751">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="076d7-752">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-752">**Why**</span></span>

<span data-ttu-id="076d7-753">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="076d7-753">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="076d7-754">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-754">**Mitigations**</span></span>

<span data-ttu-id="076d7-755">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="076d7-755">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="076d7-756">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="076d7-756">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="076d7-757">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-757">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="076d7-758">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-758">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="076d7-759">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="076d7-759">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="076d7-760">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-760">**Old behavior**</span></span>

<span data-ttu-id="076d7-761">`IDbContextOptionsExtension`, uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="076d7-761">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="076d7-762">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-762">**New behavior**</span></span>

<span data-ttu-id="076d7-763">Bu yöntemler yeni bir `IDbContextOptionsExtension.Info` özelliğinden döndürülen yeni bir @no__t 0 soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-763">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="076d7-764">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-764">**Why**</span></span>

<span data-ttu-id="076d7-765">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="076d7-765">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="076d7-766">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="076d7-766">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="076d7-767">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-767">**Mitigations**</span></span>

<span data-ttu-id="076d7-768">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="076d7-768">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="076d7-769">EF Core kaynak kodundaki farklı uzantı türleri için `IDbContextOptionsExtension` ' ın birçok uygulamasında örnekler bulunur.</span><span class="sxs-lookup"><span data-stu-id="076d7-769">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="076d7-770">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-770">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="076d7-771">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="076d7-771">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="076d7-772">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="076d7-772">**Change**</span></span>

<span data-ttu-id="076d7-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning` olarak yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="076d7-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="076d7-774">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-774">**Why**</span></span>

<span data-ttu-id="076d7-775">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="076d7-775">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="076d7-776">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-776">**Mitigations**</span></span>

<span data-ttu-id="076d7-777">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-777">Use the new name.</span></span> <span data-ttu-id="076d7-778">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="076d7-778">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="076d7-779">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="076d7-779">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="076d7-780">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="076d7-780">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="076d7-781">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-781">**Old behavior**</span></span>

<span data-ttu-id="076d7-782">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-782">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="076d7-783">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-783">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="076d7-784">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-784">**New behavior**</span></span>

<span data-ttu-id="076d7-785">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="076d7-785">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="076d7-786">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-786">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="076d7-787">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-787">**Why**</span></span>

<span data-ttu-id="076d7-788">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="076d7-788">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="076d7-789">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-789">**Mitigations**</span></span>

<span data-ttu-id="076d7-790">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-790">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="076d7-791">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="076d7-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="076d7-792">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="076d7-792">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="076d7-793">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-793">**Old behavior**</span></span>

<span data-ttu-id="076d7-794">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="076d7-794">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="076d7-795">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-795">**New behavior**</span></span>

<span data-ttu-id="076d7-796">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="076d7-796">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="076d7-797">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-797">**Why**</span></span>

<span data-ttu-id="076d7-798">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-798">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="076d7-799">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-799">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="076d7-800">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-800">**Mitigations**</span></span>

<span data-ttu-id="076d7-801">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="076d7-801">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="076d7-802">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="076d7-802">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="076d7-803">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="076d7-803">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="076d7-804">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-804">**Old behavior**</span></span>

<span data-ttu-id="076d7-805">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-805">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="076d7-806">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-806">**New behavior**</span></span>

<span data-ttu-id="076d7-807">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-807">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="076d7-808">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="076d7-808">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="076d7-809">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-809">**Why**</span></span>

<span data-ttu-id="076d7-810">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="076d7-810">This package is only intended to be used at design time.</span></span> <span data-ttu-id="076d7-811">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="076d7-811">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="076d7-812">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="076d7-812">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="076d7-813">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-813">**Mitigations**</span></span>

<span data-ttu-id="076d7-814">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="076d7-814">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="076d7-815">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="076d7-815">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="076d7-816">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="076d7-816">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="076d7-817">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="076d7-817">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="076d7-818">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-818">**Old behavior**</span></span>

<span data-ttu-id="076d7-819">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="076d7-819">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="076d7-820">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-820">**New behavior**</span></span>

<span data-ttu-id="076d7-821">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="076d7-821">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="076d7-822">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-822">**Why**</span></span>

<span data-ttu-id="076d7-823">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="076d7-823">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="076d7-824">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="076d7-824">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="076d7-825">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-825">**Mitigations**</span></span>

<span data-ttu-id="076d7-826">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="076d7-826">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="076d7-827">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="076d7-827">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="076d7-828">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="076d7-828">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="076d7-829">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="076d7-829">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="076d7-830">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-830">**Old behavior**</span></span>

<span data-ttu-id="076d7-831">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="076d7-831">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="076d7-832">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-832">**New behavior**</span></span>

<span data-ttu-id="076d7-833">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="076d7-833">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="076d7-834">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-834">**Why**</span></span>

<span data-ttu-id="076d7-835">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="076d7-835">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="076d7-836">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-836">**Mitigations**</span></span>

<span data-ttu-id="076d7-837">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="076d7-837">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="076d7-838">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="076d7-838">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="076d7-839">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="076d7-839">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="076d7-840">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="076d7-840">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="076d7-841">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-841">**Old behavior**</span></span>

<span data-ttu-id="076d7-842">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="076d7-842">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="076d7-843">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-843">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="076d7-844">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-844">**New behavior**</span></span>

<span data-ttu-id="076d7-845">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="076d7-845">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="076d7-846">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-846">**Why**</span></span>

<span data-ttu-id="076d7-847">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="076d7-847">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="076d7-848">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-848">**Mitigations**</span></span>

<span data-ttu-id="076d7-849">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="076d7-849">Use full configuration of the relationship.</span></span> <span data-ttu-id="076d7-850">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="076d7-850">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="076d7-851">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="076d7-851">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="076d7-852">Sorun izleniyor #12757</span><span class="sxs-lookup"><span data-stu-id="076d7-852">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="076d7-853">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-853">**Old behavior**</span></span>

<span data-ttu-id="076d7-854">Şema ile boş bir dize olarak yapılandırılmış bir DbFunction, şema olmadan yerleşik işlev olarak değerlendirildi.</span><span class="sxs-lookup"><span data-stu-id="076d7-854">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="076d7-855">Örneğin, aşağıdaki kod `DatePart` CLR işlevini SqlServer üzerinde `DATEPART` yerleşik işlevine eşleyecek.</span><span class="sxs-lookup"><span data-stu-id="076d7-855">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="076d7-856">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="076d7-856">**New behavior**</span></span>

<span data-ttu-id="076d7-857">Tüm DbFunction eşlemeleri Kullanıcı tanımlı işlevlere eşlenildiği kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-857">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="076d7-858">Bu nedenle boş dize değeri, işlevi model için varsayılan şemanın içine yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="076d7-858">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="076d7-859">Bu şema, Fluent API `modelBuilder.HasDefaultSchema()` veya `dbo` aracılığıyla açıkça yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="076d7-859">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="076d7-860">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="076d7-860">**Why**</span></span>

<span data-ttu-id="076d7-861">Daha önceden şemanın boş olması, işlevin yerleşik olduğunu değerlendirmek için bir yoldur, ancak bu mantık yalnızca yerleşik işlevlerin herhangi bir şemaya ait olmadığı SqlServer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="076d7-861">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="076d7-862">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="076d7-862">**Mitigations**</span></span>

<span data-ttu-id="076d7-863">DbFunction 'ın çevirisini yerleşik bir işlevle eşlemek için el ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="076d7-863">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
