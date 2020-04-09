---
title: EF Core 3.0'daki son dakika değişiklikleri - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417463"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="231e1-102">EF Core 3.0'da yer alan son kesme değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="231e1-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="231e1-103">Aşağıdaki API ve davranış değişiklikleri, varolan uygulamaları 3.0.0'a yükselterken kırma potansiyeline sahiptir.</span><span class="sxs-lookup"><span data-stu-id="231e1-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="231e1-104">Yalnızca veritabanı sağlayıcılarını etkilemesini beklediğimiz [değişiklikler sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenir.</span><span class="sxs-lookup"><span data-stu-id="231e1-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="231e1-105">Özet</span><span class="sxs-lookup"><span data-stu-id="231e1-105">Summary</span></span>

| <span data-ttu-id="231e1-106">**Son dakika değişikliği**</span><span class="sxs-lookup"><span data-stu-id="231e1-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="231e1-107">**Etki**</span><span class="sxs-lookup"><span data-stu-id="231e1-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="231e1-108">LINQ sorguları artık istemciüzerinde değerlendirilemez</span><span class="sxs-lookup"><span data-stu-id="231e1-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="231e1-109">Yüksek</span><span class="sxs-lookup"><span data-stu-id="231e1-109">High</span></span>       |
| [<span data-ttu-id="231e1-110">EF Core 3.0 hedefleri .NET Standart 2.1 yerine .NET Standart 2.0</span><span class="sxs-lookup"><span data-stu-id="231e1-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="231e1-111">Yüksek</span><span class="sxs-lookup"><span data-stu-id="231e1-111">High</span></span>      |
| [<span data-ttu-id="231e1-112">EF Core komut satırı aracı, dotnet ef, artık .NET Core SDK'nın bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="231e1-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="231e1-113">Yüksek</span><span class="sxs-lookup"><span data-stu-id="231e1-113">High</span></span>      |
| [<span data-ttu-id="231e1-114">DetectChanges, mağaza tarafından oluşturulan anahtar değerleri onurlandırıyor</span><span class="sxs-lookup"><span data-stu-id="231e1-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="231e1-115">Yüksek</span><span class="sxs-lookup"><span data-stu-id="231e1-115">High</span></span>      |
| [<span data-ttu-id="231e1-116">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="231e1-117">Yüksek</span><span class="sxs-lookup"><span data-stu-id="231e1-117">High</span></span>      |
| [<span data-ttu-id="231e1-118">Sorgu türleri varlık türleri ile birleştirilir</span><span class="sxs-lookup"><span data-stu-id="231e1-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="231e1-119">Yüksek</span><span class="sxs-lookup"><span data-stu-id="231e1-119">High</span></span>      |
| [<span data-ttu-id="231e1-120">Entity Framework Core artık ASP.NET Core paylaşılan çerçevesinin bir parçası değildir</span><span class="sxs-lookup"><span data-stu-id="231e1-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="231e1-121">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-121">Medium</span></span>      |
| [<span data-ttu-id="231e1-122">Basamaklı silme ler artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="231e1-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="231e1-123">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-123">Medium</span></span>      |
| [<span data-ttu-id="231e1-124">İlgili varlıkların hevesle yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="231e1-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="231e1-125">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-125">Medium</span></span>      |
| [<span data-ttu-id="231e1-126">DeleteBehavior.Restrict temiz semantik var</span><span class="sxs-lookup"><span data-stu-id="231e1-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="231e1-127">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-127">Medium</span></span>      |
| [<span data-ttu-id="231e1-128">Sahip olunan tür ilişkileri için yapılandırma API'sı değişti</span><span class="sxs-lookup"><span data-stu-id="231e1-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="231e1-129">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-129">Medium</span></span>      |
| [<span data-ttu-id="231e1-130">Her özellik, bağımsız bellek tümseci anahtar oluşturma</span><span class="sxs-lookup"><span data-stu-id="231e1-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="231e1-131">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-131">Medium</span></span>      |
| [<span data-ttu-id="231e1-132">Sorguları izlememe artık kimlik çözümlemesi gerçekleştir</span><span class="sxs-lookup"><span data-stu-id="231e1-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="231e1-133">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-133">Medium</span></span>      |
| [<span data-ttu-id="231e1-134">Meta veri API değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="231e1-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="231e1-135">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-135">Medium</span></span>      |
| [<span data-ttu-id="231e1-136">Sağlayıcıya özel Meta veri API değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="231e1-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="231e1-137">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-137">Medium</span></span>      |
| [<span data-ttu-id="231e1-138">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="231e1-139">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-139">Medium</span></span>      |
| [<span data-ttu-id="231e1-140">FromSql yöntemi depolanan yordam ile kullanıldığında oluşturulamıyor</span><span class="sxs-lookup"><span data-stu-id="231e1-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="231e1-141">Orta</span><span class="sxs-lookup"><span data-stu-id="231e1-141">Medium</span></span>      |
| [<span data-ttu-id="231e1-142">FromSql yöntemleri yalnızca sorgu köklerinde belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="231e1-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="231e1-143">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-143">Low</span></span>      |
| [<span data-ttu-id="231e1-144">~~Sorgu yürütme Hata Ayıklama düzeyinde günlüğe kaydedilir~~ Döndürülür</span><span class="sxs-lookup"><span data-stu-id="231e1-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="231e1-145">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-145">Low</span></span>      |
| [<span data-ttu-id="231e1-146">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="231e1-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="231e1-147">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-147">Low</span></span>      |
| [<span data-ttu-id="231e1-148">Tabloyu anaparayla paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="231e1-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="231e1-149">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-149">Low</span></span>      |
| [<span data-ttu-id="231e1-150">Eşzamanlılık belirteç sütunu olan bir tabloyu paylaşan tüm varlıklar tabloyu bir özellik ile eşlene</span><span class="sxs-lookup"><span data-stu-id="231e1-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="231e1-151">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-151">Low</span></span>      |
| [<span data-ttu-id="231e1-152">Sahibi izleme sorgusu kullanmadan sahip olunan varlıklar sorgulanamaz</span><span class="sxs-lookup"><span data-stu-id="231e1-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="231e1-153">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-153">Low</span></span>      |
| [<span data-ttu-id="231e1-154">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütuna eşlenir</span><span class="sxs-lookup"><span data-stu-id="231e1-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="231e1-155">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-155">Low</span></span>      |
| [<span data-ttu-id="231e1-156">Yabancı anahtar mülkiyet sözleşmesi artık ana özellik ile aynı adla eşleşmiş</span><span class="sxs-lookup"><span data-stu-id="231e1-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="231e1-157">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-157">Low</span></span>      |
| [<span data-ttu-id="231e1-158">İşlemKapsamı tamamlanmadan önce artık kullanılmazsa veritabanı bağlantısı artık kapatılır</span><span class="sxs-lookup"><span data-stu-id="231e1-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="231e1-159">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-159">Low</span></span>      |
| [<span data-ttu-id="231e1-160">Destek alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="231e1-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="231e1-161">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-161">Low</span></span>      |
| [<span data-ttu-id="231e1-162">Birden çok uyumlu destek alanı bulunursa at</span><span class="sxs-lookup"><span data-stu-id="231e1-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="231e1-163">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-163">Low</span></span>      |
| [<span data-ttu-id="231e1-164">Yalnızca alan özelliği adları alan adı ile eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="231e1-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="231e1-165">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-165">Low</span></span>      |
| [<span data-ttu-id="231e1-166">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache arama</span><span class="sxs-lookup"><span data-stu-id="231e1-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="231e1-167">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-167">Low</span></span>      |
| [<span data-ttu-id="231e1-168">AddEntityFramework\* boyut sınırıyla IMemoryCache ekler</span><span class="sxs-lookup"><span data-stu-id="231e1-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="231e1-169">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-169">Low</span></span>      |
| [<span data-ttu-id="231e1-170">DbContext.Entry şimdi yerel bir DetectChanges gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="231e1-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="231e1-171">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-171">Low</span></span>      |
| [<span data-ttu-id="231e1-172">String ve bayt dizi anahtarları varsayılan olarak istemci tarafından oluşturulmaz</span><span class="sxs-lookup"><span data-stu-id="231e1-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="231e1-173">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-173">Low</span></span>      |
| [<span data-ttu-id="231e1-174">ILoggerFactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="231e1-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="231e1-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-175">Low</span></span>      |
| [<span data-ttu-id="231e1-176">Tembel yükleme lisi artık navigasyon özelliklerinin tamamen yüklendiğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="231e1-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="231e1-177">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-177">Low</span></span>      |
| [<span data-ttu-id="231e1-178">Dahili hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="231e1-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="231e1-179">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-179">Low</span></span>      |
| [<span data-ttu-id="231e1-180">HasOne/HasMany için yeni davranış tek bir dize ile çağrıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="231e1-181">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-181">Low</span></span>      |
| [<span data-ttu-id="231e1-182">Birkaç async yönteminin dönüş türü Görevden ValueTask'a değiştirildi</span><span class="sxs-lookup"><span data-stu-id="231e1-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="231e1-183">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-183">Low</span></span>      |
| [<span data-ttu-id="231e1-184">İlişkisel: TypeMapping ek açıklama şimdi sadece TypeMapping olduğunu</span><span class="sxs-lookup"><span data-stu-id="231e1-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="231e1-185">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-185">Low</span></span>      |
| [<span data-ttu-id="231e1-186">Türetilmiş bir türde ToTable bir özel durum atar</span><span class="sxs-lookup"><span data-stu-id="231e1-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="231e1-187">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-187">Low</span></span>      |
| [<span data-ttu-id="231e1-188">EF Core artık SQLite FK uygulama için pragma gönderir</span><span class="sxs-lookup"><span data-stu-id="231e1-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="231e1-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-189">Low</span></span>      |
| [<span data-ttu-id="231e1-190">Microsoft.EntityFrameworkCore.Sqlite şimdi SQLitePCLRaw.bundle_e_sqlite3 bağlıdır</span><span class="sxs-lookup"><span data-stu-id="231e1-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="231e1-191">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-191">Low</span></span>      |
| [<span data-ttu-id="231e1-192">Kılavuz değerleri artık SQLite'da METİn olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="231e1-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="231e1-193">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-193">Low</span></span>      |
| [<span data-ttu-id="231e1-194">Char değerleri artık SQLite'da TEXT olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="231e1-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="231e1-195">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-195">Low</span></span>      |
| [<span data-ttu-id="231e1-196">Geçiş disleri artık değişmez kültürün takvimi kullanılarak oluşturulur</span><span class="sxs-lookup"><span data-stu-id="231e1-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="231e1-197">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-197">Low</span></span>      |
| [<span data-ttu-id="231e1-198">Uzantı bilgileri/meta veriler IDbContextOptionsExtension kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="231e1-199">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-199">Low</span></span>      |
| [<span data-ttu-id="231e1-200">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="231e1-201">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-201">Low</span></span>      |
| [<span data-ttu-id="231e1-202">Yabancı anahtar kısıtlama adları için API'yi netleştir</span><span class="sxs-lookup"><span data-stu-id="231e1-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="231e1-203">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-203">Low</span></span>      |
| [<span data-ttu-id="231e1-204">iRelationalDatabaseCreator.HasTables/HasTablesAsync genel kullanıma açıklanmıştır</span><span class="sxs-lookup"><span data-stu-id="231e1-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="231e1-205">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-205">Low</span></span>      |
| [<span data-ttu-id="231e1-206">Microsoft.EntityFrameworkCore.Design artık bir DevelopmentDependency paketidir</span><span class="sxs-lookup"><span data-stu-id="231e1-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="231e1-207">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-207">Low</span></span>      |
| [<span data-ttu-id="231e1-208">SQLitePCL.raw sürüm 2.0.0 güncellendi</span><span class="sxs-lookup"><span data-stu-id="231e1-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="231e1-209">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-209">Low</span></span>      |
| [<span data-ttu-id="231e1-210">NetTopologySuite sürüm 2.0.0 güncellendi</span><span class="sxs-lookup"><span data-stu-id="231e1-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="231e1-211">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-211">Low</span></span>      |
| [<span data-ttu-id="231e1-212">System.Data.SqlClient yerine Microsoft.Data.SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="231e1-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="231e1-213">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-213">Low</span></span>      |
| [<span data-ttu-id="231e1-214">Birden çok belirsiz kendi kendine başvuran ilişkiler yapılandırılmalıdır</span><span class="sxs-lookup"><span data-stu-id="231e1-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="231e1-215">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-215">Low</span></span>      |
| [<span data-ttu-id="231e1-216">DbFunction.Schema null veya boş dize olmak modelin varsayılan şema olarak yapılandırır</span><span class="sxs-lookup"><span data-stu-id="231e1-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="231e1-217">Düşük</span><span class="sxs-lookup"><span data-stu-id="231e1-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="231e1-218">LINQ sorguları artık istemciüzerinde değerlendirilemez</span><span class="sxs-lookup"><span data-stu-id="231e1-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="231e1-219">[İzleme Sorunu #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Ayrıca #12795 sorunu da görme](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="231e1-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="231e1-220">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-220">**Old behavior**</span></span>

<span data-ttu-id="231e1-221">3.0'dan önce, EF Core sorgunun parçası olan bir ifadeyi SQL veya parametreye dönüştüremediğinde, istemcideki ifadeyi otomatik olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="231e1-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="231e1-222">Varsayılan olarak, olası pahalı ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.</span><span class="sxs-lookup"><span data-stu-id="231e1-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="231e1-223">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-223">**New behavior**</span></span>

<span data-ttu-id="231e1-224">3.0 ile başlayan EF Core, yalnızca üst düzey projeksiyondaki `Select()` (sorgudaki son çağrı) ifadelerin istemci üzerinde değerlendirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="231e1-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="231e1-225">Sorgunun başka bir bölümündeki ifadeler SQL veya parametreye dönüştürülemediğinde bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="231e1-226">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-226">**Why**</span></span>

<span data-ttu-id="231e1-227">Sorguların otomatik istemci değerlendirmesi, önemli bölümleri çevrilemese bile birçok sorgunun yürütülmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="231e1-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="231e1-228">Bu davranış, beklenmeyen ve zarar verici davranışlara neden olabilir ve bu davranış yalnızca üretimde belirginleşebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="231e1-229">Örneğin, çevrilmeyen `Where()` bir aramadaki bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve filtrenin istemciye uygulanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="231e1-230">Tablo geliştirme aşamasında yalnızca birkaç satır içeriyorsa, ancak uygulama üretime geçtiğinde tablo milyonlarca satır içerebileceği bu durum kolayca algılanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="231e1-231">İstemci değerlendirme uyarıları da geliştirme sırasında göz ardı etmek çok kolay olduğunu kanıtladı.</span><span class="sxs-lookup"><span data-stu-id="231e1-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="231e1-232">Bunun yanı sıra, otomatik istemci değerlendirmesi, belirli ifadeler için sorgu çevirisinin iyileştirilmesinin sürümler arasında istenmeyen kırılma değişikliklerine neden olduğu sorunlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="231e1-233">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-233">**Mitigations**</span></span>

<span data-ttu-id="231e1-234">Bir sorgu tam olarak çevrilemezse, sorguyu çevrilebilecek bir biçimde yeniden yazın `AsEnumerable()`veya `ToList()`linq-to-Objects kullanılarak işlenebilir şekilde istemciye açıkça veri getirmek için benzer bir şekilde kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="231e1-235">EF Core 3.0 hedefleri .NET Standart 2.1 yerine .NET Standart 2.0</span><span class="sxs-lookup"><span data-stu-id="231e1-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="231e1-236">İzleme Sorunu #15498</span><span class="sxs-lookup"><span data-stu-id="231e1-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="231e1-237">EF Core 3.1 hedefleri .NET Standart 2.0 tekrar.</span><span class="sxs-lookup"><span data-stu-id="231e1-237">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="231e1-238">Bu da .NET Framework için desteği geri getirir.</span><span class="sxs-lookup"><span data-stu-id="231e1-238">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="231e1-239">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-239">**Old behavior**</span></span>

<span data-ttu-id="231e1-240">3.0'dan önce EF Core .NET Standard 2.0'ı hedefledi ve .NET Framework de dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktı.</span><span class="sxs-lookup"><span data-stu-id="231e1-240">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="231e1-241">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-241">**New behavior**</span></span>

<span data-ttu-id="231e1-242">3.0 ile başlayan EF Core, .NET Standard 2.1'i hedefler ve bu standardı destekleyen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="231e1-242">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="231e1-243">Bu ,NET Framework içermez.</span><span class="sxs-lookup"><span data-stu-id="231e1-243">This does not include .NET Framework.</span></span>

<span data-ttu-id="231e1-244">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-244">**Why**</span></span>

<span data-ttu-id="231e1-245">Bu, .NET teknolojileri arasında enerjiyi .NET Core ve Xamarin gibi diğer modern .NET platformlarına odaklama kararının bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-245">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="231e1-246">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-246">**Mitigations**</span></span>

<span data-ttu-id="231e1-247">EF Core 3.1 kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-247">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="231e1-248">Entity Framework Core artık ASP.NET Core paylaşılan çerçevesinin bir parçası değildir</span><span class="sxs-lookup"><span data-stu-id="231e1-248">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="231e1-249">Takip Sorunu Duyurular#325</span><span class="sxs-lookup"><span data-stu-id="231e1-249">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="231e1-250">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-250">**Old behavior**</span></span>

<span data-ttu-id="231e1-251">Core 3.0ASP.NET önce, bir paket `Microsoft.AspNetCore.App` başvurusu `Microsoft.AspNetCore.All`eklediğinizde veya , EF Core ve SQL Server sağlayıcısı gibi EF Core veri sağlayıcılarının bazılarını içerir.</span><span class="sxs-lookup"><span data-stu-id="231e1-251">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="231e1-252">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-252">**New behavior**</span></span>

<span data-ttu-id="231e1-253">3.0'dan itibaren, ASP.NET Core paylaşılan çerçevesi EF Core veya herhangi bir EF Core veri sağlayıcısını içermez.</span><span class="sxs-lookup"><span data-stu-id="231e1-253">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="231e1-254">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-254">**Why**</span></span>

<span data-ttu-id="231e1-255">Bu değişiklikten önce, EF Core almak, uygulamanın Core ve SQL Server ASP.NET hedeflenip hedeflemediğine bağlı olarak farklı adımlar gerektir.</span><span class="sxs-lookup"><span data-stu-id="231e1-255">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="231e1-256">Ayrıca, ASP.NET Core yükseltme her zaman arzu değildir EF Core ve SQL Server sağlayıcısı, yükseltme zorladı.</span><span class="sxs-lookup"><span data-stu-id="231e1-256">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="231e1-257">Bu değişiklikle, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-257">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="231e1-258">Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının ne zaman yükseltilmelerini de tam olarak denetleyebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-258">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="231e1-259">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-259">**Mitigations**</span></span>

<span data-ttu-id="231e1-260">EF Core'u ASP.NET Core 3.0 uygulamasında veya desteklenen başka bir uygulamada kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="231e1-260">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="231e1-261">EF Core komut satırı aracı, dotnet ef, artık .NET Core SDK'nın bir parçası değil</span><span class="sxs-lookup"><span data-stu-id="231e1-261">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="231e1-262">İzleme Sorunu #14016</span><span class="sxs-lookup"><span data-stu-id="231e1-262">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="231e1-263">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-263">**Old behavior**</span></span>

<span data-ttu-id="231e1-264">3.0'dan `dotnet ef` önce, araç .NET Core SDK'ya dahil edildi ve ek adımlar gerektirmeden herhangi bir projeden komut satırından kullanıma hazır dı.</span><span class="sxs-lookup"><span data-stu-id="231e1-264">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="231e1-265">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-265">**New behavior**</span></span>

<span data-ttu-id="231e1-266">3.0'dan başlayarak ,NET SDK `dotnet ef` aracı içermez, bu nedenle kullanmadan önce aracı açıkça yerel veya genel bir araç olarak yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-266">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="231e1-267">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-267">**Why**</span></span>

<span data-ttu-id="231e1-268">Bu değişiklik, EF Core `dotnet ef` 3.0'ın her zaman Bir NuGet paketi olarak dağıtılması yla tutarlı olarak NuGet'de düzenli bir .NET CLI aracı olarak dağıtmamıza ve güncellememize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="231e1-268">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="231e1-269">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-269">**Mitigations**</span></span>

<span data-ttu-id="231e1-270">Geçişleri veya iskeleyi `DbContext`yönetebilmek için `dotnet-ef` genel bir araç olarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="231e1-270">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="231e1-271">Ayrıca, bir [araç bildirimi dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak bir araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde, bunu yerel bir araç da elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="231e1-271">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="231e1-272">FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-272">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="231e1-273">İzleme Sorunu #10996</span><span class="sxs-lookup"><span data-stu-id="231e1-273">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="231e1-274">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-274">**Old behavior**</span></span>

<span data-ttu-id="231e1-275">EF Core 3.0'dan önce, bu yöntem adları normal bir dize yle veya SQL ve parametrelere enterpolasyon yapılması gereken bir dizeyle çalışmak üzere aşırı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="231e1-275">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="231e1-276">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-276">**New behavior**</span></span>

<span data-ttu-id="231e1-277">EF Core 3.0 ile `FromSqlRaw` `ExecuteSqlRaw`başlayarak, parametrelerin sorgu dizesinden ayrı olarak geçirildiği parametreli bir sorgu kullanın. `ExecuteSqlRawAsync`</span><span class="sxs-lookup"><span data-stu-id="231e1-277">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="231e1-278">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-278">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="231e1-279">Ve `FromSqlInterpolated` `ExecuteSqlInterpolated`parametrelerin `ExecuteSqlInterpolatedAsync` enterpolasyonlu sorgu dizesinin bir parçası olarak geçtiği parametreli bir sorgu oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="231e1-280">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-280">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="231e1-281">Yukarıdaki sorguların her ikisinin de aynı SQL parametreleri ile aynı parametreli SQL üreteceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="231e1-281">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="231e1-282">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-282">**Why**</span></span>

<span data-ttu-id="231e1-283">Bu gibi yöntem aşırı yüklemeleri, amaç enterpolasyonlu dize yöntemini aramak ken, ham dize yöntemini yanlışlıkla aramayı çok kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="231e1-283">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="231e1-284">Bu, sorguların olması gerekirken parametrelendirilemelerine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-284">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="231e1-285">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-285">**Mitigations**</span></span>

<span data-ttu-id="231e1-286">Yeni yöntem adlarını kullanmak için geçin.</span><span class="sxs-lookup"><span data-stu-id="231e1-286">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="231e1-287">FromSql yöntemi depolanan yordam ile kullanıldığında oluşturulamıyor</span><span class="sxs-lookup"><span data-stu-id="231e1-287">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="231e1-288">İzleme Sorunu #15392</span><span class="sxs-lookup"><span data-stu-id="231e1-288">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="231e1-289">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-289">**Old behavior**</span></span>

<span data-ttu-id="231e1-290">EF Core 3.0'dan önce FromSql yöntemi, geçirilen SQL'in üzerine besteleilip oluşturulamaya çalışılsın.</span><span class="sxs-lookup"><span data-stu-id="231e1-290">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="231e1-291">SQL depolanan bir yordam gibi tek birleştirilebilir olmadığında istemci değerlendirmesi yaptı.</span><span class="sxs-lookup"><span data-stu-id="231e1-291">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="231e1-292">Aşağıdaki sorgu, depolanan yordamı sunucuda çalıştırarak ve istemci tarafında FirstOrDefault yaparak çalıştı.</span><span class="sxs-lookup"><span data-stu-id="231e1-292">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="231e1-293">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-293">**New behavior**</span></span>

<span data-ttu-id="231e1-294">EF Core 3.0 ile başlayarak, EF Core SQL ayrıştırmaya çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="231e1-294">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="231e1-295">Yani FromSqlRaw/FromSqlInterpolated'den sonra beste yapacaksanız, EF Core alt sorguya neden olarak SQL'i oluşturur.</span><span class="sxs-lookup"><span data-stu-id="231e1-295">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="231e1-296">Bu nedenle, kompozisyonlu depolanmış bir yordam kullanıyorsanız, geçersiz SQL sözdizimi için bir özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="231e1-296">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="231e1-297">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-297">**Why**</span></span>

<span data-ttu-id="231e1-298">EF Core 3.0 otomatik istemci değerlendirmesini desteklemez, çünkü [burada](#linq-queries-are-no-longer-evaluated-on-the-client)açıklandığı gibi hataya yatkındır.</span><span class="sxs-lookup"><span data-stu-id="231e1-298">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="231e1-299">**Risk azaltma**</span><span class="sxs-lookup"><span data-stu-id="231e1-299">**Mitigation**</span></span>

<span data-ttu-id="231e1-300">FromSqlRaw/FromSqlInterpolated'de depolanmış bir yordam kullanıyorsanız, bunun üzerine oluşturulamayacağını biliyorsunuz, bu nedenle sunucu tarafında herhangi bir kompozisyonu önlemek için FromSql yöntemi çağrısından hemen sonra __AsEnumerable/AsAsyncEnumerable'ı__ ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="231e1-300">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="231e1-301">FromSql yöntemleri yalnızca sorgu köklerinde belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="231e1-301">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="231e1-302">İzleme Sorunu #15704</span><span class="sxs-lookup"><span data-stu-id="231e1-302">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="231e1-303">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-303">**Old behavior**</span></span>

<span data-ttu-id="231e1-304">EF Core 3.0'dan `FromSql` önce, yöntem sorgunun herhangi bir yerinde belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-304">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="231e1-305">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-305">**New behavior**</span></span>

<span data-ttu-id="231e1-306">EF Core 3.0 ile `FromSqlRaw` başlayarak, yeni ve `FromSqlInterpolated` yöntemler (yerine) `FromSql`sadece sorgu kökleri, `DbSet<>`yani doğrudan belirtilebilir .</span><span class="sxs-lookup"><span data-stu-id="231e1-306">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="231e1-307">Bunları başka bir yerde belirtmeye çalışmak bir derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="231e1-307">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="231e1-308">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-308">**Why**</span></span>

<span data-ttu-id="231e1-309">Başka `FromSql` bir `DbSet` yerde belirtmenin hiçbir anlamı veya katma değeri yoktur ve belirli senaryolarda belirsizliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-309">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="231e1-310">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-310">**Mitigations**</span></span>

<span data-ttu-id="231e1-311">`FromSql`çağrıları doğrudan başvurdukları yer `DbSet` olacak şekilde taşınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-311">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="231e1-312">Sorguları izlememe artık kimlik çözümlemesi gerçekleştir</span><span class="sxs-lookup"><span data-stu-id="231e1-312">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="231e1-313">İzleme Sorunu #13518</span><span class="sxs-lookup"><span data-stu-id="231e1-313">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="231e1-314">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-314">**Old behavior**</span></span>

<span data-ttu-id="231e1-315">EF Core 3.0'dan önce, belirli bir tür ve kimlikle bir varlığın her oluşumu için aynı varlık örneği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-315">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="231e1-316">Bu, sorguları izleme davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="231e1-316">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="231e1-317">Örneğin, bu sorgu:</span><span class="sxs-lookup"><span data-stu-id="231e1-317">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="231e1-318">verilen kategoriyle `Category` ilişkili `Product` her biri için aynı örneği döndürecek.</span><span class="sxs-lookup"><span data-stu-id="231e1-318">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="231e1-319">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-319">**New behavior**</span></span>

<span data-ttu-id="231e1-320">EF Core 3.0 ile başlayarak, belirli bir türe ve kimliğine sahip bir varlık döndürülen grafikte farklı yerlerde karşılaşıldığında farklı varlık örnekleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="231e1-320">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="231e1-321">Örneğin, iki ürün aynı kategoriyle `Category` ilişkilendirilse bile, yukarıdaki sorgu artık her biri `Product` için yeni bir örnek döndürecektir.</span><span class="sxs-lookup"><span data-stu-id="231e1-321">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="231e1-322">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-322">**Why**</span></span>

<span data-ttu-id="231e1-323">Kimlik çözümlemesi (diğer bir şekilde, bir varlığın daha önce karşılaşılan bir varlıkla aynı tür ve kimliğe sahip olduğunu belirleme) ek performans ve bellek ek yükü ekler.</span><span class="sxs-lookup"><span data-stu-id="231e1-323">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="231e1-324">Bu genellikle ilk etapta izleme sorgularının neden kullanıldığına ters çalışır.</span><span class="sxs-lookup"><span data-stu-id="231e1-324">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="231e1-325">Ayrıca, kimlik çözümlemesi bazen yararlı olabilir, ancak varlıklar seri hale getirilecek ve sorguları izlememek için yaygın olan bir istemciye gönderilecekse gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="231e1-325">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="231e1-326">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-326">**Mitigations**</span></span>

<span data-ttu-id="231e1-327">Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-327">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="231e1-328">~~Sorgu yürütme Hata Ayıklama düzeyinde günlüğe kaydedilir~~ Döndürülür</span><span class="sxs-lookup"><span data-stu-id="231e1-328">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="231e1-329">İzleme Sorunu #14523</span><span class="sxs-lookup"><span data-stu-id="231e1-329">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="231e1-330">EF Core 3.0'daki yeni yapılandırma, uygulama tarafından belirtilecek herhangi bir olayın günlük düzeyine izin verdiği için bu değişikliği geri aldık.</span><span class="sxs-lookup"><span data-stu-id="231e1-330">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="231e1-331">Örneğin, SQL'in günlüğe `Debug`kaydetmesini , açıkça `OnConfiguring` düzeyveya şu şekilde yapılandıracak şekilde değiştirmek için: `AddDbContext`</span><span class="sxs-lookup"><span data-stu-id="231e1-331">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="231e1-332">Geçici anahtar değerleri artık varlık örneklerine ayarlı değil</span><span class="sxs-lookup"><span data-stu-id="231e1-332">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="231e1-333">İzleme Sorunu #12378</span><span class="sxs-lookup"><span data-stu-id="231e1-333">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="231e1-334">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-334">**Old behavior**</span></span>

<span data-ttu-id="231e1-335">EF Core 3.0'dan önce, daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olacak tüm önemli özelliklere geçici değerler atanmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-335">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="231e1-336">Genellikle bu geçici değerler büyük negatif sayılardı.</span><span class="sxs-lookup"><span data-stu-id="231e1-336">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="231e1-337">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-337">**New behavior**</span></span>

<span data-ttu-id="231e1-338">3.0 ile başlayan EF Core, varlığın izleme bilgilerinin bir parçası olarak geçici anahtar değerini depolar ve anahtar özelliğin kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="231e1-338">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="231e1-339">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-339">**Why**</span></span>

<span data-ttu-id="231e1-340">Bu değişiklik, daha önce bir `DbContext` örnek tarafından izlenen bir varlık farklı `DbContext` bir örneğe taşındığında geçici anahtar değerlerin hatalı bir şekilde kalıcı olmasını önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-340">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="231e1-341">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-341">**Mitigations**</span></span>

<span data-ttu-id="231e1-342">Birincil anahtarlar varlıklar arasında ilişki kurmak için yabancı anahtarlara birincil anahtar değerleri atayabilen uygulamalar, birincil anahtarlar `Added` depoda oluşturulursa ve devletteki varlıklara aitse eski davranışa bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-342">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="231e1-343">Bu, şu lar tarafından önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="231e1-343">This can be avoided by:</span></span>
* <span data-ttu-id="231e1-344">Mağaza tarafından oluşturulan anahtarları kullanmamak.</span><span class="sxs-lookup"><span data-stu-id="231e1-344">Not using store-generated keys.</span></span>
* <span data-ttu-id="231e1-345">Gezinti özelliklerini yabancı anahtar değerlerini ayarlamak yerine ilişkiler oluşturacak şekilde ayarlama.</span><span class="sxs-lookup"><span data-stu-id="231e1-345">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="231e1-346">Varlığın izleme bilgilerinden gerçek geçici anahtar değerlerini edinin.</span><span class="sxs-lookup"><span data-stu-id="231e1-346">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="231e1-347">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` kendisi ayarlanmış olsa `blog.Id` bile geçici değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="231e1-347">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="231e1-348">DetectChanges, mağaza tarafından oluşturulan anahtar değerleri onurlandırıyor</span><span class="sxs-lookup"><span data-stu-id="231e1-348">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="231e1-349">İzleme Sorunu #14616</span><span class="sxs-lookup"><span data-stu-id="231e1-349">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="231e1-350">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-350">**Old behavior**</span></span>

<span data-ttu-id="231e1-351">EF Core 3.0'dan önce, izlenmemiş bir varlık `DetectChanges` `Added` tarafından bulunan bir varlık durumda `SaveChanges` izlenir ve çağrıldığında yeni bir satır olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="231e1-351">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="231e1-352">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-352">**New behavior**</span></span>

<span data-ttu-id="231e1-353">EF Core 3.0 ile başlayarak, bir varlık oluşturulan anahtar değerleri kullanıyorsa ve bazı `Modified` anahtar değeri ayarlanırsa, varlık durumda izlenir.</span><span class="sxs-lookup"><span data-stu-id="231e1-353">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="231e1-354">Bu, varlık için bir satırın var olduğu varsayılır `SaveChanges` ve çağrıldığında güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="231e1-354">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="231e1-355">Anahtar değeri ayarlanmıyorsa veya varlık türü oluşturulan anahtarları kullanmıyorsa, yeni varlık önceki sürümlerde `Added` olduğu gibi izlenir.</span><span class="sxs-lookup"><span data-stu-id="231e1-355">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="231e1-356">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-356">**Why**</span></span>

<span data-ttu-id="231e1-357">Bu değişiklik, mağaza tarafından oluşturulan anahtarları kullanırken bağlantısı kesilen varlık grafikleri ile çalışmayı daha kolay ve tutarlı hale getirmek için yapıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-357">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="231e1-358">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-358">**Mitigations**</span></span>

<span data-ttu-id="231e1-359">Bir varlık türü oluşturulan anahtarları kullanmak üzere yapılandırılır, ancak anahtar değerleri açıkça yeni örnekler için ayarlanırsa, bu değişiklik bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-359">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="231e1-360">Düzeltme, anahtar özelliklerini oluşturulan değerleri kullanmamak için açıkça yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-360">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="231e1-361">Örneğin, akıcı API ile:</span><span class="sxs-lookup"><span data-stu-id="231e1-361">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="231e1-362">Veya veri ek açıklamaları ile:</span><span class="sxs-lookup"><span data-stu-id="231e1-362">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="231e1-363">Basamaklı silme ler artık varsayılan olarak hemen gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="231e1-363">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="231e1-364">İzleme Sorunu #10114</span><span class="sxs-lookup"><span data-stu-id="231e1-364">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="231e1-365">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-365">**Old behavior**</span></span>

<span data-ttu-id="231e1-366">3.0'dan önce, EF Core basamaklı eylemler uyguladı (gerekli bir asıl silindiğinde veya gerekli bir anaparayla ilişki kesildiğinde bağımlı varlıkları silme) SaveChanges çağrılana kadar gerçekleşmedi.</span><span class="sxs-lookup"><span data-stu-id="231e1-366">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="231e1-367">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-367">**New behavior**</span></span>

<span data-ttu-id="231e1-368">3.0 ile başlayarak, EF Core tetikleme durumu algılanır algılanır algılanmadı basamaklı eylemler uygular.</span><span class="sxs-lookup"><span data-stu-id="231e1-368">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="231e1-369">Örneğin, asıl `context.Remove()` varlığı silmek için arama yapmak, izlenen ilgili tüm `Deleted` bağımlıların da hemen ayarlanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="231e1-369">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="231e1-370">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-370">**Why**</span></span>

<span data-ttu-id="231e1-371">Bu değişiklik, hangi varlıkların çağrılmadan _önce_ `SaveChanges` silineceğini anlamanın önemli olduğu veri bağlama ve denetleme senaryoları için deneyimi geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-371">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="231e1-372">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-372">**Mitigations**</span></span>

<span data-ttu-id="231e1-373">Önceki davranış, ''deki `context.ChangeTracker`ayarlar aracılığıyla geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-373">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="231e1-374">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-374">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="231e1-375">İlgili varlıkların hevesle yüklenmesi artık tek bir sorguda gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="231e1-375">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="231e1-376">İzleme sorunu #18022</span><span class="sxs-lookup"><span data-stu-id="231e1-376">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="231e1-377">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-377">**Old behavior**</span></span>

<span data-ttu-id="231e1-378">3.0'dan önce, operatörler `Include` aracılığıyla hevesle yüklenen koleksiyon gezintileri, ilişkili her varlık türü için bir tane olmak üzere ilişkisel veritabanında birden çok sorgu oluşturulmasına neden oldu.</span><span class="sxs-lookup"><span data-stu-id="231e1-378">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="231e1-379">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-379">**New behavior**</span></span>

<span data-ttu-id="231e1-380">3.0 ile başlayarak, EF Core ilişkisel veritabanlarında JO'larla tek bir sorgu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="231e1-380">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="231e1-381">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-381">**Why**</span></span>

<span data-ttu-id="231e1-382">Tek bir LINQ sorgusuuygulamak için birden çok sorgu verilmesi, birden çok veritabanı roundtrips gerekli olduğu gibi negatif performans da dahil olmak üzere çok sayıda soruna neden oldu ve her sorgu veritabanının farklı bir durumu gözlemlemek gibi veri tutarlılık sorunları.</span><span class="sxs-lookup"><span data-stu-id="231e1-382">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="231e1-383">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-383">**Mitigations**</span></span>

<span data-ttu-id="231e1-384">Teknik olarak bu bir kırılma değişikliği olmasa da, tek bir sorgu koleksiyon gezintilerinde çok `Include` sayıda işleç içerdiğinde uygulama performansı üzerinde önemli bir etkisi olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-384">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="231e1-385">Daha fazla bilgi ve sorguları daha verimli bir şekilde yeniden yazmak için [bu yoruma bakın.](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085)</span><span class="sxs-lookup"><span data-stu-id="231e1-385">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="231e1-386">DeleteBehavior.Restrict temiz semantik var</span><span class="sxs-lookup"><span data-stu-id="231e1-386">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="231e1-387">İzleme Sorunu #12661</span><span class="sxs-lookup"><span data-stu-id="231e1-387">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="231e1-388">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-388">**Old behavior**</span></span>

<span data-ttu-id="231e1-389">3.0'dan `DeleteBehavior.Restrict` önce, veritabanında `Restrict` anlamsal olarak yabancı anahtarlar oluşturuldu, ancak aynı zamanda iç düzeltmeyi de bariz olmayan bir şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="231e1-389">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="231e1-390">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-390">**New behavior**</span></span>

<span data-ttu-id="231e1-391">3.0 ile `DeleteBehavior.Restrict` başlayarak, yabancı anahtarların `Restrict` semantikle oluşturulmasını sağlar, yani basamaklar olmadan; ayrıca EF dahili fiksatlama etkilemeden kısıtlama ihlali atmak.</span><span class="sxs-lookup"><span data-stu-id="231e1-391">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="231e1-392">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-392">**Why**</span></span>

<span data-ttu-id="231e1-393">Bu değişiklik beklenmedik yan etkileri `DeleteBehavior` olmadan, sezgisel bir şekilde kullanma deneyimini geliştirmek için yapıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-393">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="231e1-394">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-394">**Mitigations**</span></span>

<span data-ttu-id="231e1-395">Önceki davranış kullanılarak `DeleteBehavior.ClientNoAction`geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-395">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="231e1-396">Sorgu türleri varlık türleri ile birleştirilir</span><span class="sxs-lookup"><span data-stu-id="231e1-396">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="231e1-397">İzleme Sorunu #14194</span><span class="sxs-lookup"><span data-stu-id="231e1-397">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="231e1-398">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-398">**Old behavior**</span></span>

<span data-ttu-id="231e1-399">EF Core 3.0'dan önce, [sorgu türleri](xref:core/modeling/keyless-entity-types) birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtı.</span><span class="sxs-lookup"><span data-stu-id="231e1-399">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="231e1-400">Diğer bir zamanda, anahtarolmadan varlık türlerini eşleme için bir sorgu türü kullanılırken (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) normal bir varlık türü kullanılırken (büyük olasılıkla bir tablodan, ancak büyük olasılıkla bir görünümden).</span><span class="sxs-lookup"><span data-stu-id="231e1-400">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="231e1-401">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-401">**New behavior**</span></span>

<span data-ttu-id="231e1-402">Sorgu türü artık birincil anahtarı olmayan bir varlık türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="231e1-402">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="231e1-403">Anahtarsız varlık türleri önceki sürümlerde sorgu türleri ile aynı işlevsellik vardır.</span><span class="sxs-lookup"><span data-stu-id="231e1-403">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="231e1-404">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-404">**Why**</span></span>

<span data-ttu-id="231e1-405">Bu değişiklik, sorgu türlerinin amacı etrafındaki karışıklığı azaltmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-405">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="231e1-406">Özellikle, bunlar anahtarsız varlık türleridir ve bu nedenle doğal olarak salt okunurlar, ancak yalnızca bir varlık türünün salt okunması gerektiğinden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-406">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="231e1-407">Aynı şekilde, genellikle görünümlere eşlenirler, ancak bunun tek nedeni görünümlerin genellikle anahtarları tanımlamamasıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-407">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="231e1-408">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-408">**Mitigations**</span></span>

<span data-ttu-id="231e1-409">API'nin aşağıdaki bölümleri artık geçersizdir:</span><span class="sxs-lookup"><span data-stu-id="231e1-409">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="231e1-410">**`ModelBuilder.Query<>()`**- `ModelBuilder.Entity<>().HasNoKey()` Bunun yerine hiçbir anahtarları olan bir varlık türü işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-410">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="231e1-411">Bu, birincil anahtar beklendiğinde yanlış yapılandırmayı önlemek için yine de kuralla yapılandırılamaz, ancak kuralı eşleştirmez.</span><span class="sxs-lookup"><span data-stu-id="231e1-411">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="231e1-412">**`DbQuery<>`**- `DbSet<>` Bunun yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-412">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="231e1-413">**`DbContext.Query<>()`**- `DbContext.Set<>()` Bunun yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-413">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="231e1-414">Sahip olunan tür ilişkileri için yapılandırma API'sı değişti</span><span class="sxs-lookup"><span data-stu-id="231e1-414">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="231e1-415">[İzleme Sorunu #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[İzleme Sorunu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[İzleme Sorunu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="231e1-415">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="231e1-416">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-416">**Old behavior**</span></span>

<span data-ttu-id="231e1-417">EF Core 3.0'dan önce, sahip olunan `OwnsOne` `OwnsMany` ilişkinin yapılandırması doğrudan çağrıdan sonra gerçekleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="231e1-417">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="231e1-418">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-418">**New behavior**</span></span>

<span data-ttu-id="231e1-419">EF Core 3.0 ile başlayarak, artık kullanarak sahibine bir navigasyon özelliği `WithOwner()`yapılandırmak için akıcı API vardır.</span><span class="sxs-lookup"><span data-stu-id="231e1-419">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="231e1-420">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-420">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="231e1-421">Sahibi ve sahibi arasındaki ilişki ile ilgili yapılandırma `WithOwner()` şimdi diğer ilişkilerin nasıl yapılandırıldığına benzer şekilde zincirlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-421">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="231e1-422">Sahip olunan tür kendisi için yapılandırma hala `OwnsOne()/OwnsMany()`sonra zincirlenmiş olsa da.</span><span class="sxs-lookup"><span data-stu-id="231e1-422">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="231e1-423">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-423">For example:</span></span>

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

<span data-ttu-id="231e1-424">Ayrıca, `Entity()`'veya `HasOne()` `Set()` sahip olunan bir tür hedef ile arama şimdi bir özel durum atacaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-424">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="231e1-425">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-425">**Why**</span></span>

<span data-ttu-id="231e1-426">Bu değişiklik, sahip olunan tür kendisi ve sahip olunan tür _ile ilişki_ arasında daha temiz bir ayrım oluşturmak için yapıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-426">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="231e1-427">Bu da gibi `HasForeignKey`yöntemler etrafında belirsizlik ve karışıklık kaldırır.</span><span class="sxs-lookup"><span data-stu-id="231e1-427">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="231e1-428">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-428">**Mitigations**</span></span>

<span data-ttu-id="231e1-429">Yukarıdaki örnekte gösterildiği gibi yeni API yüzeyini kullanmak için sahip olunan tür ilişkilerinin yapılandırmasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="231e1-429">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="231e1-430">Tabloyu anaparayla paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="231e1-430">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="231e1-431">İzleme Sorunu #9005</span><span class="sxs-lookup"><span data-stu-id="231e1-431">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="231e1-432">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-432">**Old behavior**</span></span>

<span data-ttu-id="231e1-433">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="231e1-433">Consider the following model:</span></span>
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
<span data-ttu-id="231e1-434">EF Core 3.0'dan önce, aynı tabloya ait yse `OrderDetails` `OrderDetails` `Order` `Order` veya açıkça aynı tabloya eşlenmişse, yeni bir tablo eklerken her zaman bir örnek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="231e1-435">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-435">**New behavior**</span></span>

<span data-ttu-id="231e1-436">3.0 ile başlayarak, EF `Order` Core `OrderDetails` bir olmadan eklemeye ve nullable sütunlar için birincil anahtar dışında tüm `OrderDetails` özellikleri eşler eklemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="231e1-436">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="231e1-437">EF Core `OrderDetails` sorgularken, `null` gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar dışında gerekli özellikleri `null`yoksa ve tüm özellikler .</span><span class="sxs-lookup"><span data-stu-id="231e1-437">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="231e1-438">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-438">**Mitigations**</span></span>

<span data-ttu-id="231e1-439">Modelinizin tüm isteğe bağlı sütunlara bağlı bir tablo paylaşımı varsa, `null` ancak buna işaret eden gezintinin olması `null`beklenmiyorsa, uygulama nın gezinti olduğunda servis taleplerini işlemek için değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-439">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="231e1-440">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir veya en`null` az bir özelliğin kendisine atanmış olmayan bir değeri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-440">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="231e1-441">Eşzamanlılık belirteç sütunu olan bir tabloyu paylaşan tüm varlıklar tabloyu bir özellik ile eşlene</span><span class="sxs-lookup"><span data-stu-id="231e1-441">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="231e1-442">İzleme Sorunu #14154</span><span class="sxs-lookup"><span data-stu-id="231e1-442">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="231e1-443">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-443">**Old behavior**</span></span>

<span data-ttu-id="231e1-444">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="231e1-444">Consider the following model:</span></span>
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
<span data-ttu-id="231e1-445">EF Core 3.0'dan önce, aynı tabloya `OrderDetails` ait `Order` veya açıkça `OrderDetails` eşlenmişse, güncelleştirme istemcideki değeri güncelleştirmez `Version` ve bir sonraki güncelleştirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="231e1-445">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="231e1-446">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-446">**New behavior**</span></span>

<span data-ttu-id="231e1-447">3.0 ile başlayarak, EF Core `Version` yeni `Order` değeri `OrderDetails`sahipse yayır.</span><span class="sxs-lookup"><span data-stu-id="231e1-447">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="231e1-448">Aksi takdirde model doğrulama sırasında bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-448">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="231e1-449">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-449">**Why**</span></span>

<span data-ttu-id="231e1-450">Bu değişiklik, aynı tabloya eşlenen varlıklardan yalnızca biri güncelleştirildiğinde, eski bir eşzamanlılık belirteci değerini önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-450">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="231e1-451">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-451">**Mitigations**</span></span>

<span data-ttu-id="231e1-452">Tabloyu paylaşan tüm varlıklar, eşzamanlılık belirteç sütununa eşlenen bir özellik içermelidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-452">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="231e1-453">Gölge durumunda bir tane oluşturmak mümkündür:</span><span class="sxs-lookup"><span data-stu-id="231e1-453">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="231e1-454">Sahibi izleme sorgusu kullanmadan sahip olunan varlıklar sorgulanamaz</span><span class="sxs-lookup"><span data-stu-id="231e1-454">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="231e1-455">İzleme Sorunu #18876</span><span class="sxs-lookup"><span data-stu-id="231e1-455">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="231e1-456">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-456">**Old behavior**</span></span>

<span data-ttu-id="231e1-457">EF Core 3.0'dan önce, sahip olunan varlıklar başka bir gezinme olarak sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-457">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="231e1-458">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-458">**New behavior**</span></span>

<span data-ttu-id="231e1-459">3.0 ile başlayarak, bir izleme sorgusu sahibi olmadan sahip olunan bir varlık projeleri eğer EF Core atar.</span><span class="sxs-lookup"><span data-stu-id="231e1-459">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="231e1-460">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-460">**Why**</span></span>

<span data-ttu-id="231e1-461">Sahip olunan varlıklar sahibi olmadan manipüle edilemez, bu nedenle vakaların büyük çoğunluğunda bu şekilde sorgulayan bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="231e1-461">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="231e1-462">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-462">**Mitigations**</span></span>

<span data-ttu-id="231e1-463">Sahip olunan varlığın daha sonra herhangi bir şekilde değiştirilecek şekilde izlenmesi gerekiyorsa, sahibi nin sorguya dahil edilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-463">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="231e1-464">Aksi takdirde `AsNoTracking()` bir çağrı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="231e1-464">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="231e1-465">Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütuna eşlenir</span><span class="sxs-lookup"><span data-stu-id="231e1-465">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="231e1-466">İzleme Sorunu #13998</span><span class="sxs-lookup"><span data-stu-id="231e1-466">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="231e1-467">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-467">**Old behavior**</span></span>

<span data-ttu-id="231e1-468">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="231e1-468">Consider the following model:</span></span>
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

<span data-ttu-id="231e1-469">EF Core 3.0'dan `ShippingAddress` önce, özellik sütunları `BulkOrder` varsayılan `Order` olarak ayırmak üzere eşlenir.</span><span class="sxs-lookup"><span data-stu-id="231e1-469">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="231e1-470">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-470">**New behavior**</span></span>

<span data-ttu-id="231e1-471">3.0 ile başlayarak, EF Core `ShippingAddress`için yalnızca bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="231e1-471">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="231e1-472">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-472">**Why**</span></span>

<span data-ttu-id="231e1-473">Eski behavoir beklenmedikti.</span><span class="sxs-lookup"><span data-stu-id="231e1-473">The old behavoir was unexpected.</span></span>

<span data-ttu-id="231e1-474">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-474">**Mitigations**</span></span>

<span data-ttu-id="231e1-475">Özellik, türemiş türler de ayrı sütuniçin açıkça eşlenebilir:</span><span class="sxs-lookup"><span data-stu-id="231e1-475">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="231e1-476">Yabancı anahtar mülkiyet sözleşmesi artık ana özellik ile aynı adla eşleşmiş</span><span class="sxs-lookup"><span data-stu-id="231e1-476">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="231e1-477">İzleme Sorunu #13274</span><span class="sxs-lookup"><span data-stu-id="231e1-477">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="231e1-478">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-478">**Old behavior**</span></span>

<span data-ttu-id="231e1-479">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="231e1-479">Consider the following model:</span></span>
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
<span data-ttu-id="231e1-480">EF Core 3.0'dan önce, `CustomerId` özellik sözleşmeye göre yabancı anahtar için kullanılacaktı.</span><span class="sxs-lookup"><span data-stu-id="231e1-480">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="231e1-481">Ancak, `Order` sahip olunan bir tür ise, o zaman bu da birincil anahtar yapmak `CustomerId` ve bu genellikle beklenti değildir.</span><span class="sxs-lookup"><span data-stu-id="231e1-481">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="231e1-482">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-482">**New behavior**</span></span>

<span data-ttu-id="231e1-483">3.0 ile başlayarak, EF Core ana özellik ile aynı ada sahipse, yabancı anahtarlar için özellikleri sözleşmeye göre kullanmaya çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="231e1-483">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="231e1-484">Ana özellik adı ile kaplanmış ana tür adı ve ana özellik adı desenleri ile kaplanmış gezinti adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-484">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="231e1-485">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-485">For example:</span></span>

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

<span data-ttu-id="231e1-486">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-486">**Why**</span></span>

<span data-ttu-id="231e1-487">Bu değişiklik, sahip olunan türde birincil anahtar özelliğihatalı bir şekilde tanımlamamak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-487">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="231e1-488">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-488">**Mitigations**</span></span>

<span data-ttu-id="231e1-489">Özellik yabancı anahtar ve dolayısıyla birincil anahtarın bir parçası olması amaçlandıysa, o zaman açıkça bu şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="231e1-489">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="231e1-490">İşlemKapsamı tamamlanmadan önce artık kullanılmazsa veritabanı bağlantısı artık kapatılır</span><span class="sxs-lookup"><span data-stu-id="231e1-490">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="231e1-491">İzleme Sorunu #14218</span><span class="sxs-lookup"><span data-stu-id="231e1-491">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="231e1-492">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-492">**Old behavior**</span></span>

<span data-ttu-id="231e1-493">EF Core 3.0'dan önce, bağlam `TransactionScope`içindeki bağlantıyı açarsa , `TransactionScope` akım etkinken bağlantı açık kalır.</span><span class="sxs-lookup"><span data-stu-id="231e1-493">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="231e1-494">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-494">**New behavior**</span></span>

<span data-ttu-id="231e1-495">3.0 ile başlayan EF Core, bağlantıyı kullanır kullanmaz kapatır.</span><span class="sxs-lookup"><span data-stu-id="231e1-495">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="231e1-496">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-496">**Why**</span></span>

<span data-ttu-id="231e1-497">Bu değişiklik aynı `TransactionScope`birden çok bağlamı kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="231e1-497">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="231e1-498">Yeni davranış da EF6 eşleşir.</span><span class="sxs-lookup"><span data-stu-id="231e1-498">The new behavior also matches EF6.</span></span>

<span data-ttu-id="231e1-499">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-499">**Mitigations**</span></span>

<span data-ttu-id="231e1-500">Bağlantının açık kalması gerekiyorsa, `OpenConnection()` EF Core'un bu çağrıyı zamanından önce kapatmamasını sağlayacaktır:</span><span class="sxs-lookup"><span data-stu-id="231e1-500">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="231e1-501">Her özellik, bağımsız bellek tümseci anahtar oluşturma</span><span class="sxs-lookup"><span data-stu-id="231e1-501">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="231e1-502">İzleme Sorunu #6872</span><span class="sxs-lookup"><span data-stu-id="231e1-502">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="231e1-503">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-503">**Old behavior**</span></span>

<span data-ttu-id="231e1-504">EF Core 3.0'dan önce, tüm bellek tüm inseda anahtar özellikleri için paylaşılan bir değer üreteci kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-504">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="231e1-505">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-505">**New behavior**</span></span>

<span data-ttu-id="231e1-506">EF Core 3.0 ile başlayarak, her bir tümsek anahtar özelliği bellek veritabanını kullanırken kendi değer üreteci alır.</span><span class="sxs-lookup"><span data-stu-id="231e1-506">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="231e1-507">Ayrıca, veritabanı silinirse, anahtar oluşturma tüm tablolar için sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="231e1-507">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="231e1-508">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-508">**Why**</span></span>

<span data-ttu-id="231e1-509">Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtar oluşturmayla daha yakından hizalamak ve bellek veritabanını kullanırken testleri birbirinden ayırabilme yeteneğini geliştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-509">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="231e1-510">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-510">**Mitigations**</span></span>

<span data-ttu-id="231e1-511">Bu, ayarlanacak belirli bellek içi anahtar değerlerine dayanan bir uygulamayı bozabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-511">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="231e1-512">Bunun yerine belirli anahtar değerlere güvenmemeyi veya yeni davranışla eşleşecek şekilde güncelleştirmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="231e1-512">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="231e1-513">Destek alanları varsayılan olarak kullanılır</span><span class="sxs-lookup"><span data-stu-id="231e1-513">Backing fields are used by default</span></span>

[<span data-ttu-id="231e1-514">İzleme Sorunu #12430</span><span class="sxs-lookup"><span data-stu-id="231e1-514">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="231e1-515">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-515">**Old behavior**</span></span>

<span data-ttu-id="231e1-516">3.0'dan önce, bir özelliğin destek alanı bilinse bile, EF Core yine de varsayılan olarak özellik getter ve ayarlayıcı yöntemlerini kullanarak özellik değerini okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="231e1-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="231e1-517">Bunun istisnası, destek alanının biliniyorsa doğrudan ayarlanacağı sorgu yürütmesiydi.</span><span class="sxs-lookup"><span data-stu-id="231e1-517">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="231e1-518">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-518">**New behavior**</span></span>

<span data-ttu-id="231e1-519">EF Core 3.0 ile başlayarak, bir özelliğin destek alanı biliniyorsa, EF Core bu özelliği her zaman destek alanını kullanarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="231e1-519">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="231e1-520">Uygulama, getter veya ayarlayıcı yöntemlerine kodlanmış ek davranışa dayanıyorsa, bu uygulama nın kırılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="231e1-521">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-521">**Why**</span></span>

<span data-ttu-id="231e1-522">Bu değişiklik, EF Core'un varlıkları içeren veritabanı işlemlerini gerçekleştirirken varsayılan olarak iş mantığını hatalı bir şekilde tetiklemesini önlemek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-522">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="231e1-523">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-523">**Mitigations**</span></span>

<span data-ttu-id="231e1-524">Pre-3.0 davranışı özelliği erişim modu yapılandırması ile `ModelBuilder`geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-524">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="231e1-525">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-525">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="231e1-526">Birden çok uyumlu destek alanı bulunursa at</span><span class="sxs-lookup"><span data-stu-id="231e1-526">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="231e1-527">İzleme Sorunu #12523</span><span class="sxs-lookup"><span data-stu-id="231e1-527">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="231e1-528">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-528">**Old behavior**</span></span>

<span data-ttu-id="231e1-529">EF Core 3.0'dan önce, birden çok alan bir özelliğin destek alanını bulma kurallarıyla eşleştiyse, bazı öncelik sırasına göre bir alan seçilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-529">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="231e1-530">Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-530">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="231e1-531">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-531">**New behavior**</span></span>

<span data-ttu-id="231e1-532">EF Core 3.0 ile başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-532">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="231e1-533">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-533">**Why**</span></span>

<span data-ttu-id="231e1-534">Bu değişiklik, yalnızca bir doğru olabilir başka bir alan üzerinde sessizce kullanmaktan kaçınmak için yapıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-534">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="231e1-535">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-535">**Mitigations**</span></span>

<span data-ttu-id="231e1-536">Belirsiz destek alanlarına sahip özellikler, açıkça kullanılacak alana sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-536">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="231e1-537">Örneğin, akıcı API kullanarak:</span><span class="sxs-lookup"><span data-stu-id="231e1-537">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="231e1-538">Yalnızca alan özelliği adları alan adı ile eşleşmelidir</span><span class="sxs-lookup"><span data-stu-id="231e1-538">Field-only property names should match the field name</span></span>

<span data-ttu-id="231e1-539">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-539">**Old behavior**</span></span>

<span data-ttu-id="231e1-540">EF Core 3.0'dan önce, bir özellik bir dize değeriyle belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunamazsa, EF Core bu özelliği kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="231e1-540">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

<span data-ttu-id="231e1-541">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-541">**New behavior**</span></span>

<span data-ttu-id="231e1-542">EF Core 3.0 ile başlayarak, yalnızca alan özelliği alan adı ile tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-542">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="231e1-543">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-543">**Why**</span></span>

<span data-ttu-id="231e1-544">Bu değişiklik, benzer adlı iki özellik için aynı alanı kullanmaktan kaçınmak için yapıldı, aynı zamanda clr özellikleri eşlenen özellikleri için aynı alan yalnızca özellikleri için eşleşen kurallar yapar.</span><span class="sxs-lookup"><span data-stu-id="231e1-544">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="231e1-545">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-545">**Mitigations**</span></span>

<span data-ttu-id="231e1-546">Yalnızca alan özellikleri, eşlendikleri alanla aynı adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-546">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="231e1-547">3.0'dan sonra EF Core'un gelecekteki sürümünde, özellik adından farklı bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307):](https://github.com/aspnet/EntityFrameworkCore/issues/15307)</span><span class="sxs-lookup"><span data-stu-id="231e1-547">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="231e1-548">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache arama</span><span class="sxs-lookup"><span data-stu-id="231e1-548">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="231e1-549">İzleme Sorunu #14756</span><span class="sxs-lookup"><span data-stu-id="231e1-549">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="231e1-550">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-550">**Old behavior**</span></span>

<span data-ttu-id="231e1-551">EF Core 3.0 `AddDbContext` önce, arama veya `AddDbContextPool` aynı zamanda AddLogging ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)aramaları aracılığıyla DI ile günlük ve bellek önbelleğe alma hizmetleri kaydeder. [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging)</span><span class="sxs-lookup"><span data-stu-id="231e1-551">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="231e1-552">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-552">**New behavior**</span></span>

<span data-ttu-id="231e1-553">EF Core 3.0 `AddDbContext` ile `AddDbContextPool` başlayarak ve artık Bağımlılık Enjeksiyon (DI) ile bu hizmetleri kayıt olacaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-553">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="231e1-554">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-554">**Why**</span></span>

<span data-ttu-id="231e1-555">EF Core 3.0, bu hizmetlerin uygulamanın DI kapsayıcısında olduğunu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="231e1-555">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="231e1-556">Ancak, `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlıysa, yine de EF Core tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-556">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="231e1-557">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-557">**Mitigations**</span></span>

<span data-ttu-id="231e1-558">Uygulamanızın bu hizmetlere ihtiyacı varsa, [bunları AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache'yi](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak DI kapsayıcısı ile açıkça kaydedin.</span><span class="sxs-lookup"><span data-stu-id="231e1-558">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="231e1-559">AddEntityFramework\* boyut sınırıyla IMemoryCache ekler</span><span class="sxs-lookup"><span data-stu-id="231e1-559">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="231e1-560">İzleme Sorunu #12905</span><span class="sxs-lookup"><span data-stu-id="231e1-560">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="231e1-561">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-561">**Old behavior**</span></span>

<span data-ttu-id="231e1-562">EF Core 3.0'dan önce, arama `AddEntityFramework*` yöntemleri de boyut sınırı olmadan DI bellek önbelleğe alma hizmetlerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="231e1-562">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="231e1-563">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-563">**New behavior**</span></span>

<span data-ttu-id="231e1-564">EF Core 3.0 `AddEntityFramework*` ile başlayarak, bir boyut sınırı ile bir IMemoryCache hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="231e1-564">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="231e1-565">Daha sonra eklenen diğer hizmetler IMemoryCache'ye bağlıysa, özel durumlara veya düşük performansa neden olan varsayılan sınıra hızla ulaşabilirler.</span><span class="sxs-lookup"><span data-stu-id="231e1-565">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="231e1-566">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-566">**Why**</span></span>

<span data-ttu-id="231e1-567">Sorgu önbelleğe alma mantığında bir hata varsa veya sorgular dinamik olarak oluşturulursa, iMemoryCache'nin sınır olmadan kullanılması denetimsiz bellek kullanımına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-567">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="231e1-568">Varsayılan sınıra sahip olmak olası bir DoS saldırısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="231e1-568">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="231e1-569">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-569">**Mitigations**</span></span>

<span data-ttu-id="231e1-570">Çoğu durumda `AddEntityFramework*` arama gerekli `AddDbContext` değildir `AddDbContextPool` veya de denir.</span><span class="sxs-lookup"><span data-stu-id="231e1-570">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="231e1-571">Bu nedenle, en iyi azaltma `AddEntityFramework*` aramayı kaldırmaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-571">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="231e1-572">Uygulamanızın bu hizmetlere ihtiyacı varsa, [AddMemoryCache'yi](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak önceden bir IMemoryCache uygulamasını açıkça DI kapsayıcısına kaydettirin.</span><span class="sxs-lookup"><span data-stu-id="231e1-572">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="231e1-573">DbContext.Entry şimdi yerel bir DetectChanges gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="231e1-573">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="231e1-574">İzleme Sorunu #13552</span><span class="sxs-lookup"><span data-stu-id="231e1-574">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="231e1-575">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-575">**Old behavior**</span></span>

<span data-ttu-id="231e1-576">EF Core 3.0'dan önce, arama `DbContext.Entry` tüm izlenen varlıklar için değişikliklerin algılanabına neden olur.</span><span class="sxs-lookup"><span data-stu-id="231e1-576">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="231e1-577">Bu da ortaya çıkan `EntityEntry` devletin güncel olmasını sağladı.</span><span class="sxs-lookup"><span data-stu-id="231e1-577">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="231e1-578">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-578">**New behavior**</span></span>

<span data-ttu-id="231e1-579">EF Core 3.0 ile `DbContext.Entry` başlayarak, arama artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="231e1-579">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="231e1-580">Bu, uygulama durumu üzerinde etkileri olabilir bu yöntem çağırarak başka bir değişiklik algılanmış olmayabilir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="231e1-580">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="231e1-581">`ChangeTracker.AutoDetectChangesEnabled` Daha `false` sonra bu yerel değişiklik algılama devre dışı bırakılacak şekilde ayarlanmışsa unutmayın.</span><span class="sxs-lookup"><span data-stu-id="231e1-581">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="231e1-582">Örneğin `ChangeTracker.Entries` ve `SaveChanges`yine de izlenen tüm varlıklarla `DetectChanges` dolu bir şekilde değişiklik algılamaya neden olan diğer yöntemler.</span><span class="sxs-lookup"><span data-stu-id="231e1-582">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="231e1-583">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-583">**Why**</span></span>

<span data-ttu-id="231e1-584">Bu değişiklik kullanarak `context.Entry`varsayılan performansını artırmak için yapıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-584">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="231e1-585">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-585">**Mitigations**</span></span>

<span data-ttu-id="231e1-586">Ön-3.0 `Entry` davranışını sağlamak için aramadan önce açıkça arayın. `ChangeTracker.DetectChanges()`</span><span class="sxs-lookup"><span data-stu-id="231e1-586">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="231e1-587">String ve bayt dizi anahtarları varsayılan olarak istemci tarafından oluşturulmaz</span><span class="sxs-lookup"><span data-stu-id="231e1-587">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="231e1-588">İzleme Sorunu #14617</span><span class="sxs-lookup"><span data-stu-id="231e1-588">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="231e1-589">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-589">**Old behavior**</span></span>

<span data-ttu-id="231e1-590">EF Core 3.0'dan önce `string` ve `byte[]` anahtar özellikleri açıkça null olmayan bir değer ayarlamadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-590">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="231e1-591">Böyle bir durumda, anahtar değeri bir GUID olarak istemci üzerinde oluşturulur, `byte[]`için bayt serileştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="231e1-591">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="231e1-592">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-592">**New behavior**</span></span>

<span data-ttu-id="231e1-593">EF Core 3.0 ile başlayarak anahtar değerinin ayarlandığını belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-593">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="231e1-594">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-594">**Why**</span></span>

<span data-ttu-id="231e1-595">İstemci tarafından oluşturulan `string` / `byte[]` değerler genellikle yararlı olmadığından ve varsayılan davranış, oluşturulan anahtar değerleri ortak bir şekilde ikna etmeyi zorlaştırdığından, bu değişiklik yapıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-595">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="231e1-596">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-596">**Mitigations**</span></span>

<span data-ttu-id="231e1-597">3.0 öncesi davranış, başka null olmayan değer ayarlanmazsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-597">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="231e1-598">Örneğin, akıcı API ile:</span><span class="sxs-lookup"><span data-stu-id="231e1-598">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="231e1-599">Veya veri ek açıklamaları ile:</span><span class="sxs-lookup"><span data-stu-id="231e1-599">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="231e1-600">ILoggerFactory artık kapsamlı bir hizmettir</span><span class="sxs-lookup"><span data-stu-id="231e1-600">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="231e1-601">İzleme Sorunu #14698</span><span class="sxs-lookup"><span data-stu-id="231e1-601">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="231e1-602">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-602">**Old behavior**</span></span>

<span data-ttu-id="231e1-603">ÖNCE EF Core 3.0, `ILoggerFactory` singleton hizmet olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="231e1-603">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="231e1-604">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-604">**New behavior**</span></span>

<span data-ttu-id="231e1-605">EF Core 3.0 `ILoggerFactory` ile başlayarak, şimdi kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-605">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="231e1-606">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-606">**Why**</span></span>

<span data-ttu-id="231e1-607">Bu değişiklik, bir logger'ın diğer `DbContext` işlevleri etkinleştiren ve dahili servis sağlayıcılarının patlaması gibi bazı patolojik davranış durumlarını ortadan kaldıran bir örnekle ilişkilendirmesine izin vermek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-607">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="231e1-608">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-608">**Mitigations**</span></span>

<span data-ttu-id="231e1-609">Bu değişiklik, EF Core dahili hizmet sağlayıcısında özel hizmetler kaydedilmedikçe ve kullanmadığı sürece uygulama kodunu etkilememelidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-609">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="231e1-610">Bu yaygın bir şey değil.</span><span class="sxs-lookup"><span data-stu-id="231e1-610">This isn't common.</span></span>
<span data-ttu-id="231e1-611">Bu gibi durumlarda, çoğu şey hala çalışacaktır, ancak bağlı `ILoggerFactory` olduğu herhangi bir singleton hizmet farklı bir şekilde elde `ILoggerFactory` etmek için değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-611">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="231e1-612">Bu gibi durumlarla karşı karşıya ysanız, lütfen [EF Core GitHub sorun izleyicisine](https://github.com/aspnet/EntityFrameworkCore/issues) bir `ILoggerFactory` sorun dosyalayın ve gelecekte bunu nasıl tekrar kıramayacağımızı daha iyi anlayabileceğimizi bize bildirin.</span><span class="sxs-lookup"><span data-stu-id="231e1-612">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="231e1-613">Tembel yükleme lisi artık navigasyon özelliklerinin tamamen yüklendiğini varsaymaz</span><span class="sxs-lookup"><span data-stu-id="231e1-613">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="231e1-614">İzleme Sorunu #12780</span><span class="sxs-lookup"><span data-stu-id="231e1-614">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="231e1-615">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-615">**Old behavior**</span></span>

<span data-ttu-id="231e1-616">EF Core 3.0'dan `DbContext` önce, a bir kez elden çıkarılmışsa, bu bağlamdan elde edilen bir varlık üzerindeki belirli bir navigasyon özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="231e1-616">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="231e1-617">Ekseksiler bunun yerine, null olmayan bir değeri varsa bir başvuru gezintisi yüklendiğini ve boş değilse koleksiyon gezintisi yüklendiğini varsayar.</span><span class="sxs-lookup"><span data-stu-id="231e1-617">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="231e1-618">Bu gibi durumlarda, tembel yükleme ye çalışmak bir no-op olacaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-618">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="231e1-619">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-619">**New behavior**</span></span>

<span data-ttu-id="231e1-620">EF Core 3.0 ile başlayarak, yakınlık lar bir gezinti özelliğinin yüklenip yüklenmediğini izler.</span><span class="sxs-lookup"><span data-stu-id="231e1-620">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="231e1-621">Bu, bağlam elden çıkarıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışmak, yüklenen gezinti boş veya boş olsa bile her zaman bir no-op olacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="231e1-621">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="231e1-622">Tersine, yüklenmeyen bir gezinti özelliğine erişmeye çalışmak, gezinti özelliği boş olmayan bir koleksiyon olsa bile bağlam atılırsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="231e1-622">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="231e1-623">Bu durum ortaya çıkarsa, uygulama kodunun geçersiz bir zamanda tembel yükleme kullanmaya çalıştığı ve uygulamanın bunu yapmamak için değiştirilmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="231e1-623">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="231e1-624">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-624">**Why**</span></span>

<span data-ttu-id="231e1-625">Bu değişiklik, elden çıkarılan `DbContext` bir örneğin üzerine tembelce yükleme yapmaya çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-625">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="231e1-626">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-626">**Mitigations**</span></span>

<span data-ttu-id="231e1-627">Uygulama kodunu, elden çıkarılmış bir bağlamla tembel yüklemegirişiminde bulunmamak için güncelleştirin veya bunu özel durum iletisinde açıklandığı gibi bir no-op olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="231e1-627">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="231e1-628">Dahili hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır</span><span class="sxs-lookup"><span data-stu-id="231e1-628">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="231e1-629">İzleme Sorunu #10236</span><span class="sxs-lookup"><span data-stu-id="231e1-629">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="231e1-630">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-630">**Old behavior**</span></span>

<span data-ttu-id="231e1-631">EF Core 3.0'dan önce, patolojik sayıda dahili hizmet sağlayıcısı oluşturan bir uygulama için bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-631">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="231e1-632">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-632">**New behavior**</span></span>

<span data-ttu-id="231e1-633">EF Core 3.0 ile başlayarak, bu uyarı artık kabul edilir ve hata ve bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-633">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="231e1-634">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-634">**Why**</span></span>

<span data-ttu-id="231e1-635">Bu değişiklik, bu patolojik durumu daha açık bir şekilde ortaya çıkararak daha iyi uygulama kodu kullanmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-635">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="231e1-636">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-636">**Mitigations**</span></span>

<span data-ttu-id="231e1-637">Bu hatayla karşılaşmanın en uygun eylem nedeni, temel nedeni anlamak ve bu kadar çok dahili hizmet sağlayıcısı oluşturmayı durdurmaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-637">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="231e1-638">Ancak, hata üzerinde yapılandırma yoluyla bir uyarı (veya göz `DbContextOptionsBuilder`ardı) geri dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-638">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="231e1-639">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-639">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="231e1-640">HasOne/HasMany için yeni davranış tek bir dize ile çağrıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-640">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="231e1-641">İzleme Sorunu #9171</span><span class="sxs-lookup"><span data-stu-id="231e1-641">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="231e1-642">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-642">**Old behavior**</span></span>

<span data-ttu-id="231e1-643">EF Core 3.0'dan `HasOne` `HasMany` önce, kod arama veya tek bir dize ile kafa karıştırıcı bir şekilde yorumlandı.</span><span class="sxs-lookup"><span data-stu-id="231e1-643">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="231e1-644">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-644">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="231e1-645">Kod, özel olabilecek `Samurai` gezinti özelliğini `Entrance` kullanarak başka bir varlık türüyle ilgili gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="231e1-645">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="231e1-646">Gerçekte, bu kod hiçbir gezinti özelliği ile `Entrance` adlandırılan bazı varlık türü için bir ilişki oluşturmak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="231e1-646">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="231e1-647">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-647">**New behavior**</span></span>

<span data-ttu-id="231e1-648">EF Core 3.0 ile başlayarak, yukarıdaki kod şimdi daha önce yapıyor olması gerektiği gibi görünüyordu ne yapar.</span><span class="sxs-lookup"><span data-stu-id="231e1-648">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="231e1-649">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-649">**Why**</span></span>

<span data-ttu-id="231e1-650">Eski davranış, özellikle yapılandırma kodu okuma ve hataları ararken, çok kafa karıştırıcı oldu.</span><span class="sxs-lookup"><span data-stu-id="231e1-650">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="231e1-651">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-651">**Mitigations**</span></span>

<span data-ttu-id="231e1-652">Bu, yalnızca tür adları için dizeleri kullanarak ve gezinti özelliğiaçıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları kırar.</span><span class="sxs-lookup"><span data-stu-id="231e1-652">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="231e1-653">Bu yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="231e1-653">This is not common.</span></span>
<span data-ttu-id="231e1-654">Önceki davranış, gezinti özelliği adı `null` için açıkça geçerek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-654">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="231e1-655">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-655">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="231e1-656">Birkaç async yönteminin dönüş türü Görevden ValueTask'a değiştirildi</span><span class="sxs-lookup"><span data-stu-id="231e1-656">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="231e1-657">İzleme Sorunu #15184</span><span class="sxs-lookup"><span data-stu-id="231e1-657">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="231e1-658">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-658">**Old behavior**</span></span>

<span data-ttu-id="231e1-659">Aşağıdaki async yöntemleri daha `Task<T>`önce döndürülen a:</span><span class="sxs-lookup"><span data-stu-id="231e1-659">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="231e1-660">`ValueGenerator.NextValueAsync()`(ve türeyen sınıflar)</span><span class="sxs-lookup"><span data-stu-id="231e1-660">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="231e1-661">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-661">**New behavior**</span></span>

<span data-ttu-id="231e1-662">Söz konusu yöntemler şimdi daha `ValueTask<T>` önce olduğu `T` gibi bir üzerinde döndürün.</span><span class="sxs-lookup"><span data-stu-id="231e1-662">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="231e1-663">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-663">**Why**</span></span>

<span data-ttu-id="231e1-664">Bu değişiklik, genel performansı artırarak, bu yöntemleri çağırarak oluşan yığın ayırmalarının sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="231e1-664">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="231e1-665">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-665">**Mitigations**</span></span>

<span data-ttu-id="231e1-666">Yukarıdaki API'leri bekleyen uygulamaların yalnızca yeniden derlenmeleri gerekir - kaynak değişiklikleri gerekmez.</span><span class="sxs-lookup"><span data-stu-id="231e1-666">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="231e1-667">Daha karmaşık bir kullanım (örn. `Task` döndürülen `Task.WhenAny()`ekibe geçmek) genellikle döndürülen `ValueTask<T>` lerin onu çağırarak `Task<T>` `AsTask()` a'ya dönüştürülmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="231e1-667">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="231e1-668">Bu değişikliğin getirdiği ayırma azaltma inkâr ları unutmayın.</span><span class="sxs-lookup"><span data-stu-id="231e1-668">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="231e1-669">İlişkisel: TypeMapping ek açıklama şimdi sadece TypeMapping olduğunu</span><span class="sxs-lookup"><span data-stu-id="231e1-669">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="231e1-670">İzleme Sorunu #9913</span><span class="sxs-lookup"><span data-stu-id="231e1-670">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="231e1-671">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-671">**Old behavior**</span></span>

<span data-ttu-id="231e1-672">Tür eşleme ek açıklamaları için ek açıklama adı "İlişkisel:TypeMapping" idi.</span><span class="sxs-lookup"><span data-stu-id="231e1-672">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="231e1-673">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-673">**New behavior**</span></span>

<span data-ttu-id="231e1-674">Tür eşleme ek açıklamaları için ek açıklama adı artık "TypeMapping"dir.</span><span class="sxs-lookup"><span data-stu-id="231e1-674">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="231e1-675">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-675">**Why**</span></span>

<span data-ttu-id="231e1-676">Tür eşlemeleri artık ilişkisel veritabanı sağlayıcılarından daha fazlası için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-676">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="231e1-677">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-677">**Mitigations**</span></span>

<span data-ttu-id="231e1-678">Bu, yalnızca tür eşlemesine doğrudan ek açıklama olarak erişen ve yaygın olmayan uygulamaları bozar.</span><span class="sxs-lookup"><span data-stu-id="231e1-678">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="231e1-679">Düzeltmek için en uygun eylem, doğrudan ek açıklama kullanmak yerine tür eşlemeleri erişmek için API yüzeyi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-679">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="231e1-680">Türetilmiş bir türde ToTable bir özel durum atar</span><span class="sxs-lookup"><span data-stu-id="231e1-680">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="231e1-681">İzleme Sorunu #11811</span><span class="sxs-lookup"><span data-stu-id="231e1-681">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="231e1-682">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-682">**Old behavior**</span></span>

<span data-ttu-id="231e1-683">EF Core 3.0'dan önce, `ToTable()` türetilmiş bir tür elendi, çünkü bu geçerli olmayan yalnızca devralma eşleme stratejisi TPH olduğundan, bu türde çağrı yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="231e1-683">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="231e1-684">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-684">**New behavior**</span></span>

<span data-ttu-id="231e1-685">EF Core 3.0 ile başlayan ve daha sonraki bir sürümde `ToTable()` TPT ve TPC desteği eklemeye hazırlık olarak, türetilmiş bir tür şimdi gelecekte beklenmedik bir harita değişikliği önlemek için bir özel durum atacaktır çağırdı.</span><span class="sxs-lookup"><span data-stu-id="231e1-685">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="231e1-686">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-686">**Why**</span></span>

<span data-ttu-id="231e1-687">Şu anda türetilen bir türü farklı bir tabloyla eşlemek geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="231e1-687">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="231e1-688">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda kırılmayı önler.</span><span class="sxs-lookup"><span data-stu-id="231e1-688">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="231e1-689">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-689">**Mitigations**</span></span>

<span data-ttu-id="231e1-690">Türetilen türleri diğer tablolarla eşlenemeye yönelik tüm girişimleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="231e1-690">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="231e1-691">ForSqlServerHasIndex HasIndex ile değiştirildi</span><span class="sxs-lookup"><span data-stu-id="231e1-691">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="231e1-692">İzleme Sorunu #12366</span><span class="sxs-lookup"><span data-stu-id="231e1-692">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="231e1-693">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-693">**Old behavior**</span></span>

<span data-ttu-id="231e1-694">EF Core 3.0'dan önce, `ForSqlServerHasIndex().ForSqlServerInclude()` 'ile `INCLUDE`kullanılan sütunları yapılandırmak için bir yol sağlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-694">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="231e1-695">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-695">**New behavior**</span></span>

<span data-ttu-id="231e1-696">EF Core 3.0 ile `Include` başlayarak, bir dizin üzerinde kullanmak artık ilişkisel düzeyde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="231e1-696">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="231e1-697">`HasIndex().ForSqlServerInclude()` adresini kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-697">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="231e1-698">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-698">**Why**</span></span>

<span data-ttu-id="231e1-699">Bu değişiklik, dizinler `Include` için API'yi tüm veritabanı sağlayıcıları için tek bir yerde birleştirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-699">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="231e1-700">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-700">**Mitigations**</span></span>

<span data-ttu-id="231e1-701">Yukarıda gösterildiği gibi yeni API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-701">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="231e1-702">Meta veri API değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="231e1-702">Metadata API changes</span></span>

[<span data-ttu-id="231e1-703">İzleme Sorunu #214</span><span class="sxs-lookup"><span data-stu-id="231e1-703">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="231e1-704">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-704">**New behavior**</span></span>

<span data-ttu-id="231e1-705">Aşağıdaki özellikler uzantı yöntemlerine dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="231e1-705">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="231e1-706">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-706">**Why**</span></span>

<span data-ttu-id="231e1-707">Bu değişiklik, yukarıda belirtilen arabirimlerin uygulanmasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="231e1-707">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="231e1-708">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-708">**Mitigations**</span></span>

<span data-ttu-id="231e1-709">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-709">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="231e1-710">Sağlayıcıya özel Meta veri API değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="231e1-710">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="231e1-711">İzleme Sorunu #214</span><span class="sxs-lookup"><span data-stu-id="231e1-711">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="231e1-712">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-712">**New behavior**</span></span>

<span data-ttu-id="231e1-713">Sağlayıcıya özel uzatma yöntemleri düzleştirilmiş olacaktır:</span><span class="sxs-lookup"><span data-stu-id="231e1-713">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="231e1-714">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-714">**Why**</span></span>

<span data-ttu-id="231e1-715">Bu değişiklik, yukarıda belirtilen uzantı yöntemlerinin uygulanmasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="231e1-715">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="231e1-716">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-716">**Mitigations**</span></span>

<span data-ttu-id="231e1-717">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-717">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="231e1-718">EF Core artık SQLite FK uygulama için pragma gönderir</span><span class="sxs-lookup"><span data-stu-id="231e1-718">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="231e1-719">İzleme Sorunu #12151</span><span class="sxs-lookup"><span data-stu-id="231e1-719">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="231e1-720">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-720">**Old behavior**</span></span>

<span data-ttu-id="231e1-721">EF Core 3.0'dan önce, SQLite bağlantısı açıldığında EF Core gönderir. `PRAGMA foreign_keys = 1`</span><span class="sxs-lookup"><span data-stu-id="231e1-721">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="231e1-722">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-722">**New behavior**</span></span>

<span data-ttu-id="231e1-723">EF Core 3.0 ile başlayarak, `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında EF Core artık göndermez.</span><span class="sxs-lookup"><span data-stu-id="231e1-723">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="231e1-724">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-724">**Why**</span></span>

<span data-ttu-id="231e1-725">Bu değişiklik, EF Core `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak kullandığı için yapıldı, bu da FK zorlamanın varsayılan olarak açık olduğu ve her bağlantı açıldığında açıkça etkinleştirilmesi gerekmediği anlamına geliyor.</span><span class="sxs-lookup"><span data-stu-id="231e1-725">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="231e1-726">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-726">**Mitigations**</span></span>

<span data-ttu-id="231e1-727">Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3'da varsayılan olarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-727">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="231e1-728">Diğer durumlarda, bağlantı dizenizde belirtilerek `Foreign Keys=True` yabancı anahtarlar etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-728">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="231e1-729">Microsoft.EntityFrameworkCore.Sqlite şimdi SQLitePCLRaw.bundle_e_sqlite3 bağlıdır</span><span class="sxs-lookup"><span data-stu-id="231e1-729">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="231e1-730">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-730">**Old behavior**</span></span>

<span data-ttu-id="231e1-731">EF Core 3.0'dan `SQLitePCLRaw.bundle_green`önce EF Core kullanılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-731">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="231e1-732">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-732">**New behavior**</span></span>

<span data-ttu-id="231e1-733">EF Core 3.0 ile başlayarak, EF Core kullanır. `SQLitePCLRaw.bundle_e_sqlite3`</span><span class="sxs-lookup"><span data-stu-id="231e1-733">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="231e1-734">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-734">**Why**</span></span>

<span data-ttu-id="231e1-735">Bu değişiklik, sqlite sürümü diğer platformlar ile tutarlı iOS kullanılan böylece yapıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-735">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="231e1-736">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-736">**Mitigations**</span></span>

<span data-ttu-id="231e1-737">iOS'ta yerel SQLite sürümünü kullanmak `Microsoft.Data.Sqlite` için farklı `SQLitePCLRaw` bir paket kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="231e1-737">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="231e1-738">Kılavuz değerleri artık SQLite'da METİn olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="231e1-738">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="231e1-739">İzleme Sorunu #15078</span><span class="sxs-lookup"><span data-stu-id="231e1-739">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="231e1-740">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-740">**Old behavior**</span></span>

<span data-ttu-id="231e1-741">Guid değerleri daha önce SQLite'da BLOB değerleri olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="231e1-741">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="231e1-742">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-742">**New behavior**</span></span>

<span data-ttu-id="231e1-743">Kılavuz değerleri artık TEXT olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="231e1-743">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="231e1-744">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-744">**Why**</span></span>

<span data-ttu-id="231e1-745">Guids'in ikili biçimi standartlaştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="231e1-745">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="231e1-746">Değerlerin TEXT olarak depolanması, veritabanını diğer teknolojilerle daha uyumlu hale getirir.</span><span class="sxs-lookup"><span data-stu-id="231e1-746">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="231e1-747">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-747">**Mitigations**</span></span>

<span data-ttu-id="231e1-748">Aşağıdaki gibi SQL çalıştırarak varolan veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="231e1-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="231e1-749">EF Core'da, bu özellikler üzerinde bir değer dönüştürücüsi yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="231e1-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="231e1-750">Microsoft.Data.Sqlite, hem BLOB hem de TEXT sütunlarından Guid değerlerini okuma yeteneğine sahiptir; ancak, parametreler ve sabitler için varsayılan biçim değiştiğinden, Guids içeren çoğu senaryo için büyük olasılıkla işlem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-750">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="231e1-751">Char değerleri artık SQLite'da TEXT olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="231e1-751">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="231e1-752">İzleme Sorunu #15020</span><span class="sxs-lookup"><span data-stu-id="231e1-752">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="231e1-753">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-753">**Old behavior**</span></span>

<span data-ttu-id="231e1-754">Char değerleri daha önce SQLite'da INTEGER değerleri olarak ayrıştırıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-754">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="231e1-755">Örneğin, *A'nın* bir char değeri tamsayı değeri 65 olarak depolandı.</span><span class="sxs-lookup"><span data-stu-id="231e1-755">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="231e1-756">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-756">**New behavior**</span></span>

<span data-ttu-id="231e1-757">Char değerleri artık TEXT olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="231e1-757">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="231e1-758">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-758">**Why**</span></span>

<span data-ttu-id="231e1-759">Değerleri TEXT olarak depolamak daha doğaldır ve veritabanını diğer teknolojilerle daha uyumlu hale getirir.</span><span class="sxs-lookup"><span data-stu-id="231e1-759">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="231e1-760">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-760">**Mitigations**</span></span>

<span data-ttu-id="231e1-761">Aşağıdaki gibi SQL çalıştırarak varolan veritabanlarını yeni biçime geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="231e1-761">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="231e1-762">EF Core'da, bu özellikler üzerinde bir değer dönüştürücüsi yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="231e1-762">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="231e1-763">Microsoft.Data.Sqlite ayrıca hem INTEGER hem de TEXT sütunlarından karakter değerlerini okuma yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir eylem gerektirmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-763">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="231e1-764">Geçiş disleri artık değişmez kültürün takvimi kullanılarak oluşturulur</span><span class="sxs-lookup"><span data-stu-id="231e1-764">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="231e1-765">İzleme Sorunu #12978</span><span class="sxs-lookup"><span data-stu-id="231e1-765">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="231e1-766">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-766">**Old behavior**</span></span>

<span data-ttu-id="231e1-767">Geçiş disleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="231e1-767">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="231e1-768">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-768">**New behavior**</span></span>

<span data-ttu-id="231e1-769">Geçiş işlleri şimdi her zaman değişmez kültürün takvimi (Gregoryen) kullanılardı.</span><span class="sxs-lookup"><span data-stu-id="231e1-769">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="231e1-770">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-770">**Why**</span></span>

<span data-ttu-id="231e1-771">Geçişsırası, veritabanını güncelleştirirken veya birleştirme çakışmalarını çözerken önemlidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-771">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="231e1-772">Değişmez takvimin kullanılması, takım üyelerinin farklı sistem takvimlerine sahip olmasından kaynaklanabilir sipariş sorunlarını önler.</span><span class="sxs-lookup"><span data-stu-id="231e1-772">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="231e1-773">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-773">**Mitigations**</span></span>

<span data-ttu-id="231e1-774">Bu değişiklik, yılın Gregoryen takviminden (Tay Budist takvimi gibi) daha büyük olduğu Gregoryen olmayan bir takvimi kullanan herkesi etkiler.</span><span class="sxs-lookup"><span data-stu-id="231e1-774">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="231e1-775">Varolan geçişlerden sonra yeni geçişlerin sıralanabilmesi için varolan geçiş lerin güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-775">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="231e1-776">Geçiş kimliği, geçişlerin tasarımcı dosyalarında Geçiş özniteliğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-776">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="231e1-777">Geçişler geçmişi tablosunun da güncellenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-777">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="231e1-778">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-778">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="231e1-779">İzleme Sorunu #16400</span><span class="sxs-lookup"><span data-stu-id="231e1-779">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="231e1-780">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-780">**Old behavior**</span></span>

<span data-ttu-id="231e1-781">EF Core 3.0'dan önce, `UseRowNumberForPaging` SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-781">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="231e1-782">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-782">**New behavior**</span></span>

<span data-ttu-id="231e1-783">EF Core 3.0 ile başlayarak, EF yalnızca sonraki SQL Server sürümleriyle uyumlu olan sayfalama için SQL oluşturur.</span><span class="sxs-lookup"><span data-stu-id="231e1-783">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="231e1-784">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-784">**Why**</span></span>

<span data-ttu-id="231e1-785">[SQL Server 2008 artık desteklenen bir ürün olmadığı](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) için bu değişikliği yapıyoruz ve bu özelliği EF Core 3.0'da yapılan sorgu değişiklikleriyle çalışacak şekilde güncelliyoruz önemli bir iş.</span><span class="sxs-lookup"><span data-stu-id="231e1-785">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="231e1-786">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-786">**Mitigations**</span></span>

<span data-ttu-id="231e1-787">Oluşturulan SQL'in desteklenmesi için SQL Server'ın daha yeni bir sürümüne güncelleştirmenizi veya daha yüksek bir uyumluluk düzeyi kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="231e1-787">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="231e1-788">Bu söyleniyor, bunu yapamıyorsanız, o zaman ayrıntıları ile [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) lütfen.</span><span class="sxs-lookup"><span data-stu-id="231e1-788">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="231e1-789">Bu kararı geri bildirimlere dayanarak tekrar gözden geçirebiliriz.</span><span class="sxs-lookup"><span data-stu-id="231e1-789">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="231e1-790">Uzantı bilgileri/meta veriler IDbContextOptionsExtension kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-790">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="231e1-791">İzleme Sorunu #16119</span><span class="sxs-lookup"><span data-stu-id="231e1-791">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="231e1-792">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-792">**Old behavior**</span></span>

<span data-ttu-id="231e1-793">`IDbContextOptionsExtension`uzantısı hakkında meta veri sağlamak için yöntemler içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="231e1-793">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="231e1-794">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-794">**New behavior**</span></span>

<span data-ttu-id="231e1-795">Bu yöntemler, yeni `DbContextOptionsExtensionInfo` `IDbContextOptionsExtension.Info` bir özellikten döndürülen yeni bir soyut taban sınıfına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-795">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="231e1-796">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-796">**Why**</span></span>

<span data-ttu-id="231e1-797">2.0'dan 3.0'a kadar olan sürümler üzerinde bu yöntemleri birkaç kez eklememiz veya değiştirmemiz gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="231e1-797">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="231e1-798">Bunları yeni bir soyut taban sınıfına dönüştürmek, varolan uzantıları bozmadan bu tür değişiklikleri yapmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="231e1-798">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="231e1-799">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-799">**Mitigations**</span></span>

<span data-ttu-id="231e1-800">Yeni deseni izlemek için uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="231e1-800">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="231e1-801">ÖRNEKLER, EF Core kaynak `IDbContextOptionsExtension` kodundaki farklı türde uzantılar için birçok uygulamada bulunur.</span><span class="sxs-lookup"><span data-stu-id="231e1-801">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="231e1-802">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="231e1-802">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="231e1-803">İzleme Sorunu #10985</span><span class="sxs-lookup"><span data-stu-id="231e1-803">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="231e1-804">**Değiştir**</span><span class="sxs-lookup"><span data-stu-id="231e1-804">**Change**</span></span>

<span data-ttu-id="231e1-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`olarak `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="231e1-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="231e1-806">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-806">**Why**</span></span>

<span data-ttu-id="231e1-807">Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.</span><span class="sxs-lookup"><span data-stu-id="231e1-807">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="231e1-808">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-808">**Mitigations**</span></span>

<span data-ttu-id="231e1-809">Yeni adı kullan.</span><span class="sxs-lookup"><span data-stu-id="231e1-809">Use the new name.</span></span> <span data-ttu-id="231e1-810">(Olay kimliği numarasının değişmediğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="231e1-810">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="231e1-811">Yabancı anahtar kısıtlama adları için API'yi netleştir</span><span class="sxs-lookup"><span data-stu-id="231e1-811">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="231e1-812">İzleme Sorunu #10730</span><span class="sxs-lookup"><span data-stu-id="231e1-812">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="231e1-813">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-813">**Old behavior**</span></span>

<span data-ttu-id="231e1-814">EF Core 3.0'dan önce, yabancı anahtar kısıtlama adları basitçe "ad" olarak adlandırılıyordu.</span><span class="sxs-lookup"><span data-stu-id="231e1-814">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="231e1-815">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-815">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="231e1-816">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-816">**New behavior**</span></span>

<span data-ttu-id="231e1-817">EF Core 3.0 ile başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-817">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="231e1-818">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="231e1-819">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-819">**Why**</span></span>

<span data-ttu-id="231e1-820">Bu değişiklik, bu alanda adlandırma tutarlılık getiriyor ve aynı zamanda bu yabancı anahtar kısıtlama adı değil, yabancı anahtar tanımlanan sütun veya özellik adı olduğunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="231e1-820">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="231e1-821">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-821">**Mitigations**</span></span>

<span data-ttu-id="231e1-822">Yeni adı kullan.</span><span class="sxs-lookup"><span data-stu-id="231e1-822">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="231e1-823">iRelationalDatabaseCreator.HasTables/HasTablesAsync genel kullanıma açıklanmıştır</span><span class="sxs-lookup"><span data-stu-id="231e1-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="231e1-824">İzleme Sorunu #15997</span><span class="sxs-lookup"><span data-stu-id="231e1-824">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="231e1-825">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-825">**Old behavior**</span></span>

<span data-ttu-id="231e1-826">EF Core 3.0'dan önce bu yöntemler korunmuştur.</span><span class="sxs-lookup"><span data-stu-id="231e1-826">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="231e1-827">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-827">**New behavior**</span></span>

<span data-ttu-id="231e1-828">EF Core 3.0 ile başlayarak, bu yöntemler herkese açıktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-828">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="231e1-829">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-829">**Why**</span></span>

<span data-ttu-id="231e1-830">Bu yöntemler, bir veritabanı nın oluşturulabilir ancak boş olup olmadığını belirlemek için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-830">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="231e1-831">Bu, geçişlerin uygulanıp uygulanmayacağını belirlerken EF dışından da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-831">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="231e1-832">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-832">**Mitigations**</span></span>

<span data-ttu-id="231e1-833">Geçersiz kılmaların erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="231e1-833">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="231e1-834">Microsoft.EntityFrameworkCore.Design artık bir DevelopmentDependency paketidir</span><span class="sxs-lookup"><span data-stu-id="231e1-834">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="231e1-835">İzleme Sorunu #11506</span><span class="sxs-lookup"><span data-stu-id="231e1-835">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="231e1-836">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-836">**Old behavior**</span></span>

<span data-ttu-id="231e1-837">EF Core 3.0'dan önce, Microsoft.EntityFrameworkCore.Design, derlemesi ona bağlı projeler tarafından başvurulan normal bir NuGet paketiydi.</span><span class="sxs-lookup"><span data-stu-id="231e1-837">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="231e1-838">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-838">**New behavior**</span></span>

<span data-ttu-id="231e1-839">EF Core 3.0 ile başlayarak, bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-839">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="231e1-840">Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve varsayılan olarak artık derlemesine başvuruda bulunamayacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="231e1-840">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="231e1-841">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-841">**Why**</span></span>

<span data-ttu-id="231e1-842">Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="231e1-842">This package is only intended to be used at design time.</span></span> <span data-ttu-id="231e1-843">Dağıtılan uygulamalar başvuru olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-843">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="231e1-844">Paketi Geliştirme Bağımlılığı yapmak bu öneriyi pekiştirir.</span><span class="sxs-lookup"><span data-stu-id="231e1-844">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="231e1-845">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-845">**Mitigations**</span></span>

<span data-ttu-id="231e1-846">EF Core'un tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference madde meta verilerini güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="231e1-846">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="231e1-847">Paket Microsoft.EntityFrameworkCore.Tools üzerinden geçişli olarak başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="231e1-847">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="231e1-848">Paketteki türlerin gerekli olduğu herhangi bir projeye bu tür açık bir başvuru eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="231e1-848">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="231e1-849">SQLitePCL.raw sürüm 2.0.0 güncellendi</span><span class="sxs-lookup"><span data-stu-id="231e1-849">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="231e1-850">İzleme Sorunu #14824</span><span class="sxs-lookup"><span data-stu-id="231e1-850">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="231e1-851">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-851">**Old behavior**</span></span>

<span data-ttu-id="231e1-852">Microsoft.EntityFrameworkCore.Sqlite daha önce SQLitePCL.raw sürümü 1.1.12 bağlı.</span><span class="sxs-lookup"><span data-stu-id="231e1-852">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="231e1-853">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-853">**New behavior**</span></span>

<span data-ttu-id="231e1-854">Paketimizi sürüm 2.0.0'a bağlı olarak güncelledik.</span><span class="sxs-lookup"><span data-stu-id="231e1-854">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="231e1-855">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-855">**Why**</span></span>

<span data-ttu-id="231e1-856">Sürüm 2.0.0 SQLitePCL.raw hedefleri .NET Standart 2.0.</span><span class="sxs-lookup"><span data-stu-id="231e1-856">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="231e1-857">Daha önce .NET Standart 1.1'i hedeflenene de geçişli paketlerin çalışması için büyük bir kapatma gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="231e1-857">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="231e1-858">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-858">**Mitigations**</span></span>

<span data-ttu-id="231e1-859">SQLitePCL.raw sürüm 2.0.0 bazı kırılma değişiklikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="231e1-859">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="231e1-860">Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="231e1-860">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="231e1-861">NetTopologySuite sürüm 2.0.0 güncellendi</span><span class="sxs-lookup"><span data-stu-id="231e1-861">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="231e1-862">İzleme Sorunu #14825</span><span class="sxs-lookup"><span data-stu-id="231e1-862">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="231e1-863">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-863">**Old behavior**</span></span>

<span data-ttu-id="231e1-864">Uzamsal paketler daha önce NetTopologySuite'in 1.15.1 sürümüne bağlıydı.</span><span class="sxs-lookup"><span data-stu-id="231e1-864">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="231e1-865">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-865">**New behavior**</span></span>

<span data-ttu-id="231e1-866">Paketimizi sürüm 2.0.0'a bağlı olarak güncelledik.</span><span class="sxs-lookup"><span data-stu-id="231e1-866">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="231e1-867">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-867">**Why**</span></span>

<span data-ttu-id="231e1-868">NetTopologySuite Sürüm 2.0.0 EF Core kullanıcıları tarafından karşılaşılan çeşitli kullanılabilirlik sorunları ele amaçlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="231e1-868">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="231e1-869">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-869">**Mitigations**</span></span>

<span data-ttu-id="231e1-870">NetTopologySuite sürüm 2.0.0 bazı kırılma değişiklikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="231e1-870">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="231e1-871">Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.</span><span class="sxs-lookup"><span data-stu-id="231e1-871">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="231e1-872">System.Data.SqlClient yerine Microsoft.Data.SqlClient kullanılır</span><span class="sxs-lookup"><span data-stu-id="231e1-872">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="231e1-873">İzleme Sorunu #15636</span><span class="sxs-lookup"><span data-stu-id="231e1-873">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="231e1-874">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-874">**Old behavior**</span></span>

<span data-ttu-id="231e1-875">Microsoft.EntityFrameworkCore.SqlServer daha önce System.Data.SqlClient'a bağlı.</span><span class="sxs-lookup"><span data-stu-id="231e1-875">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="231e1-876">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-876">**New behavior**</span></span>

<span data-ttu-id="231e1-877">Paketimizi Microsoft.Data.SqlClient'a bağlı olacak şekilde güncelledik.</span><span class="sxs-lookup"><span data-stu-id="231e1-877">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="231e1-878">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-878">**Why**</span></span>

<span data-ttu-id="231e1-879">Microsoft.Data.SqlClient, SQL Server'ın en önemli veri erişim sürücüsüdür ve System.Data.SqlClient artık geliştirmenin odak noktası değildir.</span><span class="sxs-lookup"><span data-stu-id="231e1-879">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="231e1-880">Her Zaman Şifrelenmiş gibi bazı önemli özellikler yalnızca Microsoft.Data.SqlClient'da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-880">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="231e1-881">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-881">**Mitigations**</span></span>

<span data-ttu-id="231e1-882">Kodunuz System.Data.SqlClient'a doğrudan bağımlılık gerekiyorsa, bunun yerine Microsoft.Data.SqlClient'a başvurmak üzere değiştirmeniz gerekir; iki paket API uyumluluğu çok yüksek derecede korumak gibi, bu sadece basit bir paket ve ad alanı değişikliği olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="231e1-882">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="231e1-883">Birden çok belirsiz kendi kendine başvuran ilişkiler yapılandırılmalıdır</span><span class="sxs-lookup"><span data-stu-id="231e1-883">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="231e1-884">İzleme Sorunu #13573</span><span class="sxs-lookup"><span data-stu-id="231e1-884">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="231e1-885">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-885">**Old behavior**</span></span>

<span data-ttu-id="231e1-886">Birden çok kendi kendine başvuran tek yönlü gezinme özellikleri ve eşleşen FK'lar ile bir varlık türü yanlış tek bir ilişki olarak yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="231e1-886">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="231e1-887">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-887">For example:</span></span>

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

<span data-ttu-id="231e1-888">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-888">**New behavior**</span></span>

<span data-ttu-id="231e1-889">Bu senaryo artık model binasında algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="231e1-889">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="231e1-890">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-890">**Why**</span></span>

<span data-ttu-id="231e1-891">Ortaya çıkan model belirsiz ve büyük olasılıkla genellikle bu durum için yanlış olacaktır.</span><span class="sxs-lookup"><span data-stu-id="231e1-891">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="231e1-892">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-892">**Mitigations**</span></span>

<span data-ttu-id="231e1-893">İlişkinin tam yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="231e1-893">Use full configuration of the relationship.</span></span> <span data-ttu-id="231e1-894">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="231e1-894">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="231e1-895">DbFunction.Schema null veya boş dize olmak modelin varsayılan şema olarak yapılandırır</span><span class="sxs-lookup"><span data-stu-id="231e1-895">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="231e1-896">İzleme Sorunu #12757</span><span class="sxs-lookup"><span data-stu-id="231e1-896">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="231e1-897">**Eski davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-897">**Old behavior**</span></span>

<span data-ttu-id="231e1-898">Boş bir dize olarak şema ile yapılandırılan bir DbFunction, şema olmadan yerleşik işlev olarak kabul edildi.</span><span class="sxs-lookup"><span data-stu-id="231e1-898">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="231e1-899">Örneğin aşağıdaki kod `DatePart` CLR işlevini `DATEPART` SqlServer'daki yerleşik işleve eşler.</span><span class="sxs-lookup"><span data-stu-id="231e1-899">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="231e1-900">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="231e1-900">**New behavior**</span></span>

<span data-ttu-id="231e1-901">Tüm DbFunction eşlemeleri kullanıcı tanımlı işlevlere eşlenmiş olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-901">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="231e1-902">Bu nedenle boş dize değeri modeli için varsayılan şema içinde işlev koyacağız.</span><span class="sxs-lookup"><span data-stu-id="231e1-902">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="231e1-903">Hangi şema açıkça akıcı API `modelBuilder.HasDefaultSchema()` veya `dbo` başka bir şekilde yapılandırılan olabilir.</span><span class="sxs-lookup"><span data-stu-id="231e1-903">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="231e1-904">**Neden**</span><span class="sxs-lookup"><span data-stu-id="231e1-904">**Why**</span></span>

<span data-ttu-id="231e1-905">Daha önce şema boş olması bu işlevi işlemek için bir yol yerleşik ama bu mantık sadece sqlserver için geçerli olan yerleşik işlevleri herhangi bir şema ait değildir.</span><span class="sxs-lookup"><span data-stu-id="231e1-905">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="231e1-906">**Risk Azaltıcı Etkenler**</span><span class="sxs-lookup"><span data-stu-id="231e1-906">**Mitigations**</span></span>

<span data-ttu-id="231e1-907">DbFunction'in çevirisini yerleşik bir işleve eşlemek için el ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="231e1-907">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
