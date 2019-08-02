---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497523"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="2987f-102">EF Core 3,0 ' de yer alan son değişiklikler (Şu anda önizlemede)</span><span class="sxs-lookup"><span data-stu-id="2987f-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2987f-103">Gelecekteki sürümlerin özellik kümelerinin ve zamanlamalarının her zaman değişikliğe tabi olduğunu ve bu sayfayı güncel tutmaya deneydiğimiz halde, her zaman en son planlarımızı yansıtmadığımızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="2987f-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="2987f-104">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken EF Core 2.2. x için geliştirilen uygulamaları kesme potansiyeline sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2987f-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="2987f-105">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](../../providers/provider-log.md)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2987f-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="2987f-106">Bir 3,0 önizlemeden başka bir 3,0 önizlemeye sunulan yeni özelliklerde kesintiler burada açıklanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="2987f-107">Özet</span><span class="sxs-lookup"><span data-stu-id="2987f-107">Summary</span></span>

| <span data-ttu-id="2987f-108">**Yeni değişiklik**</span><span class="sxs-lookup"><span data-stu-id="2987f-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="2987f-109">**Etkisi**</span><span class="sxs-lookup"><span data-stu-id="2987f-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="2987f-110">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="2987f-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="2987f-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2987f-111">High</span></span>       |
| [<span data-ttu-id="2987f-112">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2987f-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="2987f-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2987f-113">High</span></span>      |
| [<span data-ttu-id="2987f-114">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="2987f-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2987f-115">High</span></span>      |
| [<span data-ttu-id="2987f-116">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="2987f-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="2987f-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2987f-117">High</span></span>      |
| [<span data-ttu-id="2987f-118">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2987f-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="2987f-119">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-119">Medium</span></span>      |
| [<span data-ttu-id="2987f-120">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="2987f-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="2987f-121">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-121">Medium</span></span>      |
| [<span data-ttu-id="2987f-122">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="2987f-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="2987f-123">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-123">Medium</span></span>      |
| [<span data-ttu-id="2987f-124">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="2987f-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="2987f-125">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-125">Medium</span></span>      |
| [<span data-ttu-id="2987f-126">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="2987f-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="2987f-127">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-127">Medium</span></span>      |
| [<span data-ttu-id="2987f-128">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2987f-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="2987f-129">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-129">Medium</span></span>      |
| [<span data-ttu-id="2987f-130">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2987f-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="2987f-131">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-131">Medium</span></span>      |
| [<span data-ttu-id="2987f-132">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="2987f-133">Orta</span><span class="sxs-lookup"><span data-stu-id="2987f-133">Medium</span></span>      |
| [<span data-ttu-id="2987f-134">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="2987f-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="2987f-135">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-135">Low</span></span>      |
| [<span data-ttu-id="2987f-136">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="2987f-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="2987f-137">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-137">Low</span></span>      |
| [<span data-ttu-id="2987f-138">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="2987f-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="2987f-139">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-139">Low</span></span>      |
| [<span data-ttu-id="2987f-140">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="2987f-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="2987f-141">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-141">Low</span></span>      |
| [<span data-ttu-id="2987f-142">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="2987f-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="2987f-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-143">Low</span></span>      |
| [<span data-ttu-id="2987f-144">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="2987f-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="2987f-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-145">Low</span></span>      |
| [<span data-ttu-id="2987f-146">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="2987f-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="2987f-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-147">Low</span></span>      |
| [<span data-ttu-id="2987f-148">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="2987f-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="2987f-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-149">Low</span></span>      |
| [<span data-ttu-id="2987f-150">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="2987f-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="2987f-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-151">Low</span></span>      |
| [<span data-ttu-id="2987f-152">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="2987f-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="2987f-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-153">Low</span></span>      |
| [<span data-ttu-id="2987f-154">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="2987f-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="2987f-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-155">Low</span></span>      |
| [<span data-ttu-id="2987f-156">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="2987f-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="2987f-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-157">Low</span></span>      |
| [<span data-ttu-id="2987f-158">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="2987f-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="2987f-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-159">Low</span></span>      |
| [<span data-ttu-id="2987f-160">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="2987f-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="2987f-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-161">Low</span></span>      |
| [<span data-ttu-id="2987f-162">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="2987f-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="2987f-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-163">Low</span></span>      |
| [<span data-ttu-id="2987f-164">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="2987f-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="2987f-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-165">Low</span></span>      |
| [<span data-ttu-id="2987f-166">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="2987f-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="2987f-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-167">Low</span></span>      |
| [<span data-ttu-id="2987f-168">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="2987f-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="2987f-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-169">Low</span></span>      |
| [<span data-ttu-id="2987f-170">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="2987f-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="2987f-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-171">Low</span></span>      |
| [<span data-ttu-id="2987f-172">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="2987f-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="2987f-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-173">Low</span></span>      |
| [<span data-ttu-id="2987f-174">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2987f-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="2987f-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-175">Low</span></span>      |
| [<span data-ttu-id="2987f-176">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="2987f-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="2987f-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-177">Low</span></span>      |
| [<span data-ttu-id="2987f-178">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="2987f-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="2987f-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-179">Low</span></span>      |
| [<span data-ttu-id="2987f-180">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="2987f-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="2987f-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-181">Low</span></span>      |
| [<span data-ttu-id="2987f-182">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2987f-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="2987f-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-183">Low</span></span>      |
| [<span data-ttu-id="2987f-184">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2987f-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="2987f-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-185">Low</span></span>      |
| [<span data-ttu-id="2987f-186">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="2987f-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="2987f-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-187">Low</span></span>      |
| [<span data-ttu-id="2987f-188">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="2987f-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-189">Low</span></span>      |
| [<span data-ttu-id="2987f-190">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="2987f-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-191">Low</span></span>      |
| [<span data-ttu-id="2987f-192">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="2987f-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="2987f-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-193">Low</span></span>      |
| [<span data-ttu-id="2987f-194">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="2987f-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-195">Low</span></span>      |
| [<span data-ttu-id="2987f-196">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="2987f-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="2987f-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-197">Low</span></span>      |
| [<span data-ttu-id="2987f-198">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2987f-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="2987f-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-199">Low</span></span>      |
| [<span data-ttu-id="2987f-200">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2987f-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="2987f-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-201">Low</span></span>      |
| [<span data-ttu-id="2987f-202">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="2987f-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="2987f-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="2987f-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="2987f-204">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="2987f-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="2987f-205">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Ayrıca bkz. sorun #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="2987f-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="2987f-206">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-207">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-207">**Old behavior**</span></span>

<span data-ttu-id="2987f-208">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="2987f-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="2987f-209">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="2987f-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="2987f-210">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-210">**New behavior**</span></span>

<span data-ttu-id="2987f-211">3,0 ' den başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrı) istemci üzerinde değerlendirilme ifadelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="2987f-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="2987f-212">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2987f-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="2987f-213">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-213">**Why**</span></span>

<span data-ttu-id="2987f-214">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="2987f-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="2987f-215">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="2987f-216">Örneğin, bir `Where()` çağrıda çevrilemeyen bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="2987f-217">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2987f-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="2987f-218">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="2987f-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="2987f-219">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="2987f-220">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-220">**Mitigations**</span></span>

<span data-ttu-id="2987f-221">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın ya da verileri açıkça istemciye, LINQ- `AsEnumerable()`Objects `ToList()`kullanılarak daha sonra işlenebileceği istemciye geri getirmek için, veya kullanın.</span><span class="sxs-lookup"><span data-stu-id="2987f-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="2987f-222">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2987f-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="2987f-223">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="2987f-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="2987f-224">Bu değişiklik ASP.NET Core 3,0-Preview 1 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="2987f-225">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-225">**Old behavior**</span></span>

<span data-ttu-id="2987f-226">`Microsoft.AspNetCore.App` Veya`Microsoft.AspNetCore.All`' a bir paket başvurusu eklediğinizde 3,0 ASP.NET Core önce, EF Core ve SQL Server sağlayıcısı gibi EF Core veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="2987f-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="2987f-227">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-227">**New behavior**</span></span>

<span data-ttu-id="2987f-228">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="2987f-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="2987f-229">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-229">**Why**</span></span>

<span data-ttu-id="2987f-230">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="2987f-231">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="2987f-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="2987f-232">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="2987f-233">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="2987f-234">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-234">**Mitigations**</span></span>

<span data-ttu-id="2987f-235">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2987f-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="2987f-236">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2987f-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="2987f-237">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="2987f-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="2987f-238">Bu değişiklik EF Core 3,0-Preview 4 ' te ve ilgili .NET Core SDK sürümünde sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="2987f-239">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-239">**Old behavior**</span></span>

<span data-ttu-id="2987f-240">3,0 ' `dotnet ef` den önce araç .NET Core SDK eklenmiştir ve ek adımlar gerekmeden herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="2987f-241">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-241">**New behavior**</span></span>

<span data-ttu-id="2987f-242">3,0 ' den başlayarak .NET SDK, `dotnet ef` aracı içermez, bu nedenle onu kullanabilmeniz için yerel veya küresel bir araç olarak açıkça kurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="2987f-243">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-243">**Why**</span></span>

<span data-ttu-id="2987f-244">Bu değişiklik, NuGet üzerinde düzenli bir .net `dotnet ef` CLI aracı olarak dağıtmamızı ve güncelleştirmenizi sağlar ve bu da EF Core 3,0 her zaman bir NuGet paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="2987f-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="2987f-245">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-245">**Mitigations**</span></span>

<span data-ttu-id="2987f-246">Geçişleri veya yapı iskelesi `DbContext`'ni yönetebilmek için genel bir araç olarak yüklemek `dotnet-ef` için:</span><span class="sxs-lookup"><span data-stu-id="2987f-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="2987f-247">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2987f-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="2987f-248">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="2987f-249">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="2987f-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="2987f-250">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-251">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-251">**Old behavior**</span></span>

<span data-ttu-id="2987f-252">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="2987f-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="2987f-253">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-253">**New behavior**</span></span>

<span data-ttu-id="2987f-254">EF Core 3,0 ' den başlayarak, `FromSqlRaw`ve parametrelerinin sorgu `ExecuteSqlRawAsync` dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için, ve kullanın `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="2987f-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="2987f-255">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="2987f-256">Parametreleri, bir enterpolasyonlu sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için ve `FromSqlInterpolated` `ExecuteSqlInterpolatedAsync` kullanın. `ExecuteSqlInterpolated`</span><span class="sxs-lookup"><span data-stu-id="2987f-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="2987f-257">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="2987f-258">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2987f-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="2987f-259">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-259">**Why**</span></span>

<span data-ttu-id="2987f-260">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2987f-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="2987f-261">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="2987f-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="2987f-262">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-262">**Mitigations**</span></span>

<span data-ttu-id="2987f-263">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="2987f-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="2987f-264">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="2987f-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="2987f-265">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="2987f-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="2987f-266">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="2987f-267">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-267">**Old behavior**</span></span>

<span data-ttu-id="2987f-268">3,0 EF Core önce, `FromSql` Yöntem sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="2987f-269">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-269">**New behavior**</span></span>

<span data-ttu-id="2987f-270">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (değişen `FromSql`) yalnızca `DbSet<>`sorgu kökleri üzerinde belirtilebilir, yani doğrudan üzerinde.</span><span class="sxs-lookup"><span data-stu-id="2987f-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="2987f-271">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2987f-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="2987f-272">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-272">**Why**</span></span>

<span data-ttu-id="2987f-273">İçinde `FromSql` dışında herhangi bir yerde, `DbSet` hiç eklenmemiş bir anlamı veya eklenen değeri belirtmediyse, belirli senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="2987f-274">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-274">**Mitigations**</span></span>

<span data-ttu-id="2987f-275">`FromSql`etkinleştirmeleri, `DbSet` uygulandıkları üzerinde doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="2987f-276">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="2987f-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="2987f-277">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="2987f-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="2987f-278">Bu değişiklik EF Core 3,0-Preview 7 ' de geri döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="2987f-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="2987f-279">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="2987f-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="2987f-280">Örneğin, SQL `Debug`'in günlüğe kaydedilmesini değiştirmek için, `OnConfiguring` veya `AddDbContext`düzeyini açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="2987f-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="2987f-281">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="2987f-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="2987f-282">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="2987f-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="2987f-283">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="2987f-284">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-284">**Old behavior**</span></span>

<span data-ttu-id="2987f-285">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="2987f-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="2987f-286">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="2987f-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="2987f-287">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-287">**New behavior**</span></span>

<span data-ttu-id="2987f-288">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="2987f-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="2987f-289">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-289">**Why**</span></span>

<span data-ttu-id="2987f-290">Bu değişiklik, daha önce bir `DbContext` örnek tarafından daha önce izlenen bir varlık farklı `DbContext` bir örneğe taşındığında geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="2987f-291">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-291">**Mitigations**</span></span>

<span data-ttu-id="2987f-292">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlar üzerinde birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup bu `Added` durumda varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="2987f-293">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="2987f-293">This can be avoided by:</span></span>
* <span data-ttu-id="2987f-294">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="2987f-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="2987f-295">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="2987f-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="2987f-296">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="2987f-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="2987f-297">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` `blog.Id` kendi kendine ayarlanmış olsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="2987f-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="2987f-298">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="2987f-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="2987f-299">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="2987f-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="2987f-300">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-301">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-301">**Old behavior**</span></span>

<span data-ttu-id="2987f-302">EF Core 3,0 öncesinde, tarafından `DetectChanges` bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve çağrıldığında yeni bir satır `SaveChanges` olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="2987f-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="2987f-303">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-303">**New behavior**</span></span>

<span data-ttu-id="2987f-304">EF Core 3,0 ' den başlayarak, bir varlık oluşturulan anahtar değerlerini kullanıyorsa ve bir anahtar değeri ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="2987f-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="2987f-305">Bu, varlık için bir satırın var olduğu varsayılır ve çağrıldığında güncelleştirilir `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="2987f-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="2987f-306">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` izlenir.</span><span class="sxs-lookup"><span data-stu-id="2987f-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="2987f-307">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-307">**Why**</span></span>

<span data-ttu-id="2987f-308">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="2987f-309">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-309">**Mitigations**</span></span>

<span data-ttu-id="2987f-310">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="2987f-311">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="2987f-312">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="2987f-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="2987f-313">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="2987f-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="2987f-314">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="2987f-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="2987f-315">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="2987f-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="2987f-316">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-317">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-317">**Old behavior**</span></span>

<span data-ttu-id="2987f-318">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="2987f-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="2987f-319">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-319">**New behavior**</span></span>

<span data-ttu-id="2987f-320">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="2987f-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="2987f-321">Örneğin, bir sorumlu `context.Remove()` varlığı silmek için çağırmak, tüm izlenen ilgili gerekli bağımlılara da `Deleted` hemen ayarlanabilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="2987f-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="2987f-322">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-322">**Why**</span></span>

<span data-ttu-id="2987f-323">Bu değişiklik, çağrılmadan _önce_ `SaveChanges` hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="2987f-324">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-324">**Mitigations**</span></span>

<span data-ttu-id="2987f-325">Önceki davranış ayarları `context.ChangedTracker`aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="2987f-326">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="2987f-327">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="2987f-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="2987f-328">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="2987f-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="2987f-329">Bu değişiklik EF Core 3,0-Preview 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="2987f-330">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-330">**Old behavior**</span></span>

<span data-ttu-id="2987f-331">3,0 ' den `DeleteBehavior.Restrict` önce, sözdizimi ile `Restrict` veritabanında yabancı anahtarlar oluşturdunuz, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="2987f-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="2987f-332">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-332">**New behavior**</span></span>

<span data-ttu-id="2987f-333">3,0 ' den başlayarak `DeleteBehavior.Restrict` , yabancı anahtarların semantiği ile `Restrict` oluşturulmasını sağlar--Bu, basamaksız, hiçbir atlama yok; bu arada, EF iç düzeltmesini etkilemeden, kısıtlama ihlali üzerinde oluşturma.</span><span class="sxs-lookup"><span data-stu-id="2987f-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="2987f-334">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-334">**Why**</span></span>

<span data-ttu-id="2987f-335">Bu değişiklik, beklenmeyen yan etkilere gerek kalmadan sezgisel bir `DeleteBehavior` şekilde kullanma deneyimini geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="2987f-336">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-336">**Mitigations**</span></span>

<span data-ttu-id="2987f-337">Önceki davranış kullanılarak `DeleteBehavior.ClientNoAction`geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="2987f-338">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="2987f-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="2987f-339">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="2987f-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="2987f-340">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-341">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-341">**Old behavior**</span></span>

<span data-ttu-id="2987f-342">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/query-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="2987f-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="2987f-343">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="2987f-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="2987f-344">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-344">**New behavior**</span></span>

<span data-ttu-id="2987f-345">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2987f-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="2987f-346">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2987f-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="2987f-347">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-347">**Why**</span></span>

<span data-ttu-id="2987f-348">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="2987f-349">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="2987f-350">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="2987f-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="2987f-351">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-351">**Mitigations**</span></span>

<span data-ttu-id="2987f-352">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="2987f-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="2987f-353">**`ModelBuilder.Query<>()`** -Bunun `ModelBuilder.Entity<>().HasNoKey()` yerine bir varlık türünü anahtar olmadan işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="2987f-354">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="2987f-355">**`DbQuery<>`** -Bunun `DbSet<>` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="2987f-356">**`DbContext.Query<>()`** -Bunun `DbContext.Set<>()` yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="2987f-357">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="2987f-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="2987f-358">[](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
Sorun izleme #12444 izleme sorunu[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
izleme sorunu[#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="2987f-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="2987f-359">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-360">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-360">**Old behavior**</span></span>

<span data-ttu-id="2987f-361">3,0 EF Core önce, sahip olunan ilişki yapılandırması doğrudan `OwnsOne` veya `OwnsMany` çağrısından sonra gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="2987f-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="2987f-362">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-362">**New behavior**</span></span>

<span data-ttu-id="2987f-363">EF Core 3,0 ' den başlayarak, artık kullanarak `WithOwner()`bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="2987f-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="2987f-364">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="2987f-365">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin nasıl yapılandırıldığına `WithOwner()` benzer şekilde zincirde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="2987f-366">Sahip olduğu için yapılandırma, hala sonrasında `OwnsOne()/OwnsMany()`zincirleme olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="2987f-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="2987f-367">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-367">For example:</span></span>

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

<span data-ttu-id="2987f-368">Ayrıca, `Entity()` `HasOne()`, veya`Set()` sahibi olan bir tür hedefi ile çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="2987f-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="2987f-369">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-369">**Why**</span></span>

<span data-ttu-id="2987f-370">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="2987f-371">Bu, sırasıyla belirsizlik ve gibi `HasForeignKey`yöntemlere karışmasını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="2987f-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="2987f-372">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-372">**Mitigations**</span></span>

<span data-ttu-id="2987f-373">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2987f-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="2987f-374">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="2987f-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="2987f-375">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="2987f-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="2987f-376">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-377">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-377">**Old behavior**</span></span>

<span data-ttu-id="2987f-378">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2987f-378">Consider the following model:</span></span>
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
<span data-ttu-id="2987f-379">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloya `OrderDetails` eşlenmiş ise, yeni `Order`bir eklenirken örnek her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2987f-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="2987f-380">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-380">**New behavior**</span></span>

<span data-ttu-id="2987f-381">3,0 ile başlayarak, EF Core bir `Order` `OrderDetails` olmadan ekleme `OrderDetails` ve birincil anahtar null yapılabilir sütunlara hariç tüm özellikleri eşleme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2987f-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="2987f-382">Gerekli özelliklerinden herhangi birinin `OrderDetails` bir `null` değeri yoksa veya birincil `null`anahtarın ve tüm özelliklerin yanı sıra gerekli özellikleri yoksa, EF Core kümelerini sorgulama.</span><span class="sxs-lookup"><span data-stu-id="2987f-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="2987f-383">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-383">**Mitigations**</span></span>

<span data-ttu-id="2987f-384">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinti beklenmiyorsa `null` , gezinti olduğunda, `null`uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="2987f-385">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış bir`null` değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="2987f-386">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="2987f-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="2987f-387">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="2987f-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="2987f-388">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-389">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-389">**Old behavior**</span></span>

<span data-ttu-id="2987f-390">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2987f-390">Consider the following model:</span></span>
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
<span data-ttu-id="2987f-391">`OrderDetails` EF Core 3,0 önce, `Order` sahip olduğu veya açıkça aynı tabloyla eşleştirilmiş ise, yalnızca `OrderDetails` güncelleştirme, istemci üzerindeki değeri güncelleştirmez `Version` ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2987f-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="2987f-392">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-392">**New behavior**</span></span>

<span data-ttu-id="2987f-393">3,0 ile başlayarak, EF Core yeni `Version` `Order` değeri, sahip `OrderDetails`olduğu gibi yayar.</span><span class="sxs-lookup"><span data-stu-id="2987f-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="2987f-394">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2987f-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="2987f-395">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-395">**Why**</span></span>

<span data-ttu-id="2987f-396">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="2987f-397">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-397">**Mitigations**</span></span>

<span data-ttu-id="2987f-398">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="2987f-399">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="2987f-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="2987f-400">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="2987f-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="2987f-401">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="2987f-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="2987f-402">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-403">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-403">**Old behavior**</span></span>

<span data-ttu-id="2987f-404">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2987f-404">Consider the following model:</span></span>
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

<span data-ttu-id="2987f-405">3,0 EF Core önce, `ShippingAddress` özelliği, varsayılan olarak ve `Order` için `BulkOrder` ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="2987f-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="2987f-406">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-406">**New behavior**</span></span>

<span data-ttu-id="2987f-407">3,0 ile başlayarak, EF Core için `ShippingAddress`yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2987f-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="2987f-408">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-408">**Why**</span></span>

<span data-ttu-id="2987f-409">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="2987f-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="2987f-410">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-410">**Mitigations**</span></span>

<span data-ttu-id="2987f-411">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="2987f-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="2987f-412">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="2987f-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="2987f-413">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="2987f-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="2987f-414">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-415">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-415">**Old behavior**</span></span>

<span data-ttu-id="2987f-416">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2987f-416">Consider the following model:</span></span>
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
<span data-ttu-id="2987f-417">3,0 EF Core önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="2987f-418">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtarı yapar `CustomerId` ve bu genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="2987f-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="2987f-419">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-419">**New behavior**</span></span>

<span data-ttu-id="2987f-420">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="2987f-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="2987f-421">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="2987f-422">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-422">For example:</span></span>

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

<span data-ttu-id="2987f-423">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-423">**Why**</span></span>

<span data-ttu-id="2987f-424">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="2987f-425">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-425">**Mitigations**</span></span>

<span data-ttu-id="2987f-426">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2987f-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="2987f-427">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="2987f-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="2987f-428">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="2987f-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="2987f-429">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-430">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-430">**Old behavior**</span></span>

<span data-ttu-id="2987f-431">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope`içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="2987f-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="2987f-432">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-432">**New behavior**</span></span>

<span data-ttu-id="2987f-433">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="2987f-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="2987f-434">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-434">**Why**</span></span>

<span data-ttu-id="2987f-435">Bu değişiklik aynı `TransactionScope`bağlamda birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="2987f-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="2987f-436">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="2987f-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="2987f-437">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-437">**Mitigations**</span></span>

<span data-ttu-id="2987f-438">Bağlantının açık açık çağrısıyla `OpenConnection()` kalması gerekiyorsa EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="2987f-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="2987f-439">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="2987f-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="2987f-440">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="2987f-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="2987f-441">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-442">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-442">**Old behavior**</span></span>

<span data-ttu-id="2987f-443">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="2987f-444">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-444">**New behavior**</span></span>

<span data-ttu-id="2987f-445">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="2987f-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="2987f-446">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="2987f-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="2987f-447">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-447">**Why**</span></span>

<span data-ttu-id="2987f-448">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="2987f-449">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-449">**Mitigations**</span></span>

<span data-ttu-id="2987f-450">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="2987f-451">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="2987f-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="2987f-452">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="2987f-452">Backing fields are used by default</span></span>

[<span data-ttu-id="2987f-453">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="2987f-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="2987f-454">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="2987f-455">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-455">**Old behavior**</span></span>

<span data-ttu-id="2987f-456">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="2987f-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="2987f-457">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="2987f-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="2987f-458">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-458">**New behavior**</span></span>

<span data-ttu-id="2987f-459">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="2987f-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="2987f-460">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="2987f-461">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-461">**Why**</span></span>

<span data-ttu-id="2987f-462">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="2987f-463">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-463">**Mitigations**</span></span>

<span data-ttu-id="2987f-464">3,0 öncesi davranışı, üzerinde `ModelBuilder`özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="2987f-465">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="2987f-466">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="2987f-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="2987f-467">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="2987f-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="2987f-468">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-469">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-469">**Old behavior**</span></span>

<span data-ttu-id="2987f-470">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="2987f-471">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="2987f-472">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-472">**New behavior**</span></span>

<span data-ttu-id="2987f-473">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2987f-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="2987f-474">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-474">**Why**</span></span>

<span data-ttu-id="2987f-475">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="2987f-476">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-476">**Mitigations**</span></span>

<span data-ttu-id="2987f-477">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="2987f-478">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="2987f-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="2987f-479">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="2987f-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="2987f-480">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-481">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-481">**Old behavior**</span></span>

<span data-ttu-id="2987f-482">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve CLR türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="2987f-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="2987f-483">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-483">**New behavior**</span></span>

<span data-ttu-id="2987f-484">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="2987f-485">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-485">**Why**</span></span>

<span data-ttu-id="2987f-486">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2987f-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="2987f-487">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-487">**Mitigations**</span></span>

<span data-ttu-id="2987f-488">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="2987f-489">EF Core 3,0 ' nin sonraki önizlemede, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="2987f-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="2987f-490">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="2987f-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="2987f-491">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="2987f-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="2987f-492">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-493">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-493">**Old behavior**</span></span>

<span data-ttu-id="2987f-494">EF Core 3,0 ' dan önce `AddDbContext` , `AddDbContextPool` ' ı çağırarak günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini D. I ile birlikte [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrıları ile de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2987f-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="2987f-495">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-495">**New behavior**</span></span>

<span data-ttu-id="2987f-496">EF Core 3,0 ' `AddDbContext` den başlayarak ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2987f-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="2987f-497">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-497">**Why**</span></span>

<span data-ttu-id="2987f-498">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="2987f-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="2987f-499">Ancak, `ILoggerFactory` uygulamanın dı kapsayıcısına kayıtlıysa, EF Core tarafından kullanılmaya devam edilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="2987f-500">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-500">**Mitigations**</span></span>

<span data-ttu-id="2987f-501">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2987f-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="2987f-502">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="2987f-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="2987f-503">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="2987f-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="2987f-504">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-505">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-505">**Old behavior**</span></span>

<span data-ttu-id="2987f-506">EF Core 3,0 ' dan önce `DbContext.Entry` , çağırma tüm izlenen varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2987f-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="2987f-507">Bu, `EntityEntry` durumunda olan durumun güncel olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="2987f-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="2987f-508">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-508">**New behavior**</span></span>

<span data-ttu-id="2987f-509">EF Core 3,0 ' den başlayarak, `DbContext.Entry` çağırma artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="2987f-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="2987f-510">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2987f-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="2987f-511">`ChangeTracker.AutoDetectChangesEnabled` Buyereldeğişiklikalgılamaişleminin`false` devre dışı bırakılacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2987f-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="2987f-512">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` , ve `SaveChanges`, tüm izlenen varlıkların tamamen bir tamamına `DetectChanges` neden olur.</span><span class="sxs-lookup"><span data-stu-id="2987f-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="2987f-513">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-513">**Why**</span></span>

<span data-ttu-id="2987f-514">Bu değişiklik, kullanmanın `context.Entry`varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="2987f-515">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-515">**Mitigations**</span></span>

<span data-ttu-id="2987f-516">3,0 `ChgangeTracker.DetectChanges()` öncesi davranışı sağlamak `Entry` için çağrılmadan önce açıkça çağırın.</span><span class="sxs-lookup"><span data-stu-id="2987f-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="2987f-517">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="2987f-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="2987f-518">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="2987f-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="2987f-519">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-520">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-520">**Old behavior**</span></span>

<span data-ttu-id="2987f-521">3,0 `string` EF Core önce ve `byte[]` anahtar özellikler açıkça null olmayan bir değer ayarlamameden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="2987f-522">Böyle bir durumda, anahtar değeri istemcide, için `byte[]`bayt olarak SERILEŞTIRILDIĞI bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2987f-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="2987f-523">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-523">**New behavior**</span></span>

<span data-ttu-id="2987f-524">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="2987f-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="2987f-525">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-525">**Why**</span></span>

<span data-ttu-id="2987f-526">Bu değişiklik, istemci tarafından oluşturulan `string` / `byte[]` değerler genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="2987f-527">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-527">**Mitigations**</span></span>

<span data-ttu-id="2987f-528">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="2987f-529">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="2987f-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="2987f-530">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="2987f-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="2987f-531">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="2987f-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="2987f-532">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="2987f-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="2987f-533">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-534">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-534">**Old behavior**</span></span>

<span data-ttu-id="2987f-535">EF Core 3,0 ' dan `ILoggerFactory` önce, bir tek hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="2987f-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="2987f-536">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-536">**New behavior**</span></span>

<span data-ttu-id="2987f-537">EF Core 3,0 ' den itibaren `ILoggerFactory` , artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="2987f-538">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-538">**Why**</span></span>

<span data-ttu-id="2987f-539">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi `DbContext` bazı bazı durumları kaldıran bir örnek içeren bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="2987f-540">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-540">**Mitigations**</span></span>

<span data-ttu-id="2987f-541">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="2987f-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="2987f-542">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="2987f-542">This isn't common.</span></span>
<span data-ttu-id="2987f-543">Bu durumlarda, çoğu şey çalışmaya devam edecektir, ancak bağlı `ILoggerFactory` olan herhangi bir singleton hizmeti, `ILoggerFactory` farklı bir şekilde elde edilecek şekilde değiştirilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="2987f-544">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [GitHub sorun izleyici EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ' de bir sorun `ILoggerFactory` bildirin.</span><span class="sxs-lookup"><span data-stu-id="2987f-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="2987f-545">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="2987f-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="2987f-546">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="2987f-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="2987f-547">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-548">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-548">**Old behavior**</span></span>

<span data-ttu-id="2987f-549">EF Core 3,0 ' dan önce, `DbContext` bir kez atıldıktan sonra, söz konusu bağlamdan alınan bir varlık üzerinde verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="2987f-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="2987f-550">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="2987f-551">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="2987f-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="2987f-552">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-552">**New behavior**</span></span>

<span data-ttu-id="2987f-553">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="2987f-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="2987f-554">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="2987f-555">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2987f-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="2987f-556">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2987f-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="2987f-557">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-557">**Why**</span></span>

<span data-ttu-id="2987f-558">Bu değişiklik, atılmış `DbContext` bir örnek üzerinde geç yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="2987f-559">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-559">**Mitigations**</span></span>

<span data-ttu-id="2987f-560">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2987f-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="2987f-561">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="2987f-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="2987f-562">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="2987f-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="2987f-563">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-564">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-564">**Old behavior**</span></span>

<span data-ttu-id="2987f-565">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="2987f-566">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-566">**New behavior**</span></span>

<span data-ttu-id="2987f-567">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2987f-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="2987f-568">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-568">**Why**</span></span>

<span data-ttu-id="2987f-569">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="2987f-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="2987f-570">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-570">**Mitigations**</span></span>

<span data-ttu-id="2987f-571">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="2987f-572">Ancak, hata, üzerindeki `DbContextOptionsBuilder`yapılandırması aracılığıyla bir uyarıya dönüştürülebilir (veya yok sayılır).</span><span class="sxs-lookup"><span data-stu-id="2987f-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="2987f-573">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="2987f-574">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="2987f-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="2987f-575">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="2987f-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="2987f-576">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-577">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-577">**Old behavior**</span></span>

<span data-ttu-id="2987f-578">EF Core 3,0 öncesinde, tek bir `HasOne` dizeye `HasMany` çağrı yapan veya kod karışık bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="2987f-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="2987f-579">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="2987f-580">Kod, özel olabilecek `Samurai` `Entrance` gezinti özelliğini kullanarak başka bir varlık türüyle ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="2987f-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="2987f-581">Gerçekte, bu kod, hiçbir gezinti özelliği olmadan çağrılan `Entrance` bazı varlık türlerine bir ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="2987f-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="2987f-582">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-582">**New behavior**</span></span>

<span data-ttu-id="2987f-583">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="2987f-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="2987f-584">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-584">**Why**</span></span>

<span data-ttu-id="2987f-585">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="2987f-586">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-586">**Mitigations**</span></span>

<span data-ttu-id="2987f-587">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="2987f-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="2987f-588">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="2987f-588">This is not common.</span></span>
<span data-ttu-id="2987f-589">Önceki davranış, gezinti özelliği adı için açıkça geçirilmesiyle `null` elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="2987f-590">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="2987f-591">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="2987f-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="2987f-592">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="2987f-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="2987f-593">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-594">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-594">**Old behavior**</span></span>

<span data-ttu-id="2987f-595">Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürür:</span><span class="sxs-lookup"><span data-stu-id="2987f-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="2987f-596">`ValueGenerator.NextValueAsync()`(ve türetme sınıfları)</span><span class="sxs-lookup"><span data-stu-id="2987f-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="2987f-597">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-597">**New behavior**</span></span>

<span data-ttu-id="2987f-598">Yukarıda bahsedilen yöntemler bundan önceki ile aynı `ValueTask<T>` `T` şekilde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="2987f-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="2987f-599">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-599">**Why**</span></span>

<span data-ttu-id="2987f-600">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="2987f-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="2987f-601">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-601">**Mitigations**</span></span>

<span data-ttu-id="2987f-602">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2987f-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="2987f-603">Daha karmaşık bir kullanım `Task` (örneğin, döndürülen ' e `Task.WhenAny()`geçirme), genellikle `Task<T>` döndürülen öğesine çağırarak `ValueTask<T>` `AsTask()` döndürülen öğesine dönüştürülmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2987f-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="2987f-604">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2987f-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="2987f-605">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2987f-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="2987f-606">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="2987f-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="2987f-607">Bu değişiklik EF Core 3,0-Preview 2 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="2987f-608">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-608">**Old behavior**</span></span>

<span data-ttu-id="2987f-609">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="2987f-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="2987f-610">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-610">**New behavior**</span></span>

<span data-ttu-id="2987f-611">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="2987f-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="2987f-612">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-612">**Why**</span></span>

<span data-ttu-id="2987f-613">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2987f-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="2987f-614">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-614">**Mitigations**</span></span>

<span data-ttu-id="2987f-615">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="2987f-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="2987f-616">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="2987f-617">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="2987f-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="2987f-618">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="2987f-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="2987f-619">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-620">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-620">**Old behavior**</span></span>

<span data-ttu-id="2987f-621">EF Core 3,0 ' den `ToTable()` önce, yalnızca devralma eşleme stratejisi bu geçerli olmayan bir TPH olduğundan, türetilmiş bir tür üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2987f-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="2987f-622">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-622">**New behavior**</span></span>

<span data-ttu-id="2987f-623">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, `ToTable()` gelecekte beklenmedik bir eşleme değişikliğini önlemek için artık bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2987f-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="2987f-624">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-624">**Why**</span></span>

<span data-ttu-id="2987f-625">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="2987f-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="2987f-626">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="2987f-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="2987f-627">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-627">**Mitigations**</span></span>

<span data-ttu-id="2987f-628">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="2987f-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="2987f-629">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="2987f-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="2987f-630">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="2987f-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="2987f-631">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-632">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-632">**Old behavior**</span></span>

<span data-ttu-id="2987f-633">EF Core 3,0 öncesinde, `ForSqlServerHasIndex().ForSqlServerInclude()` ile `INCLUDE`kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="2987f-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="2987f-634">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-634">**New behavior**</span></span>

<span data-ttu-id="2987f-635">EF Core 3,0 ' den itibaren, `Include` bir dizinde kullanmak artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="2987f-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="2987f-636">Kullanın `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="2987f-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="2987f-637">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-637">**Why**</span></span>

<span data-ttu-id="2987f-638">Bu değişiklik, tüm veritabanı sağlayıcıları için tek bir yerde bulunan `Include` dizinlere yönelik API 'leri birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="2987f-639">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-639">**Mitigations**</span></span>

<span data-ttu-id="2987f-640">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="2987f-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="2987f-641">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2987f-641">Metadata API changes</span></span>

[<span data-ttu-id="2987f-642">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="2987f-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2987f-643">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-644">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-644">**New behavior**</span></span>

<span data-ttu-id="2987f-645">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="2987f-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="2987f-646">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-646">**Why**</span></span>

<span data-ttu-id="2987f-647">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="2987f-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="2987f-648">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-648">**Mitigations**</span></span>

<span data-ttu-id="2987f-649">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="2987f-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="2987f-650">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2987f-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="2987f-651">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="2987f-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2987f-652">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="2987f-653">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-653">**New behavior**</span></span>

<span data-ttu-id="2987f-654">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="2987f-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="2987f-655">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-655">**Why**</span></span>

<span data-ttu-id="2987f-656">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="2987f-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="2987f-657">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-657">**Mitigations**</span></span>

<span data-ttu-id="2987f-658">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="2987f-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="2987f-659">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="2987f-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="2987f-660">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="2987f-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="2987f-661">Bu değişiklik EF Core 3,0-Preview 3 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="2987f-662">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-662">**Old behavior**</span></span>

<span data-ttu-id="2987f-663">3,0 EF Core önce, SQLite bağlantısı açıldığında `PRAGMA foreign_keys = 1` EF Core gönderilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2987f-664">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-664">**New behavior**</span></span>

<span data-ttu-id="2987f-665">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı `PRAGMA foreign_keys = 1` açıldığında EF Core artık göndermektedir.</span><span class="sxs-lookup"><span data-stu-id="2987f-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2987f-666">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-666">**Why**</span></span>

<span data-ttu-id="2987f-667">Bu değişiklik, EF Core varsayılan olarak kullanıldığı `SQLitePCLRaw.bundle_e_sqlite3` için yapılmıştır, bu da FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2987f-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="2987f-668">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-668">**Mitigations**</span></span>

<span data-ttu-id="2987f-669">Yabancı anahtarlar varsayılan olarak, EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 içinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="2987f-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="2987f-670">Diğer durumlarda, yabancı anahtarlar bağlantı dizeniz belirtilerek `Foreign Keys=True` etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="2987f-671">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 öğesine bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="2987f-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="2987f-672">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-672">**Old behavior**</span></span>

<span data-ttu-id="2987f-673">3,0 EF Core önce EF Core kullanılır `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="2987f-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="2987f-674">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-674">**New behavior**</span></span>

<span data-ttu-id="2987f-675">EF Core 3,0 ' den başlayarak EF Core kullanılır `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="2987f-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="2987f-676">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-676">**Why**</span></span>

<span data-ttu-id="2987f-677">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="2987f-678">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-678">**Mitigations**</span></span>

<span data-ttu-id="2987f-679">İOS 'ta yerel SQLite sürümünü kullanmak için, öğesini farklı `Microsoft.Data.Sqlite` `SQLitePCLRaw` bir paket kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2987f-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2987f-680">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2987f-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2987f-681">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="2987f-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="2987f-682">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-683">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-683">**Old behavior**</span></span>

<span data-ttu-id="2987f-684">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="2987f-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="2987f-685">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-685">**New behavior**</span></span>

<span data-ttu-id="2987f-686">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="2987f-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="2987f-687">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-687">**Why**</span></span>

<span data-ttu-id="2987f-688">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="2987f-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="2987f-689">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2987f-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2987f-690">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-690">**Mitigations**</span></span>

<span data-ttu-id="2987f-691">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2987f-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="2987f-692">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2987f-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="2987f-693">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2987f-694">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2987f-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2987f-695">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="2987f-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="2987f-696">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-697">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-697">**Old behavior**</span></span>

<span data-ttu-id="2987f-698">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="2987f-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="2987f-699">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="2987f-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="2987f-700">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-700">**New behavior**</span></span>

<span data-ttu-id="2987f-701">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="2987f-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="2987f-702">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-702">**Why**</span></span>

<span data-ttu-id="2987f-703">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2987f-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2987f-704">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-704">**Mitigations**</span></span>

<span data-ttu-id="2987f-705">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2987f-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="2987f-706">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2987f-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="2987f-707">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="2987f-708">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="2987f-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="2987f-709">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="2987f-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="2987f-710">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-711">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-711">**Old behavior**</span></span>

<span data-ttu-id="2987f-712">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="2987f-713">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-713">**New behavior**</span></span>

<span data-ttu-id="2987f-714">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="2987f-715">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-715">**Why**</span></span>

<span data-ttu-id="2987f-716">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="2987f-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="2987f-717">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="2987f-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="2987f-718">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-718">**Mitigations**</span></span>

<span data-ttu-id="2987f-719">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="2987f-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="2987f-720">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="2987f-721">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="2987f-722">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="2987f-723">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="2987f-724">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="2987f-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="2987f-725">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="2987f-726">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-726">**Old behavior**</span></span>

<span data-ttu-id="2987f-727">EF Core 3,0 ' dan `UseRowNumberForPaging` önce, SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="2987f-728">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-728">**New behavior**</span></span>

<span data-ttu-id="2987f-729">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2987f-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="2987f-730">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-730">**Why**</span></span>

<span data-ttu-id="2987f-731">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2987f-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="2987f-732">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-732">**Mitigations**</span></span>

<span data-ttu-id="2987f-733">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="2987f-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="2987f-734">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="2987f-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="2987f-735">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2987f-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="2987f-736">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="2987f-737">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="2987f-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="2987f-738">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="2987f-739">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-739">**Old behavior**</span></span>

<span data-ttu-id="2987f-740">`IDbContextOptionsExtension`uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="2987f-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="2987f-741">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-741">**New behavior**</span></span>

<span data-ttu-id="2987f-742">Bu yöntemler yeni `DbContextOptionsExtensionInfo` `IDbContextOptionsExtension.Info` bir özellikten döndürülen yeni bir soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="2987f-743">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-743">**Why**</span></span>

<span data-ttu-id="2987f-744">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="2987f-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="2987f-745">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2987f-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="2987f-746">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-746">**Mitigations**</span></span>

<span data-ttu-id="2987f-747">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2987f-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="2987f-748">EF Core kaynak kodunda farklı tür uzantılara `IDbContextOptionsExtension` yönelik birçok uygulamada örnek bulunur.</span><span class="sxs-lookup"><span data-stu-id="2987f-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="2987f-749">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="2987f-750">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="2987f-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="2987f-751">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-752">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="2987f-752">**Change**</span></span>

<span data-ttu-id="2987f-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`, olarak `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="2987f-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="2987f-754">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-754">**Why**</span></span>

<span data-ttu-id="2987f-755">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="2987f-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="2987f-756">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-756">**Mitigations**</span></span>

<span data-ttu-id="2987f-757">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="2987f-757">Use the new name.</span></span> <span data-ttu-id="2987f-758">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="2987f-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="2987f-759">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="2987f-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="2987f-760">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="2987f-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="2987f-761">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-762">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-762">**Old behavior**</span></span>

<span data-ttu-id="2987f-763">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="2987f-764">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="2987f-765">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-765">**New behavior**</span></span>

<span data-ttu-id="2987f-766">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2987f-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="2987f-767">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="2987f-768">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-768">**Why**</span></span>

<span data-ttu-id="2987f-769">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="2987f-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="2987f-770">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-770">**Mitigations**</span></span>

<span data-ttu-id="2987f-771">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="2987f-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="2987f-772">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="2987f-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="2987f-773">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="2987f-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="2987f-774">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="2987f-775">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-775">**Old behavior**</span></span>

<span data-ttu-id="2987f-776">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="2987f-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="2987f-777">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-777">**New behavior**</span></span>

<span data-ttu-id="2987f-778">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="2987f-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="2987f-779">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-779">**Why**</span></span>

<span data-ttu-id="2987f-780">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2987f-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="2987f-781">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2987f-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="2987f-782">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-782">**Mitigations**</span></span>

<span data-ttu-id="2987f-783">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2987f-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="2987f-784">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="2987f-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="2987f-785">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="2987f-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="2987f-786">Bu değişiklik EF Core 3,0-Preview 4 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="2987f-787">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-787">**Old behavior**</span></span>

<span data-ttu-id="2987f-788">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="2987f-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="2987f-789">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-789">**New behavior**</span></span>

<span data-ttu-id="2987f-790">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="2987f-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="2987f-791">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2987f-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="2987f-792">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-792">**Why**</span></span>

<span data-ttu-id="2987f-793">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2987f-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="2987f-794">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2987f-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="2987f-795">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="2987f-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="2987f-796">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-796">**Mitigations**</span></span>

<span data-ttu-id="2987f-797">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2987f-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="2987f-798">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2987f-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="2987f-799">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2987f-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="2987f-800">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="2987f-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="2987f-801">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="2987f-802">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-802">**Old behavior**</span></span>

<span data-ttu-id="2987f-803">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="2987f-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="2987f-804">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-804">**New behavior**</span></span>

<span data-ttu-id="2987f-805">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="2987f-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2987f-806">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-806">**Why**</span></span>

<span data-ttu-id="2987f-807">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="2987f-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="2987f-808">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="2987f-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="2987f-809">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-809">**Mitigations**</span></span>

<span data-ttu-id="2987f-810">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="2987f-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2987f-811">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="2987f-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="2987f-812">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2987f-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="2987f-813">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="2987f-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="2987f-814">Bu değişiklik EF Core 3,0-Preview 7 ' de kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="2987f-815">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-815">**Old behavior**</span></span>

<span data-ttu-id="2987f-816">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="2987f-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="2987f-817">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-817">**New behavior**</span></span>

<span data-ttu-id="2987f-818">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="2987f-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2987f-819">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-819">**Why**</span></span>

<span data-ttu-id="2987f-820">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2987f-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="2987f-821">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-821">**Mitigations**</span></span>

<span data-ttu-id="2987f-822">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="2987f-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2987f-823">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="2987f-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="2987f-824">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="2987f-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="2987f-825">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="2987f-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="2987f-826">Bu değişiklik EF Core 3,0-Preview 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2987f-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="2987f-827">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-827">**Old behavior**</span></span>

<span data-ttu-id="2987f-828">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="2987f-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="2987f-829">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-829">For example:</span></span>

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

<span data-ttu-id="2987f-830">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2987f-830">**New behavior**</span></span>

<span data-ttu-id="2987f-831">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="2987f-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="2987f-832">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2987f-832">**Why**</span></span>

<span data-ttu-id="2987f-833">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="2987f-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="2987f-834">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2987f-834">**Mitigations**</span></span>

<span data-ttu-id="2987f-835">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2987f-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="2987f-836">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2987f-836">For example:</span></span>

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
