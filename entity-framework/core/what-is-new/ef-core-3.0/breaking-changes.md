---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 04487291f24bb702dad4b497c34234afdd5e3c9a
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005582"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="04236-102">EF Core 3,0 ' de yer alan son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="04236-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="04236-103">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken mevcut uygulamaları bozmak için olası bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="04236-104">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](../../providers/provider-log.md)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="04236-104">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="04236-105">Bir 3,0 önizlemeden başka bir 3,0 önizlemeye olan kesintiler burada açıklanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-105">Breaks from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="04236-106">Özet</span><span class="sxs-lookup"><span data-stu-id="04236-106">Summary</span></span>

| <span data-ttu-id="04236-107">**Yeni değişiklik**</span><span class="sxs-lookup"><span data-stu-id="04236-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="04236-108">**Etkisi**</span><span class="sxs-lookup"><span data-stu-id="04236-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="04236-109">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="04236-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="04236-110">Yüksek</span><span class="sxs-lookup"><span data-stu-id="04236-110">High</span></span>       |
| [<span data-ttu-id="04236-111">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="04236-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="04236-112">Yüksek</span><span class="sxs-lookup"><span data-stu-id="04236-112">High</span></span>      |
| [<span data-ttu-id="04236-113">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="04236-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="04236-114">Yüksek</span><span class="sxs-lookup"><span data-stu-id="04236-114">High</span></span>      |
| [<span data-ttu-id="04236-115">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="04236-116">Yüksek</span><span class="sxs-lookup"><span data-stu-id="04236-116">High</span></span>      |
| [<span data-ttu-id="04236-117">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="04236-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="04236-118">Yüksek</span><span class="sxs-lookup"><span data-stu-id="04236-118">High</span></span>      |
| [<span data-ttu-id="04236-119">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="04236-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="04236-120">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-120">Medium</span></span>      |
| [<span data-ttu-id="04236-121">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="04236-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="04236-122">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-122">Medium</span></span>      |
| [<span data-ttu-id="04236-123">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="04236-123">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="04236-124">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-124">Medium</span></span>      |
| [<span data-ttu-id="04236-125">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="04236-125">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="04236-126">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-126">Medium</span></span>      |
| [<span data-ttu-id="04236-127">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="04236-127">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="04236-128">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-128">Medium</span></span>      |
| [<span data-ttu-id="04236-129">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="04236-129">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="04236-130">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-130">Medium</span></span>      |
| [<span data-ttu-id="04236-131">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="04236-131">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="04236-132">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-132">Medium</span></span>      |
| [<span data-ttu-id="04236-133">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="04236-133">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="04236-134">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-134">Medium</span></span>      |
| [<span data-ttu-id="04236-135">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-135">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="04236-136">Orta</span><span class="sxs-lookup"><span data-stu-id="04236-136">Medium</span></span>      |
| [<span data-ttu-id="04236-137">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="04236-137">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="04236-138">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-138">Low</span></span>      |
| [<span data-ttu-id="04236-139">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="04236-139">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="04236-140">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-140">Low</span></span>      |
| [<span data-ttu-id="04236-141">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="04236-141">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="04236-142">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-142">Low</span></span>      |
| [<span data-ttu-id="04236-143">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="04236-143">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="04236-144">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-144">Low</span></span>      |
| [<span data-ttu-id="04236-145">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="04236-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="04236-146">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-146">Low</span></span>      |
| [<span data-ttu-id="04236-147">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="04236-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="04236-148">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-148">Low</span></span>      |
| [<span data-ttu-id="04236-149">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="04236-149">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="04236-150">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-150">Low</span></span>      |
| [<span data-ttu-id="04236-151">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="04236-151">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="04236-152">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-152">Low</span></span>      |
| [<span data-ttu-id="04236-153">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="04236-153">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="04236-154">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-154">Low</span></span>      |
| [<span data-ttu-id="04236-155">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="04236-155">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="04236-156">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-156">Low</span></span>      |
| [<span data-ttu-id="04236-157">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="04236-157">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="04236-158">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-158">Low</span></span>      |
| [<span data-ttu-id="04236-159">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="04236-159">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="04236-160">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-160">Low</span></span>      |
| [<span data-ttu-id="04236-161">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="04236-161">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="04236-162">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-162">Low</span></span>      |
| [<span data-ttu-id="04236-163">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="04236-163">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="04236-164">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-164">Low</span></span>      |
| [<span data-ttu-id="04236-165">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="04236-165">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="04236-166">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-166">Low</span></span>      |
| [<span data-ttu-id="04236-167">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="04236-167">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="04236-168">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-168">Low</span></span>      |
| [<span data-ttu-id="04236-169">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="04236-169">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="04236-170">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-170">Low</span></span>      |
| [<span data-ttu-id="04236-171">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="04236-171">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="04236-172">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-172">Low</span></span>      |
| [<span data-ttu-id="04236-173">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="04236-173">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="04236-174">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-174">Low</span></span>      |
| [<span data-ttu-id="04236-175">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="04236-175">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="04236-176">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-176">Low</span></span>      |
| [<span data-ttu-id="04236-177">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="04236-177">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="04236-178">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-178">Low</span></span>      |
| [<span data-ttu-id="04236-179">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="04236-179">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="04236-180">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-180">Low</span></span>      |
| [<span data-ttu-id="04236-181">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="04236-181">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="04236-182">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-182">Low</span></span>      |
| [<span data-ttu-id="04236-183">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="04236-183">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="04236-184">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-184">Low</span></span>      |
| [<span data-ttu-id="04236-185">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="04236-185">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="04236-186">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-186">Low</span></span>      |
| [<span data-ttu-id="04236-187">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="04236-187">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="04236-188">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-188">Low</span></span>      |
| [<span data-ttu-id="04236-189">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="04236-189">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="04236-190">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-190">Low</span></span>      |
| [<span data-ttu-id="04236-191">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-191">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="04236-192">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-192">Low</span></span>      |
| [<span data-ttu-id="04236-193">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-193">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="04236-194">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-194">Low</span></span>      |
| [<span data-ttu-id="04236-195">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="04236-195">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="04236-196">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-196">Low</span></span>      |
| [<span data-ttu-id="04236-197">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="04236-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="04236-198">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-198">Low</span></span>      |
| [<span data-ttu-id="04236-199">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="04236-199">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="04236-200">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-200">Low</span></span>      |
| [<span data-ttu-id="04236-201">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="04236-201">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="04236-202">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-202">Low</span></span>      |
| [<span data-ttu-id="04236-203">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="04236-203">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="04236-204">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-204">Low</span></span>      |
| [<span data-ttu-id="04236-205">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="04236-205">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="04236-206">Düşük</span><span class="sxs-lookup"><span data-stu-id="04236-206">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="04236-207">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="04236-207">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="04236-208">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Ayrıca bkz. sorun #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="04236-208">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="04236-209">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-209">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-210">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-210">**Old behavior**</span></span>

<span data-ttu-id="04236-211">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="04236-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="04236-212">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="04236-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="04236-213">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-213">**New behavior**</span></span>

<span data-ttu-id="04236-214">3,0 ' den başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrı) istemci üzerinde değerlendirilme ifadelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="04236-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="04236-215">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04236-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="04236-216">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-216">**Why**</span></span>

<span data-ttu-id="04236-217">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="04236-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="04236-218">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="04236-219">Örneğin, bir `Where()` çağrıda çevrilemeyen bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="04236-220">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="04236-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="04236-221">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="04236-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="04236-222">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="04236-223">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-223">**Mitigations**</span></span>

<span data-ttu-id="04236-224">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın ya da verileri açıkça istemciye, LINQ- `AsEnumerable()`Objects `ToList()`kullanılarak daha sonra işlenebileceği istemciye geri getirmek için, veya kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="04236-225">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="04236-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="04236-226">Sorun izleniyor #15498</span><span class="sxs-lookup"><span data-stu-id="04236-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="04236-227">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-227">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="04236-228">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-228">**Old behavior**</span></span>

<span data-ttu-id="04236-229">3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-229">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="04236-230">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-230">**New behavior**</span></span>

<span data-ttu-id="04236-231">3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="04236-231">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="04236-232">Bu, .NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="04236-232">This does not include .NET Framework.</span></span>

<span data-ttu-id="04236-233">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-233">**Why**</span></span>

<span data-ttu-id="04236-234">Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-234">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="04236-235">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-235">**Mitigations**</span></span>

<span data-ttu-id="04236-236">Modern bir .NET platformuna geçmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="04236-236">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="04236-237">Bu mümkün değilse, her ikisi de .NET Framework EF Core 2,1 veya EF Core 2,2 ' i kullanmaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="04236-237">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="04236-238">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="04236-238">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="04236-239">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="04236-239">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="04236-240">Bu değişiklik ASP.NET Core 3,0-Preview 1 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-240">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="04236-241">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-241">**Old behavior**</span></span>

<span data-ttu-id="04236-242">`Microsoft.AspNetCore.App` Veya`Microsoft.AspNetCore.All`' a bir paket başvurusu eklediğinizde 3,0 ASP.NET Core önce, EF Core ve SQL Server sağlayıcısı gibi EF Core veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="04236-242">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="04236-243">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-243">**New behavior**</span></span>

<span data-ttu-id="04236-244">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="04236-244">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="04236-245">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-245">**Why**</span></span>

<span data-ttu-id="04236-246">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-246">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="04236-247">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="04236-247">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="04236-248">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-248">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="04236-249">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-249">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="04236-250">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-250">**Mitigations**</span></span>

<span data-ttu-id="04236-251">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="04236-251">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="04236-252">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="04236-252">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="04236-253">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="04236-253">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="04236-254">Bu değişiklik EF Core 3,0-Preview 4 ' te ve ilgili .NET Core SDK sürümünde sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-254">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="04236-255">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-255">**Old behavior**</span></span>

<span data-ttu-id="04236-256">3,0 ' `dotnet ef` den önce araç .NET Core SDK eklenmiştir ve ek adımlar gerekmeden herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="04236-256">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="04236-257">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-257">**New behavior**</span></span>

<span data-ttu-id="04236-258">3,0 ' den başlayarak .NET SDK, `dotnet ef` aracı içermez, bu nedenle onu kullanabilmeniz için yerel veya küresel bir araç olarak açıkça kurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-258">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="04236-259">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-259">**Why**</span></span>

<span data-ttu-id="04236-260">Bu değişiklik, NuGet üzerinde düzenli bir .net `dotnet ef` CLI aracı olarak dağıtmamızı ve güncelleştirmenizi sağlar ve bu da EF Core 3,0 her zaman bir NuGet paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="04236-260">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="04236-261">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-261">**Mitigations**</span></span>

<span data-ttu-id="04236-262">Geçişleri veya yapı iskelesi `DbContext`'ni yönetebilmek için genel bir araç olarak yüklemek `dotnet-ef` için:</span><span class="sxs-lookup"><span data-stu-id="04236-262">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="04236-263">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04236-263">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="04236-264">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-264">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="04236-265">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="04236-265">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="04236-266">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-266">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-267">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-267">**Old behavior**</span></span>

<span data-ttu-id="04236-268">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="04236-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="04236-269">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-269">**New behavior**</span></span>

<span data-ttu-id="04236-270">EF Core 3,0 ' den başlayarak, `FromSqlRaw`ve parametrelerinin sorgu `ExecuteSqlRawAsync` dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için, ve kullanın `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="04236-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="04236-271">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="04236-272">Parametreleri, bir enterpolasyonlu sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için ve `FromSqlInterpolated` `ExecuteSqlInterpolatedAsync` kullanın. `ExecuteSqlInterpolated`</span><span class="sxs-lookup"><span data-stu-id="04236-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="04236-273">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="04236-274">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="04236-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="04236-275">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-275">**Why**</span></span>

<span data-ttu-id="04236-276">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="04236-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="04236-277">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="04236-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="04236-278">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-278">**Mitigations**</span></span>

<span data-ttu-id="04236-279">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="04236-279">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="04236-280">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="04236-280">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="04236-281">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="04236-281">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="04236-282">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-282">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="04236-283">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-283">**Old behavior**</span></span>

<span data-ttu-id="04236-284">3,0 EF Core önce, `FromSql` Yöntem sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-284">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="04236-285">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-285">**New behavior**</span></span>

<span data-ttu-id="04236-286">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (değişen `FromSql`) yalnızca `DbSet<>`sorgu kökleri üzerinde belirtilebilir, yani doğrudan üzerinde.</span><span class="sxs-lookup"><span data-stu-id="04236-286">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="04236-287">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="04236-287">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="04236-288">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-288">**Why**</span></span>

<span data-ttu-id="04236-289">İçinde `FromSql` dışında herhangi bir yerde, `DbSet` hiç eklenmemiş bir anlamı veya eklenen değeri belirtmediyse, belirli senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-289">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="04236-290">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-290">**Mitigations**</span></span>

<span data-ttu-id="04236-291">`FromSql`etkinleştirmeleri, `DbSet` uygulandıkları üzerinde doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-291">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="04236-292">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="04236-292">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="04236-293">Sorun izleniyor #13518</span><span class="sxs-lookup"><span data-stu-id="04236-293">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="04236-294">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-294">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="04236-295">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-295">**Old behavior**</span></span>

<span data-ttu-id="04236-296">EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-296">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="04236-297">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="04236-297">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="04236-298">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="04236-298">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="04236-299">, belirtilen kategori ile `Category` ilişkili her biri `Product` için aynı örneği döndürür.</span><span class="sxs-lookup"><span data-stu-id="04236-299">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="04236-300">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-300">**New behavior**</span></span>

<span data-ttu-id="04236-301">EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-301">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="04236-302">Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde `Category` bile, her `Product` biri için yeni bir örnek döndürür.</span><span class="sxs-lookup"><span data-stu-id="04236-302">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="04236-303">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-303">**Why**</span></span>

<span data-ttu-id="04236-304">Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="04236-304">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="04236-305">Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="04236-305">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="04236-306">Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="04236-306">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="04236-307">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-307">**Mitigations**</span></span>

<span data-ttu-id="04236-308">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-308">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="04236-309">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="04236-309">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="04236-310">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="04236-310">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="04236-311">Bu değişiklik EF Core 3,0-Preview 7 ' de geri döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="04236-311">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="04236-312">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="04236-312">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="04236-313">Örneğin, SQL `Debug`'in günlüğe kaydedilmesini değiştirmek için, `OnConfiguring` veya `AddDbContext`düzeyini açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="04236-313">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="04236-314">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="04236-314">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="04236-315">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="04236-315">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="04236-316">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-316">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="04236-317">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-317">**Old behavior**</span></span>

<span data-ttu-id="04236-318">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="04236-318">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="04236-319">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="04236-319">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="04236-320">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-320">**New behavior**</span></span>

<span data-ttu-id="04236-321">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="04236-321">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="04236-322">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-322">**Why**</span></span>

<span data-ttu-id="04236-323">Bu değişiklik, daha önce bir `DbContext` örnek tarafından daha önce izlenen bir varlık farklı `DbContext` bir örneğe taşındığında geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-323">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="04236-324">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-324">**Mitigations**</span></span>

<span data-ttu-id="04236-325">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlar üzerinde birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup bu `Added` durumda varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-325">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="04236-326">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="04236-326">This can be avoided by:</span></span>
* <span data-ttu-id="04236-327">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="04236-327">Not using store-generated keys.</span></span>
* <span data-ttu-id="04236-328">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="04236-328">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="04236-329">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="04236-329">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="04236-330">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` `blog.Id` kendi kendine ayarlanmış olsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="04236-330">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="04236-331">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="04236-331">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="04236-332">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="04236-332">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="04236-333">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-333">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-334">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-334">**Old behavior**</span></span>

<span data-ttu-id="04236-335">EF Core 3,0 öncesinde, tarafından `DetectChanges` bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve çağrıldığında yeni bir satır `SaveChanges` olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="04236-335">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="04236-336">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-336">**New behavior**</span></span>

<span data-ttu-id="04236-337">EF Core 3,0 ' den başlayarak, bir varlık oluşturulan anahtar değerlerini kullanıyorsa ve bir anahtar değeri ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="04236-337">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="04236-338">Bu, varlık için bir satırın var olduğu varsayılır ve çağrıldığında güncelleştirilir `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="04236-338">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="04236-339">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` izlenir.</span><span class="sxs-lookup"><span data-stu-id="04236-339">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="04236-340">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-340">**Why**</span></span>

<span data-ttu-id="04236-341">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-341">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="04236-342">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-342">**Mitigations**</span></span>

<span data-ttu-id="04236-343">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-343">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="04236-344">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-344">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="04236-345">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="04236-345">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="04236-346">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="04236-346">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="04236-347">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="04236-347">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="04236-348">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="04236-348">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="04236-349">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-349">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-350">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-350">**Old behavior**</span></span>

<span data-ttu-id="04236-351">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="04236-351">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="04236-352">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-352">**New behavior**</span></span>

<span data-ttu-id="04236-353">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="04236-353">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="04236-354">Örneğin, bir sorumlu `context.Remove()` varlığı silmek için çağırmak, tüm izlenen ilgili gerekli bağımlılara da `Deleted` hemen ayarlanabilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="04236-354">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="04236-355">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-355">**Why**</span></span>

<span data-ttu-id="04236-356">Bu değişiklik, çağrılmadan _önce_ `SaveChanges` hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-356">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="04236-357">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-357">**Mitigations**</span></span>

<span data-ttu-id="04236-358">Önceki davranış ayarları `context.ChangedTracker`aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-358">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="04236-359">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-359">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="04236-360">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="04236-360">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="04236-361">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="04236-361">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="04236-362">Bu değişiklik EF Core 3,0-Preview 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-362">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="04236-363">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-363">**Old behavior**</span></span>

<span data-ttu-id="04236-364">3,0 ' den `DeleteBehavior.Restrict` önce, sözdizimi ile `Restrict` veritabanında yabancı anahtarlar oluşturdunuz, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="04236-364">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="04236-365">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-365">**New behavior**</span></span>

<span data-ttu-id="04236-366">3,0 ' den başlayarak `DeleteBehavior.Restrict` , yabancı anahtarların semantiği ile `Restrict` oluşturulmasını sağlar--Bu, basamaksız, hiçbir atlama yok; bu arada, EF iç düzeltmesini etkilemeden, kısıtlama ihlali üzerinde oluşturma.</span><span class="sxs-lookup"><span data-stu-id="04236-366">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="04236-367">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-367">**Why**</span></span>

<span data-ttu-id="04236-368">Bu değişiklik, beklenmeyen yan etkilere gerek kalmadan sezgisel bir `DeleteBehavior` şekilde kullanma deneyimini geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-368">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="04236-369">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-369">**Mitigations**</span></span>

<span data-ttu-id="04236-370">Önceki davranış kullanılarak `DeleteBehavior.ClientNoAction`geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-370">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="04236-371">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="04236-371">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="04236-372">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="04236-372">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="04236-373">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-373">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-374">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-374">**Old behavior**</span></span>

<span data-ttu-id="04236-375">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/query-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="04236-375">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="04236-376">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="04236-376">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="04236-377">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-377">**New behavior**</span></span>

<span data-ttu-id="04236-378">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="04236-378">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="04236-379">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="04236-379">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="04236-380">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-380">**Why**</span></span>

<span data-ttu-id="04236-381">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-381">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="04236-382">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-382">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="04236-383">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="04236-383">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="04236-384">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-384">**Mitigations**</span></span>

<span data-ttu-id="04236-385">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="04236-385">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="04236-386">**`ModelBuilder.Query<>()`** -Bunun `ModelBuilder.Entity<>().HasNoKey()` yerine bir varlık türünü anahtar olmadan işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-386">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="04236-387">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-387">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="04236-388">**`DbQuery<>`** -Bunun `DbSet<>` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-388">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="04236-389">**`DbContext.Query<>()`** -Bunun `DbContext.Set<>()` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-389">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="04236-390">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="04236-390">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="04236-391">[](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
Sorun izleme #12444 izleme sorunu[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
izleme sorunu[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="04236-391">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="04236-392">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-392">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-393">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-393">**Old behavior**</span></span>

<span data-ttu-id="04236-394">3,0 EF Core önce, sahip olunan ilişki yapılandırması doğrudan `OwnsOne` veya `OwnsMany` çağrısından sonra gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="04236-394">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="04236-395">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-395">**New behavior**</span></span>

<span data-ttu-id="04236-396">EF Core 3,0 ' den başlayarak, artık kullanarak `WithOwner()`bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="04236-396">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="04236-397">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-397">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="04236-398">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin nasıl yapılandırıldığına `WithOwner()` benzer şekilde zincirde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-398">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="04236-399">Sahip olduğu için yapılandırma, hala sonrasında `OwnsOne()/OwnsMany()`zincirleme olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="04236-399">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="04236-400">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-400">For example:</span></span>

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

<span data-ttu-id="04236-401">Ayrıca, `Entity()` `HasOne()`, veya`Set()` sahibi olan bir tür hedefi ile çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="04236-401">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="04236-402">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-402">**Why**</span></span>

<span data-ttu-id="04236-403">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-403">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="04236-404">Bu, sırasıyla belirsizlik ve gibi `HasForeignKey`yöntemlere karışmasını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="04236-404">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="04236-405">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-405">**Mitigations**</span></span>

<span data-ttu-id="04236-406">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="04236-406">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="04236-407">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="04236-407">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="04236-408">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="04236-408">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="04236-409">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-409">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-410">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-410">**Old behavior**</span></span>

<span data-ttu-id="04236-411">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="04236-411">Consider the following model:</span></span>
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
<span data-ttu-id="04236-412">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloya `OrderDetails` eşlenmiş ise, yeni `Order`bir eklenirken örnek her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="04236-412">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="04236-413">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-413">**New behavior**</span></span>

<span data-ttu-id="04236-414">3,0 ile başlayarak, EF Core bir `Order` `OrderDetails` olmadan ekleme `OrderDetails` ve birincil anahtar null yapılabilir sütunlara hariç tüm özellikleri eşleme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="04236-414">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="04236-415">Gerekli özelliklerinden herhangi birinin `OrderDetails` bir `null` değeri yoksa veya birincil `null`anahtarın ve tüm özelliklerin yanı sıra gerekli özellikleri yoksa, EF Core kümelerini sorgulama.</span><span class="sxs-lookup"><span data-stu-id="04236-415">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="04236-416">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-416">**Mitigations**</span></span>

<span data-ttu-id="04236-417">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinti beklenmiyorsa `null` , gezinti olduğunda, `null`uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-417">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="04236-418">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış bir`null` değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-418">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="04236-419">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="04236-419">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="04236-420">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="04236-420">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="04236-421">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-421">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-422">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-422">**Old behavior**</span></span>

<span data-ttu-id="04236-423">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="04236-423">Consider the following model:</span></span>
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
<span data-ttu-id="04236-424">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloyla eşleştirilmiş ise, yalnızca `OrderDetails` güncelleştirme, istemci üzerindeki değeri güncelleştirmez `Version` ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="04236-424">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="04236-425">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-425">**New behavior**</span></span>

<span data-ttu-id="04236-426">3,0 ile başlayarak, EF Core yeni `Version` `Order` değeri, sahip `OrderDetails`olduğu gibi yayar.</span><span class="sxs-lookup"><span data-stu-id="04236-426">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="04236-427">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04236-427">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="04236-428">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-428">**Why**</span></span>

<span data-ttu-id="04236-429">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-429">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="04236-430">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-430">**Mitigations**</span></span>

<span data-ttu-id="04236-431">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-431">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="04236-432">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="04236-432">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="04236-433">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="04236-433">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="04236-434">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="04236-434">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="04236-435">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-435">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-436">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-436">**Old behavior**</span></span>

<span data-ttu-id="04236-437">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="04236-437">Consider the following model:</span></span>
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

<span data-ttu-id="04236-438">3,0 EF Core önce, `ShippingAddress` özelliği, varsayılan olarak ve `Order` için `BulkOrder` ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="04236-438">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="04236-439">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-439">**New behavior**</span></span>

<span data-ttu-id="04236-440">3,0 ile başlayarak, EF Core için `ShippingAddress`yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="04236-440">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="04236-441">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-441">**Why**</span></span>

<span data-ttu-id="04236-442">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="04236-442">The old behavoir was unexpected.</span></span>

<span data-ttu-id="04236-443">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-443">**Mitigations**</span></span>

<span data-ttu-id="04236-444">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="04236-444">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="04236-445">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="04236-445">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="04236-446">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="04236-446">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="04236-447">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-447">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-448">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-448">**Old behavior**</span></span>

<span data-ttu-id="04236-449">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="04236-449">Consider the following model:</span></span>
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
<span data-ttu-id="04236-450">3,0 EF Core önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-450">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="04236-451">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtarı yapar `CustomerId` ve bu genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="04236-451">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="04236-452">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-452">**New behavior**</span></span>

<span data-ttu-id="04236-453">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="04236-453">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="04236-454">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="04236-454">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="04236-455">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-455">For example:</span></span>

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

<span data-ttu-id="04236-456">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-456">**Why**</span></span>

<span data-ttu-id="04236-457">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-457">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="04236-458">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-458">**Mitigations**</span></span>

<span data-ttu-id="04236-459">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="04236-459">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="04236-460">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="04236-460">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="04236-461">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="04236-461">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="04236-462">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-462">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-463">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-463">**Old behavior**</span></span>

<span data-ttu-id="04236-464">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope`içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="04236-464">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="04236-465">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-465">**New behavior**</span></span>

<span data-ttu-id="04236-466">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="04236-466">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="04236-467">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-467">**Why**</span></span>

<span data-ttu-id="04236-468">Bu değişiklik aynı `TransactionScope`bağlamda birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="04236-468">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="04236-469">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="04236-469">The new behavior also matches EF6.</span></span>

<span data-ttu-id="04236-470">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-470">**Mitigations**</span></span>

<span data-ttu-id="04236-471">Bağlantının açık açık çağrısıyla `OpenConnection()` kalması gerekiyorsa EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="04236-471">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="04236-472">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="04236-472">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="04236-473">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="04236-473">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="04236-474">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-474">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-475">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-475">**Old behavior**</span></span>

<span data-ttu-id="04236-476">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-476">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="04236-477">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-477">**New behavior**</span></span>

<span data-ttu-id="04236-478">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="04236-478">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="04236-479">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="04236-479">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="04236-480">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-480">**Why**</span></span>

<span data-ttu-id="04236-481">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-481">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="04236-482">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-482">**Mitigations**</span></span>

<span data-ttu-id="04236-483">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-483">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="04236-484">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="04236-484">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="04236-485">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="04236-485">Backing fields are used by default</span></span>

[<span data-ttu-id="04236-486">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="04236-486">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="04236-487">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-487">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="04236-488">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-488">**Old behavior**</span></span>

<span data-ttu-id="04236-489">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="04236-489">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="04236-490">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="04236-490">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="04236-491">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-491">**New behavior**</span></span>

<span data-ttu-id="04236-492">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="04236-492">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="04236-493">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-493">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="04236-494">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-494">**Why**</span></span>

<span data-ttu-id="04236-495">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-495">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="04236-496">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-496">**Mitigations**</span></span>

<span data-ttu-id="04236-497">3,0 öncesi davranışı, üzerinde `ModelBuilder`özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-497">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="04236-498">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-498">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="04236-499">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="04236-499">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="04236-500">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="04236-500">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="04236-501">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-501">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-502">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-502">**Old behavior**</span></span>

<span data-ttu-id="04236-503">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="04236-503">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="04236-504">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-504">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="04236-505">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-505">**New behavior**</span></span>

<span data-ttu-id="04236-506">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04236-506">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="04236-507">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-507">**Why**</span></span>

<span data-ttu-id="04236-508">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-508">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="04236-509">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-509">**Mitigations**</span></span>

<span data-ttu-id="04236-510">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-510">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="04236-511">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="04236-511">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="04236-512">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="04236-512">Field-only property names should match the field name</span></span>

<span data-ttu-id="04236-513">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-513">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-514">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-514">**Old behavior**</span></span>

<span data-ttu-id="04236-515">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve CLR türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="04236-515">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="04236-516">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-516">**New behavior**</span></span>

<span data-ttu-id="04236-517">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-517">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="04236-518">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-518">**Why**</span></span>

<span data-ttu-id="04236-519">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="04236-519">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="04236-520">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-520">**Mitigations**</span></span>

<span data-ttu-id="04236-521">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-521">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="04236-522">3,0 sonrasında EF Core gelecek bir sürümünde, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="04236-522">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="04236-523">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="04236-523">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="04236-524">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="04236-524">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="04236-525">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-525">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-526">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-526">**Old behavior**</span></span>

<span data-ttu-id="04236-527">EF Core 3,0 ' dan önce `AddDbContext` , `AddDbContextPool` ' ı çağırarak günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini D. I ile birlikte [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrıları ile de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="04236-527">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="04236-528">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-528">**New behavior**</span></span>

<span data-ttu-id="04236-529">EF Core 3,0 ' `AddDbContext` den başlayarak ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="04236-529">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="04236-530">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-530">**Why**</span></span>

<span data-ttu-id="04236-531">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="04236-531">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="04236-532">Ancak, `ILoggerFactory` uygulamanın dı kapsayıcısına kayıtlıysa, EF Core tarafından kullanılmaya devam edilir.</span><span class="sxs-lookup"><span data-stu-id="04236-532">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="04236-533">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-533">**Mitigations**</span></span>

<span data-ttu-id="04236-534">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="04236-534">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="04236-535">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="04236-535">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="04236-536">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="04236-536">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="04236-537">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-537">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-538">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-538">**Old behavior**</span></span>

<span data-ttu-id="04236-539">EF Core 3,0 ' dan önce `DbContext.Entry` , çağırma tüm izlenen varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="04236-539">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="04236-540">Bu, `EntityEntry` durumunda olan durumun güncel olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="04236-540">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="04236-541">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-541">**New behavior**</span></span>

<span data-ttu-id="04236-542">EF Core 3,0 ' den başlayarak, `DbContext.Entry` çağırma artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="04236-542">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="04236-543">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="04236-543">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="04236-544">`ChangeTracker.AutoDetectChangesEnabled` Buyereldeğişiklikalgılamaişleminin`false` devre dışı bırakılacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="04236-544">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="04236-545">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` , ve `SaveChanges`, tüm izlenen varlıkların tamamen bir tamamına `DetectChanges` neden olur.</span><span class="sxs-lookup"><span data-stu-id="04236-545">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="04236-546">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-546">**Why**</span></span>

<span data-ttu-id="04236-547">Bu değişiklik, kullanmanın `context.Entry`varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-547">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="04236-548">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-548">**Mitigations**</span></span>

<span data-ttu-id="04236-549">3,0 `ChgangeTracker.DetectChanges()` öncesi davranışı sağlamak `Entry` için çağrılmadan önce açıkça çağırın.</span><span class="sxs-lookup"><span data-stu-id="04236-549">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="04236-550">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="04236-550">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="04236-551">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="04236-551">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="04236-552">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-552">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-553">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-553">**Old behavior**</span></span>

<span data-ttu-id="04236-554">3,0 `string` EF Core önce ve `byte[]` anahtar özellikler açıkça null olmayan bir değer ayarlamameden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-554">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="04236-555">Böyle bir durumda, anahtar değeri istemcide, için `byte[]`bayt olarak SERILEŞTIRILDIĞI bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04236-555">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="04236-556">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-556">**New behavior**</span></span>

<span data-ttu-id="04236-557">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="04236-557">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="04236-558">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-558">**Why**</span></span>

<span data-ttu-id="04236-559">Bu değişiklik, istemci tarafından oluşturulan `string` / `byte[]` değerler genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-559">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="04236-560">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-560">**Mitigations**</span></span>

<span data-ttu-id="04236-561">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-561">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="04236-562">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="04236-562">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="04236-563">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="04236-563">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="04236-564">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="04236-564">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="04236-565">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="04236-565">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="04236-566">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-566">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-567">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-567">**Old behavior**</span></span>

<span data-ttu-id="04236-568">EF Core 3,0 ' dan `ILoggerFactory` önce, bir tek hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="04236-568">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="04236-569">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-569">**New behavior**</span></span>

<span data-ttu-id="04236-570">EF Core 3,0 ' den itibaren `ILoggerFactory` , artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="04236-570">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="04236-571">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-571">**Why**</span></span>

<span data-ttu-id="04236-572">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi `DbContext` bazı bazı durumları kaldıran bir örnek içeren bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-572">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="04236-573">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-573">**Mitigations**</span></span>

<span data-ttu-id="04236-574">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="04236-574">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="04236-575">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="04236-575">This isn't common.</span></span>
<span data-ttu-id="04236-576">Bu durumlarda, çoğu şey çalışmaya devam edecektir, ancak bağlı `ILoggerFactory` olan herhangi bir singleton hizmeti, `ILoggerFactory` farklı bir şekilde elde edilecek şekilde değiştirilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-576">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="04236-577">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [GitHub sorun izleyici EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ' de bir sorun `ILoggerFactory` bildirin.</span><span class="sxs-lookup"><span data-stu-id="04236-577">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="04236-578">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="04236-578">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="04236-579">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="04236-579">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="04236-580">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-580">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-581">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-581">**Old behavior**</span></span>

<span data-ttu-id="04236-582">EF Core 3,0 ' dan önce, `DbContext` bir kez atıldıktan sonra, söz konusu bağlamdan alınan bir varlık üzerinde verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="04236-582">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="04236-583">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-583">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="04236-584">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="04236-584">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="04236-585">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-585">**New behavior**</span></span>

<span data-ttu-id="04236-586">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="04236-586">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="04236-587">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-587">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="04236-588">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="04236-588">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="04236-589">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="04236-589">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="04236-590">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-590">**Why**</span></span>

<span data-ttu-id="04236-591">Bu değişiklik, atılmış `DbContext` bir örnek üzerinde geç yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-591">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="04236-592">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-592">**Mitigations**</span></span>

<span data-ttu-id="04236-593">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="04236-593">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="04236-594">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="04236-594">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="04236-595">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="04236-595">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="04236-596">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-596">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-597">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-597">**Old behavior**</span></span>

<span data-ttu-id="04236-598">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="04236-598">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="04236-599">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-599">**New behavior**</span></span>

<span data-ttu-id="04236-600">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04236-600">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="04236-601">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-601">**Why**</span></span>

<span data-ttu-id="04236-602">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="04236-602">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="04236-603">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-603">**Mitigations**</span></span>

<span data-ttu-id="04236-604">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-604">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="04236-605">Ancak, hata, üzerindeki `DbContextOptionsBuilder`yapılandırması aracılığıyla bir uyarıya dönüştürülebilir (veya yok sayılır).</span><span class="sxs-lookup"><span data-stu-id="04236-605">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="04236-606">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-606">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="04236-607">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="04236-607">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="04236-608">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="04236-608">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="04236-609">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-609">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-610">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-610">**Old behavior**</span></span>

<span data-ttu-id="04236-611">EF Core 3,0 öncesinde, tek bir `HasOne` dizeye `HasMany` çağrı yapan veya kod karışık bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="04236-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="04236-612">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="04236-613">Kod, özel olabilecek `Samurai` `Entrance` gezinti özelliğini kullanarak başka bir varlık türüyle ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="04236-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="04236-614">Gerçekte, bu kod, hiçbir gezinti özelliği olmadan çağrılan `Entrance` bazı varlık türlerine bir ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="04236-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="04236-615">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-615">**New behavior**</span></span>

<span data-ttu-id="04236-616">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="04236-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="04236-617">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-617">**Why**</span></span>

<span data-ttu-id="04236-618">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="04236-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="04236-619">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-619">**Mitigations**</span></span>

<span data-ttu-id="04236-620">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="04236-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="04236-621">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="04236-621">This is not common.</span></span>
<span data-ttu-id="04236-622">Önceki davranış, gezinti özelliği adı için açıkça geçirilmesiyle `null` elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="04236-623">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="04236-624">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="04236-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="04236-625">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="04236-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="04236-626">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-626">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-627">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-627">**Old behavior**</span></span>

<span data-ttu-id="04236-628">Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürür:</span><span class="sxs-lookup"><span data-stu-id="04236-628">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="04236-629">`ValueGenerator.NextValueAsync()`(ve türetme sınıfları)</span><span class="sxs-lookup"><span data-stu-id="04236-629">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="04236-630">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-630">**New behavior**</span></span>

<span data-ttu-id="04236-631">Yukarıda bahsedilen yöntemler bundan önceki ile aynı `ValueTask<T>` `T` şekilde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="04236-631">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="04236-632">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-632">**Why**</span></span>

<span data-ttu-id="04236-633">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="04236-633">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="04236-634">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-634">**Mitigations**</span></span>

<span data-ttu-id="04236-635">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="04236-635">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="04236-636">Daha karmaşık bir kullanım `Task` (örneğin, döndürülen ' e `Task.WhenAny()`geçirme), genellikle `Task<T>` döndürülen öğesine çağırarak `ValueTask<T>` `AsTask()` döndürülen öğesine dönüştürülmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="04236-636">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="04236-637">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="04236-637">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="04236-638">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="04236-638">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="04236-639">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="04236-639">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="04236-640">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-640">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="04236-641">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-641">**Old behavior**</span></span>

<span data-ttu-id="04236-642">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="04236-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="04236-643">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-643">**New behavior**</span></span>

<span data-ttu-id="04236-644">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="04236-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="04236-645">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-645">**Why**</span></span>

<span data-ttu-id="04236-646">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="04236-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="04236-647">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-647">**Mitigations**</span></span>

<span data-ttu-id="04236-648">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="04236-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="04236-649">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="04236-650">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="04236-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="04236-651">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="04236-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="04236-652">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-652">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-653">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-653">**Old behavior**</span></span>

<span data-ttu-id="04236-654">EF Core 3,0 ' den `ToTable()` önce, yalnızca devralma eşleme stratejisi bu geçerli olmayan bir TPH olduğundan, türetilmiş bir tür üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="04236-654">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="04236-655">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-655">**New behavior**</span></span>

<span data-ttu-id="04236-656">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, `ToTable()` gelecekte beklenmedik bir eşleme değişikliğini önlemek için artık bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04236-656">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="04236-657">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-657">**Why**</span></span>

<span data-ttu-id="04236-658">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="04236-658">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="04236-659">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="04236-659">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="04236-660">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-660">**Mitigations**</span></span>

<span data-ttu-id="04236-661">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="04236-661">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="04236-662">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="04236-662">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="04236-663">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="04236-663">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="04236-664">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-664">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-665">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-665">**Old behavior**</span></span>

<span data-ttu-id="04236-666">EF Core 3,0 öncesinde, `ForSqlServerHasIndex().ForSqlServerInclude()` ile `INCLUDE`kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="04236-666">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="04236-667">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-667">**New behavior**</span></span>

<span data-ttu-id="04236-668">EF Core 3,0 ' den itibaren, `Include` bir dizinde kullanmak artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="04236-668">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="04236-669">Kullanın `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="04236-669">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="04236-670">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-670">**Why**</span></span>

<span data-ttu-id="04236-671">Bu değişiklik, tüm veritabanı sağlayıcıları için tek bir yerde bulunan `Include` dizinlere yönelik API 'leri birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-671">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="04236-672">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-672">**Mitigations**</span></span>

<span data-ttu-id="04236-673">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-673">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="04236-674">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="04236-674">Metadata API changes</span></span>

[<span data-ttu-id="04236-675">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="04236-675">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="04236-676">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-676">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-677">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-677">**New behavior**</span></span>

<span data-ttu-id="04236-678">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="04236-678">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="04236-679">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-679">**Why**</span></span>

<span data-ttu-id="04236-680">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="04236-680">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="04236-681">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-681">**Mitigations**</span></span>

<span data-ttu-id="04236-682">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-682">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="04236-683">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="04236-683">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="04236-684">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="04236-684">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="04236-685">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-685">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="04236-686">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-686">**New behavior**</span></span>

<span data-ttu-id="04236-687">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="04236-687">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="04236-688">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-688">**Why**</span></span>

<span data-ttu-id="04236-689">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="04236-689">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="04236-690">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-690">**Mitigations**</span></span>

<span data-ttu-id="04236-691">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-691">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="04236-692">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="04236-692">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="04236-693">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="04236-693">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="04236-694">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-694">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="04236-695">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-695">**Old behavior**</span></span>

<span data-ttu-id="04236-696">3,0 EF Core önce, SQLite bağlantısı açıldığında `PRAGMA foreign_keys = 1` EF Core gönderilir.</span><span class="sxs-lookup"><span data-stu-id="04236-696">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="04236-697">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-697">**New behavior**</span></span>

<span data-ttu-id="04236-698">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı `PRAGMA foreign_keys = 1` açıldığında EF Core artık göndermektedir.</span><span class="sxs-lookup"><span data-stu-id="04236-698">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="04236-699">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-699">**Why**</span></span>

<span data-ttu-id="04236-700">Bu değişiklik, EF Core varsayılan olarak kullanıldığı `SQLitePCLRaw.bundle_e_sqlite3` için yapılmıştır, bu da FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="04236-700">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="04236-701">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-701">**Mitigations**</span></span>

<span data-ttu-id="04236-702">Yabancı anahtarlar varsayılan olarak, EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 içinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="04236-702">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="04236-703">Diğer durumlarda, yabancı anahtarlar bağlantı dizeniz belirtilerek `Foreign Keys=True` etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-703">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="04236-704">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="04236-704">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="04236-705">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-705">**Old behavior**</span></span>

<span data-ttu-id="04236-706">3,0 EF Core önce EF Core kullanılır `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="04236-706">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="04236-707">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-707">**New behavior**</span></span>

<span data-ttu-id="04236-708">EF Core 3,0 ' den başlayarak EF Core kullanılır `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="04236-708">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="04236-709">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-709">**Why**</span></span>

<span data-ttu-id="04236-710">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-710">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="04236-711">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-711">**Mitigations**</span></span>

<span data-ttu-id="04236-712">İOS 'ta yerel SQLite sürümünü kullanmak için, öğesini farklı `Microsoft.Data.Sqlite` `SQLitePCLRaw` bir paket kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="04236-712">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="04236-713">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="04236-713">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="04236-714">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="04236-714">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="04236-715">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-715">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-716">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-716">**Old behavior**</span></span>

<span data-ttu-id="04236-717">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="04236-717">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="04236-718">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-718">**New behavior**</span></span>

<span data-ttu-id="04236-719">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="04236-719">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="04236-720">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-720">**Why**</span></span>

<span data-ttu-id="04236-721">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="04236-721">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="04236-722">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="04236-722">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="04236-723">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-723">**Mitigations**</span></span>

<span data-ttu-id="04236-724">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04236-724">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="04236-725">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04236-725">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="04236-726">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-726">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="04236-727">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="04236-727">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="04236-728">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="04236-728">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="04236-729">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-729">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-730">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-730">**Old behavior**</span></span>

<span data-ttu-id="04236-731">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="04236-731">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="04236-732">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="04236-732">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="04236-733">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-733">**New behavior**</span></span>

<span data-ttu-id="04236-734">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="04236-734">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="04236-735">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-735">**Why**</span></span>

<span data-ttu-id="04236-736">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="04236-736">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="04236-737">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-737">**Mitigations**</span></span>

<span data-ttu-id="04236-738">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04236-738">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="04236-739">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04236-739">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="04236-740">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="04236-740">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="04236-741">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="04236-741">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="04236-742">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="04236-742">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="04236-743">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-743">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-744">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-744">**Old behavior**</span></span>

<span data-ttu-id="04236-745">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-745">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="04236-746">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-746">**New behavior**</span></span>

<span data-ttu-id="04236-747">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-747">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="04236-748">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-748">**Why**</span></span>

<span data-ttu-id="04236-749">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="04236-749">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="04236-750">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="04236-750">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="04236-751">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-751">**Mitigations**</span></span>

<span data-ttu-id="04236-752">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="04236-752">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="04236-753">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-753">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="04236-754">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-754">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="04236-755">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-755">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="04236-756">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-756">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="04236-757">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="04236-757">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="04236-758">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-758">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="04236-759">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-759">**Old behavior**</span></span>

<span data-ttu-id="04236-760">EF Core 3,0 ' dan `UseRowNumberForPaging` önce, SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-760">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="04236-761">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-761">**New behavior**</span></span>

<span data-ttu-id="04236-762">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="04236-762">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="04236-763">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-763">**Why**</span></span>

<span data-ttu-id="04236-764">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="04236-764">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="04236-765">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-765">**Mitigations**</span></span>

<span data-ttu-id="04236-766">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="04236-766">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="04236-767">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="04236-767">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="04236-768">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04236-768">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="04236-769">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-769">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="04236-770">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="04236-770">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="04236-771">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-771">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="04236-772">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-772">**Old behavior**</span></span>

<span data-ttu-id="04236-773">`IDbContextOptionsExtension`uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="04236-773">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="04236-774">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-774">**New behavior**</span></span>

<span data-ttu-id="04236-775">Bu yöntemler yeni `DbContextOptionsExtensionInfo` `IDbContextOptionsExtension.Info` bir özellikten döndürülen yeni bir soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-775">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="04236-776">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-776">**Why**</span></span>

<span data-ttu-id="04236-777">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="04236-777">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="04236-778">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="04236-778">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="04236-779">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-779">**Mitigations**</span></span>

<span data-ttu-id="04236-780">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="04236-780">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="04236-781">EF Core kaynak kodunda farklı tür uzantılara `IDbContextOptionsExtension` yönelik birçok uygulamada örnek bulunur.</span><span class="sxs-lookup"><span data-stu-id="04236-781">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="04236-782">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="04236-782">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="04236-783">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="04236-783">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="04236-784">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-784">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-785">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="04236-785">**Change**</span></span>

<span data-ttu-id="04236-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`, olarak `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="04236-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="04236-787">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-787">**Why**</span></span>

<span data-ttu-id="04236-788">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="04236-788">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="04236-789">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-789">**Mitigations**</span></span>

<span data-ttu-id="04236-790">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-790">Use the new name.</span></span> <span data-ttu-id="04236-791">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="04236-791">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="04236-792">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="04236-792">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="04236-793">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="04236-793">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="04236-794">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-794">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-795">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-795">**Old behavior**</span></span>

<span data-ttu-id="04236-796">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="04236-796">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="04236-797">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-797">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="04236-798">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-798">**New behavior**</span></span>

<span data-ttu-id="04236-799">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="04236-799">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="04236-800">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-800">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="04236-801">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-801">**Why**</span></span>

<span data-ttu-id="04236-802">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="04236-802">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="04236-803">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-803">**Mitigations**</span></span>

<span data-ttu-id="04236-804">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-804">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="04236-805">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="04236-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="04236-806">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="04236-806">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="04236-807">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-807">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="04236-808">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-808">**Old behavior**</span></span>

<span data-ttu-id="04236-809">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="04236-809">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="04236-810">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-810">**New behavior**</span></span>

<span data-ttu-id="04236-811">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="04236-811">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="04236-812">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-812">**Why**</span></span>

<span data-ttu-id="04236-813">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="04236-813">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="04236-814">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="04236-814">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="04236-815">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-815">**Mitigations**</span></span>

<span data-ttu-id="04236-816">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="04236-816">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="04236-817">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="04236-817">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="04236-818">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="04236-818">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="04236-819">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-819">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="04236-820">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-820">**Old behavior**</span></span>

<span data-ttu-id="04236-821">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="04236-821">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="04236-822">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-822">**New behavior**</span></span>

<span data-ttu-id="04236-823">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="04236-823">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="04236-824">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="04236-824">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="04236-825">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-825">**Why**</span></span>

<span data-ttu-id="04236-826">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="04236-826">This package is only intended to be used at design time.</span></span> <span data-ttu-id="04236-827">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="04236-827">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="04236-828">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="04236-828">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="04236-829">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-829">**Mitigations**</span></span>

<span data-ttu-id="04236-830">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04236-830">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="04236-831">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="04236-831">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="04236-832">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="04236-832">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="04236-833">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="04236-833">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="04236-834">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-834">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="04236-835">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-835">**Old behavior**</span></span>

<span data-ttu-id="04236-836">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="04236-836">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="04236-837">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-837">**New behavior**</span></span>

<span data-ttu-id="04236-838">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="04236-838">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="04236-839">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-839">**Why**</span></span>

<span data-ttu-id="04236-840">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="04236-840">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="04236-841">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="04236-841">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="04236-842">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-842">**Mitigations**</span></span>

<span data-ttu-id="04236-843">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="04236-843">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="04236-844">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="04236-844">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="04236-845">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="04236-845">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="04236-846">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="04236-846">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="04236-847">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-847">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="04236-848">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-848">**Old behavior**</span></span>

<span data-ttu-id="04236-849">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="04236-849">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="04236-850">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-850">**New behavior**</span></span>

<span data-ttu-id="04236-851">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="04236-851">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="04236-852">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-852">**Why**</span></span>

<span data-ttu-id="04236-853">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="04236-853">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="04236-854">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-854">**Mitigations**</span></span>

<span data-ttu-id="04236-855">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="04236-855">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="04236-856">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="04236-856">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="04236-857">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="04236-857">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="04236-858">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="04236-858">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="04236-859">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="04236-859">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="04236-860">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-860">**Old behavior**</span></span>

<span data-ttu-id="04236-861">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="04236-861">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="04236-862">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-862">For example:</span></span>

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

<span data-ttu-id="04236-863">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="04236-863">**New behavior**</span></span>

<span data-ttu-id="04236-864">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="04236-864">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="04236-865">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="04236-865">**Why**</span></span>

<span data-ttu-id="04236-866">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="04236-866">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="04236-867">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="04236-867">**Mitigations**</span></span>

<span data-ttu-id="04236-868">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="04236-868">Use full configuration of the relationship.</span></span> <span data-ttu-id="04236-869">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="04236-869">For example:</span></span>

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
