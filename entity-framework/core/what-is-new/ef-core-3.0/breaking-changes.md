---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888115"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="2e0c8-102">EF Core 3,0 ' de yer alan son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="2e0c8-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="2e0c8-103">Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken mevcut uygulamaları bozmak için olası bir davranıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="2e0c8-104">Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="2e0c8-105">Özet</span><span class="sxs-lookup"><span data-stu-id="2e0c8-105">Summary</span></span>

| <span data-ttu-id="2e0c8-106">**Yeni değişiklik**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="2e0c8-107">**Etkisi**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="2e0c8-108">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="2e0c8-109">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2e0c8-109">High</span></span>       |
| [<span data-ttu-id="2e0c8-110">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="2e0c8-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="2e0c8-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2e0c8-111">High</span></span>      |
| [<span data-ttu-id="2e0c8-112">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2e0c8-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="2e0c8-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2e0c8-113">High</span></span>      |
| [<span data-ttu-id="2e0c8-114">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="2e0c8-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="2e0c8-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2e0c8-115">High</span></span>      |
| [<span data-ttu-id="2e0c8-116">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="2e0c8-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2e0c8-117">High</span></span>      |
| [<span data-ttu-id="2e0c8-118">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="2e0c8-119">Yüksek</span><span class="sxs-lookup"><span data-stu-id="2e0c8-119">High</span></span>      |
| [<span data-ttu-id="2e0c8-120">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2e0c8-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="2e0c8-121">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-121">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-122">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="2e0c8-123">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-123">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-124">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="2e0c8-125">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-125">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-126">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="2e0c8-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="2e0c8-127">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-127">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-128">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="2e0c8-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="2e0c8-129">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-129">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-130">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="2e0c8-131">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-131">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-132">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="2e0c8-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="2e0c8-133">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-133">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-134">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2e0c8-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="2e0c8-135">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-135">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-136">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2e0c8-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="2e0c8-137">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-137">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-138">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="2e0c8-139">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-139">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-140">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="2e0c8-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="2e0c8-141">Orta</span><span class="sxs-lookup"><span data-stu-id="2e0c8-141">Medium</span></span>      |
| [<span data-ttu-id="2e0c8-142">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="2e0c8-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-143">Low</span></span>      |
| [<span data-ttu-id="2e0c8-144">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="2e0c8-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-145">Low</span></span>      |
| [<span data-ttu-id="2e0c8-146">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="2e0c8-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="2e0c8-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-147">Low</span></span>      |
| [<span data-ttu-id="2e0c8-148">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="2e0c8-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-149">Low</span></span>      |
| [<span data-ttu-id="2e0c8-150">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="2e0c8-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-151">Low</span></span>      |
| [<span data-ttu-id="2e0c8-152">Sahip olunan varlıklar, izleme sorgusu kullanılarak sahip olmadan sorgulanamıyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="2e0c8-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-153">Low</span></span>      |
| [<span data-ttu-id="2e0c8-154">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="2e0c8-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-155">Low</span></span>      |
| [<span data-ttu-id="2e0c8-156">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="2e0c8-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-157">Low</span></span>      |
| [<span data-ttu-id="2e0c8-158">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="2e0c8-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-159">Low</span></span>      |
| [<span data-ttu-id="2e0c8-160">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="2e0c8-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-161">Low</span></span>      |
| [<span data-ttu-id="2e0c8-162">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="2e0c8-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="2e0c8-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-163">Low</span></span>      |
| [<span data-ttu-id="2e0c8-164">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="2e0c8-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-165">Low</span></span>      |
| [<span data-ttu-id="2e0c8-166">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="2e0c8-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-167">Low</span></span>      |
| [<span data-ttu-id="2e0c8-168">AddEntityFramework \* bir boyut sınırı ile ımemorycache ekler</span><span class="sxs-lookup"><span data-stu-id="2e0c8-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="2e0c8-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-169">Low</span></span>      |
| [<span data-ttu-id="2e0c8-170">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="2e0c8-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-171">Low</span></span>      |
| [<span data-ttu-id="2e0c8-172">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="2e0c8-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="2e0c8-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-173">Low</span></span>      |
| [<span data-ttu-id="2e0c8-174">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="2e0c8-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-175">Low</span></span>      |
| [<span data-ttu-id="2e0c8-176">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="2e0c8-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="2e0c8-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-177">Low</span></span>      |
| [<span data-ttu-id="2e0c8-178">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="2e0c8-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-179">Low</span></span>      |
| [<span data-ttu-id="2e0c8-180">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="2e0c8-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="2e0c8-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-181">Low</span></span>      |
| [<span data-ttu-id="2e0c8-182">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="2e0c8-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-183">Low</span></span>      |
| [<span data-ttu-id="2e0c8-184">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2e0c8-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="2e0c8-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-185">Low</span></span>      |
| [<span data-ttu-id="2e0c8-186">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="2e0c8-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="2e0c8-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-187">Low</span></span>      |
| [<span data-ttu-id="2e0c8-188">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="2e0c8-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="2e0c8-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-189">Low</span></span>      |
| [<span data-ttu-id="2e0c8-190">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="2e0c8-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-191">Low</span></span>      |
| [<span data-ttu-id="2e0c8-192">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="2e0c8-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-193">Low</span></span>      |
| [<span data-ttu-id="2e0c8-194">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="2e0c8-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-195">Low</span></span>      |
| [<span data-ttu-id="2e0c8-196">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="2e0c8-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="2e0c8-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-197">Low</span></span>      |
| [<span data-ttu-id="2e0c8-198">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="2e0c8-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-199">Low</span></span>      |
| [<span data-ttu-id="2e0c8-200">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="2e0c8-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-201">Low</span></span>      |
| [<span data-ttu-id="2e0c8-202">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="2e0c8-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="2e0c8-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-203">Low</span></span>      |
| [<span data-ttu-id="2e0c8-204">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="2e0c8-205">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-205">Low</span></span>      |
| [<span data-ttu-id="2e0c8-206">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="2e0c8-207">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-207">Low</span></span>      |
| [<span data-ttu-id="2e0c8-208">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="2e0c8-209">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-209">Low</span></span>      |
| [<span data-ttu-id="2e0c8-210">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="2e0c8-211">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-211">Low</span></span>      |
| [<span data-ttu-id="2e0c8-212">System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="2e0c8-213">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-213">Low</span></span>      |
| [<span data-ttu-id="2e0c8-214">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="2e0c8-215">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-215">Low</span></span>      |
| [<span data-ttu-id="2e0c8-216">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="2e0c8-217">Düşük</span><span class="sxs-lookup"><span data-stu-id="2e0c8-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="2e0c8-218">LINQ sorguları artık istemcide değerlendirilmedi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="2e0c8-219">[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[da bakın #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="2e0c8-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="2e0c8-220">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-220">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-221">3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="2e0c8-222">Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="2e0c8-223">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-223">**New behavior**</span></span>

<span data-ttu-id="2e0c8-224">3,0 ' den başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrı) istemci üzerinde değerlendirilmek üzere ifade etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="2e0c8-225">Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="2e0c8-226">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-226">**Why**</span></span>

<span data-ttu-id="2e0c8-227">Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="2e0c8-228">Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="2e0c8-229">Örneğin, çevrilemeyen bir `Where()` çağrısındaki bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="2e0c8-230">Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="2e0c8-231">Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="2e0c8-232">Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="2e0c8-233">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-233">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-234">Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın veya `AsEnumerable()`, `ToList()`ya da benzer bir şekilde, daha sonra LINQ-Objects kullanılarak daha sonra işlenebileceği istemciye açık bir şekilde veri getirmek için benzer.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="2e0c8-235">EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1</span><span class="sxs-lookup"><span data-stu-id="2e0c8-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="2e0c8-236">Sorun izleniyor #15498</span><span class="sxs-lookup"><span data-stu-id="2e0c8-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="2e0c8-237">EF Core 3,1 hedef .NET Standard 2,0 tekrar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-237">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="2e0c8-238">Bu, .NET Framework için geri destek getirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-238">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="2e0c8-239">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-239">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-240">3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-240">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="2e0c8-241">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-241">**New behavior**</span></span>

<span data-ttu-id="2e0c8-242">3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-242">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="2e0c8-243">Bu, .NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-243">This does not include .NET Framework.</span></span>

<span data-ttu-id="2e0c8-244">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-244">**Why**</span></span>

<span data-ttu-id="2e0c8-245">Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-245">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="2e0c8-246">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-246">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-247">EF Core 3,1 kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-247">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="2e0c8-248">Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2e0c8-248">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="2e0c8-249">Sorun bildirimleri izleniyor # 325</span><span class="sxs-lookup"><span data-stu-id="2e0c8-249">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="2e0c8-250">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-250">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-251">ASP.NET Core 3,0 öncesinde, `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`bir paket başvurusu eklediğinizde, EF Core ve EF Core sağlayıcı gibi SQL Server veri sağlayıcılarından bazılarını dahil eder.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-251">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="2e0c8-252">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-252">**New behavior**</span></span>

<span data-ttu-id="2e0c8-253">3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-253">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="2e0c8-254">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-254">**Why**</span></span>

<span data-ttu-id="2e0c8-255">Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-255">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="2e0c8-256">Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-256">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="2e0c8-257">Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-257">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="2e0c8-258">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-258">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="2e0c8-259">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-259">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-260">ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-260">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="2e0c8-261">EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="2e0c8-261">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="2e0c8-262">Sorun izleniyor #14016</span><span class="sxs-lookup"><span data-stu-id="2e0c8-262">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="2e0c8-263">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-263">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-264">3,0 ' dan önce, `dotnet ef` aracı .NET Core SDK eklenmiştir ve ek adımlara gerek duymadan herhangi bir projeden komut satırından kullanılmak üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-264">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="2e0c8-265">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-265">**New behavior**</span></span>

<span data-ttu-id="2e0c8-266">3,0 ' den başlayarak .NET SDK `dotnet ef` aracını içermez, bu nedenle kullanabilmeniz için önce yerel veya küresel bir araç olarak açıkça kurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-266">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="2e0c8-267">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-267">**Why**</span></span>

<span data-ttu-id="2e0c8-268">Bu değişiklik, NuGet üzerinde düzenli bir .NET CLı aracı olarak `dotnet ef` dağıtmamızı ve güncelleştirmenizi sağlar ve bu da EF Core 3,0 her zaman bir NuGet paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-268">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="2e0c8-269">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-269">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-270">`DbContext`geçişleri veya yapı iskelesi yönetmek için `dotnet-ef` genel bir araç olarak yükler:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-270">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="2e0c8-271">Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-271">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="2e0c8-272">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-272">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="2e0c8-273">Sorun izleniyor #10996</span><span class="sxs-lookup"><span data-stu-id="2e0c8-273">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="2e0c8-274">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-274">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-275">3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-275">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="2e0c8-276">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-276">**New behavior**</span></span>

<span data-ttu-id="2e0c8-277">EF Core 3,0 ' den başlayarak, parametrelerin sorgu dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlRaw`, `ExecuteSqlRaw`ve `ExecuteSqlRawAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-277">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="2e0c8-278">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-278">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="2e0c8-279">Parametrelerin, enterpolasyonlu bir sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlInterpolated`, `ExecuteSqlInterpolated`ve `ExecuteSqlInterpolatedAsync` kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="2e0c8-280">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-280">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="2e0c8-281">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-281">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="2e0c8-282">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-282">**Why**</span></span>

<span data-ttu-id="2e0c8-283">Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-283">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="2e0c8-284">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-284">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="2e0c8-285">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-285">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-286">Yeni yöntem adlarını kullanmak için geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-286">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="2e0c8-287">Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez</span><span class="sxs-lookup"><span data-stu-id="2e0c8-287">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="2e0c8-288">Sorun izleniyor #15392</span><span class="sxs-lookup"><span data-stu-id="2e0c8-288">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="2e0c8-289">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-289">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-290">3,0 EF Core önce, FromSql yöntemi, geçirilen SQL 'in üzerine oluşmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-290">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="2e0c8-291">SQL, saklı yordam gibi birleştirilmemiş bir işlem olduğu zaman istemci değerlendirmesi gerçekleştirdi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-291">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="2e0c8-292">Aşağıdaki sorgu, sunucuda saklı yordam çalıştırılarak ve istemci tarafında FirstOrDefault ' i gerçekleştirerek çalıştı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-292">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="2e0c8-293">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-293">**New behavior**</span></span>

<span data-ttu-id="2e0c8-294">EF Core 3,0 ' den başlayarak, EF Core SQL ayrıştırmaya çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-294">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="2e0c8-295">Bu nedenle, FromSqlRaw/Fromsqlenterpolasyonından sonra oluşturuyorsanız, EF Core alt sorguya neden olarak SQL 'i oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-295">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="2e0c8-296">Bu nedenle, bileşim ile saklı yordam kullanıyorsanız, geçersiz SQL söz dizimi için bir özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-296">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="2e0c8-297">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-297">**Why**</span></span>

<span data-ttu-id="2e0c8-298">EF Core 3,0, [burada](#linq-queries-are-no-longer-evaluated-on-the-client)açıklandığı gibi bir hata olduğundan, otomatik istemci değerlendirmesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-298">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="2e0c8-299">**Mayı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-299">**Mitigation**</span></span>

<span data-ttu-id="2e0c8-300">FromSqlRaw/Fromsqlenterpolasyonnda saklı bir yordam kullanıyorsanız, bunun üzerine oluşmadığını bilirsiniz; böylece sunucu tarafında herhangi bir kompozisyonu önlemek için FromSql yöntem çağrısından sonra __asslanabilir/AsAsyncEnumerable__ ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-300">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="2e0c8-301">FromSql metotları yalnızca sorgu köklerine göre belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-301">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="2e0c8-302">Sorun izleniyor #15704</span><span class="sxs-lookup"><span data-stu-id="2e0c8-302">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="2e0c8-303">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-303">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-304">3,0 EF Core önce, `FromSql` yöntemi sorgunun herhangi bir yerinden belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-304">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="2e0c8-305">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-305">**New behavior**</span></span>

<span data-ttu-id="2e0c8-306">EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (`FromSql`değiştirme) yalnızca sorgu köklerinin üzerinde belirtilebilir, yani doğrudan `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-306">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="2e0c8-307">Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-307">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="2e0c8-308">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-308">**Why**</span></span>

<span data-ttu-id="2e0c8-309">`DbSet` üzerinde `FromSql` herhangi bir yerde, hiçbir anlamı veya eklenen değeri belirtmediyse, bazı senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-309">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="2e0c8-310">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-310">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-311">`FromSql` etkinleştirmeleri, uygulandıkları `DbSet` doğrudan taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-311">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="2e0c8-312">Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz</span><span class="sxs-lookup"><span data-stu-id="2e0c8-312">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="2e0c8-313">Sorun izleniyor #13518</span><span class="sxs-lookup"><span data-stu-id="2e0c8-313">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="2e0c8-314">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-314">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-315">EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-315">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="2e0c8-316">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-316">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="2e0c8-317">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-317">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="2e0c8-318">, verilen kategoriyle ilişkilendirilen her bir `Product` için aynı `Category` örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-318">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="2e0c8-319">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-319">**New behavior**</span></span>

<span data-ttu-id="2e0c8-320">EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-320">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="2e0c8-321">Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde bile her bir `Product` için yeni bir `Category` örneği döndürecek.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-321">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="2e0c8-322">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-322">**Why**</span></span>

<span data-ttu-id="2e0c8-323">Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-323">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="2e0c8-324">Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-324">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="2e0c8-325">Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-325">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="2e0c8-326">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-326">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-327">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-327">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="2e0c8-328">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-328">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="2e0c8-329">Sorun izleniyor #14523</span><span class="sxs-lookup"><span data-stu-id="2e0c8-329">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="2e0c8-330">EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-330">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="2e0c8-331">Örneğin, SQL 'in günlüğe kaydedilmesini `Debug`değiştirmek için `OnConfiguring` veya `AddDbContext`düzeyini açıkça yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-331">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="2e0c8-332">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="2e0c8-332">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="2e0c8-333">Sorun izleniyor #12378</span><span class="sxs-lookup"><span data-stu-id="2e0c8-333">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="2e0c8-334">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-334">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-335">3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-335">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="2e0c8-336">Genellikle bu geçici değerler büyük negatif sayılardır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-336">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="2e0c8-337">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-337">**New behavior**</span></span>

<span data-ttu-id="2e0c8-338">3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-338">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="2e0c8-339">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-339">**Why**</span></span>

<span data-ttu-id="2e0c8-340">Daha önce bazı `DbContext` örnekleri tarafından izlenen bir varlık farklı bir `DbContext` örneğine taşındığında, geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-340">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="2e0c8-341">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-341">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-342">Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlara birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup `Added` durumundaki varlıklara ait olması durumunda eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-342">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="2e0c8-343">Bu, şunları önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-343">This can be avoided by:</span></span>
* <span data-ttu-id="2e0c8-344">Mağaza tarafından oluşturulan anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-344">Not using store-generated keys.</span></span>
* <span data-ttu-id="2e0c8-345">Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-345">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="2e0c8-346">Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-346">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="2e0c8-347">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue`, `blog.Id` kendisi ayarlanmamışsa bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-347">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="2e0c8-348">DetectChanges, Store tarafından oluşturulan anahtar değerlerini</span><span class="sxs-lookup"><span data-stu-id="2e0c8-348">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="2e0c8-349">Sorun izleniyor #14616</span><span class="sxs-lookup"><span data-stu-id="2e0c8-349">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="2e0c8-350">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-350">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-351">EF Core 3,0 ' dan önce, `DetectChanges` tarafından bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve `SaveChanges` çağrıldığında yeni bir satır olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-351">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="2e0c8-352">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-352">**New behavior**</span></span>

<span data-ttu-id="2e0c8-353">EF Core 3,0 ' den başlayarak, bir varlık üretilen anahtar değerlerini kullanıyorsa ve bazı anahtar değer ayarlandıysa, varlık `Modified` durumunda izlenir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-353">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="2e0c8-354">Bu, varlık için bir satırın var olduğu varsayılır ve `SaveChanges` çağrıldığında güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-354">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="2e0c8-355">Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-355">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="2e0c8-356">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-356">**Why**</span></span>

<span data-ttu-id="2e0c8-357">Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-357">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="2e0c8-358">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-358">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-359">Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-359">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="2e0c8-360">Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-360">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="2e0c8-361">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-361">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="2e0c8-362">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-362">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="2e0c8-363">Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-363">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="2e0c8-364">Sorun izleniyor #10114</span><span class="sxs-lookup"><span data-stu-id="2e0c8-364">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="2e0c8-365">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-365">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-366">3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-366">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="2e0c8-367">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-367">**New behavior**</span></span>

<span data-ttu-id="2e0c8-368">3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-368">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="2e0c8-369">Örneğin, bir sorumlu varlığı silmek için `context.Remove()` çağırmak, izlenen ilgili tüm bağımlı bağımlıların de hemen `Deleted` olarak ayarlanabilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-369">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="2e0c8-370">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-370">**Why**</span></span>

<span data-ttu-id="2e0c8-371">Bu değişiklik, `SaveChanges` çağrılmadan _önce_ hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-371">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="2e0c8-372">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-372">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-373">Önceki davranış `context.ChangeTracker`ayarları aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-373">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="2e0c8-374">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-374">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="2e0c8-375">İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-375">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="2e0c8-376">Sorun izleniyor #18022</span><span class="sxs-lookup"><span data-stu-id="2e0c8-376">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="2e0c8-377">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-377">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-378">3,0 öncesinde, `Include` işleçleri aracılığıyla koleksiyon gezginlerini yüklemek, her ilgili varlık türü için bir tane olmak üzere ilişkisel veritabanında birden çok sorgunun oluşturulmasına neden oldu.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-378">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="2e0c8-379">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-379">**New behavior**</span></span>

<span data-ttu-id="2e0c8-380">3,0 ile başlayarak, EF Core ilişkisel veritabanlarında birleştirmelere sahip tek bir sorgu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-380">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="2e0c8-381">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-381">**Why**</span></span>

<span data-ttu-id="2e0c8-382">Tek bir LINQ sorgusu uygulamak için birden çok sorgu verilmesi, birden çok veritabanı yuvarlaklığına gerek olduğu için negatif performans dahil olmak üzere çok sayıda soruna neden olmuş ve her sorgu veritabanının farklı bir durumunu gözlemleyebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-382">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="2e0c8-383">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-383">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-384">Teknik olarak bu bir değişiklik olmadığı sürece, tek bir sorgu koleksiyon gezginlerinin çok sayıda `Include` işleci içerdiğinde uygulama performansının önemli bir etkisi olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-384">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="2e0c8-385">Daha fazla bilgi edinmek ve sorguları daha verimli bir şekilde yeniden yazmak için [Bu açıklamaya bakın](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="2e0c8-385">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="2e0c8-386">DeleteBehavior. restrict Temizleme semantiğine sahip</span><span class="sxs-lookup"><span data-stu-id="2e0c8-386">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="2e0c8-387">Sorun izleniyor #12661</span><span class="sxs-lookup"><span data-stu-id="2e0c8-387">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="2e0c8-388">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-388">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-389">3,0 ' den önce, `Restrict` anlamlarında `DeleteBehavior.Restrict` yabancı anahtarlar oluşturdunuz, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-389">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="2e0c8-390">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-390">**New behavior**</span></span>

<span data-ttu-id="2e0c8-391">3,0 ile başlayarak, `DeleteBehavior.Restrict` yabancı anahtarların `Restrict` semantiğinin oluşturulmasını sağlar; Yani bu, basamaksız; sınır iç düzeltmesini etkilemeden kısıtlama ihlalinde throw.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-391">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="2e0c8-392">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-392">**Why**</span></span>

<span data-ttu-id="2e0c8-393">Bu değişiklik, beklenmeyen yan etkileri olmadan `DeleteBehavior` kullanımı kolay bir şekilde geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-393">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="2e0c8-394">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-394">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-395">Önceki davranış `DeleteBehavior.ClientNoAction`kullanılarak geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-395">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="2e0c8-396">Sorgu türleri varlık türleriyle birleştirilir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-396">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="2e0c8-397">Sorun izleniyor #14194</span><span class="sxs-lookup"><span data-stu-id="2e0c8-397">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="2e0c8-398">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-398">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-399">EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/keyless-entity-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-399">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="2e0c8-400">Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="2e0c8-400">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="2e0c8-401">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-401">**New behavior**</span></span>

<span data-ttu-id="2e0c8-402">Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-402">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="2e0c8-403">Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-403">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="2e0c8-404">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-404">**Why**</span></span>

<span data-ttu-id="2e0c8-405">Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-405">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="2e0c8-406">Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-406">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="2e0c8-407">Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-407">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="2e0c8-408">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-408">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-409">API 'nin aşağıdaki bölümleri artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-409">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="2e0c8-410">**`ModelBuilder.Query<>()`** -bir varlık türünü anahtar olmadan işaretlemek için `ModelBuilder.Entity<>().HasNoKey()` çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-410">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="2e0c8-411">Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-411">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="2e0c8-412">**`DbQuery<>`** bunun yerine `DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-412">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="2e0c8-413">**`DbContext.Query<>()`** bunun yerine `DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-413">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="2e0c8-414">Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti</span><span class="sxs-lookup"><span data-stu-id="2e0c8-414">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="2e0c8-415">Sorun izleme [#12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444) sorunu
izleme sorunu [#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[izleme sorunu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="2e0c8-415">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="2e0c8-416">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-416">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-417">3,0 EF Core önce, sahip olan ilişkinin yapılandırması `OwnsOne` veya `OwnsMany` çağrısından sonra doğrudan gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-417">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="2e0c8-418">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-418">**New behavior**</span></span>

<span data-ttu-id="2e0c8-419">EF Core 3,0 ' den başlayarak, artık `WithOwner()`kullanarak sahip için bir gezinti özelliği yapılandırmak Fluent API.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-419">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="2e0c8-420">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-420">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="2e0c8-421">Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin yapılandırılmasına benzer şekilde `WithOwner()` sonra zincirde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-421">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="2e0c8-422">Sahip olduğu için yapılandırma, `OwnsOne()/OwnsMany()`sonra hala zincirleme olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-422">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="2e0c8-423">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-423">For example:</span></span>

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

<span data-ttu-id="2e0c8-424">Ayrıca, sahip bir tür hedefi ile `Entity()`, `HasOne()`veya `Set()` çağırmak artık bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-424">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="2e0c8-425">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-425">**Why**</span></span>

<span data-ttu-id="2e0c8-426">Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-426">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="2e0c8-427">Bu, sırasıyla `HasForeignKey`gibi yöntemlere ve karışıklığı ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-427">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="2e0c8-428">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-428">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-429">Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-429">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="2e0c8-430">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-430">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="2e0c8-431">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="2e0c8-431">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="2e0c8-432">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-432">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-433">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-433">Consider the following model:</span></span>
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
<span data-ttu-id="2e0c8-434">EF Core 3,0 ' dan önce, `OrderDetails` aynı tabloyla `Order` veya açıkça eşlenmişse, yeni bir `Order`eklenirken `OrderDetails` örneği her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="2e0c8-435">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-435">**New behavior**</span></span>

<span data-ttu-id="2e0c8-436">3,0 ile başlayarak EF Core, `OrderDetails` olmayan bir `Order` eklemesine ve birincil anahtar null yapılabilir sütunlara hariç tüm `OrderDetails` özelliklerini eşleştirme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-436">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="2e0c8-437">EF Core kümelerini sorgularken, gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar ve tüm `null`Özellikler ' in yanı sıra gerekli özellikleri yoksa, `null` `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-437">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="2e0c8-438">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-438">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-439">Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinmenin `null` olması beklenmiyorsa, gezinti `null`olduğunda, uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-439">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="2e0c8-440">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış`null` bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-440">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="2e0c8-441">Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-441">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="2e0c8-442">Sorun izleniyor #14154</span><span class="sxs-lookup"><span data-stu-id="2e0c8-442">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="2e0c8-443">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-443">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-444">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-444">Consider the following model:</span></span>
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
<span data-ttu-id="2e0c8-445">EF Core 3,0 ' dan önce, `OrderDetails` aynı tabloyla `Order` ya da açıkça eşlenmişse, yalnızca `OrderDetails` güncelleştirilmesi istemci üzerindeki `Version` değerini güncelleştirmez ve sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-445">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="2e0c8-446">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-446">**New behavior**</span></span>

<span data-ttu-id="2e0c8-447">3,0 ' den itibaren, yeni `Version` değerini `OrderDetails`sahip `Order` EF Core yayar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-447">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="2e0c8-448">Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-448">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="2e0c8-449">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-449">**Why**</span></span>

<span data-ttu-id="2e0c8-450">Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-450">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="2e0c8-451">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-451">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-452">Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-452">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="2e0c8-453">Gölge durumundaki bir oluşturma olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-453">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="2e0c8-454">Sahip olunan varlıklar, izleme sorgusu kullanılarak sahip olmadan sorgulanamıyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-454">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="2e0c8-455">Sorun izleniyor #18876</span><span class="sxs-lookup"><span data-stu-id="2e0c8-455">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="2e0c8-456">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-456">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-457">EF Core 3,0 ' dan önce, sahip olan varlıklar başka bir gezinti olarak sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-457">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="2e0c8-458">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-458">**New behavior**</span></span>

<span data-ttu-id="2e0c8-459">3,0 ile başlayarak, bir izleme sorgusu sahip olmayan bir varlık projesinde proje EF Core oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-459">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="2e0c8-460">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-460">**Why**</span></span>

<span data-ttu-id="2e0c8-461">Sahip olunan varlıklar sahip olmadan değiştirilemez, bu nedenle bu şekilde sorgulama durumlarının büyük çoğunluğunda bir hata olur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-461">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="2e0c8-462">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-462">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-463">Sahip olduğu varlık daha sonra değiştirilecek şekilde izleniyorsa, sahibin sorguya eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-463">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="2e0c8-464">Aksi takdirde `AsNoTracking()` çağrısı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-464">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="2e0c8-465">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-465">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="2e0c8-466">Sorun izleniyor #13998</span><span class="sxs-lookup"><span data-stu-id="2e0c8-466">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="2e0c8-467">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-467">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-468">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-468">Consider the following model:</span></span>
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

<span data-ttu-id="2e0c8-469">EF Core 3,0 ' dan önce, `ShippingAddress` özelliği varsayılan olarak `BulkOrder` ve `Order` ayrı sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-469">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="2e0c8-470">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-470">**New behavior**</span></span>

<span data-ttu-id="2e0c8-471">3,0 ile başlayarak, EF Core `ShippingAddress`için yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-471">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="2e0c8-472">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-472">**Why**</span></span>

<span data-ttu-id="2e0c8-473">Eski davranınır beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-473">The old behavoir was unexpected.</span></span>

<span data-ttu-id="2e0c8-474">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-474">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-475">Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-475">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="2e0c8-476">Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-476">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="2e0c8-477">Sorun izleniyor #13274</span><span class="sxs-lookup"><span data-stu-id="2e0c8-477">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="2e0c8-478">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-478">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-479">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-479">Consider the following model:</span></span>
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
<span data-ttu-id="2e0c8-480">EF Core 3,0 ' dan önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-480">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="2e0c8-481">Ancak, `Order` sahipli bir tür ise, bu da birincil anahtar `CustomerId` olur ve bu genellikle beklenmez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-481">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="2e0c8-482">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-482">**New behavior**</span></span>

<span data-ttu-id="2e0c8-483">3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-483">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="2e0c8-484">Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-484">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="2e0c8-485">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-485">For example:</span></span>

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

<span data-ttu-id="2e0c8-486">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-486">**Why**</span></span>

<span data-ttu-id="2e0c8-487">Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-487">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="2e0c8-488">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-488">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-489">Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-489">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="2e0c8-490">TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-490">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="2e0c8-491">Sorun izleniyor #14218</span><span class="sxs-lookup"><span data-stu-id="2e0c8-491">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="2e0c8-492">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-492">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-493">EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope`içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-493">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="2e0c8-494">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-494">**New behavior**</span></span>

<span data-ttu-id="2e0c8-495">3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-495">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="2e0c8-496">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-496">**Why**</span></span>

<span data-ttu-id="2e0c8-497">Bu değişiklik aynı `TransactionScope`birden çok bağlam kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-497">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="2e0c8-498">Yeni davranış ayrıca EF6 ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-498">The new behavior also matches EF6.</span></span>

<span data-ttu-id="2e0c8-499">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-499">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-500">Bağlantının açık açık olarak kalması gerekiyorsa `OpenConnection()`, EF Core zamanından önce kapanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-500">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="2e0c8-501">Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-501">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="2e0c8-502">Sorun izleniyor #6872</span><span class="sxs-lookup"><span data-stu-id="2e0c8-502">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="2e0c8-503">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-503">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-504">3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-504">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="2e0c8-505">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-505">**New behavior**</span></span>

<span data-ttu-id="2e0c8-506">EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-506">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="2e0c8-507">Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-507">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="2e0c8-508">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-508">**Why**</span></span>

<span data-ttu-id="2e0c8-509">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-509">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="2e0c8-510">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-510">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-511">Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-511">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="2e0c8-512">Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-512">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="2e0c8-513">Yedekleme alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-513">Backing fields are used by default</span></span>

[<span data-ttu-id="2e0c8-514">Sorun izleniyor #12430</span><span class="sxs-lookup"><span data-stu-id="2e0c8-514">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="2e0c8-515">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-515">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-516">3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="2e0c8-517">Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-517">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="2e0c8-518">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-518">**New behavior**</span></span>

<span data-ttu-id="2e0c8-519">EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-519">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="2e0c8-520">Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="2e0c8-521">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-521">**Why**</span></span>

<span data-ttu-id="2e0c8-522">Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-522">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="2e0c8-523">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-523">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-524">3,0 öncesi davranış `ModelBuilder`özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-524">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="2e0c8-525">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-525">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="2e0c8-526">Birden çok uyumlu yedekleme alanı bulunursa throw</span><span class="sxs-lookup"><span data-stu-id="2e0c8-526">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="2e0c8-527">Sorun izleniyor #12523</span><span class="sxs-lookup"><span data-stu-id="2e0c8-527">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="2e0c8-528">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-528">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-529">EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-529">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="2e0c8-530">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-530">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="2e0c8-531">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-531">**New behavior**</span></span>

<span data-ttu-id="2e0c8-532">EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-532">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="2e0c8-533">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-533">**Why**</span></span>

<span data-ttu-id="2e0c8-534">Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-534">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="2e0c8-535">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-535">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-536">Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-536">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="2e0c8-537">Örneğin, Fluent API kullanımı:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-537">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="2e0c8-538">Yalnızca alan özellik adları alan adıyla eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-538">Field-only property names should match the field name</span></span>

<span data-ttu-id="2e0c8-539">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-539">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-540">EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-540">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

<span data-ttu-id="2e0c8-541">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-541">**New behavior**</span></span>

<span data-ttu-id="2e0c8-542">EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-542">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="2e0c8-543">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-543">**Why**</span></span>

<span data-ttu-id="2e0c8-544">Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-544">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="2e0c8-545">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-545">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-546">Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-546">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="2e0c8-547">3,0 sonrasında EF Core gelecek bir sürümünde, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="2e0c8-547">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="2e0c8-548">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-548">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="2e0c8-549">Sorun izleniyor #14756</span><span class="sxs-lookup"><span data-stu-id="2e0c8-549">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="2e0c8-550">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-550">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-551">EF Core 3,0 ' dan önce, `AddDbContext` veya `AddDbContextPool` çağırma işlemi, [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrılarına çağrı ile günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-551">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="2e0c8-552">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-552">**New behavior**</span></span>

<span data-ttu-id="2e0c8-553">EF Core 3,0 ' den başlayarak `AddDbContext` ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-553">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="2e0c8-554">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-554">**Why**</span></span>

<span data-ttu-id="2e0c8-555">EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-555">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="2e0c8-556">Ancak, `ILoggerFactory` uygulamanın dı kapsayıcısına kayıtlıysa, EF Core tarafından hala kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-556">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="2e0c8-557">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-557">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-558">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-558">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="2e0c8-559">AddEntityFramework \* bir boyut sınırı ile ımemorycache ekler</span><span class="sxs-lookup"><span data-stu-id="2e0c8-559">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="2e0c8-560">Sorun izleniyor #12905</span><span class="sxs-lookup"><span data-stu-id="2e0c8-560">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="2e0c8-561">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-561">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-562">EF Core 3,0 ' dan önce, `AddEntityFramework*` yöntemlerin çağrılması, bellek önbelleğe alma hizmetlerini bir boyut sınırı olmadan DI ile de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-562">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="2e0c8-563">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-563">**New behavior**</span></span>

<span data-ttu-id="2e0c8-564">EF Core 3,0 ' den başlayarak `AddEntityFramework*` bir ımemorycache hizmetini boyut sınırı ile kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-564">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="2e0c8-565">Daha sonra eklenen diğer hizmetler ımemorycache 'e bağımlıysa, özel durumlara veya performans düşüklüğe neden olan varsayılan sınıra hızlıca ulaşabilirler.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-565">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="2e0c8-566">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-566">**Why**</span></span>

<span data-ttu-id="2e0c8-567">Sorgu önbelleğe alma mantığındaki bir hata varsa veya sorgular dinamik olarak oluşturulduysa, ımemorycache 'i sınır olmadan kullanmak denetlenmeyen bellek kullanımına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-567">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="2e0c8-568">Varsayılan sınırın olması olası bir DoS saldırısının etkisini azaltır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-568">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="2e0c8-569">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-569">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-570">Çoğu durumda `AddEntityFramework*` çağrısı `AddDbContext` veya `AddDbContextPool` da çağrılırsa gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-570">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="2e0c8-571">Bu nedenle, en iyi risk azaltma `AddEntityFramework*` çağrısını kaldırmayacak.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-571">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="2e0c8-572">Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, daha önce [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)' i kullanarak bir ıMEMORYCACHE uygulamasını dı kapsayıcısı ile açık olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-572">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="2e0c8-573">DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-573">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="2e0c8-574">Sorun izleniyor #13552</span><span class="sxs-lookup"><span data-stu-id="2e0c8-574">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="2e0c8-575">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-575">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-576">EF Core 3,0 önce, `DbContext.Entry` çağırma, izlenen tüm varlıklar için değişikliklerin algılanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-576">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="2e0c8-577">Bu, `EntityEntry` açığa çıkarılan durumun güncel olduğunu ortaya çıkardık.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-577">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="2e0c8-578">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-578">**New behavior**</span></span>

<span data-ttu-id="2e0c8-579">EF Core 3,0 ' den başlayarak, çağırma `DbContext.Entry` artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili tüm izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-579">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="2e0c8-580">Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-580">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="2e0c8-581">`ChangeTracker.AutoDetectChangesEnabled` `false` olarak ayarlanırsa, bu yerel değişikliğin algılanması de devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-581">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="2e0c8-582">Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` ve `SaveChanges`--yine de izlenen tüm varlıkların tam `DetectChanges` neden olur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-582">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="2e0c8-583">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-583">**Why**</span></span>

<span data-ttu-id="2e0c8-584">Bu değişiklik, `context.Entry`kullanmanın varsayılan performansını geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-584">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="2e0c8-585">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-585">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-586">3,0 öncesi davranışı sağlamak için `Entry` çağrılmadan önce `ChangeTracker.DetectChanges()` çağırın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-586">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="2e0c8-587">Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur</span><span class="sxs-lookup"><span data-stu-id="2e0c8-587">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="2e0c8-588">Sorun izleniyor #14617</span><span class="sxs-lookup"><span data-stu-id="2e0c8-588">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="2e0c8-589">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-589">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-590">EF Core 3,0 önce `string` ve `byte[]` anahtar özellikleri açık bir şekilde null olmayan bir değer ayarlamadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-590">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="2e0c8-591">Böyle bir durumda, anahtar değeri istemci üzerinde `byte[]`için bayt olarak serileştirilmiş bir GUID olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-591">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="2e0c8-592">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-592">**New behavior**</span></span>

<span data-ttu-id="2e0c8-593">EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-593">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="2e0c8-594">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-594">**Why**</span></span>

<span data-ttu-id="2e0c8-595">Bu değişiklik, istemci tarafından oluşturulan `string`/`byte[]` değerleri genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-595">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="2e0c8-596">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-596">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-597">Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-597">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="2e0c8-598">Örneğin, Fluent API:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-598">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="2e0c8-599">Ya da veri ek açıklamalarıyla:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-599">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="2e0c8-600">Iloggerfactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-600">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="2e0c8-601">Sorun izleniyor #14698</span><span class="sxs-lookup"><span data-stu-id="2e0c8-601">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="2e0c8-602">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-602">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-603">EF Core 3,0 tarihinden önce `ILoggerFactory` bir tek hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-603">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="2e0c8-604">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-604">**New behavior**</span></span>

<span data-ttu-id="2e0c8-605">EF Core 3,0 ' den başlayarak `ILoggerFactory` artık kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-605">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="2e0c8-606">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-606">**Why**</span></span>

<span data-ttu-id="2e0c8-607">Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi bazı bazı durumları kaldıran `DbContext` örneğiyle bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-607">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="2e0c8-608">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-608">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-609">Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-609">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="2e0c8-610">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-610">This isn't common.</span></span>
<span data-ttu-id="2e0c8-611">Bu durumlarda, çoğu şey çalışmaya devam edecektir, ancak `ILoggerFactory` bağlı olan herhangi bir singleton hizmeti, `ILoggerFactory` farklı bir şekilde almak için değiştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-611">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="2e0c8-612">Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) ' nde bir sorun `ILoggerFactory` bildirin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-612">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="2e0c8-613">Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="2e0c8-613">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="2e0c8-614">Sorun izleniyor #12780</span><span class="sxs-lookup"><span data-stu-id="2e0c8-614">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="2e0c8-615">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-615">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-616">EF Core 3,0 ' dan önce, bir `DbContext` atıldıktan sonra, söz konusu bağlamdan alınan bir varlık üzerinde verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-616">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="2e0c8-617">Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-617">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="2e0c8-618">Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-618">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="2e0c8-619">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-619">**New behavior**</span></span>

<span data-ttu-id="2e0c8-620">EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-620">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="2e0c8-621">Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-621">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="2e0c8-622">Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-622">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="2e0c8-623">Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-623">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="2e0c8-624">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-624">**Why**</span></span>

<span data-ttu-id="2e0c8-625">Bu değişiklik, atılmış bir `DbContext` örneğine geç yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-625">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="2e0c8-626">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-626">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-627">Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-627">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="2e0c8-628">İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-628">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="2e0c8-629">Sorun izleniyor #10236</span><span class="sxs-lookup"><span data-stu-id="2e0c8-629">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="2e0c8-630">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-630">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-631">3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-631">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="2e0c8-632">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-632">**New behavior**</span></span>

<span data-ttu-id="2e0c8-633">EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-633">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="2e0c8-634">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-634">**Why**</span></span>

<span data-ttu-id="2e0c8-635">Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha</span><span class="sxs-lookup"><span data-stu-id="2e0c8-635">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="2e0c8-636">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-636">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-637">Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-637">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="2e0c8-638">Ancak, hata `DbContextOptionsBuilder`yapılandırması aracılığıyla bir uyarıya (veya yoksayıldı) geri dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-638">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="2e0c8-639">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-639">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="2e0c8-640">HasOne/HasMany için tek bir dize ile çağrılan yeni davranış</span><span class="sxs-lookup"><span data-stu-id="2e0c8-640">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="2e0c8-641">Sorun izleniyor #9171</span><span class="sxs-lookup"><span data-stu-id="2e0c8-641">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="2e0c8-642">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-642">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-643">EF Core 3,0 öncesinde, tek bir dize ile `HasOne` veya `HasMany` kodu, kafa karıştırıcı bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-643">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="2e0c8-644">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-644">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="2e0c8-645">Kod, özel olabilecek `Entrance` gezinti özelliğini kullanarak `Samurai` başka bir varlık türüyle ilişkili olduğu gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-645">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="2e0c8-646">Gerçekte, bu kod, gezinti özelliği olmayan `Entrance` adlı bazı varlık türlerine bir ilişki oluşturmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-646">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="2e0c8-647">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-647">**New behavior**</span></span>

<span data-ttu-id="2e0c8-648">EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-648">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="2e0c8-649">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-649">**Why**</span></span>

<span data-ttu-id="2e0c8-650">Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-650">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="2e0c8-651">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-651">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-652">Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-652">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="2e0c8-653">Bu, yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-653">This is not common.</span></span>
<span data-ttu-id="2e0c8-654">Önceki davranış, gezinti özelliği adı için `null` açıkça geçirerek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-654">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="2e0c8-655">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-655">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="2e0c8-656">Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-656">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="2e0c8-657">Sorun izleniyor #15184</span><span class="sxs-lookup"><span data-stu-id="2e0c8-657">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="2e0c8-658">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-658">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-659">Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürdü:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-659">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="2e0c8-660">`ValueGenerator.NextValueAsync()` (ve türetme sınıfları)</span><span class="sxs-lookup"><span data-stu-id="2e0c8-660">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="2e0c8-661">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-661">**New behavior**</span></span>

<span data-ttu-id="2e0c8-662">Yukarıda bahsedilen yöntemler bundan sonra aynı `T` daha `ValueTask<T>` döndürür.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-662">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="2e0c8-663">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-663">**Why**</span></span>

<span data-ttu-id="2e0c8-664">Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-664">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="2e0c8-665">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-665">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-666">Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-666">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="2e0c8-667">Daha karmaşık bir kullanım (örn. döndürülen `Task` `Task.WhenAny()`geçirilmesi), genellikle döndürülen `ValueTask<T>` `AsTask()` çağırarak `Task<T>` dönüştürmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-667">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="2e0c8-668">Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-668">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="2e0c8-669">Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2e0c8-669">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="2e0c8-670">Sorun izleniyor #9913</span><span class="sxs-lookup"><span data-stu-id="2e0c8-670">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="2e0c8-671">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-671">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-672">Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-672">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="2e0c8-673">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-673">**New behavior**</span></span>

<span data-ttu-id="2e0c8-674">Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-674">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="2e0c8-675">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-675">**Why**</span></span>

<span data-ttu-id="2e0c8-676">Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-676">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="2e0c8-677">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-677">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-678">Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-678">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="2e0c8-679">Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-679">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="2e0c8-680">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="2e0c8-680">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="2e0c8-681">Sorun izleniyor #11811</span><span class="sxs-lookup"><span data-stu-id="2e0c8-681">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="2e0c8-682">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-682">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-683">EF Core 3,0 ' dan önce, yalnızca devralma eşleme stratejisi bu geçerli olmayan bir TPH olduğundan, türetilmiş bir tür üzerinde çağrılan `ToTable()` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-683">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="2e0c8-684">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-684">**New behavior**</span></span>

<span data-ttu-id="2e0c8-685">EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, bir türetilmiş tür üzerinde çağrılan `ToTable()` artık gelecekte beklenmedik bir eşleme değişikliğini önlemek için bir özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-685">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="2e0c8-686">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-686">**Why**</span></span>

<span data-ttu-id="2e0c8-687">Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-687">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="2e0c8-688">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-688">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="2e0c8-689">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-689">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-690">Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-690">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="2e0c8-691">Forsqlserverhasındex, HasIndex ile değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="2e0c8-691">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="2e0c8-692">Sorun izleniyor #12366</span><span class="sxs-lookup"><span data-stu-id="2e0c8-692">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="2e0c8-693">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-693">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-694">EF Core 3,0 ' dan önce `ForSqlServerHasIndex().ForSqlServerInclude()` `INCLUDE`ile kullanılan sütunları yapılandırmak için bir yol sağladı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-694">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="2e0c8-695">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-695">**New behavior**</span></span>

<span data-ttu-id="2e0c8-696">EF Core 3,0 ' den itibaren, bir dizinde `Include` kullanmak artık ilişkisel düzeyde destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-696">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="2e0c8-697">`HasIndex().ForSqlServerInclude()`kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-697">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="2e0c8-698">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-698">**Why**</span></span>

<span data-ttu-id="2e0c8-699">Bu değişiklik, tüm veritabanı sağlayıcıları için `Include` olan dizinler için API 'yi tek bir yerde birleştirmek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-699">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="2e0c8-700">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-700">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-701">Yukarıda gösterildiği gibi yeni API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-701">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="2e0c8-702">Meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2e0c8-702">Metadata API changes</span></span>

[<span data-ttu-id="2e0c8-703">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="2e0c8-703">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2e0c8-704">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-704">**New behavior**</span></span>

<span data-ttu-id="2e0c8-705">Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-705">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="2e0c8-706">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-706">**Why**</span></span>

<span data-ttu-id="2e0c8-707">Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-707">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="2e0c8-708">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-708">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-709">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-709">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="2e0c8-710">Sağlayıcıya özel meta veri API 'SI değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="2e0c8-710">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="2e0c8-711">Sorun izleniyor #214</span><span class="sxs-lookup"><span data-stu-id="2e0c8-711">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2e0c8-712">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-712">**New behavior**</span></span>

<span data-ttu-id="2e0c8-713">Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-713">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="2e0c8-714">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-714">**Why**</span></span>

<span data-ttu-id="2e0c8-715">Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-715">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="2e0c8-716">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-716">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-717">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-717">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="2e0c8-718">EF Core, SQLite FK zorlaması için artık pragma göndermez</span><span class="sxs-lookup"><span data-stu-id="2e0c8-718">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="2e0c8-719">Sorun izleniyor #12151</span><span class="sxs-lookup"><span data-stu-id="2e0c8-719">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="2e0c8-720">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-720">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-721">3,0 EF Core önce, bir SQLite bağlantısı açıldığında EF Core `PRAGMA foreign_keys = 1` gönderebilirim.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-721">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2e0c8-722">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-722">**New behavior**</span></span>

<span data-ttu-id="2e0c8-723">EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı açıldığında EF Core artık `PRAGMA foreign_keys = 1` göndermez.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-723">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2e0c8-724">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-724">**Why**</span></span>

<span data-ttu-id="2e0c8-725">Bu değişiklik, EF Core varsayılan olarak `SQLitePCLRaw.bundle_e_sqlite3` kullandığından, bu, sırasıyla FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-725">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="2e0c8-726">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-726">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-727">EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 öğesinde yabancı anahtarlar varsayılan olarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-727">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="2e0c8-728">Diğer durumlarda, yabancı anahtarlar bağlantı dizeniz `Foreign Keys=True` belirtilerek etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-728">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="2e0c8-729">Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-729">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="2e0c8-730">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-730">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-731">3,0 EF Core önce EF Core `SQLitePCLRaw.bundle_green`kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-731">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="2e0c8-732">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-732">**New behavior**</span></span>

<span data-ttu-id="2e0c8-733">EF Core 3,0 ' den başlayarak EF Core `SQLitePCLRaw.bundle_e_sqlite3`kullanır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-733">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="2e0c8-734">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-734">**Why**</span></span>

<span data-ttu-id="2e0c8-735">Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-735">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="2e0c8-736">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-736">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-737">İOS 'ta yerel SQLite sürümünü kullanmak için `Microsoft.Data.Sqlite` farklı bir `SQLitePCLRaw` paketi kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-737">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2e0c8-738">GUID değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-738">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2e0c8-739">Sorun izleniyor #15078</span><span class="sxs-lookup"><span data-stu-id="2e0c8-739">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="2e0c8-740">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-740">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-741">GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-741">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="2e0c8-742">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-742">**New behavior**</span></span>

<span data-ttu-id="2e0c8-743">GUID değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-743">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="2e0c8-744">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-744">**Why**</span></span>

<span data-ttu-id="2e0c8-745">GUID 'lerin ikili biçimi standartlaştırılmış değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-745">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="2e0c8-746">Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-746">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2e0c8-747">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-747">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-748">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="2e0c8-749">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="2e0c8-750">Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-750">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2e0c8-751">Char değerleri artık SQLite üzerinde metın olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-751">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2e0c8-752">Sorun izleniyor #15020</span><span class="sxs-lookup"><span data-stu-id="2e0c8-752">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="2e0c8-753">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-753">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-754">Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-754">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="2e0c8-755">Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-755">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="2e0c8-756">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-756">**New behavior**</span></span>

<span data-ttu-id="2e0c8-757">Char değerleri artık metın olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-757">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="2e0c8-758">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-758">**Why**</span></span>

<span data-ttu-id="2e0c8-759">Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-759">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2e0c8-760">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-760">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-761">Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-761">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="2e0c8-762">EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-762">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="2e0c8-763">Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-763">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="2e0c8-764">Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur</span><span class="sxs-lookup"><span data-stu-id="2e0c8-764">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="2e0c8-765">Sorun izleniyor #12978</span><span class="sxs-lookup"><span data-stu-id="2e0c8-765">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="2e0c8-766">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-766">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-767">Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-767">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="2e0c8-768">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-768">**New behavior**</span></span>

<span data-ttu-id="2e0c8-769">Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-769">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="2e0c8-770">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-770">**Why**</span></span>

<span data-ttu-id="2e0c8-771">Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-771">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="2e0c8-772">Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-772">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="2e0c8-773">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-773">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-774">Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi).</span><span class="sxs-lookup"><span data-stu-id="2e0c8-774">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="2e0c8-775">Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-775">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="2e0c8-776">Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-776">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="2e0c8-777">Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-777">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="2e0c8-778">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-778">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="2e0c8-779">Sorun izleniyor #16400</span><span class="sxs-lookup"><span data-stu-id="2e0c8-779">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="2e0c8-780">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-780">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-781">EF Core 3,0 ' dan önce `UseRowNumberForPaging`, SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-781">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="2e0c8-782">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-782">**New behavior**</span></span>

<span data-ttu-id="2e0c8-783">EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-783">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="2e0c8-784">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-784">**Why**</span></span>

<span data-ttu-id="2e0c8-785">[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-785">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="2e0c8-786">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-786">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-787">Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-787">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="2e0c8-788">Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-788">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="2e0c8-789">Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-789">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="2e0c8-790">Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-790">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="2e0c8-791">Sorun izleniyor #16119</span><span class="sxs-lookup"><span data-stu-id="2e0c8-791">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="2e0c8-792">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-792">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-793">`IDbContextOptionsExtension`, uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-793">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="2e0c8-794">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-794">**New behavior**</span></span>

<span data-ttu-id="2e0c8-795">Bu yöntemler yeni bir `IDbContextOptionsExtension.Info` özelliğinden döndürülen yeni bir `DbContextOptionsExtensionInfo` soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-795">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="2e0c8-796">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-796">**Why**</span></span>

<span data-ttu-id="2e0c8-797">2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-797">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="2e0c8-798">Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-798">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="2e0c8-799">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-799">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-800">Yeni kalıbı izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-800">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="2e0c8-801">EF Core kaynak kodundaki farklı tür uzantılara yönelik `IDbContextOptionsExtension` birçok uygulamada örnek bulunur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-801">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="2e0c8-802">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-802">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="2e0c8-803">Sorun izleniyor #10985</span><span class="sxs-lookup"><span data-stu-id="2e0c8-803">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="2e0c8-804">**Değişebilir**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-804">**Change**</span></span>

<span data-ttu-id="2e0c8-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`olarak yeniden adlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="2e0c8-806">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-806">**Why**</span></span>

<span data-ttu-id="2e0c8-807">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-807">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="2e0c8-808">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-808">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-809">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-809">Use the new name.</span></span> <span data-ttu-id="2e0c8-810">(Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="2e0c8-810">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="2e0c8-811">Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme</span><span class="sxs-lookup"><span data-stu-id="2e0c8-811">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="2e0c8-812">Sorun izleniyor #10730</span><span class="sxs-lookup"><span data-stu-id="2e0c8-812">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="2e0c8-813">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-813">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-814">3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-814">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="2e0c8-815">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-815">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="2e0c8-816">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-816">**New behavior**</span></span>

<span data-ttu-id="2e0c8-817">EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-817">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="2e0c8-818">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="2e0c8-819">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-819">**Why**</span></span>

<span data-ttu-id="2e0c8-820">Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-820">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="2e0c8-821">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-821">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-822">Yeni adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-822">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="2e0c8-823">Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı</span><span class="sxs-lookup"><span data-stu-id="2e0c8-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="2e0c8-824">Sorun izleniyor #15997</span><span class="sxs-lookup"><span data-stu-id="2e0c8-824">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="2e0c8-825">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-825">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-826">3,0 EF Core önce bu yöntemler korundu.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-826">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="2e0c8-827">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-827">**New behavior**</span></span>

<span data-ttu-id="2e0c8-828">EF Core 3,0 ' den itibaren bu yöntemler geneldir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-828">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="2e0c8-829">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-829">**Why**</span></span>

<span data-ttu-id="2e0c8-830">Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-830">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="2e0c8-831">Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-831">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="2e0c8-832">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-832">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-833">Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-833">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="2e0c8-834">Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-834">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="2e0c8-835">Sorun izleniyor #11506</span><span class="sxs-lookup"><span data-stu-id="2e0c8-835">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="2e0c8-836">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-836">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-837">EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-837">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="2e0c8-838">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-838">**New behavior**</span></span>

<span data-ttu-id="2e0c8-839">EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-839">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="2e0c8-840">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-840">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="2e0c8-841">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-841">**Why**</span></span>

<span data-ttu-id="2e0c8-842">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-842">This package is only intended to be used at design time.</span></span> <span data-ttu-id="2e0c8-843">Dağıtılan uygulamalar buna başvurmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-843">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="2e0c8-844">Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-844">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="2e0c8-845">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-845">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-846">EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-846">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="2e0c8-847">Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-847">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="2e0c8-848">Bu tür bir açık başvuru, paketteki türlerin gerekli olduğu herhangi bir projeye eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-848">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="2e0c8-849">SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-849">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="2e0c8-850">Sorun izleniyor #14824</span><span class="sxs-lookup"><span data-stu-id="2e0c8-850">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="2e0c8-851">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-851">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-852">Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-852">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="2e0c8-853">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-853">**New behavior**</span></span>

<span data-ttu-id="2e0c8-854">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-854">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2e0c8-855">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-855">**Why**</span></span>

<span data-ttu-id="2e0c8-856">SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-856">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="2e0c8-857">Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-857">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="2e0c8-858">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-858">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-859">SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-859">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2e0c8-860">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-860">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="2e0c8-861">Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="2e0c8-861">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="2e0c8-862">Sorun izleniyor #14825</span><span class="sxs-lookup"><span data-stu-id="2e0c8-862">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="2e0c8-863">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-863">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-864">Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-864">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="2e0c8-865">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-865">**New behavior**</span></span>

<span data-ttu-id="2e0c8-866">Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-866">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2e0c8-867">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-867">**Why**</span></span>

<span data-ttu-id="2e0c8-868">EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-868">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="2e0c8-869">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-869">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-870">Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-870">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2e0c8-871">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-871">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="2e0c8-872">System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-872">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="2e0c8-873">Sorun izleniyor #15636</span><span class="sxs-lookup"><span data-stu-id="2e0c8-873">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="2e0c8-874">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-874">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-875">Microsoft. EntityFrameworkCore. SqlServer daha önce System. Data. SqlClient 'a bağımlı.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-875">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="2e0c8-876">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-876">**New behavior**</span></span>

<span data-ttu-id="2e0c8-877">Paketimizi Microsoft. Data. SqlClient 'e göre güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-877">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2e0c8-878">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-878">**Why**</span></span>

<span data-ttu-id="2e0c8-879">Microsoft. Data. SqlClient, SQL Server ileriye dönük tanıtım verileri erişim sürücüsüdür ve System. Data. SqlClient artık geliştirme odağı değildir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-879">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="2e0c8-880">Always Encrypted gibi bazı önemli özellikler yalnızca Microsoft. Data. SqlClient ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-880">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2e0c8-881">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-881">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-882">Kodunuz System. Data. SqlClient üzerinde doğrudan bir bağımlılık alırsa bunun yerine Microsoft. Data. SqlClient öğesine başvuracak şekilde değiştirmeniz gerekir; iki paket çok yüksek düzeyde API uyumluluğu korudıkça, bu yalnızca basit bir paket ve ad alanı değişikliği olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-882">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="2e0c8-883">Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor</span><span class="sxs-lookup"><span data-stu-id="2e0c8-883">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="2e0c8-884">Sorun izleniyor #13573</span><span class="sxs-lookup"><span data-stu-id="2e0c8-884">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="2e0c8-885">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-885">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-886">Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-886">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="2e0c8-887">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-887">For example:</span></span>

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

<span data-ttu-id="2e0c8-888">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-888">**New behavior**</span></span>

<span data-ttu-id="2e0c8-889">Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-889">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="2e0c8-890">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-890">**Why**</span></span>

<span data-ttu-id="2e0c8-891">Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-891">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="2e0c8-892">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-892">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-893">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-893">Use full configuration of the relationship.</span></span> <span data-ttu-id="2e0c8-894">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2e0c8-894">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="2e0c8-895">DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır</span><span class="sxs-lookup"><span data-stu-id="2e0c8-895">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="2e0c8-896">Sorun izleniyor #12757</span><span class="sxs-lookup"><span data-stu-id="2e0c8-896">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="2e0c8-897">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-897">**Old behavior**</span></span>

<span data-ttu-id="2e0c8-898">Şema ile boş bir dize olarak yapılandırılmış bir DbFunction, şema olmadan yerleşik işlev olarak değerlendirildi.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-898">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="2e0c8-899">Örneğin, aşağıdaki kod `DatePart` CLR işlevini SqlServer üzerinde `DATEPART` yerleşik işlevine eşleyecek.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-899">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="2e0c8-900">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-900">**New behavior**</span></span>

<span data-ttu-id="2e0c8-901">Tüm DbFunction eşlemeleri Kullanıcı tanımlı işlevlere eşlenildiği kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-901">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="2e0c8-902">Bu nedenle boş dize değeri, işlevi model için varsayılan şemanın içine yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-902">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="2e0c8-903">Şema, Fluent API `modelBuilder.HasDefaultSchema()` aracılığıyla açıkça yapılandırılabilir veya `dbo` Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-903">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="2e0c8-904">**Kaydol**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-904">**Why**</span></span>

<span data-ttu-id="2e0c8-905">Daha önceden şemanın boş olması, işlevin yerleşik olduğunu değerlendirmek için bir yoldur, ancak bu mantık yalnızca yerleşik işlevlerin herhangi bir şemaya ait olmadığı SqlServer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-905">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="2e0c8-906">**Karşı**</span><span class="sxs-lookup"><span data-stu-id="2e0c8-906">**Mitigations**</span></span>

<span data-ttu-id="2e0c8-907">DbFunction 'ın çevirisini yerleşik bir işlevle eşlemek için el ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2e0c8-907">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
