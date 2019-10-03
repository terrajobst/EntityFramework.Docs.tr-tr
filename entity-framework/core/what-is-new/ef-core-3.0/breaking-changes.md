---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 0dd4c5c4aa1a5d241fb48abf1372a678d0f7a7a3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813626"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="72e0e-102">EF Core 3,0 ' de yer alan son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="72e0e-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="72e0e-103">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken mevcut uygulamaları bozmak için olası bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="72e0e-104">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="72e0e-105">Özet</span><span class="sxs-lookup"><span data-stu-id="72e0e-105">Summary</span></span>

| <span data-ttu-id="72e0e-106">**Yeni değişiklik**</span><span class="sxs-lookup"><span data-stu-id="72e0e-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="72e0e-107">**Etkisi**</span><span class="sxs-lookup"><span data-stu-id="72e0e-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="72e0e-108">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="72e0e-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="72e0e-109">Yüksek</span><span class="sxs-lookup"><span data-stu-id="72e0e-109">High</span></span>       |
| [<span data-ttu-id="72e0e-110">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="72e0e-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="72e0e-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="72e0e-111">High</span></span>      |
| [<span data-ttu-id="72e0e-112">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="72e0e-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="72e0e-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="72e0e-113">High</span></span>      |
| [<span data-ttu-id="72e0e-114">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="72e0e-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="72e0e-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="72e0e-115">High</span></span>      |
| [<span data-ttu-id="72e0e-116">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="72e0e-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="72e0e-117">High</span></span>      |
| [<span data-ttu-id="72e0e-118">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="72e0e-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="72e0e-119">Yüksek</span><span class="sxs-lookup"><span data-stu-id="72e0e-119">High</span></span>      |
| [<span data-ttu-id="72e0e-120">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="72e0e-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="72e0e-121">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-121">Medium</span></span>      |
| [<span data-ttu-id="72e0e-122">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="72e0e-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="72e0e-123">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-123">Medium</span></span>      |
| [<span data-ttu-id="72e0e-124">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="72e0e-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="72e0e-125">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-125">Medium</span></span>      |
| [<span data-ttu-id="72e0e-126">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="72e0e-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="72e0e-127">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-127">Medium</span></span>      |
| [<span data-ttu-id="72e0e-128">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="72e0e-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="72e0e-129">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-129">Medium</span></span>      |
| [<span data-ttu-id="72e0e-130">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="72e0e-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="72e0e-131">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-131">Medium</span></span>      |
| [<span data-ttu-id="72e0e-132">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="72e0e-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="72e0e-133">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-133">Medium</span></span>      |
| [<span data-ttu-id="72e0e-134">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="72e0e-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="72e0e-135">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-135">Medium</span></span>      |
| [<span data-ttu-id="72e0e-136">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="72e0e-137">Orta</span><span class="sxs-lookup"><span data-stu-id="72e0e-137">Medium</span></span>      |
| [<span data-ttu-id="72e0e-138">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="72e0e-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="72e0e-139">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-139">Low</span></span>      |
| [<span data-ttu-id="72e0e-140">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="72e0e-141">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-141">Low</span></span>      |
| [<span data-ttu-id="72e0e-142">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="72e0e-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="72e0e-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-143">Low</span></span>      |
| [<span data-ttu-id="72e0e-144">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="72e0e-144">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="72e0e-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-145">Low</span></span>      |
| [<span data-ttu-id="72e0e-146">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="72e0e-146">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="72e0e-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-147">Low</span></span>      |
| [<span data-ttu-id="72e0e-148">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="72e0e-148">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="72e0e-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-149">Low</span></span>      |
| [<span data-ttu-id="72e0e-150">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-150">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="72e0e-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-151">Low</span></span>      |
| [<span data-ttu-id="72e0e-152">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="72e0e-152">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="72e0e-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-153">Low</span></span>      |
| [<span data-ttu-id="72e0e-154">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="72e0e-154">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="72e0e-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-155">Low</span></span>      |
| [<span data-ttu-id="72e0e-156">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="72e0e-156">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="72e0e-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-157">Low</span></span>      |
| [<span data-ttu-id="72e0e-158">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="72e0e-158">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="72e0e-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-159">Low</span></span>      |
| [<span data-ttu-id="72e0e-160">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-160">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="72e0e-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-161">Low</span></span>      |
| [<span data-ttu-id="72e0e-162">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-162">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="72e0e-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-163">Low</span></span>      |
| [<span data-ttu-id="72e0e-164">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="72e0e-164">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="72e0e-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-165">Low</span></span>      |
| [<span data-ttu-id="72e0e-166">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="72e0e-166">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="72e0e-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-167">Low</span></span>      |
| [<span data-ttu-id="72e0e-168">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="72e0e-168">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="72e0e-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-169">Low</span></span>      |
| [<span data-ttu-id="72e0e-170">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="72e0e-170">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="72e0e-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-171">Low</span></span>      |
| [<span data-ttu-id="72e0e-172">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="72e0e-172">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="72e0e-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-173">Low</span></span>      |
| [<span data-ttu-id="72e0e-174">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-174">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="72e0e-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-175">Low</span></span>      |
| [<span data-ttu-id="72e0e-176">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="72e0e-176">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="72e0e-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-177">Low</span></span>      |
| [<span data-ttu-id="72e0e-178">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="72e0e-178">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="72e0e-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-179">Low</span></span>      |
| [<span data-ttu-id="72e0e-180">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="72e0e-180">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="72e0e-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-181">Low</span></span>      |
| [<span data-ttu-id="72e0e-182">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="72e0e-182">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="72e0e-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-183">Low</span></span>      |
| [<span data-ttu-id="72e0e-184">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="72e0e-184">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="72e0e-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-185">Low</span></span>      |
| [<span data-ttu-id="72e0e-186">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="72e0e-186">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="72e0e-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-187">Low</span></span>      |
| [<span data-ttu-id="72e0e-188">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="72e0e-188">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="72e0e-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-189">Low</span></span>      |
| [<span data-ttu-id="72e0e-190">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-190">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="72e0e-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-191">Low</span></span>      |
| [<span data-ttu-id="72e0e-192">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-192">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="72e0e-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-193">Low</span></span>      |
| [<span data-ttu-id="72e0e-194">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="72e0e-194">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="72e0e-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-195">Low</span></span>      |
| [<span data-ttu-id="72e0e-196">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="72e0e-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-197">Low</span></span>      |
| [<span data-ttu-id="72e0e-198">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="72e0e-198">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="72e0e-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-199">Low</span></span>      |
| [<span data-ttu-id="72e0e-200">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-200">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="72e0e-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-201">Low</span></span>      |
| [<span data-ttu-id="72e0e-202">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-202">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="72e0e-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-203">Low</span></span>      |
| [<span data-ttu-id="72e0e-204">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-204">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="72e0e-205">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-205">Low</span></span>      |
| [<span data-ttu-id="72e0e-206">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="72e0e-206">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="72e0e-207">Düşük</span><span class="sxs-lookup"><span data-stu-id="72e0e-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="72e0e-208">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="72e0e-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="72e0e-209">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Ayrıca bkz. sorun #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="72e0e-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="72e0e-210">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-210">**Old behavior**</span></span>

<span data-ttu-id="72e0e-211">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="72e0e-212">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="72e0e-213">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-213">**New behavior**</span></span>

<span data-ttu-id="72e0e-214">3,0 ' den başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrı) istemci üzerinde değerlendirilme ifadelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="72e0e-215">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="72e0e-216">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-216">**Why**</span></span>

<span data-ttu-id="72e0e-217">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="72e0e-218">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="72e0e-219">Örneğin, bir `Where()` çağrıda çevrilemeyen bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="72e0e-220">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="72e0e-221">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="72e0e-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="72e0e-222">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="72e0e-223">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-223">**Mitigations**</span></span>

<span data-ttu-id="72e0e-224">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın ya da verileri açıkça istemciye, LINQ- `AsEnumerable()`Objects `ToList()`kullanılarak daha sonra işlenebileceği istemciye geri getirmek için, veya kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="72e0e-225">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="72e0e-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="72e0e-226">Sorun izleniyor #15498</span><span class="sxs-lookup"><span data-stu-id="72e0e-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="72e0e-227">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-227">**Old behavior**</span></span>

<span data-ttu-id="72e0e-228">3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-228">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="72e0e-229">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-229">**New behavior**</span></span>

<span data-ttu-id="72e0e-230">3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-230">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="72e0e-231">Bu, .NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="72e0e-231">This does not include .NET Framework.</span></span>

<span data-ttu-id="72e0e-232">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-232">**Why**</span></span>

<span data-ttu-id="72e0e-233">Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-233">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="72e0e-234">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-234">**Mitigations**</span></span>

<span data-ttu-id="72e0e-235">Modern bir .NET platformuna geçmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="72e0e-235">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="72e0e-236">Bu mümkün değilse, her ikisi de .NET Framework EF Core 2,1 veya EF Core 2,2 ' i kullanmaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-236">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="72e0e-237">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="72e0e-237">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="72e0e-238">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="72e0e-238">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="72e0e-239">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-239">**Old behavior**</span></span>

<span data-ttu-id="72e0e-240">`Microsoft.AspNetCore.App` Veya`Microsoft.AspNetCore.All`' a bir paket başvurusu eklediğinizde 3,0 ASP.NET Core önce, EF Core ve SQL Server sağlayıcısı gibi EF Core veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="72e0e-240">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="72e0e-241">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-241">**New behavior**</span></span>

<span data-ttu-id="72e0e-242">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="72e0e-242">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="72e0e-243">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-243">**Why**</span></span>

<span data-ttu-id="72e0e-244">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-244">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="72e0e-245">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="72e0e-245">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="72e0e-246">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-246">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="72e0e-247">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-247">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="72e0e-248">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-248">**Mitigations**</span></span>

<span data-ttu-id="72e0e-249">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-249">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="72e0e-250">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="72e0e-250">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="72e0e-251">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="72e0e-251">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="72e0e-252">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-252">**Old behavior**</span></span>

<span data-ttu-id="72e0e-253">3,0 ' `dotnet ef` den önce araç .NET Core SDK eklenmiştir ve ek adımlar gerekmeden herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-253">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="72e0e-254">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-254">**New behavior**</span></span>

<span data-ttu-id="72e0e-255">3,0 ' den başlayarak .NET SDK, `dotnet ef` aracı içermez, bu nedenle onu kullanabilmeniz için yerel veya küresel bir araç olarak açıkça kurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-255">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="72e0e-256">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-256">**Why**</span></span>

<span data-ttu-id="72e0e-257">Bu değişiklik, NuGet üzerinde düzenli bir .net `dotnet ef` CLI aracı olarak dağıtmamızı ve güncelleştirmenizi sağlar ve bu da EF Core 3,0 her zaman bir NuGet paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-257">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="72e0e-258">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-258">**Mitigations**</span></span>

<span data-ttu-id="72e0e-259">Geçişleri veya yapı iskelesi `DbContext`'ni yönetebilmek için genel bir araç olarak yüklemek `dotnet-ef` için:</span><span class="sxs-lookup"><span data-stu-id="72e0e-259">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="72e0e-260">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-260">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="72e0e-261">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="72e0e-262">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="72e0e-262">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="72e0e-263">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-263">**Old behavior**</span></span>

<span data-ttu-id="72e0e-264">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-264">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="72e0e-265">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-265">**New behavior**</span></span>

<span data-ttu-id="72e0e-266">EF Core 3,0 ' den başlayarak, `FromSqlRaw`ve parametrelerinin sorgu `ExecuteSqlRawAsync` dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için, ve kullanın `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="72e0e-266">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="72e0e-267">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-267">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="72e0e-268">Parametreleri, bir enterpolasyonlu sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için ve `FromSqlInterpolated` `ExecuteSqlInterpolatedAsync` kullanın. `ExecuteSqlInterpolated`</span><span class="sxs-lookup"><span data-stu-id="72e0e-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="72e0e-269">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-269">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="72e0e-270">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-270">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="72e0e-271">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-271">**Why**</span></span>

<span data-ttu-id="72e0e-272">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-272">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="72e0e-273">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-273">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="72e0e-274">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-274">**Mitigations**</span></span>

<span data-ttu-id="72e0e-275">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-275">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="72e0e-276">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="72e0e-276">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="72e0e-277">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="72e0e-277">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="72e0e-278">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-278">**Old behavior**</span></span>

<span data-ttu-id="72e0e-279">3,0 EF Core önce, `FromSql` Yöntem sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-279">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="72e0e-280">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-280">**New behavior**</span></span>

<span data-ttu-id="72e0e-281">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (değişen `FromSql`) yalnızca `DbSet<>`sorgu kökleri üzerinde belirtilebilir, yani doğrudan üzerinde.</span><span class="sxs-lookup"><span data-stu-id="72e0e-281">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="72e0e-282">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-282">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="72e0e-283">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-283">**Why**</span></span>

<span data-ttu-id="72e0e-284">İçinde `FromSql` dışında herhangi bir yerde, `DbSet` hiç eklenmemiş bir anlamı veya eklenen değeri belirtmediyse, belirli senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-284">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="72e0e-285">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-285">**Mitigations**</span></span>

<span data-ttu-id="72e0e-286">`FromSql`etkinleştirmeleri, `DbSet` uygulandıkları üzerinde doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-286">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="72e0e-287">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="72e0e-287">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="72e0e-288">Sorun izleniyor #13518</span><span class="sxs-lookup"><span data-stu-id="72e0e-288">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="72e0e-289">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-289">**Old behavior**</span></span>

<span data-ttu-id="72e0e-290">EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-290">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="72e0e-291">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-291">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="72e0e-292">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="72e0e-292">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="72e0e-293">, belirtilen kategori ile `Category` ilişkili her biri `Product` için aynı örneği döndürür.</span><span class="sxs-lookup"><span data-stu-id="72e0e-293">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="72e0e-294">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-294">**New behavior**</span></span>

<span data-ttu-id="72e0e-295">EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-295">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="72e0e-296">Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde `Category` bile, her `Product` biri için yeni bir örnek döndürür.</span><span class="sxs-lookup"><span data-stu-id="72e0e-296">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="72e0e-297">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-297">**Why**</span></span>

<span data-ttu-id="72e0e-298">Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="72e0e-298">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="72e0e-299">Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-299">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="72e0e-300">Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-300">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="72e0e-301">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-301">**Mitigations**</span></span>

<span data-ttu-id="72e0e-302">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-302">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="72e0e-303">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-303">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="72e0e-304">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="72e0e-304">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="72e0e-305">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="72e0e-305">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="72e0e-306">Örneğin, SQL `Debug`'in günlüğe kaydedilmesini değiştirmek için, `OnConfiguring` veya `AddDbContext`düzeyini açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="72e0e-306">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="72e0e-307">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="72e0e-307">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="72e0e-308">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="72e0e-308">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="72e0e-309">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-309">**Old behavior**</span></span>

<span data-ttu-id="72e0e-310">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-310">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="72e0e-311">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-311">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="72e0e-312">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-312">**New behavior**</span></span>

<span data-ttu-id="72e0e-313">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-313">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="72e0e-314">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-314">**Why**</span></span>

<span data-ttu-id="72e0e-315">Bu değişiklik, daha önce bir `DbContext` örnek tarafından daha önce izlenen bir varlık farklı `DbContext` bir örneğe taşındığında geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-315">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="72e0e-316">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-316">**Mitigations**</span></span>

<span data-ttu-id="72e0e-317">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlar üzerinde birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup bu `Added` durumda varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-317">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="72e0e-318">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="72e0e-318">This can be avoided by:</span></span>
* <span data-ttu-id="72e0e-319">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="72e0e-319">Not using store-generated keys.</span></span>
* <span data-ttu-id="72e0e-320">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="72e0e-320">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="72e0e-321">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-321">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="72e0e-322">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` `blog.Id` kendi kendine ayarlanmış olsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="72e0e-322">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="72e0e-323">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="72e0e-323">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="72e0e-324">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="72e0e-324">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="72e0e-325">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-325">**Old behavior**</span></span>

<span data-ttu-id="72e0e-326">EF Core 3,0 öncesinde, tarafından `DetectChanges` bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve çağrıldığında yeni bir satır `SaveChanges` olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-326">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="72e0e-327">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-327">**New behavior**</span></span>

<span data-ttu-id="72e0e-328">EF Core 3,0 ' den başlayarak, bir varlık oluşturulan anahtar değerlerini kullanıyorsa ve bir anahtar değeri ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-328">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="72e0e-329">Bu, varlık için bir satırın var olduğu varsayılır ve çağrıldığında güncelleştirilir `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="72e0e-329">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="72e0e-330">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` izlenir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-330">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="72e0e-331">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-331">**Why**</span></span>

<span data-ttu-id="72e0e-332">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-332">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="72e0e-333">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-333">**Mitigations**</span></span>

<span data-ttu-id="72e0e-334">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-334">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="72e0e-335">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-335">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="72e0e-336">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="72e0e-336">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="72e0e-337">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="72e0e-337">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="72e0e-338">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="72e0e-338">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="72e0e-339">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="72e0e-339">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="72e0e-340">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-340">**Old behavior**</span></span>

<span data-ttu-id="72e0e-341">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="72e0e-341">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="72e0e-342">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-342">**New behavior**</span></span>

<span data-ttu-id="72e0e-343">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="72e0e-343">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="72e0e-344">Örneğin, bir sorumlu `context.Remove()` varlığı silmek için çağırmak, tüm izlenen ilgili gerekli bağımlılara da `Deleted` hemen ayarlanabilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-344">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="72e0e-345">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-345">**Why**</span></span>

<span data-ttu-id="72e0e-346">Bu değişiklik, çağrılmadan _önce_ `SaveChanges` hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-346">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="72e0e-347">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-347">**Mitigations**</span></span>

<span data-ttu-id="72e0e-348">Önceki davranış ayarları `context.ChangedTracker`aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-348">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="72e0e-349">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-349">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="72e0e-350">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="72e0e-350">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="72e0e-351">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="72e0e-351">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="72e0e-352">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-352">**Old behavior**</span></span>

<span data-ttu-id="72e0e-353">3,0 ' den `DeleteBehavior.Restrict` önce, sözdizimi ile `Restrict` veritabanında yabancı anahtarlar oluşturdunuz, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-353">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="72e0e-354">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-354">**New behavior**</span></span>

<span data-ttu-id="72e0e-355">3,0 ' den başlayarak `DeleteBehavior.Restrict` , yabancı anahtarların semantiği ile `Restrict` oluşturulmasını sağlar--Bu, basamaksız, hiçbir atlama yok; bu arada, EF iç düzeltmesini etkilemeden, kısıtlama ihlali üzerinde oluşturma.</span><span class="sxs-lookup"><span data-stu-id="72e0e-355">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="72e0e-356">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-356">**Why**</span></span>

<span data-ttu-id="72e0e-357">Bu değişiklik, beklenmeyen yan etkilere gerek kalmadan sezgisel bir `DeleteBehavior` şekilde kullanma deneyimini geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-357">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="72e0e-358">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-358">**Mitigations**</span></span>

<span data-ttu-id="72e0e-359">Önceki davranış kullanılarak `DeleteBehavior.ClientNoAction`geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-359">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="72e0e-360">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="72e0e-360">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="72e0e-361">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="72e0e-361">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="72e0e-362">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-362">**Old behavior**</span></span>

<span data-ttu-id="72e0e-363">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/keyless-entity-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-363">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="72e0e-364">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="72e0e-364">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="72e0e-365">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-365">**New behavior**</span></span>

<span data-ttu-id="72e0e-366">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-366">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="72e0e-367">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-367">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="72e0e-368">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-368">**Why**</span></span>

<span data-ttu-id="72e0e-369">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-369">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="72e0e-370">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-370">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="72e0e-371">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-371">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="72e0e-372">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-372">**Mitigations**</span></span>

<span data-ttu-id="72e0e-373">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="72e0e-373">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="72e0e-374">**`ModelBuilder.Query<>()`** -Bunun `ModelBuilder.Entity<>().HasNoKey()` yerine bir varlık türünü anahtar olmadan işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-374">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="72e0e-375">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-375">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="72e0e-376">**`DbQuery<>`** -Bunun `DbSet<>` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-376">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="72e0e-377">**`DbContext.Query<>()`** -Bunun `DbContext.Set<>()` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-377">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="72e0e-378">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="72e0e-378">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="72e0e-379">[](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
Sorun izleme #12444 izleme sorunu[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
izleme sorunu[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="72e0e-379">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="72e0e-380">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-380">**Old behavior**</span></span>

<span data-ttu-id="72e0e-381">3,0 EF Core önce, sahip olunan ilişki yapılandırması doğrudan `OwnsOne` veya `OwnsMany` çağrısından sonra gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-381">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="72e0e-382">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-382">**New behavior**</span></span>

<span data-ttu-id="72e0e-383">EF Core 3,0 ' den başlayarak, artık kullanarak `WithOwner()`bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="72e0e-383">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="72e0e-384">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-384">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="72e0e-385">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin nasıl yapılandırıldığına `WithOwner()` benzer şekilde zincirde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-385">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="72e0e-386">Sahip olduğu için yapılandırma, hala sonrasında `OwnsOne()/OwnsMany()`zincirleme olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-386">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="72e0e-387">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-387">For example:</span></span>

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

<span data-ttu-id="72e0e-388">Ayrıca, `Entity()` `HasOne()`, veya`Set()` sahibi olan bir tür hedefi ile çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="72e0e-388">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="72e0e-389">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-389">**Why**</span></span>

<span data-ttu-id="72e0e-390">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-390">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="72e0e-391">Bu, sırasıyla belirsizlik ve gibi `HasForeignKey`yöntemlere karışmasını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-391">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="72e0e-392">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-392">**Mitigations**</span></span>

<span data-ttu-id="72e0e-393">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-393">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="72e0e-394">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="72e0e-394">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="72e0e-395">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="72e0e-395">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="72e0e-396">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-396">**Old behavior**</span></span>

<span data-ttu-id="72e0e-397">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="72e0e-397">Consider the following model:</span></span>
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
<span data-ttu-id="72e0e-398">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloya `OrderDetails` eşlenmiş ise, yeni `Order`bir eklenirken örnek her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-398">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="72e0e-399">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-399">**New behavior**</span></span>

<span data-ttu-id="72e0e-400">3,0 ile başlayarak, EF Core bir `Order` `OrderDetails` olmadan ekleme `OrderDetails` ve birincil anahtar null yapılabilir sütunlara hariç tüm özellikleri eşleme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-400">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="72e0e-401">Gerekli özelliklerinden herhangi birinin `OrderDetails` bir `null` değeri yoksa veya birincil `null`anahtarın ve tüm özelliklerin yanı sıra gerekli özellikleri yoksa, EF Core kümelerini sorgulama.</span><span class="sxs-lookup"><span data-stu-id="72e0e-401">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="72e0e-402">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-402">**Mitigations**</span></span>

<span data-ttu-id="72e0e-403">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinti beklenmiyorsa `null` , gezinti olduğunda, `null`uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-403">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="72e0e-404">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış bir`null` değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-404">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="72e0e-405">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="72e0e-405">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="72e0e-406">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="72e0e-406">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="72e0e-407">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-407">**Old behavior**</span></span>

<span data-ttu-id="72e0e-408">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="72e0e-408">Consider the following model:</span></span>
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
<span data-ttu-id="72e0e-409">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloyla eşleştirilmiş ise, yalnızca `OrderDetails` güncelleştirme, istemci üzerindeki değeri güncelleştirmez `Version` ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-409">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="72e0e-410">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-410">**New behavior**</span></span>

<span data-ttu-id="72e0e-411">3,0 ile başlayarak, EF Core yeni `Version` `Order` değeri, sahip `OrderDetails`olduğu gibi yayar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-411">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="72e0e-412">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-412">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="72e0e-413">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-413">**Why**</span></span>

<span data-ttu-id="72e0e-414">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-414">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="72e0e-415">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-415">**Mitigations**</span></span>

<span data-ttu-id="72e0e-416">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-416">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="72e0e-417">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="72e0e-417">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="72e0e-418">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="72e0e-418">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="72e0e-419">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="72e0e-419">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="72e0e-420">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-420">**Old behavior**</span></span>

<span data-ttu-id="72e0e-421">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="72e0e-421">Consider the following model:</span></span>
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

<span data-ttu-id="72e0e-422">3,0 EF Core önce, `ShippingAddress` özelliği, varsayılan olarak ve `Order` için `BulkOrder` ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-422">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="72e0e-423">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-423">**New behavior**</span></span>

<span data-ttu-id="72e0e-424">3,0 ile başlayarak, EF Core için `ShippingAddress`yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-424">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="72e0e-425">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-425">**Why**</span></span>

<span data-ttu-id="72e0e-426">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="72e0e-426">The old behavoir was unexpected.</span></span>

<span data-ttu-id="72e0e-427">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-427">**Mitigations**</span></span>

<span data-ttu-id="72e0e-428">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="72e0e-428">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="72e0e-429">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-429">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="72e0e-430">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="72e0e-430">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="72e0e-431">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-431">**Old behavior**</span></span>

<span data-ttu-id="72e0e-432">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="72e0e-432">Consider the following model:</span></span>
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
<span data-ttu-id="72e0e-433">3,0 EF Core önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-433">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="72e0e-434">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtarı yapar `CustomerId` ve bu genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="72e0e-434">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="72e0e-435">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-435">**New behavior**</span></span>

<span data-ttu-id="72e0e-436">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="72e0e-436">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="72e0e-437">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-437">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="72e0e-438">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-438">For example:</span></span>

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

<span data-ttu-id="72e0e-439">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-439">**Why**</span></span>

<span data-ttu-id="72e0e-440">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-440">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="72e0e-441">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-441">**Mitigations**</span></span>

<span data-ttu-id="72e0e-442">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-442">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="72e0e-443">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="72e0e-443">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="72e0e-444">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="72e0e-444">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="72e0e-445">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-445">**Old behavior**</span></span>

<span data-ttu-id="72e0e-446">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope`içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-446">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="72e0e-447">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-447">**New behavior**</span></span>

<span data-ttu-id="72e0e-448">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-448">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="72e0e-449">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-449">**Why**</span></span>

<span data-ttu-id="72e0e-450">Bu değişiklik aynı `TransactionScope`bağlamda birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-450">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="72e0e-451">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-451">The new behavior also matches EF6.</span></span>

<span data-ttu-id="72e0e-452">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-452">**Mitigations**</span></span>

<span data-ttu-id="72e0e-453">Bağlantının açık açık çağrısıyla `OpenConnection()` kalması gerekiyorsa EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="72e0e-453">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="72e0e-454">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="72e0e-454">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="72e0e-455">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="72e0e-455">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="72e0e-456">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-456">**Old behavior**</span></span>

<span data-ttu-id="72e0e-457">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-457">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="72e0e-458">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-458">**New behavior**</span></span>

<span data-ttu-id="72e0e-459">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-459">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="72e0e-460">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-460">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="72e0e-461">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-461">**Why**</span></span>

<span data-ttu-id="72e0e-462">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-462">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="72e0e-463">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-463">**Mitigations**</span></span>

<span data-ttu-id="72e0e-464">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-464">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="72e0e-465">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="72e0e-465">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="72e0e-466">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="72e0e-466">Backing fields are used by default</span></span>

[<span data-ttu-id="72e0e-467">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="72e0e-467">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="72e0e-468">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-468">**Old behavior**</span></span>

<span data-ttu-id="72e0e-469">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-469">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="72e0e-470">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-470">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="72e0e-471">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-471">**New behavior**</span></span>

<span data-ttu-id="72e0e-472">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-472">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="72e0e-473">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-473">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="72e0e-474">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-474">**Why**</span></span>

<span data-ttu-id="72e0e-475">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-475">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="72e0e-476">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-476">**Mitigations**</span></span>

<span data-ttu-id="72e0e-477">3,0 öncesi davranışı, üzerinde `ModelBuilder`özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-477">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="72e0e-478">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-478">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="72e0e-479">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="72e0e-479">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="72e0e-480">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="72e0e-480">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="72e0e-481">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-481">**Old behavior**</span></span>

<span data-ttu-id="72e0e-482">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-482">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="72e0e-483">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-483">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="72e0e-484">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-484">**New behavior**</span></span>

<span data-ttu-id="72e0e-485">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-485">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="72e0e-486">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-486">**Why**</span></span>

<span data-ttu-id="72e0e-487">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-487">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="72e0e-488">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-488">**Mitigations**</span></span>

<span data-ttu-id="72e0e-489">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-489">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="72e0e-490">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="72e0e-490">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="72e0e-491">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="72e0e-491">Field-only property names should match the field name</span></span>

<span data-ttu-id="72e0e-492">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-492">**Old behavior**</span></span>

<span data-ttu-id="72e0e-493">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-493">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="72e0e-494">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-494">**New behavior**</span></span>

<span data-ttu-id="72e0e-495">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-495">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="72e0e-496">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-496">**Why**</span></span>

<span data-ttu-id="72e0e-497">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-497">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="72e0e-498">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-498">**Mitigations**</span></span>

<span data-ttu-id="72e0e-499">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-499">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="72e0e-500">3,0 sonrasında EF Core gelecek bir sürümünde, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="72e0e-500">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="72e0e-501">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-501">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="72e0e-502">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="72e0e-502">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="72e0e-503">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-503">**Old behavior**</span></span>

<span data-ttu-id="72e0e-504">EF Core 3,0 ' dan önce `AddDbContext` , `AddDbContextPool` ' ı çağırarak günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini D. I ile birlikte [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrıları ile de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="72e0e-504">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="72e0e-505">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-505">**New behavior**</span></span>

<span data-ttu-id="72e0e-506">EF Core 3,0 ' `AddDbContext` den başlayarak ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-506">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="72e0e-507">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-507">**Why**</span></span>

<span data-ttu-id="72e0e-508">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="72e0e-508">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="72e0e-509">Ancak, `ILoggerFactory` uygulamanın dı kapsayıcısına kayıtlıysa, EF Core tarafından kullanılmaya devam edilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-509">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="72e0e-510">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-510">**Mitigations**</span></span>

<span data-ttu-id="72e0e-511">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-511">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="72e0e-512">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-512">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="72e0e-513">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="72e0e-513">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="72e0e-514">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-514">**Old behavior**</span></span>

<span data-ttu-id="72e0e-515">EF Core 3,0 ' dan önce `DbContext.Entry` , çağırma tüm izlenen varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-515">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="72e0e-516">Bu, `EntityEntry` durumunda olan durumun güncel olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="72e0e-516">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="72e0e-517">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-517">**New behavior**</span></span>

<span data-ttu-id="72e0e-518">EF Core 3,0 ' den başlayarak, `DbContext.Entry` çağırma artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-518">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="72e0e-519">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-519">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="72e0e-520">`ChangeTracker.AutoDetectChangesEnabled` Buyereldeğişiklikalgılamaişleminin`false` devre dışı bırakılacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-520">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="72e0e-521">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` , ve `SaveChanges`, tüm izlenen varlıkların tamamen bir tamamına `DetectChanges` neden olur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-521">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="72e0e-522">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-522">**Why**</span></span>

<span data-ttu-id="72e0e-523">Bu değişiklik, kullanmanın `context.Entry`varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-523">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="72e0e-524">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-524">**Mitigations**</span></span>

<span data-ttu-id="72e0e-525">3,0 `ChgangeTracker.DetectChanges()` öncesi davranışı sağlamak `Entry` için çağrılmadan önce açıkça çağırın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-525">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="72e0e-526">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="72e0e-526">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="72e0e-527">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="72e0e-527">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="72e0e-528">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-528">**Old behavior**</span></span>

<span data-ttu-id="72e0e-529">3,0 `string` EF Core önce ve `byte[]` anahtar özellikler açıkça null olmayan bir değer ayarlamameden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-529">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="72e0e-530">Böyle bir durumda, anahtar değeri istemcide, için `byte[]`bayt olarak SERILEŞTIRILDIĞI bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-530">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="72e0e-531">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-531">**New behavior**</span></span>

<span data-ttu-id="72e0e-532">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-532">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="72e0e-533">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-533">**Why**</span></span>

<span data-ttu-id="72e0e-534">Bu değişiklik, istemci tarafından oluşturulan `string` / `byte[]` değerler genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-534">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="72e0e-535">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-535">**Mitigations**</span></span>

<span data-ttu-id="72e0e-536">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-536">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="72e0e-537">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="72e0e-537">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="72e0e-538">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="72e0e-538">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="72e0e-539">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="72e0e-539">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="72e0e-540">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="72e0e-540">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="72e0e-541">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-541">**Old behavior**</span></span>

<span data-ttu-id="72e0e-542">EF Core 3,0 ' dan `ILoggerFactory` önce, bir tek hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-542">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="72e0e-543">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-543">**New behavior**</span></span>

<span data-ttu-id="72e0e-544">EF Core 3,0 ' den itibaren `ILoggerFactory` , artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-544">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="72e0e-545">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-545">**Why**</span></span>

<span data-ttu-id="72e0e-546">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi `DbContext` bazı bazı durumları kaldıran bir örnek içeren bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-546">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="72e0e-547">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-547">**Mitigations**</span></span>

<span data-ttu-id="72e0e-548">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-548">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="72e0e-549">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-549">This isn't common.</span></span>
<span data-ttu-id="72e0e-550">Bu durumlarda, çoğu şey çalışmaya devam edecektir, ancak bağlı `ILoggerFactory` olan herhangi bir singleton hizmeti, `ILoggerFactory` farklı bir şekilde elde edilecek şekilde değiştirilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-550">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="72e0e-551">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [GitHub sorun izleyici EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ' de bir sorun `ILoggerFactory` bildirin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-551">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="72e0e-552">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="72e0e-552">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="72e0e-553">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="72e0e-553">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="72e0e-554">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-554">**Old behavior**</span></span>

<span data-ttu-id="72e0e-555">EF Core 3,0 ' dan önce, `DbContext` bir kez atıldıktan sonra, söz konusu bağlamdan alınan bir varlık üzerinde verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="72e0e-555">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="72e0e-556">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-556">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="72e0e-557">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-557">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="72e0e-558">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-558">**New behavior**</span></span>

<span data-ttu-id="72e0e-559">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="72e0e-559">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="72e0e-560">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-560">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="72e0e-561">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-561">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="72e0e-562">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-562">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="72e0e-563">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-563">**Why**</span></span>

<span data-ttu-id="72e0e-564">Bu değişiklik, atılmış `DbContext` bir örnek üzerinde geç yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-564">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="72e0e-565">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-565">**Mitigations**</span></span>

<span data-ttu-id="72e0e-566">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-566">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="72e0e-567">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="72e0e-567">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="72e0e-568">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="72e0e-568">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="72e0e-569">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-569">**Old behavior**</span></span>

<span data-ttu-id="72e0e-570">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-570">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="72e0e-571">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-571">**New behavior**</span></span>

<span data-ttu-id="72e0e-572">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-572">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="72e0e-573">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-573">**Why**</span></span>

<span data-ttu-id="72e0e-574">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="72e0e-574">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="72e0e-575">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-575">**Mitigations**</span></span>

<span data-ttu-id="72e0e-576">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-576">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="72e0e-577">Ancak, hata, üzerindeki `DbContextOptionsBuilder`yapılandırması aracılığıyla bir uyarıya dönüştürülebilir (veya yok sayılır).</span><span class="sxs-lookup"><span data-stu-id="72e0e-577">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="72e0e-578">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-578">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="72e0e-579">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="72e0e-579">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="72e0e-580">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="72e0e-580">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="72e0e-581">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-581">**Old behavior**</span></span>

<span data-ttu-id="72e0e-582">EF Core 3,0 öncesinde, tek bir `HasOne` dizeye `HasMany` çağrı yapan veya kod karışık bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-582">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="72e0e-583">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-583">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="72e0e-584">Kod, özel olabilecek `Samurai` `Entrance` gezinti özelliğini kullanarak başka bir varlık türüyle ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="72e0e-584">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="72e0e-585">Gerçekte, bu kod, hiçbir gezinti özelliği olmadan çağrılan `Entrance` bazı varlık türlerine bir ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-585">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="72e0e-586">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-586">**New behavior**</span></span>

<span data-ttu-id="72e0e-587">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="72e0e-587">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="72e0e-588">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-588">**Why**</span></span>

<span data-ttu-id="72e0e-589">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-589">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="72e0e-590">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-590">**Mitigations**</span></span>

<span data-ttu-id="72e0e-591">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="72e0e-591">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="72e0e-592">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-592">This is not common.</span></span>
<span data-ttu-id="72e0e-593">Önceki davranış, gezinti özelliği adı için açıkça geçirilmesiyle `null` elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-593">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="72e0e-594">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-594">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="72e0e-595">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-595">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="72e0e-596">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="72e0e-596">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="72e0e-597">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-597">**Old behavior**</span></span>

<span data-ttu-id="72e0e-598">Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürür:</span><span class="sxs-lookup"><span data-stu-id="72e0e-598">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="72e0e-599">`ValueGenerator.NextValueAsync()`(ve türetme sınıfları)</span><span class="sxs-lookup"><span data-stu-id="72e0e-599">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="72e0e-600">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-600">**New behavior**</span></span>

<span data-ttu-id="72e0e-601">Yukarıda bahsedilen yöntemler bundan önceki ile aynı `ValueTask<T>` `T` şekilde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="72e0e-601">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="72e0e-602">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-602">**Why**</span></span>

<span data-ttu-id="72e0e-603">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-603">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="72e0e-604">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-604">**Mitigations**</span></span>

<span data-ttu-id="72e0e-605">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-605">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="72e0e-606">Daha karmaşık bir kullanım `Task` (örneğin, döndürülen ' e `Task.WhenAny()`geçirme), genellikle `Task<T>` döndürülen öğesine çağırarak `ValueTask<T>` `AsTask()` döndürülen öğesine dönüştürülmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-606">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="72e0e-607">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-607">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="72e0e-608">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="72e0e-608">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="72e0e-609">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="72e0e-609">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="72e0e-610">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-610">**Old behavior**</span></span>

<span data-ttu-id="72e0e-611">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-611">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="72e0e-612">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-612">**New behavior**</span></span>

<span data-ttu-id="72e0e-613">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-613">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="72e0e-614">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-614">**Why**</span></span>

<span data-ttu-id="72e0e-615">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-615">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="72e0e-616">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-616">**Mitigations**</span></span>

<span data-ttu-id="72e0e-617">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-617">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="72e0e-618">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-618">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="72e0e-619">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="72e0e-619">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="72e0e-620">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="72e0e-620">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="72e0e-621">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-621">**Old behavior**</span></span>

<span data-ttu-id="72e0e-622">EF Core 3,0 ' den `ToTable()` önce, yalnızca devralma eşleme stratejisi bu geçerli olmayan bir TPH olduğundan, türetilmiş bir tür üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-622">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="72e0e-623">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-623">**New behavior**</span></span>

<span data-ttu-id="72e0e-624">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, `ToTable()` gelecekte beklenmedik bir eşleme değişikliğini önlemek için artık bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-624">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="72e0e-625">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-625">**Why**</span></span>

<span data-ttu-id="72e0e-626">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-626">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="72e0e-627">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-627">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="72e0e-628">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-628">**Mitigations**</span></span>

<span data-ttu-id="72e0e-629">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-629">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="72e0e-630">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="72e0e-630">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="72e0e-631">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="72e0e-631">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="72e0e-632">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-632">**Old behavior**</span></span>

<span data-ttu-id="72e0e-633">EF Core 3,0 öncesinde, `ForSqlServerHasIndex().ForSqlServerInclude()` ile `INCLUDE`kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="72e0e-634">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-634">**New behavior**</span></span>

<span data-ttu-id="72e0e-635">EF Core 3,0 ' den itibaren, `Include` bir dizinde kullanmak artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="72e0e-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="72e0e-636">Kullanın `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="72e0e-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="72e0e-637">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-637">**Why**</span></span>

<span data-ttu-id="72e0e-638">Bu değişiklik, tüm veritabanı sağlayıcıları için tek bir yerde bulunan `Include` dizinlere yönelik API 'leri birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="72e0e-639">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-639">**Mitigations**</span></span>

<span data-ttu-id="72e0e-640">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="72e0e-641">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="72e0e-641">Metadata API changes</span></span>

[<span data-ttu-id="72e0e-642">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="72e0e-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="72e0e-643">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-643">**New behavior**</span></span>

<span data-ttu-id="72e0e-644">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="72e0e-644">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="72e0e-645">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-645">**Why**</span></span>

<span data-ttu-id="72e0e-646">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-646">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="72e0e-647">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-647">**Mitigations**</span></span>

<span data-ttu-id="72e0e-648">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-648">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="72e0e-649">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="72e0e-649">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="72e0e-650">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="72e0e-650">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="72e0e-651">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-651">**New behavior**</span></span>

<span data-ttu-id="72e0e-652">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="72e0e-652">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="72e0e-653">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-653">**Why**</span></span>

<span data-ttu-id="72e0e-654">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-654">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="72e0e-655">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-655">**Mitigations**</span></span>

<span data-ttu-id="72e0e-656">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-656">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="72e0e-657">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="72e0e-657">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="72e0e-658">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="72e0e-658">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="72e0e-659">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-659">**Old behavior**</span></span>

<span data-ttu-id="72e0e-660">3,0 EF Core önce, SQLite bağlantısı açıldığında `PRAGMA foreign_keys = 1` EF Core gönderilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-660">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="72e0e-661">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-661">**New behavior**</span></span>

<span data-ttu-id="72e0e-662">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı `PRAGMA foreign_keys = 1` açıldığında EF Core artık göndermektedir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-662">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="72e0e-663">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-663">**Why**</span></span>

<span data-ttu-id="72e0e-664">Bu değişiklik, EF Core varsayılan olarak kullanıldığı `SQLitePCLRaw.bundle_e_sqlite3` için yapılmıştır, bu da FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-664">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="72e0e-665">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-665">**Mitigations**</span></span>

<span data-ttu-id="72e0e-666">Yabancı anahtarlar varsayılan olarak, EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 içinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-666">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="72e0e-667">Diğer durumlarda, yabancı anahtarlar bağlantı dizeniz belirtilerek `Foreign Keys=True` etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-667">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="72e0e-668">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="72e0e-668">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="72e0e-669">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-669">**Old behavior**</span></span>

<span data-ttu-id="72e0e-670">3,0 EF Core önce EF Core kullanılır `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="72e0e-670">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="72e0e-671">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-671">**New behavior**</span></span>

<span data-ttu-id="72e0e-672">EF Core 3,0 ' den başlayarak EF Core kullanılır `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="72e0e-672">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="72e0e-673">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-673">**Why**</span></span>

<span data-ttu-id="72e0e-674">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-674">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="72e0e-675">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-675">**Mitigations**</span></span>

<span data-ttu-id="72e0e-676">İOS 'ta yerel SQLite sürümünü kullanmak için, öğesini farklı `Microsoft.Data.Sqlite` `SQLitePCLRaw` bir paket kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-676">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="72e0e-677">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="72e0e-677">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="72e0e-678">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="72e0e-678">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="72e0e-679">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-679">**Old behavior**</span></span>

<span data-ttu-id="72e0e-680">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-680">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="72e0e-681">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-681">**New behavior**</span></span>

<span data-ttu-id="72e0e-682">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-682">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="72e0e-683">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-683">**Why**</span></span>

<span data-ttu-id="72e0e-684">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-684">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="72e0e-685">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-685">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="72e0e-686">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-686">**Mitigations**</span></span>

<span data-ttu-id="72e0e-687">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-687">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="72e0e-688">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-688">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="72e0e-689">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-689">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="72e0e-690">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="72e0e-690">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="72e0e-691">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="72e0e-691">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="72e0e-692">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-692">**Old behavior**</span></span>

<span data-ttu-id="72e0e-693">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-693">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="72e0e-694">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-694">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="72e0e-695">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-695">**New behavior**</span></span>

<span data-ttu-id="72e0e-696">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-696">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="72e0e-697">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-697">**Why**</span></span>

<span data-ttu-id="72e0e-698">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-698">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="72e0e-699">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-699">**Mitigations**</span></span>

<span data-ttu-id="72e0e-700">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-700">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="72e0e-701">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-701">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="72e0e-702">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-702">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="72e0e-703">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="72e0e-703">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="72e0e-704">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="72e0e-704">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="72e0e-705">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-705">**Old behavior**</span></span>

<span data-ttu-id="72e0e-706">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-706">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="72e0e-707">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-707">**New behavior**</span></span>

<span data-ttu-id="72e0e-708">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-708">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="72e0e-709">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-709">**Why**</span></span>

<span data-ttu-id="72e0e-710">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-710">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="72e0e-711">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="72e0e-711">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="72e0e-712">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-712">**Mitigations**</span></span>

<span data-ttu-id="72e0e-713">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="72e0e-713">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="72e0e-714">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-714">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="72e0e-715">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-715">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="72e0e-716">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-716">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="72e0e-717">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-717">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="72e0e-718">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="72e0e-718">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="72e0e-719">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-719">**Old behavior**</span></span>

<span data-ttu-id="72e0e-720">EF Core 3,0 ' dan `UseRowNumberForPaging` önce, SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-720">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="72e0e-721">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-721">**New behavior**</span></span>

<span data-ttu-id="72e0e-722">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-722">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="72e0e-723">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-723">**Why**</span></span>

<span data-ttu-id="72e0e-724">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-724">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="72e0e-725">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-725">**Mitigations**</span></span>

<span data-ttu-id="72e0e-726">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-726">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="72e0e-727">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-727">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="72e0e-728">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-728">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="72e0e-729">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-729">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="72e0e-730">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="72e0e-730">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="72e0e-731">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-731">**Old behavior**</span></span>

<span data-ttu-id="72e0e-732">`IDbContextOptionsExtension`uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="72e0e-732">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="72e0e-733">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-733">**New behavior**</span></span>

<span data-ttu-id="72e0e-734">Bu yöntemler yeni `DbContextOptionsExtensionInfo` `IDbContextOptionsExtension.Info` bir özellikten döndürülen yeni bir soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-734">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="72e0e-735">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-735">**Why**</span></span>

<span data-ttu-id="72e0e-736">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="72e0e-736">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="72e0e-737">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-737">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="72e0e-738">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-738">**Mitigations**</span></span>

<span data-ttu-id="72e0e-739">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-739">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="72e0e-740">EF Core kaynak kodunda farklı tür uzantılara `IDbContextOptionsExtension` yönelik birçok uygulamada örnek bulunur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-740">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="72e0e-741">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-741">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="72e0e-742">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="72e0e-742">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="72e0e-743">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="72e0e-743">**Change**</span></span>

<span data-ttu-id="72e0e-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`, olarak `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="72e0e-745">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-745">**Why**</span></span>

<span data-ttu-id="72e0e-746">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-746">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="72e0e-747">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-747">**Mitigations**</span></span>

<span data-ttu-id="72e0e-748">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-748">Use the new name.</span></span> <span data-ttu-id="72e0e-749">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="72e0e-749">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="72e0e-750">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="72e0e-750">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="72e0e-751">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="72e0e-751">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="72e0e-752">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-752">**Old behavior**</span></span>

<span data-ttu-id="72e0e-753">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-753">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="72e0e-754">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-754">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="72e0e-755">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-755">**New behavior**</span></span>

<span data-ttu-id="72e0e-756">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-756">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="72e0e-757">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-757">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="72e0e-758">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-758">**Why**</span></span>

<span data-ttu-id="72e0e-759">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-759">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="72e0e-760">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-760">**Mitigations**</span></span>

<span data-ttu-id="72e0e-761">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-761">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="72e0e-762">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="72e0e-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="72e0e-763">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="72e0e-763">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="72e0e-764">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-764">**Old behavior**</span></span>

<span data-ttu-id="72e0e-765">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="72e0e-765">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="72e0e-766">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-766">**New behavior**</span></span>

<span data-ttu-id="72e0e-767">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-767">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="72e0e-768">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-768">**Why**</span></span>

<span data-ttu-id="72e0e-769">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-769">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="72e0e-770">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-770">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="72e0e-771">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-771">**Mitigations**</span></span>

<span data-ttu-id="72e0e-772">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="72e0e-772">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="72e0e-773">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="72e0e-773">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="72e0e-774">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="72e0e-774">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="72e0e-775">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-775">**Old behavior**</span></span>

<span data-ttu-id="72e0e-776">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-776">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="72e0e-777">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-777">**New behavior**</span></span>

<span data-ttu-id="72e0e-778">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-778">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="72e0e-779">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-779">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="72e0e-780">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-780">**Why**</span></span>

<span data-ttu-id="72e0e-781">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-781">This package is only intended to be used at design time.</span></span> <span data-ttu-id="72e0e-782">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-782">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="72e0e-783">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="72e0e-783">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="72e0e-784">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-784">**Mitigations**</span></span>

<span data-ttu-id="72e0e-785">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="72e0e-785">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="72e0e-786">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-786">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="72e0e-787">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-787">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="72e0e-788">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="72e0e-788">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="72e0e-789">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-789">**Old behavior**</span></span>

<span data-ttu-id="72e0e-790">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-790">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="72e0e-791">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-791">**New behavior**</span></span>

<span data-ttu-id="72e0e-792">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="72e0e-792">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="72e0e-793">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-793">**Why**</span></span>

<span data-ttu-id="72e0e-794">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="72e0e-794">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="72e0e-795">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-795">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="72e0e-796">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-796">**Mitigations**</span></span>

<span data-ttu-id="72e0e-797">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-797">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="72e0e-798">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-798">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="72e0e-799">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="72e0e-799">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="72e0e-800">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="72e0e-800">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="72e0e-801">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-801">**Old behavior**</span></span>

<span data-ttu-id="72e0e-802">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="72e0e-802">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="72e0e-803">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-803">**New behavior**</span></span>

<span data-ttu-id="72e0e-804">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="72e0e-804">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="72e0e-805">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-805">**Why**</span></span>

<span data-ttu-id="72e0e-806">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="72e0e-806">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="72e0e-807">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-807">**Mitigations**</span></span>

<span data-ttu-id="72e0e-808">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-808">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="72e0e-809">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-809">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="72e0e-810">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="72e0e-810">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="72e0e-811">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="72e0e-811">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="72e0e-812">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-812">**Old behavior**</span></span>

<span data-ttu-id="72e0e-813">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="72e0e-813">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="72e0e-814">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-814">For example:</span></span>

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

<span data-ttu-id="72e0e-815">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-815">**New behavior**</span></span>

<span data-ttu-id="72e0e-816">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="72e0e-816">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="72e0e-817">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-817">**Why**</span></span>

<span data-ttu-id="72e0e-818">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="72e0e-818">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="72e0e-819">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-819">**Mitigations**</span></span>

<span data-ttu-id="72e0e-820">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-820">Use full configuration of the relationship.</span></span> <span data-ttu-id="72e0e-821">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="72e0e-821">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="72e0e-822">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="72e0e-822">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="72e0e-823">Sorun izleniyor #12757</span><span class="sxs-lookup"><span data-stu-id="72e0e-823">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="72e0e-824">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-824">**Old behavior**</span></span>

<span data-ttu-id="72e0e-825">Şema ile boş bir dize olarak yapılandırılmış bir DbFunction, şema olmadan yerleşik işlev olarak değerlendirildi.</span><span class="sxs-lookup"><span data-stu-id="72e0e-825">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="72e0e-826">Örneğin, aşağıdaki kod, clr `DatePart` işlevini SqlServer üzerinde `DATEPART` yerleşik işlev olarak eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-826">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="72e0e-827">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="72e0e-827">**New behavior**</span></span>

<span data-ttu-id="72e0e-828">Tüm DbFunction eşlemeleri Kullanıcı tanımlı işlevlere eşlenildiği kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-828">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="72e0e-829">Bu nedenle boş dize değeri, işlevi model için varsayılan şemanın içine yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-829">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="72e0e-830">Şema, Fluent API `modelBuilder.HasDefaultSchema()` aracılığıyla açıkça yapılandırılabilir veya `dbo` Aksi halde.</span><span class="sxs-lookup"><span data-stu-id="72e0e-830">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="72e0e-831">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="72e0e-831">**Why**</span></span>

<span data-ttu-id="72e0e-832">Daha önceden şemanın boş olması, işlevin yerleşik olduğunu değerlendirmek için bir yoldur, ancak bu mantık yalnızca yerleşik işlevlerin herhangi bir şemaya ait olmadığı SqlServer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="72e0e-832">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="72e0e-833">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="72e0e-833">**Mitigations**</span></span>

<span data-ttu-id="72e0e-834">DbFunction 'ın çevirisini yerleşik bir işlevle eşlemek için el ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="72e0e-834">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
