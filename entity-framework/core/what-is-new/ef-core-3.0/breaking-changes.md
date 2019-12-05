---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: d614103169837238810fabd0a8889043c851ef14
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824871"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="5dc4c-102">EF Core 3,0 ' de yer alan son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="5dc4c-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="5dc4c-103">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken mevcut uygulamaları bozmak için olası bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="5dc4c-104">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="5dc4c-105">Özet</span><span class="sxs-lookup"><span data-stu-id="5dc4c-105">Summary</span></span>

| <span data-ttu-id="5dc4c-106">**Yeni değişiklik**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="5dc4c-107">**Etkisi**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="5dc4c-108">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="5dc4c-109">Yüksek</span><span class="sxs-lookup"><span data-stu-id="5dc4c-109">High</span></span>       |
| [<span data-ttu-id="5dc4c-110">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="5dc4c-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="5dc4c-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="5dc4c-111">High</span></span>      |
| [<span data-ttu-id="5dc4c-112">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="5dc4c-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="5dc4c-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="5dc4c-113">High</span></span>      |
| [<span data-ttu-id="5dc4c-114">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="5dc4c-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="5dc4c-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="5dc4c-115">High</span></span>      |
| [<span data-ttu-id="5dc4c-116">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="5dc4c-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="5dc4c-117">High</span></span>      |
| [<span data-ttu-id="5dc4c-118">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="5dc4c-119">Yüksek</span><span class="sxs-lookup"><span data-stu-id="5dc4c-119">High</span></span>      |
| [<span data-ttu-id="5dc4c-120">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="5dc4c-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="5dc4c-121">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-121">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-122">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="5dc4c-123">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-123">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-124">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="5dc4c-125">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-125">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-126">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="5dc4c-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="5dc4c-127">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-127">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-128">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="5dc4c-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="5dc4c-129">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-129">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-130">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="5dc4c-131">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-131">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-132">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="5dc4c-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="5dc4c-133">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-133">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-134">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="5dc4c-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="5dc4c-135">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-135">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-136">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="5dc4c-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="5dc4c-137">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-137">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-138">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="5dc4c-139">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-139">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-140">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="5dc4c-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="5dc4c-141">Orta</span><span class="sxs-lookup"><span data-stu-id="5dc4c-141">Medium</span></span>      |
| [<span data-ttu-id="5dc4c-142">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="5dc4c-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-143">Low</span></span>      |
| [<span data-ttu-id="5dc4c-144">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="5dc4c-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-145">Low</span></span>      |
| [<span data-ttu-id="5dc4c-146">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="5dc4c-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="5dc4c-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-147">Low</span></span>      |
| [<span data-ttu-id="5dc4c-148">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="5dc4c-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-149">Low</span></span>      |
| [<span data-ttu-id="5dc4c-150">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="5dc4c-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-151">Low</span></span>      |
| [<span data-ttu-id="5dc4c-152">Sahip olunan varlıklar, izleme sorgusu kullanılarak sahip olmadan sorgulanamıyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="5dc4c-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-153">Low</span></span>      |
| [<span data-ttu-id="5dc4c-154">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="5dc4c-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-155">Low</span></span>      |
| [<span data-ttu-id="5dc4c-156">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="5dc4c-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-157">Low</span></span>      |
| [<span data-ttu-id="5dc4c-158">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="5dc4c-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-159">Low</span></span>      |
| [<span data-ttu-id="5dc4c-160">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="5dc4c-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-161">Low</span></span>      |
| [<span data-ttu-id="5dc4c-162">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="5dc4c-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="5dc4c-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-163">Low</span></span>      |
| [<span data-ttu-id="5dc4c-164">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="5dc4c-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-165">Low</span></span>      |
| [<span data-ttu-id="5dc4c-166">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="5dc4c-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-167">Low</span></span>      |
| [<span data-ttu-id="5dc4c-168">AddEntityFramework \* bir boyut sınırı ile ımemorycache ekler</span><span class="sxs-lookup"><span data-stu-id="5dc4c-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="5dc4c-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-169">Low</span></span>      |
| [<span data-ttu-id="5dc4c-170">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="5dc4c-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-171">Low</span></span>      |
| [<span data-ttu-id="5dc4c-172">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="5dc4c-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="5dc4c-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-173">Low</span></span>      |
| [<span data-ttu-id="5dc4c-174">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="5dc4c-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-175">Low</span></span>      |
| [<span data-ttu-id="5dc4c-176">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="5dc4c-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="5dc4c-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-177">Low</span></span>      |
| [<span data-ttu-id="5dc4c-178">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="5dc4c-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-179">Low</span></span>      |
| [<span data-ttu-id="5dc4c-180">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="5dc4c-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="5dc4c-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-181">Low</span></span>      |
| [<span data-ttu-id="5dc4c-182">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="5dc4c-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-183">Low</span></span>      |
| [<span data-ttu-id="5dc4c-184">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="5dc4c-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="5dc4c-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-185">Low</span></span>      |
| [<span data-ttu-id="5dc4c-186">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="5dc4c-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="5dc4c-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-187">Low</span></span>      |
| [<span data-ttu-id="5dc4c-188">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="5dc4c-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="5dc4c-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-189">Low</span></span>      |
| [<span data-ttu-id="5dc4c-190">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="5dc4c-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-191">Low</span></span>      |
| [<span data-ttu-id="5dc4c-192">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="5dc4c-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-193">Low</span></span>      |
| [<span data-ttu-id="5dc4c-194">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="5dc4c-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-195">Low</span></span>      |
| [<span data-ttu-id="5dc4c-196">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="5dc4c-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="5dc4c-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-197">Low</span></span>      |
| [<span data-ttu-id="5dc4c-198">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="5dc4c-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-199">Low</span></span>      |
| [<span data-ttu-id="5dc4c-200">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="5dc4c-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-201">Low</span></span>      |
| [<span data-ttu-id="5dc4c-202">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="5dc4c-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="5dc4c-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-203">Low</span></span>      |
| [<span data-ttu-id="5dc4c-204">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="5dc4c-205">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-205">Low</span></span>      |
| [<span data-ttu-id="5dc4c-206">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="5dc4c-207">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-207">Low</span></span>      |
| [<span data-ttu-id="5dc4c-208">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="5dc4c-209">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-209">Low</span></span>      |
| [<span data-ttu-id="5dc4c-210">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="5dc4c-211">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-211">Low</span></span>      |
| [<span data-ttu-id="5dc4c-212">System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="5dc4c-213">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-213">Low</span></span>      |
| [<span data-ttu-id="5dc4c-214">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="5dc4c-215">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-215">Low</span></span>      |
| [<span data-ttu-id="5dc4c-216">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="5dc4c-217">Düşük</span><span class="sxs-lookup"><span data-stu-id="5dc4c-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="5dc4c-218">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="5dc4c-219">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[da bakın #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="5dc4c-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="5dc4c-220">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-220">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-221">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="5dc4c-222">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="5dc4c-223">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-223">**New behavior**</span></span>

<span data-ttu-id="5dc4c-224">3,0 ' den başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrı) istemci üzerinde değerlendirilmek üzere ifade etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="5dc4c-225">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="5dc4c-226">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-226">**Why**</span></span>

<span data-ttu-id="5dc4c-227">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="5dc4c-228">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="5dc4c-229">Örneğin, çevrilemeyen bir `Where()` çağrısındaki bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="5dc4c-230">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="5dc4c-231">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="5dc4c-232">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="5dc4c-233">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-233">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-234">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın veya `AsEnumerable()`, `ToList()`ya da benzer bir şekilde, daha sonra LINQ-Objects kullanılarak daha sonra işlenebileceği istemciye açık bir şekilde veri getirmek için benzer.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="5dc4c-235">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="5dc4c-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="5dc4c-236">Sorun izleniyor #15498</span><span class="sxs-lookup"><span data-stu-id="5dc4c-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="5dc4c-237">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-237">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-238">3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-238">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="5dc4c-239">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-239">**New behavior**</span></span>

<span data-ttu-id="5dc4c-240">3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-240">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="5dc4c-241">Bu, .NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-241">This does not include .NET Framework.</span></span>

<span data-ttu-id="5dc4c-242">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-242">**Why**</span></span>

<span data-ttu-id="5dc4c-243">Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-243">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="5dc4c-244">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-244">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-245">Modern bir .NET platformuna geçmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-245">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="5dc4c-246">Bu mümkün değilse, her ikisi de .NET Framework EF Core 2,1 veya EF Core 2,2 ' i kullanmaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-246">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="5dc4c-247">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="5dc4c-247">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="5dc4c-248">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="5dc4c-248">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="5dc4c-249">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-249">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-250">ASP.NET Core 3,0 öncesinde, `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`bir paket başvurusu eklediğinizde, EF Core ve EF Core sağlayıcı gibi SQL Server veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-250">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="5dc4c-251">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-251">**New behavior**</span></span>

<span data-ttu-id="5dc4c-252">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-252">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="5dc4c-253">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-253">**Why**</span></span>

<span data-ttu-id="5dc4c-254">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-254">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="5dc4c-255">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-255">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="5dc4c-256">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-256">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="5dc4c-257">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-257">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="5dc4c-258">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-258">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-259">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-259">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="5dc4c-260">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="5dc4c-260">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="5dc4c-261">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="5dc4c-261">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="5dc4c-262">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-262">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-263">3,0 ' dan önce, `dotnet ef` aracı .NET Core SDK eklenmiştir ve ek adımlara gerek duymadan herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-263">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="5dc4c-264">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-264">**New behavior**</span></span>

<span data-ttu-id="5dc4c-265">3,0 ' den başlayarak .NET SDK `dotnet ef` aracını içermez, bu nedenle kullanabilmeniz için önce yerel veya küresel bir araç olarak açıkça kurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-265">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="5dc4c-266">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-266">**Why**</span></span>

<span data-ttu-id="5dc4c-267">Bu değişiklik, NuGet üzerinde düzenli bir .NET CLı aracı olarak `dotnet ef` dağıtmamızı ve güncelleştirmenizi sağlar ve bu da EF Core 3,0 her zaman bir NuGet paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-267">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="5dc4c-268">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-268">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-269">`DbContext`geçişleri veya yapı iskelesi yönetmek için `dotnet-ef` genel bir araç olarak yükler:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-269">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="5dc4c-270">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-270">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="5dc4c-271">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-271">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="5dc4c-272">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="5dc4c-272">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="5dc4c-273">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-273">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-274">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-274">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="5dc4c-275">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-275">**New behavior**</span></span>

<span data-ttu-id="5dc4c-276">EF Core 3,0 ' den başlayarak, parametrelerin sorgu dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlRaw`, `ExecuteSqlRaw`ve `ExecuteSqlRawAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-276">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="5dc4c-277">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-277">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="5dc4c-278">Parametrelerin, enterpolasyonlu bir sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlInterpolated`, `ExecuteSqlInterpolated`ve `ExecuteSqlInterpolatedAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-278">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="5dc4c-279">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-279">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="5dc4c-280">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-280">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="5dc4c-281">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-281">**Why**</span></span>

<span data-ttu-id="5dc4c-282">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-282">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="5dc4c-283">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-283">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="5dc4c-284">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-284">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-285">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-285">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="5dc4c-286">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="5dc4c-286">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="5dc4c-287">Sorun izleniyor #15392</span><span class="sxs-lookup"><span data-stu-id="5dc4c-287">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="5dc4c-288">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-288">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-289">3,0 EF Core önce, FromSql yöntemi, geçirilen SQL 'in üzerine oluşmayabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-289">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="5dc4c-290">SQL, saklı yordam gibi birleştirilmemiş bir işlem olduğu zaman istemci değerlendirmesi gerçekleştirdi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-290">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="5dc4c-291">Aşağıdaki sorgu, sunucuda saklı yordam çalıştırılarak ve istemci tarafında FirstOrDefault ' i gerçekleştirerek çalıştı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-291">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="5dc4c-292">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-292">**New behavior**</span></span>

<span data-ttu-id="5dc4c-293">EF Core 3,0 ' den başlayarak, EF Core SQL ayrıştırmaya çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-293">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="5dc4c-294">Bu nedenle, FromSqlRaw/Fromsqlenterpolasyonından sonra oluşturuyorsanız, EF Core alt sorguya neden olarak SQL 'i oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-294">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="5dc4c-295">Bu nedenle, bileşim ile saklı yordam kullanıyorsanız, geçersiz SQL söz dizimi için bir özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-295">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="5dc4c-296">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-296">**Why**</span></span>

<span data-ttu-id="5dc4c-297">EF Core 3,0, [burada](#linq-queries-are-no-longer-evaluated-on-the-client)açıklandığı gibi bir hata olduğundan, otomatik istemci değerlendirmesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-297">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="5dc4c-298">**Mayı**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-298">**Mitigation**</span></span>

<span data-ttu-id="5dc4c-299">FromSqlRaw/Fromsqlenterpolasyonnda saklı bir yordam kullanıyorsanız, bunun üzerine oluşmadığını bilirsiniz; böylece sunucu tarafında herhangi bir kompozisyonu önlemek için FromSql yöntem çağrısından sonra __asslanabilir/AsAsyncEnumerable__ ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-299">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="5dc4c-300">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-300">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="5dc4c-301">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="5dc4c-301">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="5dc4c-302">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-302">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-303">3,0 EF Core önce, `FromSql` yöntemi sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-303">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="5dc4c-304">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-304">**New behavior**</span></span>

<span data-ttu-id="5dc4c-305">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (`FromSql`değiştirme) yalnızca sorgu köklerinin üzerinde belirtilebilir, yani doğrudan `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-305">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="5dc4c-306">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-306">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="5dc4c-307">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-307">**Why**</span></span>

<span data-ttu-id="5dc4c-308">`DbSet` üzerinde `FromSql` herhangi bir yerde, hiçbir anlamı veya eklenen değeri belirtmediyse, bazı senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-308">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="5dc4c-309">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-309">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-310">`FromSql` etkinleştirmeleri, uygulandıkları `DbSet` doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-310">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="5dc4c-311">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="5dc4c-311">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="5dc4c-312">Sorun izleniyor #13518</span><span class="sxs-lookup"><span data-stu-id="5dc4c-312">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="5dc4c-313">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-313">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-314">EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-314">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="5dc4c-315">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-315">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="5dc4c-316">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-316">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="5dc4c-317">, verilen kategoriyle ilişkilendirilen her bir `Product` için aynı `Category` örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-317">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="5dc4c-318">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-318">**New behavior**</span></span>

<span data-ttu-id="5dc4c-319">EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-319">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="5dc4c-320">Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde bile her bir `Product` için yeni bir `Category` örneği döndürecek.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-320">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="5dc4c-321">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-321">**Why**</span></span>

<span data-ttu-id="5dc4c-322">Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-322">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="5dc4c-323">Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-323">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="5dc4c-324">Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-324">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="5dc4c-325">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-325">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-326">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-326">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="5dc4c-327">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-327">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="5dc4c-328">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="5dc4c-328">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="5dc4c-329">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-329">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="5dc4c-330">Örneğin, SQL 'in günlüğe kaydedilmesini `Debug`değiştirmek için `OnConfiguring` veya `AddDbContext`düzeyini açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-330">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="5dc4c-331">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="5dc4c-331">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="5dc4c-332">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="5dc4c-332">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="5dc4c-333">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-333">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-334">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-334">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="5dc4c-335">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-335">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="5dc4c-336">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-336">**New behavior**</span></span>

<span data-ttu-id="5dc4c-337">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-337">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="5dc4c-338">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-338">**Why**</span></span>

<span data-ttu-id="5dc4c-339">Daha önce bazı `DbContext` örnekleri tarafından izlenen bir varlık farklı bir `DbContext` örneğine taşındığında, geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-339">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="5dc4c-340">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-340">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-341">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlara birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup `Added` durumundaki varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-341">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="5dc4c-342">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-342">This can be avoided by:</span></span>
* <span data-ttu-id="5dc4c-343">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-343">Not using store-generated keys.</span></span>
* <span data-ttu-id="5dc4c-344">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-344">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="5dc4c-345">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-345">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="5dc4c-346">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue`, `blog.Id` kendisi ayarlanmamışsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-346">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="5dc4c-347">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="5dc4c-347">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="5dc4c-348">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="5dc4c-348">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="5dc4c-349">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-349">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-350">EF Core 3,0 ' dan önce, `DetectChanges` tarafından bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve `SaveChanges` çağrıldığında yeni bir satır olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-350">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="5dc4c-351">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-351">**New behavior**</span></span>

<span data-ttu-id="5dc4c-352">EF Core 3,0 ' den başlayarak, bir varlık üretilen anahtar değerlerini kullanıyorsa ve bazı anahtar değer ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-352">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="5dc4c-353">Bu, varlık için bir satırın var olduğu varsayılır ve `SaveChanges` çağrıldığında güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-353">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="5dc4c-354">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-354">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="5dc4c-355">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-355">**Why**</span></span>

<span data-ttu-id="5dc4c-356">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-356">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="5dc4c-357">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-357">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-358">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-358">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="5dc4c-359">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-359">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="5dc4c-360">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-360">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="5dc4c-361">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-361">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="5dc4c-362">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-362">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="5dc4c-363">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="5dc4c-363">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="5dc4c-364">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-364">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-365">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-365">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="5dc4c-366">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-366">**New behavior**</span></span>

<span data-ttu-id="5dc4c-367">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-367">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="5dc4c-368">Örneğin, bir sorumlu varlığı silmek için `context.Remove()` çağırmak, izlenen ilgili tüm bağımlı bağımlıların de hemen `Deleted` olarak ayarlanabilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-368">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="5dc4c-369">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-369">**Why**</span></span>

<span data-ttu-id="5dc4c-370">Bu değişiklik, `SaveChanges` çağrılmadan _önce_ hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-370">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="5dc4c-371">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-371">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-372">Önceki davranış `context.ChangedTracker`ayarları aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-372">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="5dc4c-373">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-373">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="5dc4c-374">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-374">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="5dc4c-375">Sorun izleniyor #18022</span><span class="sxs-lookup"><span data-stu-id="5dc4c-375">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="5dc4c-376">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-376">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-377">3,0 öncesinde, `Include` işleçleri aracılığıyla koleksiyon gezginlerini yüklemek, her ilgili varlık türü için bir tane olmak üzere ilişkisel veritabanında birden çok sorgunun oluşturulmasına neden oldu.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-377">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="5dc4c-378">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-378">**New behavior**</span></span>

<span data-ttu-id="5dc4c-379">3,0 ile başlayarak, EF Core ilişkisel veritabanlarında birleştirmelere sahip tek bir sorgu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-379">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="5dc4c-380">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-380">**Why**</span></span>

<span data-ttu-id="5dc4c-381">Tek bir LINQ sorgusu uygulamak için birden çok sorgu verilmesi, birden çok veritabanı yuvarlaklığına gerek olduğu için negatif performans dahil olmak üzere çok sayıda soruna neden olmuş ve her sorgu veritabanının farklı bir durumunu gözlemleyebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-381">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="5dc4c-382">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-382">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-383">Teknik olarak bu bir değişiklik olmadığı sürece, tek bir sorgu koleksiyon gezginlerinin çok sayıda `Include` işleci içerdiğinde uygulama performansının önemli bir etkisi olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-383">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="5dc4c-384">Daha fazla bilgi edinmek ve sorguları daha verimli bir şekilde yeniden yazmak için [Bu açıklamaya bakın](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="5dc4c-384">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="5dc4c-385">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="5dc4c-385">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="5dc4c-386">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="5dc4c-386">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="5dc4c-387">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-387">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-388">3,0 ' den önce, `Restrict` anlamlarında `DeleteBehavior.Restrict` yabancı anahtarlar oluşturdunuz, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-388">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="5dc4c-389">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-389">**New behavior**</span></span>

<span data-ttu-id="5dc4c-390">3,0 ile başlayarak, `DeleteBehavior.Restrict` yabancı anahtarların `Restrict` semantiğinin oluşturulmasını sağlar; Yani bu, basamaksız; sınır iç düzeltmesini etkilemeden kısıtlama ihlalinde throw.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-390">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="5dc4c-391">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-391">**Why**</span></span>

<span data-ttu-id="5dc4c-392">Bu değişiklik, beklenmeyen yan etkileri olmadan `DeleteBehavior` kullanımı kolay bir şekilde geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-392">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="5dc4c-393">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-393">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-394">Önceki davranış `DeleteBehavior.ClientNoAction`kullanılarak geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-394">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="5dc4c-395">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-395">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="5dc4c-396">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="5dc4c-396">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="5dc4c-397">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-397">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-398">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/keyless-entity-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-398">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="5dc4c-399">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="5dc4c-399">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="5dc4c-400">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-400">**New behavior**</span></span>

<span data-ttu-id="5dc4c-401">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-401">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="5dc4c-402">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-402">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="5dc4c-403">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-403">**Why**</span></span>

<span data-ttu-id="5dc4c-404">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-404">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="5dc4c-405">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-405">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="5dc4c-406">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-406">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="5dc4c-407">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-407">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-408">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-408">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="5dc4c-409">**`ModelBuilder.Query<>()`** -bir varlık türünü anahtar olmadan işaretlemek için `ModelBuilder.Entity<>().HasNoKey()` çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-409">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="5dc4c-410">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-410">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="5dc4c-411">**`DbQuery<>`** bunun yerine `DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-411">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="5dc4c-412">**`DbContext.Query<>()`** bunun yerine `DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-412">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="5dc4c-413">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="5dc4c-413">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="5dc4c-414">Sorun izleme [#12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444) sorunu
izleme sorunu [#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[izleme sorunu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="5dc4c-414">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="5dc4c-415">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-415">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-416">3,0 EF Core önce, sahip olan ilişkinin yapılandırması `OwnsOne` veya `OwnsMany` çağrısından sonra doğrudan gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-416">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="5dc4c-417">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-417">**New behavior**</span></span>

<span data-ttu-id="5dc4c-418">EF Core 3,0 ' den başlayarak, artık `WithOwner()`kullanarak sahip için bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-418">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="5dc4c-419">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-419">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="5dc4c-420">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin yapılandırılmasına benzer şekilde `WithOwner()` sonra zincirde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-420">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="5dc4c-421">Sahip olduğu için yapılandırma, `OwnsOne()/OwnsMany()`sonra hala zincirleme olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-421">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="5dc4c-422">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-422">For example:</span></span>

```csharp
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

<span data-ttu-id="5dc4c-423">Ayrıca, sahip bir tür hedefi ile `Entity()`, `HasOne()`veya `Set()` çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-423">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="5dc4c-424">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-424">**Why**</span></span>

<span data-ttu-id="5dc4c-425">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-425">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="5dc4c-426">Bu, sırasıyla `HasForeignKey`gibi yöntemlere ve karışıklığı ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-426">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="5dc4c-427">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-427">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-428">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-428">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="5dc4c-429">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-429">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="5dc4c-430">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="5dc4c-430">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="5dc4c-431">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-431">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-432">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-432">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="5dc4c-433">EF Core 3,0 ' dan önce, `OrderDetails` aynı tabloyla `Order` veya açıkça eşlenmişse, yeni bir `Order`eklenirken `OrderDetails` örneği her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-433">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="5dc4c-434">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-434">**New behavior**</span></span>

<span data-ttu-id="5dc4c-435">3,0 ile başlayarak EF Core, `OrderDetails` olmayan bir `Order` eklemesine ve birincil anahtar null yapılabilir sütunlara hariç tüm `OrderDetails` özelliklerini eşleştirme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-435">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="5dc4c-436">EF Core kümelerini sorgularken, gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar ve tüm `null`Özellikler ' in yanı sıra gerekli özellikleri yoksa, `null` `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-436">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="5dc4c-437">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-437">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-438">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinmenin `null` olması beklenmiyorsa, gezinti `null`olduğunda, uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-438">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="5dc4c-439">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış`null` bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-439">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="5dc4c-440">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-440">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="5dc4c-441">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="5dc4c-441">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="5dc4c-442">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-442">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-443">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-443">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="5dc4c-444">EF Core 3,0 ' dan önce, `OrderDetails` aynı tabloyla `Order` ya da açıkça eşlenmişse, yalnızca `OrderDetails` güncelleştirilmesi istemci üzerindeki `Version` değerini güncelleştirmez ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-444">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="5dc4c-445">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-445">**New behavior**</span></span>

<span data-ttu-id="5dc4c-446">3,0 ' den itibaren, yeni `Version` değerini `OrderDetails`sahip `Order` EF Core yayar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-446">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="5dc4c-447">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-447">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="5dc4c-448">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-448">**Why**</span></span>

<span data-ttu-id="5dc4c-449">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-449">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="5dc4c-450">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-450">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-451">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-451">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="5dc4c-452">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-452">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="5dc4c-453">Sahip olunan varlıklar, izleme sorgusu kullanılarak sahip olmadan sorgulanamıyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-453">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="5dc4c-454">Sorun izleniyor #18876</span><span class="sxs-lookup"><span data-stu-id="5dc4c-454">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="5dc4c-455">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-455">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-456">EF Core 3,0 ' dan önce, sahip olan varlıklar başka bir gezinti olarak sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-456">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="5dc4c-457">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-457">**New behavior**</span></span>

<span data-ttu-id="5dc4c-458">3,0 ile başlayarak, bir izleme sorgusu sahip olmayan bir varlık projesinde proje EF Core oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-458">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="5dc4c-459">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-459">**Why**</span></span>

<span data-ttu-id="5dc4c-460">Sahip olunan varlıklar sahip olmadan değiştirilemez, bu nedenle bu şekilde sorgulama durumlarının büyük çoğunluğunda bir hata olur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-460">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="5dc4c-461">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-461">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-462">Sahip olduğu varlık daha sonra değiştirilecek şekilde izleniyorsa, sahibin sorguya eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-462">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="5dc4c-463">Aksi takdirde `AsNoTracking()` çağrısı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-463">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="5dc4c-464">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-464">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="5dc4c-465">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="5dc4c-465">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="5dc4c-466">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-466">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-467">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-467">Consider the following model:</span></span>
```csharp
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

<span data-ttu-id="5dc4c-468">EF Core 3,0 ' dan önce, `ShippingAddress` özelliği varsayılan olarak `BulkOrder` ve `Order` ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-468">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="5dc4c-469">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-469">**New behavior**</span></span>

<span data-ttu-id="5dc4c-470">3,0 ile başlayarak, EF Core `ShippingAddress`için yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-470">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="5dc4c-471">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-471">**Why**</span></span>

<span data-ttu-id="5dc4c-472">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-472">The old behavoir was unexpected.</span></span>

<span data-ttu-id="5dc4c-473">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-473">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-474">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-474">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```csharp
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="5dc4c-475">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-475">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="5dc4c-476">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="5dc4c-476">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="5dc4c-477">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-477">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-478">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-478">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="5dc4c-479">EF Core 3,0 ' dan önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-479">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="5dc4c-480">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtar `CustomerId` olur ve bu genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-480">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="5dc4c-481">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-481">**New behavior**</span></span>

<span data-ttu-id="5dc4c-482">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-482">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="5dc4c-483">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-483">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="5dc4c-484">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-484">For example:</span></span>

```csharp
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

```csharp
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

<span data-ttu-id="5dc4c-485">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-485">**Why**</span></span>

<span data-ttu-id="5dc4c-486">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-486">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="5dc4c-487">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-487">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-488">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-488">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="5dc4c-489">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-489">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="5dc4c-490">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="5dc4c-490">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="5dc4c-491">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-491">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-492">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope`içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-492">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```csharp
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

<span data-ttu-id="5dc4c-493">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-493">**New behavior**</span></span>

<span data-ttu-id="5dc4c-494">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-494">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="5dc4c-495">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-495">**Why**</span></span>

<span data-ttu-id="5dc4c-496">Bu değişiklik aynı `TransactionScope`birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-496">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="5dc4c-497">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-497">The new behavior also matches EF6.</span></span>

<span data-ttu-id="5dc4c-498">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-498">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-499">Bağlantının açık açık olarak kalması gerekiyorsa `OpenConnection()`, EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-499">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```csharp
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="5dc4c-500">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-500">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="5dc4c-501">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="5dc4c-501">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="5dc4c-502">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-502">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-503">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-503">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="5dc4c-504">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-504">**New behavior**</span></span>

<span data-ttu-id="5dc4c-505">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-505">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="5dc4c-506">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-506">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="5dc4c-507">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-507">**Why**</span></span>

<span data-ttu-id="5dc4c-508">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-508">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="5dc4c-509">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-509">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-510">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-510">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="5dc4c-511">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-511">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="5dc4c-512">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-512">Backing fields are used by default</span></span>

[<span data-ttu-id="5dc4c-513">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="5dc4c-513">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="5dc4c-514">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-514">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-515">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-515">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="5dc4c-516">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-516">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="5dc4c-517">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-517">**New behavior**</span></span>

<span data-ttu-id="5dc4c-518">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-518">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="5dc4c-519">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-519">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="5dc4c-520">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-520">**Why**</span></span>

<span data-ttu-id="5dc4c-521">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-521">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="5dc4c-522">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-522">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-523">3,0 öncesi davranış `ModelBuilder`özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-523">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="5dc4c-524">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-524">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="5dc4c-525">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="5dc4c-525">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="5dc4c-526">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="5dc4c-526">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="5dc4c-527">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-527">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-528">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-528">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="5dc4c-529">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-529">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="5dc4c-530">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-530">**New behavior**</span></span>

<span data-ttu-id="5dc4c-531">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-531">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="5dc4c-532">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-532">**Why**</span></span>

<span data-ttu-id="5dc4c-533">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-533">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="5dc4c-534">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-534">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-535">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-535">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="5dc4c-536">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-536">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="5dc4c-537">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-537">Field-only property names should match the field name</span></span>

<span data-ttu-id="5dc4c-538">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-538">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-539">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-539">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="5dc4c-540">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-540">**New behavior**</span></span>

<span data-ttu-id="5dc4c-541">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-541">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="5dc4c-542">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-542">**Why**</span></span>

<span data-ttu-id="5dc4c-543">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-543">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="5dc4c-544">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-544">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-545">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-545">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="5dc4c-546">3,0 sonrasında EF Core gelecek bir sürümünde, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="5dc4c-546">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="5dc4c-547">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-547">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="5dc4c-548">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="5dc4c-548">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="5dc4c-549">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-549">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-550">EF Core 3,0 ' dan önce, `AddDbContext` veya `AddDbContextPool` çağırma işlemi, [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrılarına çağrı ile günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-550">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="5dc4c-551">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-551">**New behavior**</span></span>

<span data-ttu-id="5dc4c-552">EF Core 3,0 ' den başlayarak `AddDbContext` ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-552">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="5dc4c-553">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-553">**Why**</span></span>

<span data-ttu-id="5dc4c-554">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-554">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="5dc4c-555">Ancak, `ILoggerFactory` uygulamanın dı kapsayıcısına kayıtlıysa, EF Core tarafından hala kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-555">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="5dc4c-556">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-556">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-557">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-557">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="5dc4c-558">AddEntityFramework \* bir boyut sınırı ile ımemorycache ekler</span><span class="sxs-lookup"><span data-stu-id="5dc4c-558">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="5dc4c-559">Sorun izleniyor #12905</span><span class="sxs-lookup"><span data-stu-id="5dc4c-559">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="5dc4c-560">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-560">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-561">EF Core 3,0 ' dan önce, `AddEntityFramework*` yöntemlerin çağrılması, bellek önbelleğe alma hizmetlerini bir boyut sınırı olmadan DI ile de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-561">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="5dc4c-562">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-562">**New behavior**</span></span>

<span data-ttu-id="5dc4c-563">EF Core 3,0 ' den başlayarak `AddEntityFramework*` bir ımemorycache hizmetini boyut sınırı ile kaydeder.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-563">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="5dc4c-564">Daha sonra eklenen diğer hizmetler ımemorycache 'e bağımlıysa, özel durumlara veya performans düşüklüğe neden olan varsayılan sınıra hızlıca ulaşabilirler.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-564">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="5dc4c-565">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-565">**Why**</span></span>

<span data-ttu-id="5dc4c-566">Sorgu önbelleğe alma mantığındaki bir hata varsa veya sorgular dinamik olarak oluşturulduysa, ımemorycache 'i sınır olmadan kullanmak denetlenmeyen bellek kullanımına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-566">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="5dc4c-567">Varsayılan sınırın olması olası bir DoS saldırısının etkisini azaltır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-567">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="5dc4c-568">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-568">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-569">Çoğu durumda `AddEntityFramework*` çağrısı `AddDbContext` veya `AddDbContextPool` da çağrılırsa gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-569">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="5dc4c-570">Bu nedenle, en iyi risk azaltma `AddEntityFramework*` çağrısını kaldırmayacak.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-570">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="5dc4c-571">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, daha önce [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)' i kullanarak bir ıMEMORYCACHE uygulamasını dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-571">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="5dc4c-572">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-572">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="5dc4c-573">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="5dc4c-573">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="5dc4c-574">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-574">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-575">EF Core 3,0 önce, `DbContext.Entry` çağırma, izlenen tüm varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-575">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="5dc4c-576">Bu, `EntityEntry` açığa çıkarılan durumun güncel olduğunu ortaya çıkardık.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-576">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="5dc4c-577">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-577">**New behavior**</span></span>

<span data-ttu-id="5dc4c-578">EF Core 3,0 ' den başlayarak, çağırma `DbContext.Entry` artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili tüm izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-578">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="5dc4c-579">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-579">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="5dc4c-580">`ChangeTracker.AutoDetectChangesEnabled` `false` olarak ayarlanırsa, bu yerel değişikliğin algılanması de devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-580">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="5dc4c-581">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` ve `SaveChanges`--yine de izlenen tüm varlıkların tam `DetectChanges` neden olur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-581">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="5dc4c-582">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-582">**Why**</span></span>

<span data-ttu-id="5dc4c-583">Bu değişiklik, `context.Entry`kullanmanın varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-583">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="5dc4c-584">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-584">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-585">3,0 öncesi davranışı sağlamak için `Entry` çağrılmadan önce `ChgangeTracker.DetectChanges()` çağırın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-585">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="5dc4c-586">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="5dc4c-586">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="5dc4c-587">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="5dc4c-587">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="5dc4c-588">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-588">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-589">EF Core 3,0 önce `string` ve `byte[]` anahtar özellikleri açık bir şekilde null olmayan bir değer ayarlamadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-589">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="5dc4c-590">Böyle bir durumda, anahtar değeri istemci üzerinde `byte[]`için bayt olarak serileştirilmiş bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-590">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="5dc4c-591">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-591">**New behavior**</span></span>

<span data-ttu-id="5dc4c-592">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-592">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="5dc4c-593">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-593">**Why**</span></span>

<span data-ttu-id="5dc4c-594">Bu değişiklik, istemci tarafından oluşturulan `string`/`byte[]` değerleri genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-594">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="5dc4c-595">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-595">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-596">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-596">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="5dc4c-597">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-597">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="5dc4c-598">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-598">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="5dc4c-599">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-599">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="5dc4c-600">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="5dc4c-600">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="5dc4c-601">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-601">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-602">EF Core 3,0 tarihinden önce `ILoggerFactory` bir tek hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-602">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="5dc4c-603">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-603">**New behavior**</span></span>

<span data-ttu-id="5dc4c-604">EF Core 3,0 ' den başlayarak `ILoggerFactory` artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-604">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="5dc4c-605">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-605">**Why**</span></span>

<span data-ttu-id="5dc4c-606">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi bazı bazı durumları kaldıran `DbContext` örneğiyle bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-606">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="5dc4c-607">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-607">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-608">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-608">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="5dc4c-609">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-609">This isn't common.</span></span>
<span data-ttu-id="5dc4c-610">Bu durumlarda, çoğu şey çalışmaya devam edecektir, ancak `ILoggerFactory` bağlı olan herhangi bir singleton hizmeti, `ILoggerFactory` farklı bir şekilde almak için değiştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-610">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="5dc4c-611">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) ' nde bir sorun `ILoggerFactory` bildirin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-611">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="5dc4c-612">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="5dc4c-612">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="5dc4c-613">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="5dc4c-613">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="5dc4c-614">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-614">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-615">EF Core 3,0 ' dan önce, bir `DbContext` atıldıktan sonra, söz konusu bağlamdan alınan bir varlık üzerinde verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-615">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="5dc4c-616">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-616">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="5dc4c-617">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-617">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="5dc4c-618">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-618">**New behavior**</span></span>

<span data-ttu-id="5dc4c-619">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-619">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="5dc4c-620">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-620">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="5dc4c-621">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-621">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="5dc4c-622">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-622">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="5dc4c-623">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-623">**Why**</span></span>

<span data-ttu-id="5dc4c-624">Bu değişiklik, atılmış bir `DbContext` örneğine geç yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-624">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="5dc4c-625">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-625">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-626">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-626">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="5dc4c-627">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-627">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="5dc4c-628">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="5dc4c-628">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="5dc4c-629">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-629">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-630">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-630">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="5dc4c-631">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-631">**New behavior**</span></span>

<span data-ttu-id="5dc4c-632">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-632">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="5dc4c-633">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-633">**Why**</span></span>

<span data-ttu-id="5dc4c-634">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="5dc4c-634">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="5dc4c-635">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-635">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-636">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-636">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="5dc4c-637">Ancak, hata `DbContextOptionsBuilder`yapılandırması aracılığıyla bir uyarıya (veya yoksayıldı) geri dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-637">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="5dc4c-638">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-638">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="5dc4c-639">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="5dc4c-639">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="5dc4c-640">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="5dc4c-640">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="5dc4c-641">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-641">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-642">EF Core 3,0 öncesinde, tek bir dize ile `HasOne` veya `HasMany` kodu, kafa karıştırıcı bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-642">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="5dc4c-643">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-643">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="5dc4c-644">Kod, özel olabilecek `Entrance` gezinti özelliğini kullanarak `Samurai` başka bir varlık türüyle ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-644">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="5dc4c-645">Gerçekte, bu kod, gezinti özelliği olmayan `Entrance` adlı bazı varlık türlerine bir ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-645">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="5dc4c-646">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-646">**New behavior**</span></span>

<span data-ttu-id="5dc4c-647">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-647">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="5dc4c-648">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-648">**Why**</span></span>

<span data-ttu-id="5dc4c-649">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-649">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="5dc4c-650">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-650">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-651">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-651">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="5dc4c-652">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-652">This is not common.</span></span>
<span data-ttu-id="5dc4c-653">Önceki davranış, gezinti özelliği adı için `null` açıkça geçirerek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-653">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="5dc4c-654">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-654">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="5dc4c-655">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-655">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="5dc4c-656">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="5dc4c-656">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="5dc4c-657">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-657">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-658">Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürdü:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-658">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="5dc4c-659">`ValueGenerator.NextValueAsync()` (ve türetme sınıfları)</span><span class="sxs-lookup"><span data-stu-id="5dc4c-659">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="5dc4c-660">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-660">**New behavior**</span></span>

<span data-ttu-id="5dc4c-661">Yukarıda bahsedilen yöntemler bundan sonra aynı `T` daha `ValueTask<T>` döndürür.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-661">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="5dc4c-662">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-662">**Why**</span></span>

<span data-ttu-id="5dc4c-663">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-663">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="5dc4c-664">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-664">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-665">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-665">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="5dc4c-666">Daha karmaşık bir kullanım (örn. döndürülen `Task` `Task.WhenAny()`geçirilmesi), genellikle döndürülen `ValueTask<T>` `AsTask()` çağırarak `Task<T>` dönüştürmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-666">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="5dc4c-667">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-667">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="5dc4c-668">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="5dc4c-668">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="5dc4c-669">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="5dc4c-669">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="5dc4c-670">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-670">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-671">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-671">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="5dc4c-672">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-672">**New behavior**</span></span>

<span data-ttu-id="5dc4c-673">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-673">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="5dc4c-674">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-674">**Why**</span></span>

<span data-ttu-id="5dc4c-675">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-675">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="5dc4c-676">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-676">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-677">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-677">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="5dc4c-678">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-678">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="5dc4c-679">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="5dc4c-679">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="5dc4c-680">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="5dc4c-680">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="5dc4c-681">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-681">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-682">EF Core 3,0 ' dan önce, yalnızca devralma eşleme stratejisi bu geçerli olmayan bir TPH olduğundan, türetilmiş bir tür üzerinde çağrılan `ToTable()` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-682">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="5dc4c-683">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-683">**New behavior**</span></span>

<span data-ttu-id="5dc4c-684">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, bir türetilmiş tür üzerinde çağrılan `ToTable()` artık gelecekte beklenmedik bir eşleme değişikliğini önlemek için bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-684">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="5dc4c-685">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-685">**Why**</span></span>

<span data-ttu-id="5dc4c-686">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-686">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="5dc4c-687">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-687">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="5dc4c-688">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-688">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-689">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-689">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="5dc4c-690">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="5dc4c-690">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="5dc4c-691">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="5dc4c-691">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="5dc4c-692">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-692">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-693">EF Core 3,0 ' dan önce `ForSqlServerHasIndex().ForSqlServerInclude()` `INCLUDE`ile kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-693">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="5dc4c-694">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-694">**New behavior**</span></span>

<span data-ttu-id="5dc4c-695">EF Core 3,0 ' den itibaren, bir dizinde `Include` kullanmak artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-695">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="5dc4c-696">`HasIndex().ForSqlServerInclude()`kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-696">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="5dc4c-697">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-697">**Why**</span></span>

<span data-ttu-id="5dc4c-698">Bu değişiklik, tüm veritabanı sağlayıcıları için `Include` olan dizinler için API 'yi tek bir yerde birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-698">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="5dc4c-699">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-699">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-700">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-700">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="5dc4c-701">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="5dc4c-701">Metadata API changes</span></span>

[<span data-ttu-id="5dc4c-702">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="5dc4c-702">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="5dc4c-703">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-703">**New behavior**</span></span>

<span data-ttu-id="5dc4c-704">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-704">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="5dc4c-705">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-705">**Why**</span></span>

<span data-ttu-id="5dc4c-706">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-706">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="5dc4c-707">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-707">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-708">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-708">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="5dc4c-709">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="5dc4c-709">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="5dc4c-710">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="5dc4c-710">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="5dc4c-711">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-711">**New behavior**</span></span>

<span data-ttu-id="5dc4c-712">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-712">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="5dc4c-713">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-713">**Why**</span></span>

<span data-ttu-id="5dc4c-714">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-714">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="5dc4c-715">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-715">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-716">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-716">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="5dc4c-717">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="5dc4c-717">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="5dc4c-718">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="5dc4c-718">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="5dc4c-719">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-719">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-720">3,0 EF Core önce, bir SQLite bağlantısı açıldığında EF Core `PRAGMA foreign_keys = 1` gönderebilirim.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-720">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5dc4c-721">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-721">**New behavior**</span></span>

<span data-ttu-id="5dc4c-722">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı açıldığında EF Core artık `PRAGMA foreign_keys = 1` göndermez.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-722">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5dc4c-723">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-723">**Why**</span></span>

<span data-ttu-id="5dc4c-724">Bu değişiklik, EF Core varsayılan olarak `SQLitePCLRaw.bundle_e_sqlite3` kullandığından, bu, sırasıyla FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-724">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="5dc4c-725">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-725">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-726">EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 öğesinde yabancı anahtarlar varsayılan olarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-726">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="5dc4c-727">Diğer durumlarda, yabancı anahtarlar bağlantı dizeniz `Foreign Keys=True` belirtilerek etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-727">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="5dc4c-728">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-728">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="5dc4c-729">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-729">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-730">3,0 EF Core önce EF Core `SQLitePCLRaw.bundle_green`kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-730">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="5dc4c-731">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-731">**New behavior**</span></span>

<span data-ttu-id="5dc4c-732">EF Core 3,0 ' den başlayarak EF Core `SQLitePCLRaw.bundle_e_sqlite3`kullanır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-732">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="5dc4c-733">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-733">**Why**</span></span>

<span data-ttu-id="5dc4c-734">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-734">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="5dc4c-735">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-735">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-736">İOS 'ta yerel SQLite sürümünü kullanmak için `Microsoft.Data.Sqlite` farklı bir `SQLitePCLRaw` paketi kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-736">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5dc4c-737">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-737">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5dc4c-738">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="5dc4c-738">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="5dc4c-739">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-739">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-740">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-740">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="5dc4c-741">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-741">**New behavior**</span></span>

<span data-ttu-id="5dc4c-742">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-742">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="5dc4c-743">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-743">**Why**</span></span>

<span data-ttu-id="5dc4c-744">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-744">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="5dc4c-745">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-745">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="5dc4c-746">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-746">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-747">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-747">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="5dc4c-748">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-748">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="5dc4c-749">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-749">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5dc4c-750">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-750">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5dc4c-751">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="5dc4c-751">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="5dc4c-752">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-752">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-753">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-753">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="5dc4c-754">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-754">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="5dc4c-755">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-755">**New behavior**</span></span>

<span data-ttu-id="5dc4c-756">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-756">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="5dc4c-757">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-757">**Why**</span></span>

<span data-ttu-id="5dc4c-758">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-758">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="5dc4c-759">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-759">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-760">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-760">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="5dc4c-761">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-761">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="5dc4c-762">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-762">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="5dc4c-763">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="5dc4c-763">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="5dc4c-764">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="5dc4c-764">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="5dc4c-765">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-765">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-766">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-766">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="5dc4c-767">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-767">**New behavior**</span></span>

<span data-ttu-id="5dc4c-768">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-768">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="5dc4c-769">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-769">**Why**</span></span>

<span data-ttu-id="5dc4c-770">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-770">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="5dc4c-771">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-771">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="5dc4c-772">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-772">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-773">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="5dc4c-773">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="5dc4c-774">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-774">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="5dc4c-775">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-775">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="5dc4c-776">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-776">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="5dc4c-777">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-777">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="5dc4c-778">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="5dc4c-778">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="5dc4c-779">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-779">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-780">EF Core 3,0 ' dan önce `UseRowNumberForPaging`, SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-780">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="5dc4c-781">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-781">**New behavior**</span></span>

<span data-ttu-id="5dc4c-782">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-782">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="5dc4c-783">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-783">**Why**</span></span>

<span data-ttu-id="5dc4c-784">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-784">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="5dc4c-785">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-785">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-786">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-786">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="5dc4c-787">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-787">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="5dc4c-788">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-788">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="5dc4c-789">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-789">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="5dc4c-790">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="5dc4c-790">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="5dc4c-791">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-791">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-792">`IDbContextOptionsExtension`, uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-792">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="5dc4c-793">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-793">**New behavior**</span></span>

<span data-ttu-id="5dc4c-794">Bu yöntemler yeni bir `IDbContextOptionsExtension.Info` özelliğinden döndürülen yeni bir `DbContextOptionsExtensionInfo` soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-794">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="5dc4c-795">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-795">**Why**</span></span>

<span data-ttu-id="5dc4c-796">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-796">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="5dc4c-797">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-797">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="5dc4c-798">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-798">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-799">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-799">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="5dc4c-800">EF Core kaynak kodundaki farklı tür uzantılara yönelik `IDbContextOptionsExtension` birçok uygulamada örnek bulunur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-800">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="5dc4c-801">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-801">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="5dc4c-802">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="5dc4c-802">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="5dc4c-803">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-803">**Change**</span></span>

<span data-ttu-id="5dc4c-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`olarak yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="5dc4c-805">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-805">**Why**</span></span>

<span data-ttu-id="5dc4c-806">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-806">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="5dc4c-807">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-807">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-808">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-808">Use the new name.</span></span> <span data-ttu-id="5dc4c-809">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="5dc4c-809">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="5dc4c-810">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="5dc4c-810">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="5dc4c-811">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="5dc4c-811">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="5dc4c-812">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-812">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-813">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-813">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="5dc4c-814">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-814">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="5dc4c-815">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-815">**New behavior**</span></span>

<span data-ttu-id="5dc4c-816">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-816">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="5dc4c-817">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-817">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="5dc4c-818">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-818">**Why**</span></span>

<span data-ttu-id="5dc4c-819">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-819">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="5dc4c-820">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-820">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-821">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-821">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="5dc4c-822">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="5dc4c-822">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="5dc4c-823">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="5dc4c-823">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="5dc4c-824">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-824">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-825">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-825">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="5dc4c-826">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-826">**New behavior**</span></span>

<span data-ttu-id="5dc4c-827">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-827">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="5dc4c-828">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-828">**Why**</span></span>

<span data-ttu-id="5dc4c-829">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-829">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="5dc4c-830">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-830">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="5dc4c-831">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-831">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-832">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-832">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="5dc4c-833">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-833">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="5dc4c-834">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="5dc4c-834">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="5dc4c-835">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-835">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-836">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-836">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="5dc4c-837">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-837">**New behavior**</span></span>

<span data-ttu-id="5dc4c-838">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-838">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="5dc4c-839">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-839">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="5dc4c-840">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-840">**Why**</span></span>

<span data-ttu-id="5dc4c-841">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-841">This package is only intended to be used at design time.</span></span> <span data-ttu-id="5dc4c-842">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-842">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="5dc4c-843">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-843">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="5dc4c-844">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-844">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-845">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-845">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="5dc4c-846">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-846">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="5dc4c-847">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-847">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="5dc4c-848">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="5dc4c-848">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="5dc4c-849">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-849">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-850">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-850">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="5dc4c-851">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-851">**New behavior**</span></span>

<span data-ttu-id="5dc4c-852">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-852">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="5dc4c-853">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-853">**Why**</span></span>

<span data-ttu-id="5dc4c-854">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-854">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="5dc4c-855">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-855">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="5dc4c-856">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-856">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-857">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-857">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="5dc4c-858">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-858">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="5dc4c-859">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="5dc4c-859">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="5dc4c-860">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="5dc4c-860">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="5dc4c-861">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-861">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-862">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-862">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="5dc4c-863">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-863">**New behavior**</span></span>

<span data-ttu-id="5dc4c-864">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-864">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="5dc4c-865">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-865">**Why**</span></span>

<span data-ttu-id="5dc4c-866">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-866">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="5dc4c-867">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-867">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-868">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-868">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="5dc4c-869">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-869">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="5dc4c-870">System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-870">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="5dc4c-871">Sorun izleniyor #15636</span><span class="sxs-lookup"><span data-stu-id="5dc4c-871">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="5dc4c-872">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-872">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-873">Microsoft. EntityFrameworkCore. SqlServer daha önce System. Data. SqlClient 'a bağımlı.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-873">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="5dc4c-874">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-874">**New behavior**</span></span>

<span data-ttu-id="5dc4c-875">Paketimizi Microsoft. Data. SqlClient 'e göre güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-875">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="5dc4c-876">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-876">**Why**</span></span>

<span data-ttu-id="5dc4c-877">Microsoft. Data. SqlClient, SQL Server ileriye dönük tanıtım verileri erişim sürücüsüdür ve System. Data. SqlClient artık geliştirme odağı değildir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-877">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="5dc4c-878">Always Encrypted gibi bazı önemli özellikler yalnızca Microsoft. Data. SqlClient ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-878">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="5dc4c-879">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-879">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-880">Kodunuz System. Data. SqlClient üzerinde doğrudan bir bağımlılık alırsa bunun yerine Microsoft. Data. SqlClient öğesine başvuracak şekilde değiştirmeniz gerekir; iki paket çok yüksek düzeyde API uyumluluğu korudıkça, bu yalnızca basit bir paket ve ad alanı değişikliği olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-880">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="5dc4c-881">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="5dc4c-881">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="5dc4c-882">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="5dc4c-882">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="5dc4c-883">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-883">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-884">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-884">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="5dc4c-885">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-885">For example:</span></span>

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="5dc4c-886">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-886">**New behavior**</span></span>

<span data-ttu-id="5dc4c-887">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-887">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="5dc4c-888">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-888">**Why**</span></span>

<span data-ttu-id="5dc4c-889">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-889">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="5dc4c-890">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-890">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-891">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-891">Use full configuration of the relationship.</span></span> <span data-ttu-id="5dc4c-892">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5dc4c-892">For example:</span></span>

```csharp
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="5dc4c-893">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="5dc4c-893">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="5dc4c-894">Sorun izleniyor #12757</span><span class="sxs-lookup"><span data-stu-id="5dc4c-894">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="5dc4c-895">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-895">**Old behavior**</span></span>

<span data-ttu-id="5dc4c-896">Şema ile boş bir dize olarak yapılandırılmış bir DbFunction, şema olmadan yerleşik işlev olarak değerlendirildi.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-896">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="5dc4c-897">Örneğin, aşağıdaki kod `DatePart` CLR işlevini SqlServer üzerinde `DATEPART` yerleşik işlevine eşleyecek.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-897">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="5dc4c-898">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-898">**New behavior**</span></span>

<span data-ttu-id="5dc4c-899">Tüm DbFunction eşlemeleri Kullanıcı tanımlı işlevlere eşlenildiği kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-899">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="5dc4c-900">Bu nedenle boş dize değeri, işlevi model için varsayılan şemanın içine yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-900">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="5dc4c-901">Şema, Fluent API `modelBuilder.HasDefaultSchema()` aracılığıyla açıkça yapılandırılabilir veya `dbo` Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-901">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="5dc4c-902">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-902">**Why**</span></span>

<span data-ttu-id="5dc4c-903">Daha önceden şemanın boş olması, işlevin yerleşik olduğunu değerlendirmek için bir yoldur, ancak bu mantık yalnızca yerleşik işlevlerin herhangi bir şemaya ait olmadığı SqlServer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-903">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="5dc4c-904">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="5dc4c-904">**Mitigations**</span></span>

<span data-ttu-id="5dc4c-905">DbFunction 'ın çevirisini yerleşik bir işlevle eşlemek için el ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5dc4c-905">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
