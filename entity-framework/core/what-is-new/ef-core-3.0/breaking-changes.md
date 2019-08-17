---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565317"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="ffea9-102">EF Core 3,0 ' de yer alan son değişiklikler (Şu anda önizlemede)</span><span class="sxs-lookup"><span data-stu-id="ffea9-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ffea9-103">Gelecekteki sürümlerin özellik kümelerinin ve zamanlamalarının her zaman değişikliğe tabi olduğunu ve bu sayfayı güncel tutmaya deneydiğimiz halde, her zaman en son planlarımızı yansıtmadığımızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="ffea9-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="ffea9-104">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken EF Core 2.2. x için geliştirilen uygulamaları kesme potansiyeline sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="ffea9-105">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](../../providers/provider-log.md)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="ffea9-106">Bir 3,0 önizlemeden başka bir 3,0 önizlemeye sunulan yeni özelliklerde kesintiler burada açıklanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="ffea9-107">Özet</span><span class="sxs-lookup"><span data-stu-id="ffea9-107">Summary</span></span>

| <span data-ttu-id="ffea9-108">**Yeni değişiklik**</span><span class="sxs-lookup"><span data-stu-id="ffea9-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="ffea9-109">**Etkisi**</span><span class="sxs-lookup"><span data-stu-id="ffea9-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="ffea9-110">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="ffea9-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="ffea9-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="ffea9-111">High</span></span>       |
| [<span data-ttu-id="ffea9-112">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="ffea9-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="ffea9-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="ffea9-113">High</span></span>      |
| [<span data-ttu-id="ffea9-114">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="ffea9-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="ffea9-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="ffea9-115">High</span></span>      |
| [<span data-ttu-id="ffea9-116">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="ffea9-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="ffea9-117">High</span></span>      |
| [<span data-ttu-id="ffea9-118">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="ffea9-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="ffea9-119">Yüksek</span><span class="sxs-lookup"><span data-stu-id="ffea9-119">High</span></span>      |
| [<span data-ttu-id="ffea9-120">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="ffea9-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="ffea9-121">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-121">Medium</span></span>      |
| [<span data-ttu-id="ffea9-122">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="ffea9-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="ffea9-123">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-123">Medium</span></span>      |
| [<span data-ttu-id="ffea9-124">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="ffea9-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="ffea9-125">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-125">Medium</span></span>      |
| [<span data-ttu-id="ffea9-126">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="ffea9-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="ffea9-127">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-127">Medium</span></span>      |
| [<span data-ttu-id="ffea9-128">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="ffea9-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="ffea9-129">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-129">Medium</span></span>      |
| [<span data-ttu-id="ffea9-130">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="ffea9-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="ffea9-131">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-131">Medium</span></span>      |
| [<span data-ttu-id="ffea9-132">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="ffea9-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="ffea9-133">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-133">Medium</span></span>      |
| [<span data-ttu-id="ffea9-134">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="ffea9-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="ffea9-135">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-135">Medium</span></span>      |
| [<span data-ttu-id="ffea9-136">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="ffea9-137">Orta</span><span class="sxs-lookup"><span data-stu-id="ffea9-137">Medium</span></span>      |
| [<span data-ttu-id="ffea9-138">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="ffea9-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="ffea9-139">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-139">Low</span></span>      |
| [<span data-ttu-id="ffea9-140">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="ffea9-141">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-141">Low</span></span>      |
| [<span data-ttu-id="ffea9-142">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="ffea9-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="ffea9-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-143">Low</span></span>      |
| [<span data-ttu-id="ffea9-144">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="ffea9-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="ffea9-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-145">Low</span></span>      |
| [<span data-ttu-id="ffea9-146">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="ffea9-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="ffea9-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-147">Low</span></span>      |
| [<span data-ttu-id="ffea9-148">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="ffea9-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="ffea9-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-149">Low</span></span>      |
| [<span data-ttu-id="ffea9-150">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="ffea9-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="ffea9-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-151">Low</span></span>      |
| [<span data-ttu-id="ffea9-152">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="ffea9-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-153">Low</span></span>      |
| [<span data-ttu-id="ffea9-154">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="ffea9-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="ffea9-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-155">Low</span></span>      |
| [<span data-ttu-id="ffea9-156">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="ffea9-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="ffea9-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-157">Low</span></span>      |
| [<span data-ttu-id="ffea9-158">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="ffea9-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="ffea9-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-159">Low</span></span>      |
| [<span data-ttu-id="ffea9-160">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="ffea9-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="ffea9-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-161">Low</span></span>      |
| [<span data-ttu-id="ffea9-162">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="ffea9-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-163">Low</span></span>      |
| [<span data-ttu-id="ffea9-164">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="ffea9-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-165">Low</span></span>      |
| [<span data-ttu-id="ffea9-166">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="ffea9-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="ffea9-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-167">Low</span></span>      |
| [<span data-ttu-id="ffea9-168">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="ffea9-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="ffea9-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-169">Low</span></span>      |
| [<span data-ttu-id="ffea9-170">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="ffea9-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="ffea9-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-171">Low</span></span>      |
| [<span data-ttu-id="ffea9-172">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="ffea9-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="ffea9-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-173">Low</span></span>      |
| [<span data-ttu-id="ffea9-174">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="ffea9-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="ffea9-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-175">Low</span></span>      |
| [<span data-ttu-id="ffea9-176">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="ffea9-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-177">Low</span></span>      |
| [<span data-ttu-id="ffea9-178">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="ffea9-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="ffea9-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-179">Low</span></span>      |
| [<span data-ttu-id="ffea9-180">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="ffea9-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="ffea9-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-181">Low</span></span>      |
| [<span data-ttu-id="ffea9-182">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="ffea9-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="ffea9-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-183">Low</span></span>      |
| [<span data-ttu-id="ffea9-184">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="ffea9-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="ffea9-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-185">Low</span></span>      |
| [<span data-ttu-id="ffea9-186">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="ffea9-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="ffea9-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-187">Low</span></span>      |
| [<span data-ttu-id="ffea9-188">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="ffea9-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="ffea9-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-189">Low</span></span>      |
| [<span data-ttu-id="ffea9-190">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="ffea9-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="ffea9-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-191">Low</span></span>      |
| [<span data-ttu-id="ffea9-192">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="ffea9-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-193">Low</span></span>      |
| [<span data-ttu-id="ffea9-194">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="ffea9-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-195">Low</span></span>      |
| [<span data-ttu-id="ffea9-196">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="ffea9-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="ffea9-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-197">Low</span></span>      |
| [<span data-ttu-id="ffea9-198">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="ffea9-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-199">Low</span></span>      |
| [<span data-ttu-id="ffea9-200">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="ffea9-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="ffea9-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-201">Low</span></span>      |
| [<span data-ttu-id="ffea9-202">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="ffea9-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-203">Low</span></span>      |
| [<span data-ttu-id="ffea9-204">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="ffea9-205">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-205">Low</span></span>      |
| [<span data-ttu-id="ffea9-206">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="ffea9-207">Düşük</span><span class="sxs-lookup"><span data-stu-id="ffea9-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="ffea9-208">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="ffea9-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="ffea9-209">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Ayrıca bkz. sorun #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="ffea9-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="ffea9-210">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-211">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-211">**Old behavior**</span></span>

<span data-ttu-id="ffea9-212">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="ffea9-213">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="ffea9-214">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-214">**New behavior**</span></span>

<span data-ttu-id="ffea9-215">3,0 ' den başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrı) istemci üzerinde değerlendirilme ifadelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="ffea9-216">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="ffea9-217">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-217">**Why**</span></span>

<span data-ttu-id="ffea9-218">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="ffea9-219">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="ffea9-220">Örneğin, bir `Where()` çağrıda çevrilemeyen bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="ffea9-221">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="ffea9-222">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="ffea9-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="ffea9-223">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="ffea9-224">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-224">**Mitigations**</span></span>

<span data-ttu-id="ffea9-225">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın ya da verileri açıkça istemciye, LINQ- `AsEnumerable()`Objects `ToList()`kullanılarak daha sonra işlenebileceği istemciye geri getirmek için, veya kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="ffea9-226">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="ffea9-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="ffea9-227">Sorun izleniyor #15498</span><span class="sxs-lookup"><span data-stu-id="ffea9-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="ffea9-228">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="ffea9-229">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-229">**Old behavior**</span></span>

<span data-ttu-id="ffea9-230">3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="ffea9-231">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-231">**New behavior**</span></span>

<span data-ttu-id="ffea9-232">3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="ffea9-233">Bu, .NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="ffea9-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="ffea9-234">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-234">**Why**</span></span>

<span data-ttu-id="ffea9-235">Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="ffea9-236">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-236">**Mitigations**</span></span>

<span data-ttu-id="ffea9-237">Modern bir .NET platformuna geçmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="ffea9-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="ffea9-238">Bu mümkün değilse, her ikisi de .NET Framework EF Core 2,1 veya EF Core 2,2 ' i kullanmaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="ffea9-239">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="ffea9-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="ffea9-240">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="ffea9-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="ffea9-241">Bu değişiklik ASP.NET Core 3,0-Preview 1 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="ffea9-242">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-242">**Old behavior**</span></span>

<span data-ttu-id="ffea9-243">`Microsoft.AspNetCore.App` Veya`Microsoft.AspNetCore.All`' a bir paket başvurusu eklediğinizde 3,0 ASP.NET Core önce, EF Core ve SQL Server sağlayıcısı gibi EF Core veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="ffea9-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="ffea9-244">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-244">**New behavior**</span></span>

<span data-ttu-id="ffea9-245">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="ffea9-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="ffea9-246">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-246">**Why**</span></span>

<span data-ttu-id="ffea9-247">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="ffea9-248">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="ffea9-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="ffea9-249">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="ffea9-250">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="ffea9-251">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-251">**Mitigations**</span></span>

<span data-ttu-id="ffea9-252">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="ffea9-253">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="ffea9-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="ffea9-254">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="ffea9-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="ffea9-255">Bu değişiklik EF Core 3,0-Preview 4 ' te ve ilgili .NET Core SDK sürümünde sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="ffea9-256">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-256">**Old behavior**</span></span>

<span data-ttu-id="ffea9-257">3,0 ' `dotnet ef` den önce araç .NET Core SDK eklenmiştir ve ek adımlar gerekmeden herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="ffea9-258">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-258">**New behavior**</span></span>

<span data-ttu-id="ffea9-259">3,0 ' den başlayarak .NET SDK, `dotnet ef` aracı içermez, bu nedenle onu kullanabilmeniz için yerel veya küresel bir araç olarak açıkça kurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="ffea9-260">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-260">**Why**</span></span>

<span data-ttu-id="ffea9-261">Bu değişiklik, NuGet üzerinde düzenli bir .net `dotnet ef` CLI aracı olarak dağıtmamızı ve güncelleştirmenizi sağlar ve bu da EF Core 3,0 her zaman bir NuGet paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="ffea9-262">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-262">**Mitigations**</span></span>

<span data-ttu-id="ffea9-263">Geçişleri veya yapı iskelesi `DbContext`'ni yönetebilmek için genel bir araç olarak yüklemek `dotnet-ef` için:</span><span class="sxs-lookup"><span data-stu-id="ffea9-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="ffea9-264">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="ffea9-265">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="ffea9-266">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="ffea9-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="ffea9-267">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-268">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-268">**Old behavior**</span></span>

<span data-ttu-id="ffea9-269">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="ffea9-270">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-270">**New behavior**</span></span>

<span data-ttu-id="ffea9-271">EF Core 3,0 ' den başlayarak, `FromSqlRaw`ve parametrelerinin sorgu `ExecuteSqlRawAsync` dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için, ve kullanın `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="ffea9-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="ffea9-272">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="ffea9-273">Parametreleri, bir enterpolasyonlu sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için ve `FromSqlInterpolated` `ExecuteSqlInterpolatedAsync` kullanın. `ExecuteSqlInterpolated`</span><span class="sxs-lookup"><span data-stu-id="ffea9-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="ffea9-274">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="ffea9-275">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="ffea9-276">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-276">**Why**</span></span>

<span data-ttu-id="ffea9-277">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="ffea9-278">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="ffea9-279">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-279">**Mitigations**</span></span>

<span data-ttu-id="ffea9-280">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="ffea9-281">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="ffea9-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="ffea9-282">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="ffea9-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="ffea9-283">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="ffea9-284">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-284">**Old behavior**</span></span>

<span data-ttu-id="ffea9-285">3,0 EF Core önce, `FromSql` Yöntem sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="ffea9-286">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-286">**New behavior**</span></span>

<span data-ttu-id="ffea9-287">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (değişen `FromSql`) yalnızca `DbSet<>`sorgu kökleri üzerinde belirtilebilir, yani doğrudan üzerinde.</span><span class="sxs-lookup"><span data-stu-id="ffea9-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="ffea9-288">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="ffea9-289">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-289">**Why**</span></span>

<span data-ttu-id="ffea9-290">İçinde `FromSql` dışında herhangi bir yerde, `DbSet` hiç eklenmemiş bir anlamı veya eklenen değeri belirtmediyse, belirli senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="ffea9-291">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-291">**Mitigations**</span></span>

<span data-ttu-id="ffea9-292">`FromSql`etkinleştirmeleri, `DbSet` uygulandıkları üzerinde doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="ffea9-293">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="ffea9-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="ffea9-294">Sorun izleniyor #13518</span><span class="sxs-lookup"><span data-stu-id="ffea9-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="ffea9-295">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="ffea9-296">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-296">**Old behavior**</span></span>

<span data-ttu-id="ffea9-297">EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="ffea9-298">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="ffea9-299">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="ffea9-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="ffea9-300">, belirtilen kategori ile `Category` ilişkili her biri `Product` için aynı örneği döndürür.</span><span class="sxs-lookup"><span data-stu-id="ffea9-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="ffea9-301">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-301">**New behavior**</span></span>

<span data-ttu-id="ffea9-302">EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="ffea9-303">Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde `Category` bile, her `Product` biri için yeni bir örnek döndürür.</span><span class="sxs-lookup"><span data-stu-id="ffea9-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="ffea9-304">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-304">**Why**</span></span>

<span data-ttu-id="ffea9-305">Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="ffea9-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="ffea9-306">Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="ffea9-307">Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="ffea9-308">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-308">**Mitigations**</span></span>

<span data-ttu-id="ffea9-309">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="ffea9-310">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="ffea9-311">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="ffea9-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="ffea9-312">Bu değişiklik EF Core 3,0-Preview 7 ' de geri döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="ffea9-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="ffea9-313">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="ffea9-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="ffea9-314">Örneğin, SQL `Debug`'in günlüğe kaydedilmesini değiştirmek için, `OnConfiguring` veya `AddDbContext`düzeyini açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ffea9-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="ffea9-315">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="ffea9-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="ffea9-316">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="ffea9-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="ffea9-317">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ffea9-318">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-318">**Old behavior**</span></span>

<span data-ttu-id="ffea9-319">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="ffea9-320">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="ffea9-321">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-321">**New behavior**</span></span>

<span data-ttu-id="ffea9-322">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="ffea9-323">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-323">**Why**</span></span>

<span data-ttu-id="ffea9-324">Bu değişiklik, daha önce bir `DbContext` örnek tarafından daha önce izlenen bir varlık farklı `DbContext` bir örneğe taşındığında geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="ffea9-325">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-325">**Mitigations**</span></span>

<span data-ttu-id="ffea9-326">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlar üzerinde birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup bu `Added` durumda varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="ffea9-327">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="ffea9-327">This can be avoided by:</span></span>
* <span data-ttu-id="ffea9-328">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="ffea9-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="ffea9-329">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="ffea9-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="ffea9-330">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="ffea9-331">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` `blog.Id` kendi kendine ayarlanmış olsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="ffea9-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="ffea9-332">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="ffea9-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="ffea9-333">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="ffea9-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="ffea9-334">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-335">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-335">**Old behavior**</span></span>

<span data-ttu-id="ffea9-336">EF Core 3,0 öncesinde, tarafından `DetectChanges` bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve çağrıldığında yeni bir satır `SaveChanges` olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="ffea9-337">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-337">**New behavior**</span></span>

<span data-ttu-id="ffea9-338">EF Core 3,0 ' den başlayarak, bir varlık oluşturulan anahtar değerlerini kullanıyorsa ve bir anahtar değeri ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="ffea9-339">Bu, varlık için bir satırın var olduğu varsayılır ve çağrıldığında güncelleştirilir `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="ffea9-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="ffea9-340">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` izlenir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="ffea9-341">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-341">**Why**</span></span>

<span data-ttu-id="ffea9-342">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="ffea9-343">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-343">**Mitigations**</span></span>

<span data-ttu-id="ffea9-344">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="ffea9-345">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="ffea9-346">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="ffea9-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="ffea9-347">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="ffea9-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="ffea9-348">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="ffea9-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="ffea9-349">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="ffea9-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="ffea9-350">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-351">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-351">**Old behavior**</span></span>

<span data-ttu-id="ffea9-352">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="ffea9-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="ffea9-353">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-353">**New behavior**</span></span>

<span data-ttu-id="ffea9-354">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="ffea9-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="ffea9-355">Örneğin, bir sorumlu `context.Remove()` varlığı silmek için çağırmak, tüm izlenen ilgili gerekli bağımlılara da `Deleted` hemen ayarlanabilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="ffea9-356">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-356">**Why**</span></span>

<span data-ttu-id="ffea9-357">Bu değişiklik, çağrılmadan _önce_ `SaveChanges` hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="ffea9-358">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-358">**Mitigations**</span></span>

<span data-ttu-id="ffea9-359">Önceki davranış ayarları `context.ChangedTracker`aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="ffea9-360">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="ffea9-361">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="ffea9-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="ffea9-362">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="ffea9-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="ffea9-363">Bu değişiklik EF Core 3,0-Preview 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="ffea9-364">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-364">**Old behavior**</span></span>

<span data-ttu-id="ffea9-365">3,0 ' den `DeleteBehavior.Restrict` önce, sözdizimi ile `Restrict` veritabanında yabancı anahtarlar oluşturdunuz, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="ffea9-366">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-366">**New behavior**</span></span>

<span data-ttu-id="ffea9-367">3,0 ' den başlayarak `DeleteBehavior.Restrict` , yabancı anahtarların semantiği ile `Restrict` oluşturulmasını sağlar--Bu, basamaksız, hiçbir atlama yok; bu arada, EF iç düzeltmesini etkilemeden, kısıtlama ihlali üzerinde oluşturma.</span><span class="sxs-lookup"><span data-stu-id="ffea9-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="ffea9-368">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-368">**Why**</span></span>

<span data-ttu-id="ffea9-369">Bu değişiklik, beklenmeyen yan etkilere gerek kalmadan sezgisel bir `DeleteBehavior` şekilde kullanma deneyimini geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="ffea9-370">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-370">**Mitigations**</span></span>

<span data-ttu-id="ffea9-371">Önceki davranış kullanılarak `DeleteBehavior.ClientNoAction`geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="ffea9-372">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="ffea9-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="ffea9-373">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="ffea9-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="ffea9-374">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-375">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-375">**Old behavior**</span></span>

<span data-ttu-id="ffea9-376">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/query-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="ffea9-377">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="ffea9-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="ffea9-378">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-378">**New behavior**</span></span>

<span data-ttu-id="ffea9-379">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="ffea9-380">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="ffea9-381">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-381">**Why**</span></span>

<span data-ttu-id="ffea9-382">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="ffea9-383">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="ffea9-384">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="ffea9-385">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-385">**Mitigations**</span></span>

<span data-ttu-id="ffea9-386">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="ffea9-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="ffea9-387">**`ModelBuilder.Query<>()`** -Bunun `ModelBuilder.Entity<>().HasNoKey()` yerine bir varlık türünü anahtar olmadan işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="ffea9-388">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="ffea9-389">**`DbQuery<>`** -Bunun `DbSet<>` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="ffea9-390">**`DbContext.Query<>()`** -Bunun `DbContext.Set<>()` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="ffea9-391">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="ffea9-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="ffea9-392">[](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
Sorun izleme #12444 izleme sorunu[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
izleme sorunu[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="ffea9-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="ffea9-393">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-394">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-394">**Old behavior**</span></span>

<span data-ttu-id="ffea9-395">3,0 EF Core önce, sahip olunan ilişki yapılandırması doğrudan `OwnsOne` veya `OwnsMany` çağrısından sonra gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="ffea9-396">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-396">**New behavior**</span></span>

<span data-ttu-id="ffea9-397">EF Core 3,0 ' den başlayarak, artık kullanarak `WithOwner()`bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="ffea9-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="ffea9-398">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="ffea9-399">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin nasıl yapılandırıldığına `WithOwner()` benzer şekilde zincirde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="ffea9-400">Sahip olduğu için yapılandırma, hala sonrasında `OwnsOne()/OwnsMany()`zincirleme olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="ffea9-401">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-401">For example:</span></span>

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

<span data-ttu-id="ffea9-402">Ayrıca, `Entity()` `HasOne()`, veya`Set()` sahibi olan bir tür hedefi ile çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="ffea9-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="ffea9-403">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-403">**Why**</span></span>

<span data-ttu-id="ffea9-404">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="ffea9-405">Bu, sırasıyla belirsizlik ve gibi `HasForeignKey`yöntemlere karışmasını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="ffea9-406">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-406">**Mitigations**</span></span>

<span data-ttu-id="ffea9-407">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="ffea9-408">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="ffea9-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="ffea9-409">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="ffea9-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="ffea9-410">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-411">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-411">**Old behavior**</span></span>

<span data-ttu-id="ffea9-412">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ffea9-412">Consider the following model:</span></span>
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
<span data-ttu-id="ffea9-413">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloya `OrderDetails` eşlenmiş ise, yeni `Order`bir eklenirken örnek her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="ffea9-414">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-414">**New behavior**</span></span>

<span data-ttu-id="ffea9-415">3,0 ile başlayarak, EF Core bir `Order` `OrderDetails` olmadan ekleme `OrderDetails` ve birincil anahtar null yapılabilir sütunlara hariç tüm özellikleri eşleme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="ffea9-416">Gerekli özelliklerinden herhangi birinin `OrderDetails` bir `null` değeri yoksa veya birincil `null`anahtarın ve tüm özelliklerin yanı sıra gerekli özellikleri yoksa, EF Core kümelerini sorgulama.</span><span class="sxs-lookup"><span data-stu-id="ffea9-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="ffea9-417">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-417">**Mitigations**</span></span>

<span data-ttu-id="ffea9-418">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinti beklenmiyorsa `null` , gezinti olduğunda, `null`uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="ffea9-419">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış bir`null` değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="ffea9-420">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="ffea9-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="ffea9-421">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="ffea9-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="ffea9-422">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-423">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-423">**Old behavior**</span></span>

<span data-ttu-id="ffea9-424">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ffea9-424">Consider the following model:</span></span>
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
<span data-ttu-id="ffea9-425">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloyla eşleştirilmiş ise, yalnızca `OrderDetails` güncelleştirme, istemci üzerindeki değeri güncelleştirmez `Version` ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="ffea9-426">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-426">**New behavior**</span></span>

<span data-ttu-id="ffea9-427">3,0 ile başlayarak, EF Core yeni `Version` `Order` değeri, sahip `OrderDetails`olduğu gibi yayar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="ffea9-428">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="ffea9-429">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-429">**Why**</span></span>

<span data-ttu-id="ffea9-430">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="ffea9-431">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-431">**Mitigations**</span></span>

<span data-ttu-id="ffea9-432">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="ffea9-433">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="ffea9-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="ffea9-434">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="ffea9-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="ffea9-435">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="ffea9-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="ffea9-436">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-437">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-437">**Old behavior**</span></span>

<span data-ttu-id="ffea9-438">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ffea9-438">Consider the following model:</span></span>
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

<span data-ttu-id="ffea9-439">3,0 EF Core önce, `ShippingAddress` özelliği, varsayılan olarak ve `Order` için `BulkOrder` ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="ffea9-440">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-440">**New behavior**</span></span>

<span data-ttu-id="ffea9-441">3,0 ile başlayarak, EF Core için `ShippingAddress`yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="ffea9-442">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-442">**Why**</span></span>

<span data-ttu-id="ffea9-443">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="ffea9-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="ffea9-444">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-444">**Mitigations**</span></span>

<span data-ttu-id="ffea9-445">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="ffea9-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="ffea9-446">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="ffea9-447">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="ffea9-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="ffea9-448">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-449">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-449">**Old behavior**</span></span>

<span data-ttu-id="ffea9-450">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ffea9-450">Consider the following model:</span></span>
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
<span data-ttu-id="ffea9-451">3,0 EF Core önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="ffea9-452">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtarı yapar `CustomerId` ve bu genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="ffea9-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="ffea9-453">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-453">**New behavior**</span></span>

<span data-ttu-id="ffea9-454">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="ffea9-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="ffea9-455">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="ffea9-456">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-456">For example:</span></span>

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

<span data-ttu-id="ffea9-457">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-457">**Why**</span></span>

<span data-ttu-id="ffea9-458">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="ffea9-459">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-459">**Mitigations**</span></span>

<span data-ttu-id="ffea9-460">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="ffea9-461">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="ffea9-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="ffea9-462">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="ffea9-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="ffea9-463">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-464">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-464">**Old behavior**</span></span>

<span data-ttu-id="ffea9-465">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope`içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="ffea9-466">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-466">**New behavior**</span></span>

<span data-ttu-id="ffea9-467">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="ffea9-468">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-468">**Why**</span></span>

<span data-ttu-id="ffea9-469">Bu değişiklik aynı `TransactionScope`bağlamda birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="ffea9-470">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="ffea9-471">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-471">**Mitigations**</span></span>

<span data-ttu-id="ffea9-472">Bağlantının açık açık çağrısıyla `OpenConnection()` kalması gerekiyorsa EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="ffea9-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="ffea9-473">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="ffea9-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="ffea9-474">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="ffea9-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="ffea9-475">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-476">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-476">**Old behavior**</span></span>

<span data-ttu-id="ffea9-477">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="ffea9-478">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-478">**New behavior**</span></span>

<span data-ttu-id="ffea9-479">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="ffea9-480">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="ffea9-481">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-481">**Why**</span></span>

<span data-ttu-id="ffea9-482">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="ffea9-483">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-483">**Mitigations**</span></span>

<span data-ttu-id="ffea9-484">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="ffea9-485">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="ffea9-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="ffea9-486">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="ffea9-486">Backing fields are used by default</span></span>

[<span data-ttu-id="ffea9-487">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="ffea9-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="ffea9-488">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ffea9-489">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-489">**Old behavior**</span></span>

<span data-ttu-id="ffea9-490">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="ffea9-491">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="ffea9-492">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-492">**New behavior**</span></span>

<span data-ttu-id="ffea9-493">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="ffea9-494">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="ffea9-495">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-495">**Why**</span></span>

<span data-ttu-id="ffea9-496">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="ffea9-497">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-497">**Mitigations**</span></span>

<span data-ttu-id="ffea9-498">3,0 öncesi davranışı, üzerinde `ModelBuilder`özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="ffea9-499">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="ffea9-500">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="ffea9-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="ffea9-501">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="ffea9-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="ffea9-502">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-503">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-503">**Old behavior**</span></span>

<span data-ttu-id="ffea9-504">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="ffea9-505">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="ffea9-506">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-506">**New behavior**</span></span>

<span data-ttu-id="ffea9-507">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="ffea9-508">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-508">**Why**</span></span>

<span data-ttu-id="ffea9-509">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="ffea9-510">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-510">**Mitigations**</span></span>

<span data-ttu-id="ffea9-511">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="ffea9-512">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="ffea9-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="ffea9-513">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="ffea9-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="ffea9-514">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-515">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-515">**Old behavior**</span></span>

<span data-ttu-id="ffea9-516">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve CLR türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="ffea9-517">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-517">**New behavior**</span></span>

<span data-ttu-id="ffea9-518">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="ffea9-519">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-519">**Why**</span></span>

<span data-ttu-id="ffea9-520">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="ffea9-521">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-521">**Mitigations**</span></span>

<span data-ttu-id="ffea9-522">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="ffea9-523">EF Core 3,0 ' nin sonraki önizlemede, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="ffea9-523">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="ffea9-524">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="ffea9-525">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="ffea9-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="ffea9-526">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-527">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-527">**Old behavior**</span></span>

<span data-ttu-id="ffea9-528">EF Core 3,0 ' dan önce `AddDbContext` , `AddDbContextPool` ' ı çağırarak günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini D. I ile birlikte [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrıları ile de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ffea9-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="ffea9-529">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-529">**New behavior**</span></span>

<span data-ttu-id="ffea9-530">EF Core 3,0 ' `AddDbContext` den başlayarak ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="ffea9-531">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-531">**Why**</span></span>

<span data-ttu-id="ffea9-532">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ffea9-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="ffea9-533">Ancak, `ILoggerFactory` uygulamanın dı kapsayıcısına kayıtlıysa, EF Core tarafından kullanılmaya devam edilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="ffea9-534">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-534">**Mitigations**</span></span>

<span data-ttu-id="ffea9-535">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="ffea9-536">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="ffea9-537">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="ffea9-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ffea9-538">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-539">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-539">**Old behavior**</span></span>

<span data-ttu-id="ffea9-540">EF Core 3,0 ' dan önce `DbContext.Entry` , çağırma tüm izlenen varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="ffea9-541">Bu, `EntityEntry` durumunda olan durumun güncel olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="ffea9-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="ffea9-542">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-542">**New behavior**</span></span>

<span data-ttu-id="ffea9-543">EF Core 3,0 ' den başlayarak, `DbContext.Entry` çağırma artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="ffea9-544">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="ffea9-545">`ChangeTracker.AutoDetectChangesEnabled` Buyereldeğişiklikalgılamaişleminin`false` devre dışı bırakılacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="ffea9-546">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` , ve `SaveChanges`, tüm izlenen varlıkların tamamen bir tamamına `DetectChanges` neden olur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="ffea9-547">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-547">**Why**</span></span>

<span data-ttu-id="ffea9-548">Bu değişiklik, kullanmanın `context.Entry`varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="ffea9-549">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-549">**Mitigations**</span></span>

<span data-ttu-id="ffea9-550">3,0 `ChgangeTracker.DetectChanges()` öncesi davranışı sağlamak `Entry` için çağrılmadan önce açıkça çağırın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="ffea9-551">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="ffea9-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="ffea9-552">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="ffea9-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="ffea9-553">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-554">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-554">**Old behavior**</span></span>

<span data-ttu-id="ffea9-555">3,0 `string` EF Core önce ve `byte[]` anahtar özellikler açıkça null olmayan bir değer ayarlamameden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="ffea9-556">Böyle bir durumda, anahtar değeri istemcide, için `byte[]`bayt olarak SERILEŞTIRILDIĞI bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="ffea9-557">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-557">**New behavior**</span></span>

<span data-ttu-id="ffea9-558">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="ffea9-559">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-559">**Why**</span></span>

<span data-ttu-id="ffea9-560">Bu değişiklik, istemci tarafından oluşturulan `string` / `byte[]` değerler genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="ffea9-561">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-561">**Mitigations**</span></span>

<span data-ttu-id="ffea9-562">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="ffea9-563">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="ffea9-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="ffea9-564">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="ffea9-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="ffea9-565">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="ffea9-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="ffea9-566">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="ffea9-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="ffea9-567">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-568">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-568">**Old behavior**</span></span>

<span data-ttu-id="ffea9-569">EF Core 3,0 ' dan `ILoggerFactory` önce, bir tek hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="ffea9-570">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-570">**New behavior**</span></span>

<span data-ttu-id="ffea9-571">EF Core 3,0 ' den itibaren `ILoggerFactory` , artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="ffea9-572">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-572">**Why**</span></span>

<span data-ttu-id="ffea9-573">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi `DbContext` bazı bazı durumları kaldıran bir örnek içeren bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="ffea9-574">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-574">**Mitigations**</span></span>

<span data-ttu-id="ffea9-575">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="ffea9-576">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-576">This isn't common.</span></span>
<span data-ttu-id="ffea9-577">Bu durumlarda, çoğu şey çalışmaya devam edecektir, ancak bağlı `ILoggerFactory` olan herhangi bir singleton hizmeti, `ILoggerFactory` farklı bir şekilde elde edilecek şekilde değiştirilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="ffea9-578">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [GitHub sorun izleyici EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ' de bir sorun `ILoggerFactory` bildirin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="ffea9-579">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="ffea9-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="ffea9-580">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="ffea9-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="ffea9-581">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-582">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-582">**Old behavior**</span></span>

<span data-ttu-id="ffea9-583">EF Core 3,0 ' dan önce, `DbContext` bir kez atıldıktan sonra, söz konusu bağlamdan alınan bir varlık üzerinde verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="ffea9-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="ffea9-584">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="ffea9-585">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="ffea9-586">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-586">**New behavior**</span></span>

<span data-ttu-id="ffea9-587">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="ffea9-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="ffea9-588">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="ffea9-589">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="ffea9-590">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="ffea9-591">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-591">**Why**</span></span>

<span data-ttu-id="ffea9-592">Bu değişiklik, atılmış `DbContext` bir örnek üzerinde geç yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="ffea9-593">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-593">**Mitigations**</span></span>

<span data-ttu-id="ffea9-594">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="ffea9-595">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="ffea9-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="ffea9-596">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="ffea9-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="ffea9-597">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-598">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-598">**Old behavior**</span></span>

<span data-ttu-id="ffea9-599">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="ffea9-600">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-600">**New behavior**</span></span>

<span data-ttu-id="ffea9-601">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="ffea9-602">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-602">**Why**</span></span>

<span data-ttu-id="ffea9-603">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="ffea9-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="ffea9-604">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-604">**Mitigations**</span></span>

<span data-ttu-id="ffea9-605">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="ffea9-606">Ancak, hata, üzerindeki `DbContextOptionsBuilder`yapılandırması aracılığıyla bir uyarıya dönüştürülebilir (veya yok sayılır).</span><span class="sxs-lookup"><span data-stu-id="ffea9-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="ffea9-607">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="ffea9-608">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="ffea9-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="ffea9-609">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="ffea9-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="ffea9-610">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-611">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-611">**Old behavior**</span></span>

<span data-ttu-id="ffea9-612">EF Core 3,0 öncesinde, tek bir `HasOne` dizeye `HasMany` çağrı yapan veya kod karışık bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="ffea9-613">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="ffea9-614">Kod, özel olabilecek `Samurai` `Entrance` gezinti özelliğini kullanarak başka bir varlık türüyle ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="ffea9-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="ffea9-615">Gerçekte, bu kod, hiçbir gezinti özelliği olmadan çağrılan `Entrance` bazı varlık türlerine bir ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="ffea9-616">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-616">**New behavior**</span></span>

<span data-ttu-id="ffea9-617">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="ffea9-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="ffea9-618">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-618">**Why**</span></span>

<span data-ttu-id="ffea9-619">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="ffea9-620">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-620">**Mitigations**</span></span>

<span data-ttu-id="ffea9-621">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="ffea9-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="ffea9-622">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-622">This is not common.</span></span>
<span data-ttu-id="ffea9-623">Önceki davranış, gezinti özelliği adı için açıkça geçirilmesiyle `null` elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="ffea9-624">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="ffea9-625">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="ffea9-626">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="ffea9-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="ffea9-627">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-628">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-628">**Old behavior**</span></span>

<span data-ttu-id="ffea9-629">Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürür:</span><span class="sxs-lookup"><span data-stu-id="ffea9-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="ffea9-630">`ValueGenerator.NextValueAsync()`(ve türetme sınıfları)</span><span class="sxs-lookup"><span data-stu-id="ffea9-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="ffea9-631">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-631">**New behavior**</span></span>

<span data-ttu-id="ffea9-632">Yukarıda bahsedilen yöntemler bundan önceki ile aynı `ValueTask<T>` `T` şekilde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ffea9-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="ffea9-633">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-633">**Why**</span></span>

<span data-ttu-id="ffea9-634">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="ffea9-635">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-635">**Mitigations**</span></span>

<span data-ttu-id="ffea9-636">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="ffea9-637">Daha karmaşık bir kullanım `Task` (örneğin, döndürülen ' e `Task.WhenAny()`geçirme), genellikle `Task<T>` döndürülen öğesine çağırarak `ValueTask<T>` `AsTask()` döndürülen öğesine dönüştürülmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="ffea9-638">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="ffea9-639">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="ffea9-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="ffea9-640">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="ffea9-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="ffea9-641">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ffea9-642">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-642">**Old behavior**</span></span>

<span data-ttu-id="ffea9-643">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="ffea9-644">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-644">**New behavior**</span></span>

<span data-ttu-id="ffea9-645">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="ffea9-646">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-646">**Why**</span></span>

<span data-ttu-id="ffea9-647">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="ffea9-648">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-648">**Mitigations**</span></span>

<span data-ttu-id="ffea9-649">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="ffea9-650">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="ffea9-651">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="ffea9-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="ffea9-652">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="ffea9-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="ffea9-653">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-654">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-654">**Old behavior**</span></span>

<span data-ttu-id="ffea9-655">EF Core 3,0 ' den `ToTable()` önce, yalnızca devralma eşleme stratejisi bu geçerli olmayan bir TPH olduğundan, türetilmiş bir tür üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="ffea9-656">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-656">**New behavior**</span></span>

<span data-ttu-id="ffea9-657">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, `ToTable()` gelecekte beklenmedik bir eşleme değişikliğini önlemek için artık bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="ffea9-658">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-658">**Why**</span></span>

<span data-ttu-id="ffea9-659">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="ffea9-660">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="ffea9-661">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-661">**Mitigations**</span></span>

<span data-ttu-id="ffea9-662">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="ffea9-663">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="ffea9-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="ffea9-664">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="ffea9-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="ffea9-665">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-666">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-666">**Old behavior**</span></span>

<span data-ttu-id="ffea9-667">EF Core 3,0 öncesinde, `ForSqlServerHasIndex().ForSqlServerInclude()` ile `INCLUDE`kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="ffea9-668">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-668">**New behavior**</span></span>

<span data-ttu-id="ffea9-669">EF Core 3,0 ' den itibaren, `Include` bir dizinde kullanmak artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="ffea9-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="ffea9-670">Kullanın `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="ffea9-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="ffea9-671">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-671">**Why**</span></span>

<span data-ttu-id="ffea9-672">Bu değişiklik, tüm veritabanı sağlayıcıları için tek bir yerde bulunan `Include` dizinlere yönelik API 'leri birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="ffea9-673">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-673">**Mitigations**</span></span>

<span data-ttu-id="ffea9-674">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="ffea9-675">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="ffea9-675">Metadata API changes</span></span>

[<span data-ttu-id="ffea9-676">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="ffea9-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="ffea9-677">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-678">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-678">**New behavior**</span></span>

<span data-ttu-id="ffea9-679">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="ffea9-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="ffea9-680">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-680">**Why**</span></span>

<span data-ttu-id="ffea9-681">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="ffea9-682">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-682">**Mitigations**</span></span>

<span data-ttu-id="ffea9-683">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="ffea9-684">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="ffea9-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="ffea9-685">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="ffea9-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="ffea9-686">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="ffea9-687">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-687">**New behavior**</span></span>

<span data-ttu-id="ffea9-688">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="ffea9-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="ffea9-689">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-689">**Why**</span></span>

<span data-ttu-id="ffea9-690">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="ffea9-691">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-691">**Mitigations**</span></span>

<span data-ttu-id="ffea9-692">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="ffea9-693">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="ffea9-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="ffea9-694">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="ffea9-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="ffea9-695">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ffea9-696">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-696">**Old behavior**</span></span>

<span data-ttu-id="ffea9-697">3,0 EF Core önce, SQLite bağlantısı açıldığında `PRAGMA foreign_keys = 1` EF Core gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ffea9-698">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-698">**New behavior**</span></span>

<span data-ttu-id="ffea9-699">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı `PRAGMA foreign_keys = 1` açıldığında EF Core artık göndermektedir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ffea9-700">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-700">**Why**</span></span>

<span data-ttu-id="ffea9-701">Bu değişiklik, EF Core varsayılan olarak kullanıldığı `SQLitePCLRaw.bundle_e_sqlite3` için yapılmıştır, bu da FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="ffea9-702">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-702">**Mitigations**</span></span>

<span data-ttu-id="ffea9-703">Yabancı anahtarlar varsayılan olarak, EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 içinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="ffea9-704">Diğer durumlarda, yabancı anahtarlar bağlantı dizeniz belirtilerek `Foreign Keys=True` etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="ffea9-705">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="ffea9-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="ffea9-706">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-706">**Old behavior**</span></span>

<span data-ttu-id="ffea9-707">3,0 EF Core önce EF Core kullanılır `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="ffea9-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="ffea9-708">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-708">**New behavior**</span></span>

<span data-ttu-id="ffea9-709">EF Core 3,0 ' den başlayarak EF Core kullanılır `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="ffea9-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="ffea9-710">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-710">**Why**</span></span>

<span data-ttu-id="ffea9-711">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="ffea9-712">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-712">**Mitigations**</span></span>

<span data-ttu-id="ffea9-713">İOS 'ta yerel SQLite sürümünü kullanmak için, öğesini farklı `Microsoft.Data.Sqlite` `SQLitePCLRaw` bir paket kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="ffea9-714">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="ffea9-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="ffea9-715">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="ffea9-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="ffea9-716">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-717">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-717">**Old behavior**</span></span>

<span data-ttu-id="ffea9-718">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="ffea9-719">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-719">**New behavior**</span></span>

<span data-ttu-id="ffea9-720">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="ffea9-721">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-721">**Why**</span></span>

<span data-ttu-id="ffea9-722">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="ffea9-723">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="ffea9-724">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-724">**Mitigations**</span></span>

<span data-ttu-id="ffea9-725">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="ffea9-726">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="ffea9-727">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="ffea9-728">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="ffea9-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="ffea9-729">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="ffea9-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="ffea9-730">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-731">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-731">**Old behavior**</span></span>

<span data-ttu-id="ffea9-732">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="ffea9-733">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="ffea9-734">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-734">**New behavior**</span></span>

<span data-ttu-id="ffea9-735">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="ffea9-736">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-736">**Why**</span></span>

<span data-ttu-id="ffea9-737">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="ffea9-738">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-738">**Mitigations**</span></span>

<span data-ttu-id="ffea9-739">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="ffea9-740">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="ffea9-741">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="ffea9-742">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="ffea9-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="ffea9-743">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="ffea9-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="ffea9-744">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-745">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-745">**Old behavior**</span></span>

<span data-ttu-id="ffea9-746">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="ffea9-747">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-747">**New behavior**</span></span>

<span data-ttu-id="ffea9-748">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="ffea9-749">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-749">**Why**</span></span>

<span data-ttu-id="ffea9-750">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="ffea9-751">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="ffea9-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="ffea9-752">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-752">**Mitigations**</span></span>

<span data-ttu-id="ffea9-753">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="ffea9-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="ffea9-754">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="ffea9-755">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="ffea9-756">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="ffea9-757">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="ffea9-758">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="ffea9-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="ffea9-759">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="ffea9-760">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-760">**Old behavior**</span></span>

<span data-ttu-id="ffea9-761">EF Core 3,0 ' dan `UseRowNumberForPaging` önce, SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="ffea9-762">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-762">**New behavior**</span></span>

<span data-ttu-id="ffea9-763">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="ffea9-764">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-764">**Why**</span></span>

<span data-ttu-id="ffea9-765">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="ffea9-766">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-766">**Mitigations**</span></span>

<span data-ttu-id="ffea9-767">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="ffea9-768">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="ffea9-769">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="ffea9-770">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="ffea9-771">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="ffea9-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="ffea9-772">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="ffea9-773">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-773">**Old behavior**</span></span>

<span data-ttu-id="ffea9-774">`IDbContextOptionsExtension`uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="ffea9-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="ffea9-775">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-775">**New behavior**</span></span>

<span data-ttu-id="ffea9-776">Bu yöntemler yeni `DbContextOptionsExtensionInfo` `IDbContextOptionsExtension.Info` bir özellikten döndürülen yeni bir soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="ffea9-777">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-777">**Why**</span></span>

<span data-ttu-id="ffea9-778">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="ffea9-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="ffea9-779">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="ffea9-780">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-780">**Mitigations**</span></span>

<span data-ttu-id="ffea9-781">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="ffea9-782">EF Core kaynak kodunda farklı tür uzantılara `IDbContextOptionsExtension` yönelik birçok uygulamada örnek bulunur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="ffea9-783">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="ffea9-784">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="ffea9-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="ffea9-785">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-786">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="ffea9-786">**Change**</span></span>

<span data-ttu-id="ffea9-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`, olarak `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="ffea9-788">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-788">**Why**</span></span>

<span data-ttu-id="ffea9-789">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="ffea9-790">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-790">**Mitigations**</span></span>

<span data-ttu-id="ffea9-791">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-791">Use the new name.</span></span> <span data-ttu-id="ffea9-792">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="ffea9-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="ffea9-793">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="ffea9-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="ffea9-794">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="ffea9-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="ffea9-795">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-796">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-796">**Old behavior**</span></span>

<span data-ttu-id="ffea9-797">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="ffea9-798">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="ffea9-799">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-799">**New behavior**</span></span>

<span data-ttu-id="ffea9-800">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="ffea9-801">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="ffea9-802">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-802">**Why**</span></span>

<span data-ttu-id="ffea9-803">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="ffea9-804">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-804">**Mitigations**</span></span>

<span data-ttu-id="ffea9-805">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="ffea9-806">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="ffea9-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="ffea9-807">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="ffea9-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="ffea9-808">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="ffea9-809">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-809">**Old behavior**</span></span>

<span data-ttu-id="ffea9-810">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="ffea9-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="ffea9-811">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-811">**New behavior**</span></span>

<span data-ttu-id="ffea9-812">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="ffea9-813">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-813">**Why**</span></span>

<span data-ttu-id="ffea9-814">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="ffea9-815">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="ffea9-816">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-816">**Mitigations**</span></span>

<span data-ttu-id="ffea9-817">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ffea9-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="ffea9-818">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="ffea9-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="ffea9-819">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="ffea9-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="ffea9-820">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ffea9-821">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-821">**Old behavior**</span></span>

<span data-ttu-id="ffea9-822">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="ffea9-823">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-823">**New behavior**</span></span>

<span data-ttu-id="ffea9-824">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="ffea9-825">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="ffea9-826">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-826">**Why**</span></span>

<span data-ttu-id="ffea9-827">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="ffea9-828">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="ffea9-829">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="ffea9-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="ffea9-830">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-830">**Mitigations**</span></span>

<span data-ttu-id="ffea9-831">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffea9-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="ffea9-832">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="ffea9-833">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="ffea9-834">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="ffea9-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="ffea9-835">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="ffea9-836">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-836">**Old behavior**</span></span>

<span data-ttu-id="ffea9-837">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="ffea9-838">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-838">**New behavior**</span></span>

<span data-ttu-id="ffea9-839">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="ffea9-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="ffea9-840">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-840">**Why**</span></span>

<span data-ttu-id="ffea9-841">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="ffea9-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="ffea9-842">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="ffea9-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="ffea9-843">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-843">**Mitigations**</span></span>

<span data-ttu-id="ffea9-844">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="ffea9-845">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="ffea9-846">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="ffea9-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="ffea9-847">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="ffea9-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="ffea9-848">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="ffea9-849">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-849">**Old behavior**</span></span>

<span data-ttu-id="ffea9-850">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="ffea9-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="ffea9-851">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-851">**New behavior**</span></span>

<span data-ttu-id="ffea9-852">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="ffea9-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="ffea9-853">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-853">**Why**</span></span>

<span data-ttu-id="ffea9-854">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="ffea9-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="ffea9-855">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-855">**Mitigations**</span></span>

<span data-ttu-id="ffea9-856">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="ffea9-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="ffea9-857">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="ffea9-858">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="ffea9-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="ffea9-859">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="ffea9-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="ffea9-860">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="ffea9-861">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-861">**Old behavior**</span></span>

<span data-ttu-id="ffea9-862">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="ffea9-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="ffea9-863">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-863">For example:</span></span>

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

<span data-ttu-id="ffea9-864">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="ffea9-864">**New behavior**</span></span>

<span data-ttu-id="ffea9-865">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="ffea9-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="ffea9-866">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="ffea9-866">**Why**</span></span>

<span data-ttu-id="ffea9-867">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="ffea9-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="ffea9-868">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="ffea9-868">**Mitigations**</span></span>

<span data-ttu-id="ffea9-869">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="ffea9-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="ffea9-870">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffea9-870">For example:</span></span>

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
