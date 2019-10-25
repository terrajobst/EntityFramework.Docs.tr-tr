---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: b2e3881e3454377dab7851cba999ed6b891def4e
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812129"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="e0bbb-102">EF Core 3,0 ' de yer alan son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="e0bbb-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="e0bbb-103">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken mevcut uygulamaları bozmak için olası bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="e0bbb-104">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="e0bbb-105">Özet</span><span class="sxs-lookup"><span data-stu-id="e0bbb-105">Summary</span></span>

| <span data-ttu-id="e0bbb-106">**Son değişiklik**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="e0bbb-107">**ACT**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="e0bbb-108">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="e0bbb-109">Yüksek</span><span class="sxs-lookup"><span data-stu-id="e0bbb-109">High</span></span>       |
| [<span data-ttu-id="e0bbb-110">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="e0bbb-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="e0bbb-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="e0bbb-111">High</span></span>      |
| [<span data-ttu-id="e0bbb-112">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="e0bbb-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="e0bbb-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="e0bbb-113">High</span></span>      |
| [<span data-ttu-id="e0bbb-114">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="e0bbb-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="e0bbb-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="e0bbb-115">High</span></span>      |
| [<span data-ttu-id="e0bbb-116">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="e0bbb-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="e0bbb-117">High</span></span>      |
| [<span data-ttu-id="e0bbb-118">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="e0bbb-119">Yüksek</span><span class="sxs-lookup"><span data-stu-id="e0bbb-119">High</span></span>      |
| [<span data-ttu-id="e0bbb-120">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="e0bbb-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="e0bbb-121">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-121">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-122">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="e0bbb-123">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-123">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-124">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="e0bbb-125">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-125">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-126">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="e0bbb-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="e0bbb-127">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-127">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-128">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="e0bbb-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="e0bbb-129">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-129">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-130">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="e0bbb-131">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-131">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-132">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="e0bbb-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="e0bbb-133">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-133">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-134">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="e0bbb-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="e0bbb-135">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-135">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-136">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="e0bbb-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="e0bbb-137">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-137">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-138">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="e0bbb-139">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-139">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-140">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="e0bbb-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="e0bbb-141">Orta</span><span class="sxs-lookup"><span data-stu-id="e0bbb-141">Medium</span></span>      |
| [<span data-ttu-id="e0bbb-142">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="e0bbb-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-143">Low</span></span>      |
| [<span data-ttu-id="e0bbb-144">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="e0bbb-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-145">Low</span></span>      |
| [<span data-ttu-id="e0bbb-146">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="e0bbb-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="e0bbb-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-147">Low</span></span>      |
| [<span data-ttu-id="e0bbb-148">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="e0bbb-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-149">Low</span></span>      |
| [<span data-ttu-id="e0bbb-150">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="e0bbb-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-151">Low</span></span>      |
| [<span data-ttu-id="e0bbb-152">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="e0bbb-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-153">Low</span></span>      |
| [<span data-ttu-id="e0bbb-154">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="e0bbb-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-155">Low</span></span>      |
| [<span data-ttu-id="e0bbb-156">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="e0bbb-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-157">Low</span></span>      |
| [<span data-ttu-id="e0bbb-158">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="e0bbb-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-159">Low</span></span>      |
| [<span data-ttu-id="e0bbb-160">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="e0bbb-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="e0bbb-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-161">Low</span></span>      |
| [<span data-ttu-id="e0bbb-162">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="e0bbb-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-163">Low</span></span>      |
| [<span data-ttu-id="e0bbb-164">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="e0bbb-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-165">Low</span></span>      |
| [<span data-ttu-id="e0bbb-166">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="e0bbb-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-167">Low</span></span>      |
| [<span data-ttu-id="e0bbb-168">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="e0bbb-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="e0bbb-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-169">Low</span></span>      |
| [<span data-ttu-id="e0bbb-170">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="e0bbb-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-171">Low</span></span>      |
| [<span data-ttu-id="e0bbb-172">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="e0bbb-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="e0bbb-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-173">Low</span></span>      |
| [<span data-ttu-id="e0bbb-174">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="e0bbb-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-175">Low</span></span>      |
| [<span data-ttu-id="e0bbb-176">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="e0bbb-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="e0bbb-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-177">Low</span></span>      |
| [<span data-ttu-id="e0bbb-178">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="e0bbb-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-179">Low</span></span>      |
| [<span data-ttu-id="e0bbb-180">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="e0bbb-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="e0bbb-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-181">Low</span></span>      |
| [<span data-ttu-id="e0bbb-182">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="e0bbb-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="e0bbb-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-183">Low</span></span>      |
| [<span data-ttu-id="e0bbb-184">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="e0bbb-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="e0bbb-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-185">Low</span></span>      |
| [<span data-ttu-id="e0bbb-186">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="e0bbb-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-187">Low</span></span>      |
| [<span data-ttu-id="e0bbb-188">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="e0bbb-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-189">Low</span></span>      |
| [<span data-ttu-id="e0bbb-190">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="e0bbb-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-191">Low</span></span>      |
| [<span data-ttu-id="e0bbb-192">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="e0bbb-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="e0bbb-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-193">Low</span></span>      |
| [<span data-ttu-id="e0bbb-194">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="e0bbb-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-195">Low</span></span>      |
| [<span data-ttu-id="e0bbb-196">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="e0bbb-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-197">Low</span></span>      |
| [<span data-ttu-id="e0bbb-198">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="e0bbb-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="e0bbb-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-199">Low</span></span>      |
| [<span data-ttu-id="e0bbb-200">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="e0bbb-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-201">Low</span></span>      |
| [<span data-ttu-id="e0bbb-202">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="e0bbb-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-203">Low</span></span>      |
| [<span data-ttu-id="e0bbb-204">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="e0bbb-205">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-205">Low</span></span>      |
| [<span data-ttu-id="e0bbb-206">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="e0bbb-207">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-207">Low</span></span>      |
| [<span data-ttu-id="e0bbb-208">System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-208">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="e0bbb-209">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-209">Low</span></span>      |
| [<span data-ttu-id="e0bbb-210">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-210">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="e0bbb-211">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-211">Low</span></span>      |
| [<span data-ttu-id="e0bbb-212">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-212">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="e0bbb-213">Düşük</span><span class="sxs-lookup"><span data-stu-id="e0bbb-213">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="e0bbb-214">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-214">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="e0bbb-215">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[ayrıca bkz. #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="e0bbb-215">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="e0bbb-216">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-216">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-217">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-217">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="e0bbb-218">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-218">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="e0bbb-219">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-219">**New behavior**</span></span>

<span data-ttu-id="e0bbb-220">3,0 ile başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrısı) istemci üzerinde değerlendirilmek için yalnızca ifadeye izin verir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-220">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="e0bbb-221">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-221">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="e0bbb-222">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-222">**Why**</span></span>

<span data-ttu-id="e0bbb-223">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-223">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="e0bbb-224">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-224">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="e0bbb-225">Örneğin, çevrilemeyen `Where()` çağrısındaki bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-225">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="e0bbb-226">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-226">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="e0bbb-227">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-227">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="e0bbb-228">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-228">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="e0bbb-229">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-229">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-230">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın veya `AsEnumerable()`, `ToList()` ' i kullanabilir ya da daha sonra verileri LINQ-Objects kullanılarak daha sonra işlenebileceği istemciye açık bir şekilde geri getirmek için benzer.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-230">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="e0bbb-231">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="e0bbb-231">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="e0bbb-232">Sorun izleniyor #15498</span><span class="sxs-lookup"><span data-stu-id="e0bbb-232">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="e0bbb-233">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-233">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-234">3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-234">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="e0bbb-235">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-235">**New behavior**</span></span>

<span data-ttu-id="e0bbb-236">3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-236">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="e0bbb-237">Bu, .NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-237">This does not include .NET Framework.</span></span>

<span data-ttu-id="e0bbb-238">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-238">**Why**</span></span>

<span data-ttu-id="e0bbb-239">Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-239">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="e0bbb-240">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-240">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-241">Modern bir .NET platformuna geçmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-241">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="e0bbb-242">Bu mümkün değilse, her ikisi de .NET Framework EF Core 2,1 veya EF Core 2,2 ' i kullanmaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-242">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="e0bbb-243">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="e0bbb-243">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="e0bbb-244">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="e0bbb-244">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="e0bbb-245">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-245">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-246">ASP.NET Core 3,0 önce, `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All` ' e bir paket başvurusu eklediğinizde, EF Core ve EF Core sağlayıcı gibi SQL Server veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-246">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="e0bbb-247">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-247">**New behavior**</span></span>

<span data-ttu-id="e0bbb-248">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-248">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="e0bbb-249">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-249">**Why**</span></span>

<span data-ttu-id="e0bbb-250">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-250">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="e0bbb-251">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-251">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="e0bbb-252">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-252">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="e0bbb-253">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-253">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="e0bbb-254">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-254">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-255">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-255">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="e0bbb-256">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="e0bbb-256">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="e0bbb-257">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="e0bbb-257">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="e0bbb-258">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-258">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-259">3,0 tarihinden önce, `dotnet ef` aracı .NET Core SDK eklenmiştir ve ek adımlara gerek duymadan herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-259">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="e0bbb-260">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-260">**New behavior**</span></span>

<span data-ttu-id="e0bbb-261">3,0 ' den başlayarak, .NET SDK `dotnet ef` aracını içermez, bu nedenle kullanabilmeniz için onu yerel veya genel bir araç olarak açıkça yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-261">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="e0bbb-262">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-262">**Why**</span></span>

<span data-ttu-id="e0bbb-263">Bu değişiklik, EF Core 3,0 'nin her zaman bir NuGet paketi olarak dağıtılması açısından, NuGet üzerinde düzenli bir .NET CLı aracı olarak `dotnet ef` ' ı dağıtmamızı ve güncelleştirmemizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-263">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="e0bbb-264">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-264">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-265">`DbContext`geçişleri veya yapı iskelesi yönetmek için `dotnet-ef` genel bir araç olarak yükler:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-265">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="e0bbb-266">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-266">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="e0bbb-267">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-267">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="e0bbb-268">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="e0bbb-268">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="e0bbb-269">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-269">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-270">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="e0bbb-271">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-271">**New behavior**</span></span>

<span data-ttu-id="e0bbb-272">EF Core 3,0 ' den başlayarak, parametrelerin sorgu dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlRaw`, `ExecuteSqlRaw` ve `ExecuteSqlRawAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="e0bbb-273">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="e0bbb-274">Parametrelerin, enterpolasyonlu bir sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlInterpolated`, `ExecuteSqlInterpolated` ve `ExecuteSqlInterpolatedAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="e0bbb-275">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="e0bbb-276">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="e0bbb-277">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-277">**Why**</span></span>

<span data-ttu-id="e0bbb-278">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="e0bbb-279">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="e0bbb-280">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-280">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-281">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-281">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="e0bbb-282">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="e0bbb-282">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="e0bbb-283">Sorun izleniyor #15392</span><span class="sxs-lookup"><span data-stu-id="e0bbb-283">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="e0bbb-284">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-284">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-285">3,0 EF Core önce, FromSql yöntemi, geçirilen SQL 'in üzerine oluşmayabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-285">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="e0bbb-286">SQL, saklı yordam gibi birleştirilmemiş bir işlem olduğu zaman istemci değerlendirmesi gerçekleştirdi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-286">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="e0bbb-287">Aşağıdaki sorgu, sunucuda saklı yordam çalıştırılarak ve istemci tarafında FirstOrDefault ' i gerçekleştirerek çalıştı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-287">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="e0bbb-288">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-288">**New behavior**</span></span>

<span data-ttu-id="e0bbb-289">EF Core 3,0 ' den başlayarak, EF Core SQL ayrıştırmaya çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-289">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="e0bbb-290">Bu nedenle, FromSqlRaw/Fromsqlenterpolasyonından sonra oluşturuyorsanız, EF Core alt sorguya neden olarak SQL 'i oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-290">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="e0bbb-291">Bu nedenle, bileşim ile saklı yordam kullanıyorsanız, geçersiz SQL söz dizimi için bir özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-291">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="e0bbb-292">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-292">**Why**</span></span>

<span data-ttu-id="e0bbb-293">EF Core 3,0, [burada](#linq-queries-are-no-longer-evaluated-on-the-client)açıklandığı gibi bir hata olduğundan, otomatik istemci değerlendirmesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-293">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="e0bbb-294">**Mayı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-294">**Mitigation**</span></span>

<span data-ttu-id="e0bbb-295">FromSqlRaw/Fromsqlenterpolasyonnda saklı bir yordam kullanıyorsanız, bunun üzerine oluşmadığını bilirsiniz; böylece sunucu tarafında herhangi bir kompozisyonu önlemek için FromSql yöntem çağrısından sonra __asslanabilir/AsAsyncEnumerable__ ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-295">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="e0bbb-296">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-296">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="e0bbb-297">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="e0bbb-297">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="e0bbb-298">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-298">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-299">EF Core 3,0 önce, `FromSql` yöntemi sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-299">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="e0bbb-300">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-300">**New behavior**</span></span>

<span data-ttu-id="e0bbb-301">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (`FromSql` ' yi değiştirme) yalnızca sorgu köklerinin üzerinde belirtilebilir, yani doğrudan `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-301">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="e0bbb-302">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-302">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="e0bbb-303">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-303">**Why**</span></span>

<span data-ttu-id="e0bbb-304">`DbSet` üzerinde `FromSql` herhangi bir yerde, hiçbir anlamı veya eklenen değeri belirtmediyse, bazı senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-304">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="e0bbb-305">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-305">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-306">`FromSql` çağırmaları, uygulandıkları `DbSet` ' e doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-306">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="e0bbb-307">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="e0bbb-307">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="e0bbb-308">Sorun izleniyor #13518</span><span class="sxs-lookup"><span data-stu-id="e0bbb-308">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="e0bbb-309">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-309">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-310">EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-310">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="e0bbb-311">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-311">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="e0bbb-312">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-312">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="e0bbb-313">, belirtilen kategori ile ilişkili her bir `Product` için aynı `Category` örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-313">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="e0bbb-314">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-314">**New behavior**</span></span>

<span data-ttu-id="e0bbb-315">EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-315">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="e0bbb-316">Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde bile her `Product` için yeni bir `Category` örneği döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-316">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="e0bbb-317">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-317">**Why**</span></span>

<span data-ttu-id="e0bbb-318">Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-318">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="e0bbb-319">Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-319">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="e0bbb-320">Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-320">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="e0bbb-321">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-321">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-322">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-322">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="e0bbb-323">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-323">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="e0bbb-324">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="e0bbb-324">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="e0bbb-325">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-325">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="e0bbb-326">Örneğin, SQL 'in günlüğe kaydedilmesini `Debug` ' a geçmek için `OnConfiguring` veya `AddDbContext` ' deki düzeyi açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-326">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="e0bbb-327">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="e0bbb-327">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="e0bbb-328">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="e0bbb-328">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="e0bbb-329">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-329">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-330">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-330">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="e0bbb-331">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-331">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="e0bbb-332">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-332">**New behavior**</span></span>

<span data-ttu-id="e0bbb-333">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-333">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="e0bbb-334">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-334">**Why**</span></span>

<span data-ttu-id="e0bbb-335">Daha önce bazı `DbContext` örnekleri tarafından izlenen bir varlık farklı bir `DbContext` örneğine taşındığında, geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-335">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="e0bbb-336">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-336">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-337">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlara birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup `Added` durumundaki varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-337">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="e0bbb-338">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-338">This can be avoided by:</span></span>
* <span data-ttu-id="e0bbb-339">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-339">Not using store-generated keys.</span></span>
* <span data-ttu-id="e0bbb-340">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-340">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="e0bbb-341">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-341">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="e0bbb-342">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` `blog.Id` kendisi ayarlanmamışsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-342">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="e0bbb-343">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="e0bbb-343">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="e0bbb-344">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="e0bbb-344">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="e0bbb-345">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-345">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-346">EF Core 3,0 ' dan önce, `DetectChanges` tarafından bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve `SaveChanges` çağrıldığında yeni bir satır olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-346">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="e0bbb-347">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-347">**New behavior**</span></span>

<span data-ttu-id="e0bbb-348">EF Core 3,0 ' den başlayarak, bir varlık oluşturulan anahtar değerlerini kullanıyorsa ve bazı anahtar değer ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-348">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="e0bbb-349">Bu, varlık için bir satırın var olduğu varsayılır ve `SaveChanges` çağrıldığında güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-349">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="e0bbb-350">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-350">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="e0bbb-351">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-351">**Why**</span></span>

<span data-ttu-id="e0bbb-352">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-352">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="e0bbb-353">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-353">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-354">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-354">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="e0bbb-355">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-355">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="e0bbb-356">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-356">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="e0bbb-357">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-357">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="e0bbb-358">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-358">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="e0bbb-359">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="e0bbb-359">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="e0bbb-360">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-360">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-361">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-361">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="e0bbb-362">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-362">**New behavior**</span></span>

<span data-ttu-id="e0bbb-363">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-363">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="e0bbb-364">Örneğin, bir asıl varlığı silmek için `context.Remove()` ' ı çağırmak, izlenen ilişkili ilgili tüm bağımlıların da hemen `Deleted` olarak ayarlanmasının oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-364">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="e0bbb-365">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-365">**Why**</span></span>

<span data-ttu-id="e0bbb-366">Bu değişiklik, `SaveChanges` çağrılmadan _önce_ hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-366">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="e0bbb-367">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-367">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-368">Önceki davranış `context.ChangedTracker` ayarları aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-368">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="e0bbb-369">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-369">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="e0bbb-370">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-370">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="e0bbb-371">Sorun izleniyor #18022</span><span class="sxs-lookup"><span data-stu-id="e0bbb-371">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="e0bbb-372">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-372">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-373">3,0 öncesinde, `Include` işleçleri aracılığıyla koleksiyon gezginlerini yüklemek, her ilgili varlık türü için bir tane olmak üzere ilişkisel veritabanında birden çok sorgunun oluşturulmasına neden oldu.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-373">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="e0bbb-374">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-374">**New behavior**</span></span>

<span data-ttu-id="e0bbb-375">3,0 ile başlayarak, EF Core ilişkisel veritabanlarında birleştirmelere sahip tek bir sorgu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-375">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="e0bbb-376">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-376">**Why**</span></span>

<span data-ttu-id="e0bbb-377">Tek bir LINQ sorgusu uygulamak için birden çok sorgu verilmesi, birden çok veritabanı yuvarlaklığına gerek olduğu için negatif performans dahil olmak üzere çok sayıda soruna neden olmuş ve her sorgu veritabanının farklı bir durumunu gözlemleyebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-377">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="e0bbb-378">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-378">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-379">Teknik olarak bu bir değişiklik olmadığı sürece, tek bir sorgu koleksiyon gezginlerde çok sayıda `Include` işleci içerdiğinde uygulama performansının önemli bir etkisi olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-379">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="e0bbb-380">Daha fazla bilgi edinmek ve sorguları daha verimli bir şekilde yeniden yazmak için [Bu açıklamaya bakın](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="e0bbb-380">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="e0bbb-381">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="e0bbb-381">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="e0bbb-382">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="e0bbb-382">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="e0bbb-383">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-383">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-384">3,0 öncesinde, `DeleteBehavior.Restrict`, veritabanında `Restrict` semantiklerle yabancı anahtarlar oluşturdu, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-384">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="e0bbb-385">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-385">**New behavior**</span></span>

<span data-ttu-id="e0bbb-386">3,0 ile başlayarak, `DeleteBehavior.Restrict`, yabancı anahtarların `Restrict` semantiğinin oluşturulmasını sağlar; Yani bu, basamaksız; sınır iç düzeltmesini etkilemeden kısıtlama ihlalinde throw.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-386">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="e0bbb-387">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-387">**Why**</span></span>

<span data-ttu-id="e0bbb-388">Bu değişiklik, beklenmeyen yan etkileri olmadan `DeleteBehavior` ' ı sezgisel bir şekilde kullanma deneyimini geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-388">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="e0bbb-389">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-389">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-390">Önceki davranış `DeleteBehavior.ClientNoAction` kullanılarak geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-390">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="e0bbb-391">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-391">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="e0bbb-392">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="e0bbb-392">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="e0bbb-393">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-393">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-394">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/keyless-entity-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-394">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="e0bbb-395">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="e0bbb-395">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="e0bbb-396">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-396">**New behavior**</span></span>

<span data-ttu-id="e0bbb-397">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-397">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="e0bbb-398">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-398">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="e0bbb-399">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-399">**Why**</span></span>

<span data-ttu-id="e0bbb-400">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-400">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="e0bbb-401">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-401">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="e0bbb-402">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-402">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="e0bbb-403">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-403">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-404">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-404">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="e0bbb-405">**`ModelBuilder.Query<>()`** -`ModelBuilder.Entity<>().HasNoKey()` ' nin bir varlık türünü anahtar olmadan işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-405">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="e0bbb-406">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-406">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="e0bbb-407">**`DbQuery<>`** -`DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-407">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="e0bbb-408">**`DbContext.Query<>()`** -`DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-408">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="e0bbb-409">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="e0bbb-409">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="e0bbb-410">[Sorun izleme #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
 izleme sorunu[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[izleme sorunu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="e0bbb-410">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="e0bbb-411">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-411">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-412">3,0 EF Core önce, sahip olan ilişkinin yapılandırması `OwnsOne` veya `OwnsMany` çağrısından sonra doğrudan gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-412">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="e0bbb-413">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-413">**New behavior**</span></span>

<span data-ttu-id="e0bbb-414">EF Core 3,0 ' den başlayarak, artık `WithOwner()` kullanarak sahip için bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-414">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="e0bbb-415">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-415">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="e0bbb-416">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin yapılandırılmasına benzer şekilde `WithOwner()` ' dan sonra zinciredilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-416">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="e0bbb-417">Sahip olduğu için yapılandırma `OwnsOne()/OwnsMany()` ' dan sonra hala zincirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-417">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="e0bbb-418">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-418">For example:</span></span>

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

<span data-ttu-id="e0bbb-419">Ayrıca, sahip bir tür hedefi ile `Entity()`, `HasOne()` veya `Set()` çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-419">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="e0bbb-420">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-420">**Why**</span></span>

<span data-ttu-id="e0bbb-421">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-421">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="e0bbb-422">Bu, ' de `HasForeignKey` gibi yöntemler etrafında belirsizlik ve karışıklık ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-422">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="e0bbb-423">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-423">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-424">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-424">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="e0bbb-425">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-425">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="e0bbb-426">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="e0bbb-426">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="e0bbb-427">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-427">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-428">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-428">Consider the following model:</span></span>
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
<span data-ttu-id="e0bbb-429">EF Core 3,0 ' dan önce, `OrderDetails` aynı tabloyla `Order` veya açıkça eşlenmişse, yeni bir `Order`eklenirken `OrderDetails` örneği her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-429">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="e0bbb-430">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-430">**New behavior**</span></span>

<span data-ttu-id="e0bbb-431">EF Core 3,0 ' den başlayarak, bir `OrderDetails` olmadan `Order` ekleme ve birincil anahtar null yapılabilir sütunlara hariç tüm `OrderDetails` özelliklerini eşleme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-431">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="e0bbb-432">EF Core kümelerini sorgularken, gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar ve tüm `null`Özellikler ' in yanı sıra gerekli özellikleri yoksa, `null` `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-432">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="e0bbb-433">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-433">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-434">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinmenin `null` olması beklenmiyorsa, gezinti `null` olduğunda, uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-434">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="e0bbb-435">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış`null` bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-435">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="e0bbb-436">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-436">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="e0bbb-437">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="e0bbb-437">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="e0bbb-438">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-438">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-439">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-439">Consider the following model:</span></span>
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
<span data-ttu-id="e0bbb-440">EF Core 3,0 ' dan önce, `OrderDetails` `Order` ' e aitse veya aynı tabloya açık olarak eşlenmişse, yalnızca `OrderDetails` ' yi güncelleştirmek istemcide `Version` değerini güncelleştirmeyecektir ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-440">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="e0bbb-441">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-441">**New behavior**</span></span>

<span data-ttu-id="e0bbb-442">3,0 ' den itibaren, yeni `Version` değerini `OrderDetails`sahip `Order` EF Core yayar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-442">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="e0bbb-443">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-443">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="e0bbb-444">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-444">**Why**</span></span>

<span data-ttu-id="e0bbb-445">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-445">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="e0bbb-446">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-446">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-447">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-447">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="e0bbb-448">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-448">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="e0bbb-449">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-449">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="e0bbb-450">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="e0bbb-450">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="e0bbb-451">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-451">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-452">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-452">Consider the following model:</span></span>
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

<span data-ttu-id="e0bbb-453">EF Core 3,0 ' dan önce, `ShippingAddress` özelliği varsayılan olarak `BulkOrder` ve `Order` sütunları için ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-453">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="e0bbb-454">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-454">**New behavior**</span></span>

<span data-ttu-id="e0bbb-455">3,0 ile başlayarak, EF Core `ShippingAddress` için yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-455">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="e0bbb-456">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-456">**Why**</span></span>

<span data-ttu-id="e0bbb-457">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-457">The old behavoir was unexpected.</span></span>

<span data-ttu-id="e0bbb-458">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-458">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-459">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-459">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="e0bbb-460">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-460">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="e0bbb-461">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="e0bbb-461">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="e0bbb-462">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-462">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-463">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-463">Consider the following model:</span></span>
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
<span data-ttu-id="e0bbb-464">EF Core 3,0 ' dan önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-464">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="e0bbb-465">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtar `CustomerId` olur ve bu genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-465">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="e0bbb-466">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-466">**New behavior**</span></span>

<span data-ttu-id="e0bbb-467">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-467">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="e0bbb-468">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-468">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="e0bbb-469">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-469">For example:</span></span>

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

<span data-ttu-id="e0bbb-470">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-470">**Why**</span></span>

<span data-ttu-id="e0bbb-471">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-471">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="e0bbb-472">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-472">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-473">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-473">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="e0bbb-474">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-474">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="e0bbb-475">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="e0bbb-475">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="e0bbb-476">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-476">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-477">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope` içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-477">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="e0bbb-478">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-478">**New behavior**</span></span>

<span data-ttu-id="e0bbb-479">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-479">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="e0bbb-480">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-480">**Why**</span></span>

<span data-ttu-id="e0bbb-481">Bu değişiklik, aynı `TransactionScope` ' da birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-481">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="e0bbb-482">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-482">The new behavior also matches EF6.</span></span>

<span data-ttu-id="e0bbb-483">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-483">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-484">Bağlantının `OpenConnection()` ' a açık açık çağrı kalması gerekiyorsa, EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-484">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="e0bbb-485">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-485">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="e0bbb-486">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="e0bbb-486">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="e0bbb-487">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-487">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-488">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-488">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="e0bbb-489">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-489">**New behavior**</span></span>

<span data-ttu-id="e0bbb-490">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-490">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="e0bbb-491">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-491">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="e0bbb-492">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-492">**Why**</span></span>

<span data-ttu-id="e0bbb-493">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-493">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="e0bbb-494">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-494">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-495">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-495">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="e0bbb-496">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-496">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="e0bbb-497">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-497">Backing fields are used by default</span></span>

[<span data-ttu-id="e0bbb-498">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="e0bbb-498">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="e0bbb-499">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-499">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-500">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-500">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="e0bbb-501">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-501">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="e0bbb-502">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-502">**New behavior**</span></span>

<span data-ttu-id="e0bbb-503">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-503">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="e0bbb-504">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-504">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="e0bbb-505">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-505">**Why**</span></span>

<span data-ttu-id="e0bbb-506">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-506">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="e0bbb-507">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-507">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-508">3,0 öncesi davranış `ModelBuilder` ' da özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-508">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="e0bbb-509">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-509">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="e0bbb-510">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="e0bbb-510">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="e0bbb-511">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="e0bbb-511">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="e0bbb-512">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-512">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-513">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-513">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="e0bbb-514">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-514">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="e0bbb-515">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-515">**New behavior**</span></span>

<span data-ttu-id="e0bbb-516">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-516">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="e0bbb-517">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-517">**Why**</span></span>

<span data-ttu-id="e0bbb-518">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-518">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="e0bbb-519">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-519">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-520">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-520">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="e0bbb-521">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-521">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="e0bbb-522">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-522">Field-only property names should match the field name</span></span>

<span data-ttu-id="e0bbb-523">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-523">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-524">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-524">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="e0bbb-525">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-525">**New behavior**</span></span>

<span data-ttu-id="e0bbb-526">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-526">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="e0bbb-527">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-527">**Why**</span></span>

<span data-ttu-id="e0bbb-528">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-528">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="e0bbb-529">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-529">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-530">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-530">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="e0bbb-531">3,0 sonrasında EF Core gelecek bir sürümünde, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="e0bbb-531">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="e0bbb-532">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-532">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="e0bbb-533">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="e0bbb-533">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="e0bbb-534">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-534">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-535">EF Core 3,0 ' dan önce, `AddDbContext` veya `AddDbContextPool` ' i çağırmak, günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini D. I ile birlikte [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrıları aracılığıyla da kaydeder.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-535">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="e0bbb-536">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-536">**New behavior**</span></span>

<span data-ttu-id="e0bbb-537">EF Core 3,0 ' den başlayarak, `AddDbContext` ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-537">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="e0bbb-538">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-538">**Why**</span></span>

<span data-ttu-id="e0bbb-539">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-539">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="e0bbb-540">Ancak, `ILoggerFactory`, uygulamanın DI kapsayıcısında kayıtlıysa, EF Core tarafından kullanılmaya devam edilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-540">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="e0bbb-541">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-541">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-542">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-542">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="e0bbb-543">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-543">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="e0bbb-544">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="e0bbb-544">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="e0bbb-545">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-545">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-546">EF Core 3,0 ' dan önce, `DbContext.Entry` ' ı çağırmak izlenen tüm varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-546">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="e0bbb-547">Bu, `EntityEntry` ' da kullanıma sunulan durumun güncel olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-547">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="e0bbb-548">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-548">**New behavior**</span></span>

<span data-ttu-id="e0bbb-549">EF Core 3,0 ' den başlayarak, `DbContext.Entry` ' ı çağırmak artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-549">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="e0bbb-550">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-550">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="e0bbb-551">`ChangeTracker.AutoDetectChangesEnabled` `false` olarak ayarlanırsa, bu yerel değişikliğin algılanması de devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-551">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="e0bbb-552">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` ve `SaveChanges`--yine de izlenen tüm varlıkların tam `DetectChanges` neden olur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-552">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="e0bbb-553">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-553">**Why**</span></span>

<span data-ttu-id="e0bbb-554">Bu değişiklik, `context.Entry` kullanmanın varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-554">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="e0bbb-555">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-555">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-556">3,0 öncesi davranışı sağlamak için `Entry` çağrılmadan önce `ChgangeTracker.DetectChanges()` ' ı çağırın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-556">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="e0bbb-557">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="e0bbb-557">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="e0bbb-558">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="e0bbb-558">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="e0bbb-559">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-559">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-560">EF Core 3,0 önce `string` ve `byte[]` anahtar özellikleri açık bir şekilde null olmayan bir değer ayarlamadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-560">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="e0bbb-561">Böyle bir durumda, anahtar değeri istemci üzerinde `byte[]`için bayt olarak serileştirilmiş bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-561">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="e0bbb-562">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-562">**New behavior**</span></span>

<span data-ttu-id="e0bbb-563">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-563">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="e0bbb-564">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-564">**Why**</span></span>

<span data-ttu-id="e0bbb-565">Bu değişiklik, istemci tarafından oluşturulan `string`/`byte[]` değerleri genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-565">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="e0bbb-566">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-566">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-567">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-567">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="e0bbb-568">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-568">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="e0bbb-569">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-569">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="e0bbb-570">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-570">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="e0bbb-571">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="e0bbb-571">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="e0bbb-572">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-572">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-573">EF Core 3,0 ' dan önce, `ILoggerFactory` tek bir hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-573">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="e0bbb-574">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-574">**New behavior**</span></span>

<span data-ttu-id="e0bbb-575">EF Core 3,0 ' den başlayarak, `ILoggerFactory` artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-575">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="e0bbb-576">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-576">**Why**</span></span>

<span data-ttu-id="e0bbb-577">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi Pathik davranışlarının bazı örneklerini kaldıran `DbContext` örneğiyle bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-577">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="e0bbb-578">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-578">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-579">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-579">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="e0bbb-580">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-580">This isn't common.</span></span>
<span data-ttu-id="e0bbb-581">Bu durumlarda, çoğu şey çalışmaya devam eder, ancak `ILoggerFactory` ' a bağlı olan herhangi bir tek hizmetin, `ILoggerFactory` ' i farklı bir şekilde alması için değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-581">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="e0bbb-582">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) ' nde bir sorun `ILoggerFactory` bildirin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-582">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="e0bbb-583">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="e0bbb-583">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="e0bbb-584">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="e0bbb-584">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="e0bbb-585">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-585">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-586">EF Core 3,0 ' dan önce, bir `DbContext` ' ı bir kez atıldıktan sonra, söz konusu bağlamdan alınan bir varlıkta verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-586">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="e0bbb-587">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-587">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="e0bbb-588">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-588">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="e0bbb-589">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-589">**New behavior**</span></span>

<span data-ttu-id="e0bbb-590">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-590">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="e0bbb-591">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-591">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="e0bbb-592">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-592">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="e0bbb-593">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-593">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="e0bbb-594">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-594">**Why**</span></span>

<span data-ttu-id="e0bbb-595">Bu değişiklik, atılmış bir `DbContext` örneğinde yavaş yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-595">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="e0bbb-596">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-596">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-597">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-597">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="e0bbb-598">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-598">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="e0bbb-599">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="e0bbb-599">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="e0bbb-600">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-600">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-601">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-601">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="e0bbb-602">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-602">**New behavior**</span></span>

<span data-ttu-id="e0bbb-603">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-603">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="e0bbb-604">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-604">**Why**</span></span>

<span data-ttu-id="e0bbb-605">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="e0bbb-605">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="e0bbb-606">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-606">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-607">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-607">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="e0bbb-608">Ancak, hata, `DbContextOptionsBuilder` ' da yapılandırma yoluyla bir uyarıya (veya yoksayıldı) geri dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-608">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="e0bbb-609">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-609">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="e0bbb-610">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="e0bbb-610">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="e0bbb-611">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="e0bbb-611">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="e0bbb-612">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-612">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-613">EF Core 3,0 öncesinde, tek bir dizeyle `HasOne` veya `HasMany` çağıran kod kafa karıştırıcı bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="e0bbb-614">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="e0bbb-615">Kod, özel olabilecek `Entrance` gezinti özelliğini kullanarak `Samurai` ' ı diğer bir varlık türü ile ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="e0bbb-616">Gerçekte, bu kod, gezinti özelliği olmayan `Entrance` adlı bir varlık türüne ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="e0bbb-617">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-617">**New behavior**</span></span>

<span data-ttu-id="e0bbb-618">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="e0bbb-619">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-619">**Why**</span></span>

<span data-ttu-id="e0bbb-620">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="e0bbb-621">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-621">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-622">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="e0bbb-623">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-623">This is not common.</span></span>
<span data-ttu-id="e0bbb-624">Önceki davranış, gezinti özelliği adı için `null` ' ı açıkça geçirerek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="e0bbb-625">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="e0bbb-626">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="e0bbb-627">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="e0bbb-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="e0bbb-628">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-628">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-629">Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürdü:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="e0bbb-630">`ValueGenerator.NextValueAsync()` (ve türetilen sınıflar)</span><span class="sxs-lookup"><span data-stu-id="e0bbb-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="e0bbb-631">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-631">**New behavior**</span></span>

<span data-ttu-id="e0bbb-632">Yukarıda bahsedilen yöntemler bundan sonra aynı `T` üzerinden `ValueTask<T>` döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="e0bbb-633">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-633">**Why**</span></span>

<span data-ttu-id="e0bbb-634">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="e0bbb-635">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-635">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-636">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="e0bbb-637">Daha karmaşık bir kullanım (örn. döndürülen `Task` `Task.WhenAny()`geçirilmesi), genellikle döndürülen `ValueTask<T>` `AsTask()` çağırarak `Task<T>` dönüştürmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="e0bbb-638">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="e0bbb-639">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="e0bbb-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="e0bbb-640">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="e0bbb-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="e0bbb-641">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-641">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-642">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="e0bbb-643">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-643">**New behavior**</span></span>

<span data-ttu-id="e0bbb-644">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="e0bbb-645">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-645">**Why**</span></span>

<span data-ttu-id="e0bbb-646">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="e0bbb-647">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-647">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-648">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="e0bbb-649">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="e0bbb-650">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="e0bbb-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="e0bbb-651">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="e0bbb-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="e0bbb-652">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-652">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-653">EF Core 3,0 öncesinde, türetilmiş bir tür üzerinde çağrılan `ToTable()`, yalnızca devralma eşleme stratejisi bu geçersiz olduğu için yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-653">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="e0bbb-654">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-654">**New behavior**</span></span>

<span data-ttu-id="e0bbb-655">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, türetilmiş bir tür üzerinde çağrılan `ToTable()`, bundan sonra beklenmeyen bir eşleme değişikliğini önlemek için bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-655">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="e0bbb-656">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-656">**Why**</span></span>

<span data-ttu-id="e0bbb-657">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-657">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="e0bbb-658">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-658">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="e0bbb-659">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-659">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-660">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-660">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="e0bbb-661">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="e0bbb-661">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="e0bbb-662">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="e0bbb-662">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="e0bbb-663">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-663">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-664">EF Core 3,0 ' dan önce, `ForSqlServerHasIndex().ForSqlServerInclude()` `INCLUDE` ile kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-664">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="e0bbb-665">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-665">**New behavior**</span></span>

<span data-ttu-id="e0bbb-666">EF Core 3,0 ' den itibaren, bir dizinde `Include` kullanılması artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-666">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="e0bbb-667">`HasIndex().ForSqlServerInclude()`kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-667">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="e0bbb-668">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-668">**Why**</span></span>

<span data-ttu-id="e0bbb-669">Bu değişiklik, tüm veritabanı sağlayıcıları için `Include` olan dizinler için API 'yi tek bir yerde birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-669">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="e0bbb-670">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-670">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-671">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-671">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="e0bbb-672">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="e0bbb-672">Metadata API changes</span></span>

[<span data-ttu-id="e0bbb-673">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="e0bbb-673">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="e0bbb-674">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-674">**New behavior**</span></span>

<span data-ttu-id="e0bbb-675">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-675">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="e0bbb-676">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-676">**Why**</span></span>

<span data-ttu-id="e0bbb-677">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-677">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="e0bbb-678">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-678">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-679">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-679">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="e0bbb-680">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="e0bbb-680">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="e0bbb-681">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="e0bbb-681">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="e0bbb-682">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-682">**New behavior**</span></span>

<span data-ttu-id="e0bbb-683">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-683">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="e0bbb-684">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-684">**Why**</span></span>

<span data-ttu-id="e0bbb-685">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-685">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="e0bbb-686">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-686">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-687">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-687">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="e0bbb-688">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="e0bbb-688">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="e0bbb-689">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="e0bbb-689">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="e0bbb-690">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-690">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-691">3,0 EF Core önce, bir SQLite bağlantısı açıldığında EF Core `PRAGMA foreign_keys = 1` gönderebilirim.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-691">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="e0bbb-692">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-692">**New behavior**</span></span>

<span data-ttu-id="e0bbb-693">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı açıldığında EF Core artık `PRAGMA foreign_keys = 1` göndermez.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-693">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="e0bbb-694">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-694">**Why**</span></span>

<span data-ttu-id="e0bbb-695">Bu değişiklik, EF Core varsayılan olarak `SQLitePCLRaw.bundle_e_sqlite3` kullandığından, bu, sırasıyla FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-695">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="e0bbb-696">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-696">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-697">Yabancı anahtarlar varsayılan olarak, EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 içinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-697">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="e0bbb-698">Diğer durumlar için, bağlantı dizeniz içinde `Foreign Keys=True` belirterek yabancı anahtarlar etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-698">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="e0bbb-699">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-699">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="e0bbb-700">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-700">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-701">3,0 EF Core önce EF Core `SQLitePCLRaw.bundle_green`kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-701">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="e0bbb-702">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-702">**New behavior**</span></span>

<span data-ttu-id="e0bbb-703">EF Core 3,0 ' den başlayarak EF Core `SQLitePCLRaw.bundle_e_sqlite3`kullanır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-703">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="e0bbb-704">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-704">**Why**</span></span>

<span data-ttu-id="e0bbb-705">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-705">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="e0bbb-706">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-706">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-707">İOS 'ta yerel SQLite sürümünü kullanmak için `Microsoft.Data.Sqlite` ' ı farklı bir `SQLitePCLRaw` paketi kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-707">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="e0bbb-708">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-708">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="e0bbb-709">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="e0bbb-709">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="e0bbb-710">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-710">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-711">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-711">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="e0bbb-712">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-712">**New behavior**</span></span>

<span data-ttu-id="e0bbb-713">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-713">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="e0bbb-714">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-714">**Why**</span></span>

<span data-ttu-id="e0bbb-715">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-715">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="e0bbb-716">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-716">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="e0bbb-717">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-717">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-718">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-718">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="e0bbb-719">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-719">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="e0bbb-720">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-720">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="e0bbb-721">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-721">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="e0bbb-722">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="e0bbb-722">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="e0bbb-723">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-723">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-724">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-724">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="e0bbb-725">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-725">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="e0bbb-726">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-726">**New behavior**</span></span>

<span data-ttu-id="e0bbb-727">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-727">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="e0bbb-728">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-728">**Why**</span></span>

<span data-ttu-id="e0bbb-729">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-729">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="e0bbb-730">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-730">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-731">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-731">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="e0bbb-732">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-732">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="e0bbb-733">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-733">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="e0bbb-734">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="e0bbb-734">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="e0bbb-735">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="e0bbb-735">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="e0bbb-736">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-736">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-737">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-737">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="e0bbb-738">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-738">**New behavior**</span></span>

<span data-ttu-id="e0bbb-739">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-739">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="e0bbb-740">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-740">**Why**</span></span>

<span data-ttu-id="e0bbb-741">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-741">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="e0bbb-742">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-742">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="e0bbb-743">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-743">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-744">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="e0bbb-744">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="e0bbb-745">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-745">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="e0bbb-746">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-746">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="e0bbb-747">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-747">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="e0bbb-748">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-748">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="e0bbb-749">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="e0bbb-749">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="e0bbb-750">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-750">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-751">EF Core 3,0 ' dan önce, `UseRowNumberForPaging` SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-751">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="e0bbb-752">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-752">**New behavior**</span></span>

<span data-ttu-id="e0bbb-753">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-753">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="e0bbb-754">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-754">**Why**</span></span>

<span data-ttu-id="e0bbb-755">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-755">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="e0bbb-756">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-756">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-757">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-757">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="e0bbb-758">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-758">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="e0bbb-759">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-759">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="e0bbb-760">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-760">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="e0bbb-761">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="e0bbb-761">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="e0bbb-762">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-762">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-763">`IDbContextOptionsExtension`, uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-763">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="e0bbb-764">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-764">**New behavior**</span></span>

<span data-ttu-id="e0bbb-765">Bu yöntemler yeni bir `IDbContextOptionsExtension.Info` özelliğinden döndürülen yeni bir `DbContextOptionsExtensionInfo` soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-765">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="e0bbb-766">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-766">**Why**</span></span>

<span data-ttu-id="e0bbb-767">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-767">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="e0bbb-768">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-768">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="e0bbb-769">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-769">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-770">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-770">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="e0bbb-771">EF Core kaynak kodundaki farklı uzantı türleri için `IDbContextOptionsExtension` ' ın birçok uygulamasında örnekler bulunur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-771">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="e0bbb-772">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-772">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="e0bbb-773">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="e0bbb-773">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="e0bbb-774">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-774">**Change**</span></span>

<span data-ttu-id="e0bbb-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning` olarak yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="e0bbb-776">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-776">**Why**</span></span>

<span data-ttu-id="e0bbb-777">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-777">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="e0bbb-778">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-778">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-779">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-779">Use the new name.</span></span> <span data-ttu-id="e0bbb-780">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="e0bbb-780">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="e0bbb-781">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="e0bbb-781">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="e0bbb-782">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="e0bbb-782">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="e0bbb-783">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-783">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-784">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-784">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="e0bbb-785">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-785">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="e0bbb-786">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-786">**New behavior**</span></span>

<span data-ttu-id="e0bbb-787">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-787">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="e0bbb-788">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-788">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="e0bbb-789">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-789">**Why**</span></span>

<span data-ttu-id="e0bbb-790">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-790">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="e0bbb-791">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-791">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-792">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-792">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="e0bbb-793">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="e0bbb-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="e0bbb-794">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="e0bbb-794">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="e0bbb-795">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-795">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-796">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-796">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="e0bbb-797">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-797">**New behavior**</span></span>

<span data-ttu-id="e0bbb-798">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-798">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="e0bbb-799">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-799">**Why**</span></span>

<span data-ttu-id="e0bbb-800">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-800">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="e0bbb-801">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-801">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="e0bbb-802">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-802">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-803">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-803">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="e0bbb-804">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-804">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="e0bbb-805">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="e0bbb-805">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="e0bbb-806">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-806">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-807">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-807">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="e0bbb-808">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-808">**New behavior**</span></span>

<span data-ttu-id="e0bbb-809">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-809">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="e0bbb-810">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-810">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="e0bbb-811">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-811">**Why**</span></span>

<span data-ttu-id="e0bbb-812">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-812">This package is only intended to be used at design time.</span></span> <span data-ttu-id="e0bbb-813">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-813">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="e0bbb-814">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-814">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="e0bbb-815">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-815">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-816">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-816">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="e0bbb-817">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-817">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="e0bbb-818">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-818">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="e0bbb-819">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="e0bbb-819">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="e0bbb-820">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-820">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-821">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-821">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="e0bbb-822">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-822">**New behavior**</span></span>

<span data-ttu-id="e0bbb-823">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-823">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="e0bbb-824">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-824">**Why**</span></span>

<span data-ttu-id="e0bbb-825">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-825">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="e0bbb-826">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-826">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="e0bbb-827">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-827">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-828">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-828">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="e0bbb-829">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-829">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="e0bbb-830">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="e0bbb-830">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="e0bbb-831">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="e0bbb-831">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="e0bbb-832">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-832">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-833">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-833">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="e0bbb-834">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-834">**New behavior**</span></span>

<span data-ttu-id="e0bbb-835">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-835">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="e0bbb-836">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-836">**Why**</span></span>

<span data-ttu-id="e0bbb-837">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-837">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="e0bbb-838">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-838">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-839">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-839">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="e0bbb-840">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-840">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="e0bbb-841">System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-841">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="e0bbb-842">Sorun izleniyor #15636</span><span class="sxs-lookup"><span data-stu-id="e0bbb-842">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="e0bbb-843">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-843">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-844">Microsoft. EntityFrameworkCore. SqlServer daha önce System. Data. SqlClient 'a bağımlı.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-844">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="e0bbb-845">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-845">**New behavior**</span></span>

<span data-ttu-id="e0bbb-846">Paketimizi Microsoft. Data. SqlClient 'e göre güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-846">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="e0bbb-847">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-847">**Why**</span></span>

<span data-ttu-id="e0bbb-848">Microsoft. Data. SqlClient, SQL Server ileriye dönük tanıtım verileri erişim sürücüsüdür ve System. Data. SqlClient artık geliştirme odağı değildir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-848">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="e0bbb-849">Always Encrypted gibi bazı önemli özellikler yalnızca Microsoft. Data. SqlClient ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-849">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="e0bbb-850">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-850">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-851">Kodunuz System. Data. SqlClient üzerinde doğrudan bir bağımlılık alırsa bunun yerine Microsoft. Data. SqlClient öğesine başvuracak şekilde değiştirmeniz gerekir; iki paket çok yüksek düzeyde API uyumluluğu korudıkça, bu yalnızca basit bir paket ve ad alanı değişikliği olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-851">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="e0bbb-852">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="e0bbb-852">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="e0bbb-853">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="e0bbb-853">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="e0bbb-854">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-854">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-855">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-855">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="e0bbb-856">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-856">For example:</span></span>

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

<span data-ttu-id="e0bbb-857">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-857">**New behavior**</span></span>

<span data-ttu-id="e0bbb-858">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-858">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="e0bbb-859">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-859">**Why**</span></span>

<span data-ttu-id="e0bbb-860">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-860">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="e0bbb-861">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-861">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-862">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-862">Use full configuration of the relationship.</span></span> <span data-ttu-id="e0bbb-863">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0bbb-863">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="e0bbb-864">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="e0bbb-864">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="e0bbb-865">Sorun izleniyor #12757</span><span class="sxs-lookup"><span data-stu-id="e0bbb-865">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="e0bbb-866">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-866">**Old behavior**</span></span>

<span data-ttu-id="e0bbb-867">Şema ile boş bir dize olarak yapılandırılmış bir DbFunction, şema olmadan yerleşik işlev olarak değerlendirildi.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-867">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="e0bbb-868">Örneğin, aşağıdaki kod `DatePart` CLR işlevini SqlServer üzerinde `DATEPART` yerleşik işlevine eşleyecek.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-868">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="e0bbb-869">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-869">**New behavior**</span></span>

<span data-ttu-id="e0bbb-870">Tüm DbFunction eşlemeleri Kullanıcı tanımlı işlevlere eşlenildiği kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-870">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="e0bbb-871">Bu nedenle boş dize değeri, işlevi model için varsayılan şemanın içine yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-871">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="e0bbb-872">Bu şema, Fluent API `modelBuilder.HasDefaultSchema()` veya `dbo` aracılığıyla açıkça yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-872">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="e0bbb-873">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-873">**Why**</span></span>

<span data-ttu-id="e0bbb-874">Daha önceden şemanın boş olması, işlevin yerleşik olduğunu değerlendirmek için bir yoldur, ancak bu mantık yalnızca yerleşik işlevlerin herhangi bir şemaya ait olmadığı SqlServer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-874">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="e0bbb-875">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="e0bbb-875">**Mitigations**</span></span>

<span data-ttu-id="e0bbb-876">DbFunction 'ın çevirisini yerleşik bir işlevle eşlemek için el ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e0bbb-876">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
