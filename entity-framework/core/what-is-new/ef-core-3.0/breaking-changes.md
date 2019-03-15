---
title: EF Core 3.0 - EF Core yeni değişiklikler
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 748db8a71a04a2d696ef21a03319906b9fc776be
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829232"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="736ab-102">EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil edilen değişiklikler</span><span class="sxs-lookup"><span data-stu-id="736ab-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="736ab-103">Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="736ab-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="736ab-104">Aşağıdaki API ve davranışı değişiklikleri EF Core için geliştirilen uygulamaları ayırmak için olası sahip bunları 3.0.0 için yükseltirken 2.2.x.</span><span class="sxs-lookup"><span data-stu-id="736ab-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="736ab-105">Veritabanı sağlayıcıları yalnızca etkileyen bekliyoruz değişiklikleri altında belgelenir [sağlayıcısı değişiklikleri](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="736ab-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="736ab-106">Bir 3.0 Önizlemesi'nden başka bir 3.0 Önizleme için kullanıma sunulan yeni özellikler sonlarını burada belgelenmemiş.</span><span class="sxs-lookup"><span data-stu-id="736ab-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="736ab-107">LINQ sorguları, artık istemcide değerlendirilir</span><span class="sxs-lookup"><span data-stu-id="736ab-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="736ab-108">[Sorun #14935 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[sorun #12795 Ayrıca bkz:](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="736ab-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="736ab-109">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="736ab-110">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-110">**Old behavior**</span></span>

<span data-ttu-id="736ab-111">EF Core, SQL veya bir parametre için bir sorgunun parçası olan bir ifade dönüştürülemedi, 3.0 önce bunu otomatik olarak istemcide ifadesi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="736ab-112">Varsayılan olarak, istemci değerlendirmesi yüksek maliyetlere neden olabilecek bir ifade yalnızca bir uyarı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="736ab-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="736ab-113">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-113">**New behavior**</span></span>

<span data-ttu-id="736ab-114">3.0 ile başlayarak, EF Core yalnızca ifadeleri üst düzey projeksiyon izin verir (son `Select()` sorguda çağrısı) istemcide değerlendirilecek.</span><span class="sxs-lookup"><span data-stu-id="736ab-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="736ab-115">İfadeleri herhangi bir sorgunun parçası olarak SQL veya bir parametre olarak dönüştürülemez, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="736ab-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="736ab-116">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-116">**Why**</span></span>

<span data-ttu-id="736ab-117">Otomatik istemci değerlendirme sorguların bile bunları önemli bölümleri çevrilemez yürütülecek çok sayıda sorgu sağlar.</span><span class="sxs-lookup"><span data-stu-id="736ab-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="736ab-118">Bu davranış yalnızca üretimde yetkisiz değiştirmeye karşı korumalı hale gelebilir beklenmeyen ve zararlı davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="736ab-119">Örneğin, bir koşulda bir `Where()` çevrilemez çağrısı, veritabanı sunucusu ve istemcide uygulanması için filtreyi aktarılacak tablosundan tüm satırları açabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="736ab-120">Tablo, geliştirme, ancak isabet sabit yalnızca birkaç satır içeriyorsa uygulama üretime burada tablo milyonlarca satır içerebilir taşındığında bu durum kolayca algılanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="736ab-121">İstemci değerlendirme uyarılar ayrıca geliştirme sırasında göz ardı edilmesi çok kolay kanıtladılar.</span><span class="sxs-lookup"><span data-stu-id="736ab-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="736ab-122">Belirli ifadeler için iyileştirme sorgu çevirisi sürümler arasında istenmeyen bozucu değişiklikleri nedeniyle oluşan sorunları bu yanı sıra, otomatik istemci değerlendirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="736ab-123">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-123">**Mitigations**</span></span>

<span data-ttu-id="736ab-124">Bir sorgu tam olarak çevrilemez sonra ya da çevrilebilir veya kullanan bir form sorguda yeniden `AsEnumerable()`, `ToList()`, veya açıkça verileri Burada, ardından daha büyük olabilir istemciye geri getirmek için benzer LINQ nesneleri kullanılarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="736ab-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="736ab-125">Entity Framework Core artık ASP.NET Core paylaşılan framework'ün bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="736ab-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="736ab-126">İzleme sorunu duyuruları #325</span><span class="sxs-lookup"><span data-stu-id="736ab-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="736ab-127">Bu değişiklik, ASP.NET Core 3.0 Önizleme 1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="736ab-128">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-128">**Old behavior**</span></span>

<span data-ttu-id="736ab-129">ASP.NET Core 3.0 Paketi başvurusu eklendiğinde önce `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`, EF Core içerir ve EF Core veri sağlayıcıları bazıları istediğiniz SQL Server sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="736ab-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="736ab-130">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-130">**New behavior**</span></span>

<span data-ttu-id="736ab-131">3. 0'dan başlayarak, ASP.NET Core paylaşılan çerçeve EF Core veya tüm EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="736ab-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="736ab-132">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-132">**Why**</span></span>

<span data-ttu-id="736ab-133">Bu değişiklikten önce EF Core alma olup uygulama ASP.NET Core ve SQL Server veya hedeflenen bağlı olarak farklı adımlar gereklidir.</span><span class="sxs-lookup"><span data-stu-id="736ab-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="736ab-134">Ayrıca, ASP.NET Core yükseltme EF Core ve SQL Server sağlayıcısı, her zaman tercih değildir yükseltmesini zorlandı.</span><span class="sxs-lookup"><span data-stu-id="736ab-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="736ab-135">Bu değişiklik, EF Core alma aynı tüm sağlayıcıları, desteklenen .NET uygulamaları ve uygulama türleri arasında deneyimidir.</span><span class="sxs-lookup"><span data-stu-id="736ab-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="736ab-136">Geliştiriciler, tam olarak EF Core ve EF Core veri sağlayıcıları yükseltildiğinde de denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="736ab-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="736ab-137">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-137">**Mitigations**</span></span>

<span data-ttu-id="736ab-138">EF çekirdekli ASP.NET Core 3.0 uygulama veya desteklenen başka bir uygulama içinde kullanmak için açıkça uygulamanızın kullanacağı EF Core veritabanı sağlayıcısı için bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="736ab-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="736ab-139">Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir</span><span class="sxs-lookup"><span data-stu-id="736ab-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="736ab-140">İzleme sorun #14523</span><span class="sxs-lookup"><span data-stu-id="736ab-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="736ab-141">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-142">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-142">**Old behavior**</span></span>

<span data-ttu-id="736ab-143">EF Core 3.0 önce sorgu ve diğer komutları yürütme sırasında günlüğe kaydedilen `Info` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="736ab-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="736ab-144">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-144">**New behavior**</span></span>

<span data-ttu-id="736ab-145">EF Core 3.0 ile başlayarak, komut/SQL yürütme günlüğe kaydetme sırasında işlemi `Debug` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="736ab-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="736ab-146">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-146">**Why**</span></span>

<span data-ttu-id="736ab-147">Konumundaki paraziti azaltmak için bu değişiklik yapılmıştır `Info` günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="736ab-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="736ab-148">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-148">**Mitigations**</span></span>

<span data-ttu-id="736ab-149">Bu günlük olayı tarafından tanımlanan `RelationalEventId.CommandExecuting` 20100 olay kimliği.</span><span class="sxs-lookup"><span data-stu-id="736ab-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="736ab-150">SQL oturum `Info` yeniden düzey, düzeyi açıkça yapılandırmanıza `OnConfiguring` veya `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="736ab-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="736ab-151">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="736ab-152">Geçici bir anahtar değere artık varlık örneklerini ayarlanır</span><span class="sxs-lookup"><span data-stu-id="736ab-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="736ab-153">İzleme sorun #12378</span><span class="sxs-lookup"><span data-stu-id="736ab-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="736ab-154">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="736ab-155">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-155">**Old behavior**</span></span>

<span data-ttu-id="736ab-156">EF Core 3.0 önce veritabanı tarafından oluşturulan gerçek değer daha sonra sahip olabileceği tüm anahtar özellikleri geçici değerler atanmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="736ab-157">Genellikle bu geçici değerleri büyük negatif sayılar yoktu.</span><span class="sxs-lookup"><span data-stu-id="736ab-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="736ab-158">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-158">**New behavior**</span></span>

<span data-ttu-id="736ab-159">3.0 ile başlayarak, EF Core, varlığın izleme bilgilerini bir parçası olarak geçici bir anahtar değeri depolar ve anahtar özelliği kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="736ab-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="736ab-160">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-160">**Why**</span></span>

<span data-ttu-id="736ab-161">Geçici bir anahtar değere deneyebileceğinizi daha önce bazı tarafından izlenen bir varlık kalıcı hale gelmesini önlemek için bu değişiklik yapılmıştır `DbContext` örneği farklı bir taşınır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="736ab-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="736ab-162">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-162">**Mitigations**</span></span>

<span data-ttu-id="736ab-163">Varlıklar arasında ilişkilendirmeler form üzerine yabancı anahtarlar birincil anahtar değerlerini atamak uygulamaları birincil anahtarları depoda üretilmiş ve varlıklara ait eski davranışı bağımlı `Added` durumu.</span><span class="sxs-lookup"><span data-stu-id="736ab-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="736ab-164">Tarafından önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="736ab-164">This can be avoided by:</span></span>
* <span data-ttu-id="736ab-165">Depoda üretilmiş anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="736ab-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="736ab-166">Yabancı anahtar değerleri ayarlamak yerine form ilişkiler için Gezinti özelliklerini ayarlama.</span><span class="sxs-lookup"><span data-stu-id="736ab-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="736ab-167">Gerçek geçici anahtar değerlerinin varlığın izleme bilgileri edinin.</span><span class="sxs-lookup"><span data-stu-id="736ab-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="736ab-168">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` geçici değer olsa bile iade `blog.Id` kendisini ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="736ab-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="736ab-169">Depoda üretilmiş anahtar değerlerini DetectChanges geliştirir</span><span class="sxs-lookup"><span data-stu-id="736ab-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="736ab-170">İzleme sorun #14616</span><span class="sxs-lookup"><span data-stu-id="736ab-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="736ab-171">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-172">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-172">**Old behavior**</span></span>

<span data-ttu-id="736ab-173">EF Core 3.0 önce izlenmeyen bir varlık tarafından bulunan `DetectChanges` olarak izlenmesi `Added` durum ve eklenen yeni bir olarak ne zaman satır `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="736ab-174">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-174">**New behavior**</span></span>

<span data-ttu-id="736ab-175">Oluşturulan anahtar değerlerini bir varlık kullanıyorsa EF Core 3.0 ile başlayan ve bazı anahtar değerini ayarlayın ve ardından varlığı olarak takip `Modified` durumu.</span><span class="sxs-lookup"><span data-stu-id="736ab-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="736ab-176">Bu varlık için bir satır var olduğu varsayılır ve bu anlamına gelir ne zaman güncelleştirilmiş `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="736ab-177">Anahtar değeri ayarlanmamış, veya varlık türü kullanmıyor oluşturulan anahtarları varsa yeni bir varlık olarak hala izlenecektir `Added` önceki sürümlerinde olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="736ab-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="736ab-178">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-178">**Why**</span></span>

<span data-ttu-id="736ab-179">Bu değişiklik, daha kolay ve bağlantısı kesilmiş varlık grafiklerle depoda üretilmiş anahtarlar kullanılırken çalışmak için daha tutarlı hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="736ab-180">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-180">**Mitigations**</span></span>

<span data-ttu-id="736ab-181">Bu değişiklik, bir varlık türü için üretilmiş anahtarlar yapılandırılır, ancak anahtar değerlerine açıkça yeni örnekleri için ayarlanan uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="736ab-182">Düzeltme üretilmiş değerleri anahtar özelliklerine açıkça yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="736ab-183">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="736ab-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="736ab-184">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="736ab-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="736ab-185">Art arda silme işlemi hemen hemen varsayılan olarak gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="736ab-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="736ab-186">İzleme sorun #10114</span><span class="sxs-lookup"><span data-stu-id="736ab-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="736ab-187">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-188">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-188">**Old behavior**</span></span>

<span data-ttu-id="736ab-189">SaveChanges çağrıldı kadar EF Core uygulanan basamaklı eylemlerinin (gerekli sorumlu silindiğinde veya ilişki gerekli sorumlusuna yazıyordunuz bağımlı varlıkları silmek) 3.0 önce gerçekleşmedi.</span><span class="sxs-lookup"><span data-stu-id="736ab-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="736ab-190">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-190">**New behavior**</span></span>

<span data-ttu-id="736ab-191">Tetikleme koşulu algılandığında hemen sonra 3.0 ile başlayarak, EF Core basamaklı eylemlerinin geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="736ab-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="736ab-192">Örneğin, çağırma `context.Remove()` asıl bir varlığı silme tüm izlenen sonucu ilgili gerekli bağımlılıklar da ayarlanacak şekilde `Deleted` hemen.</span><span class="sxs-lookup"><span data-stu-id="736ab-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="736ab-193">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-193">**Why**</span></span>

<span data-ttu-id="736ab-194">Veri bağlama ve hangi varlıkları silinecek anlamanız önemli olduğu senaryolar denetimi deneyimini geliştirmek için bu değişiklik yapılmıştır _önce_ `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="736ab-195">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-195">**Mitigations**</span></span>

<span data-ttu-id="736ab-196">Davranış ayarları aracılığıyla geri yüklenebilir `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="736ab-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="736ab-197">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="736ab-198">Sorgu türleri varlık türleri ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="736ab-199">İzleme sorun #14194</span><span class="sxs-lookup"><span data-stu-id="736ab-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="736ab-200">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-201">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-201">**Old behavior**</span></span>

<span data-ttu-id="736ab-202">EF Core 3.0 önce [sorgu türü](xref:core/modeling/query-types) olan birincil anahtar, yapılandırılmış bir biçimde tanımlamıyor verileri sorgulamak için bir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="736ab-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="736ab-203">Diğer bir deyişle, bir sorgu türü kullanıldı sırasında normal bir varlık anahtarları (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) olmadan varlık türleriyle eşlemek için bir anahtar kullanılabilir (daha büyük olasılıkla bir tablodan ancak büyük olasılıkla bir görünümden) olduğunda türü kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="736ab-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="736ab-204">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-204">**New behavior**</span></span>

<span data-ttu-id="736ab-205">Bir sorgu türü artık yalnızca birincil anahtarı olmayan bir varlık türü olur.</span><span class="sxs-lookup"><span data-stu-id="736ab-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="736ab-206">Anahtarsız varlık türleri, önceki sürümlerde sorgu türleri aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="736ab-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="736ab-207">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-207">**Why**</span></span>

<span data-ttu-id="736ab-208">Sorgu türleri amacı etrafında karışıklık azaltmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="736ab-209">Özellikle, anahtarsız varlık türleridir ve bu nedenle kendiliğinden salt okunur olduklarından, ancak yalnızca bir varlık türü salt okunur olması gerekir çünkü bunlar kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="736ab-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="736ab-210">Benzer şekilde, genellikle görünümlerine eşlenmiş, ancak yalnızca görünümleri genellikle anahtarlar tanımlama yok olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="736ab-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="736ab-211">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-211">**Mitigations**</span></span>

<span data-ttu-id="736ab-212">Aşağıdaki bölümleri API artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="736ab-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="736ab-213">**`ModelBuilder.Query<>()`** -Bunun yerine `ModelBuilder.Entity<>().HasNoKey()` hiçbir anahtarına sahip bir varlık türü işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="736ab-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="736ab-214">Bu yine de bir birincil anahtar bekleniyordu, ancak kuralı eşleşmiyor, yanlış yapılandırma önlemek için kural olarak yapılandırılamadığı.</span><span class="sxs-lookup"><span data-stu-id="736ab-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="736ab-215">**`DbQuery<>`** -Bunun yerine `DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="736ab-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="736ab-216">**`DbContext.Query<>()`** -Bunun yerine `DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="736ab-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="736ab-217">Sahip olunan tür ilişkileri için API yapılandırması değişti</span><span class="sxs-lookup"><span data-stu-id="736ab-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="736ab-218">[Sorun #12444 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sorun #9148 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sorun #14153 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="736ab-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="736ab-219">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-220">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-220">**Old behavior**</span></span>

<span data-ttu-id="736ab-221">EF Core 3.0 önce hemen sonra yapılandırma sahibi ilişkinin gerçekleştirildi `OwnsOne` veya `OwnsMany` çağırın.</span><span class="sxs-lookup"><span data-stu-id="736ab-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="736ab-222">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-222">**New behavior**</span></span>

<span data-ttu-id="736ab-223">EF Core 3.0 ile başlayarak, var. Şimdi fluent API'sini kullanarak sahibine bir gezinti özelliğini yapılandırmak için `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="736ab-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="736ab-224">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="736ab-225">Yapılandırma sahibi arasındaki ilişkiyi ilgili ve ait hemen sonra zincirlenmesi gereken `WithOwner()` diğer ilişkileri nasıl yapılandırılacağını benzer şekilde.</span><span class="sxs-lookup"><span data-stu-id="736ab-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="736ab-226">Sahip olunan türü hala sonra zincirlenmesine için yapılandırma while `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="736ab-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="736ab-227">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-227">For example:</span></span>

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

<span data-ttu-id="736ab-228">Ayrıca çağırma `Entity()`, `HasOne()`, veya `Set()` sahip olunan bir türü ile hedef şimdi bir özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="736ab-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="736ab-229">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-229">**Why**</span></span>

<span data-ttu-id="736ab-230">Sahip olunan türü yapılandırma arasında daha net bir ayrım oluşturmak için bu değişiklik yapılmıştır ve _ilişkisi_ ait türü.</span><span class="sxs-lookup"><span data-stu-id="736ab-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="736ab-231">Bu sırayla belirsizlik ve yöntemler gibi geçici karışıklık kaldırır `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="736ab-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="736ab-232">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-232">**Mitigations**</span></span>

<span data-ttu-id="736ab-233">Yukarıdaki örnekte gösterildiği gibi yeni bir API yüzeyi kullanmak için sahip olunan tür ilişkileri yapılandırmasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="736ab-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="736ab-234">Yabancı anahtar özellik kuralı asıl özelliğiyle aynı ada artık eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="736ab-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="736ab-235">İzleme sorun #13274</span><span class="sxs-lookup"><span data-stu-id="736ab-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="736ab-236">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-237">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-237">**Old behavior**</span></span>

<span data-ttu-id="736ab-238">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="736ab-238">Consider the following model:</span></span>
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
<span data-ttu-id="736ab-239">EF Core 3.0 önce `CustomerId` özelliği kullanılan Kural gereği için yabancı anahtarı.</span><span class="sxs-lookup"><span data-stu-id="736ab-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="736ab-240">Ancak, varsa `Order` bu da yapacağı sonra sahip olunan bir türü olan `CustomerId` birincil anahtar ve bu değilse genellikle beklentisi.</span><span class="sxs-lookup"><span data-stu-id="736ab-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="736ab-241">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-241">**New behavior**</span></span>

<span data-ttu-id="736ab-242">3.0 ile başlayarak, EF Core asıl özelliğiyle aynı ada sahip oldukları özellikleri yabancı anahtarlar için kural olarak kullanılacak denemez.</span><span class="sxs-lookup"><span data-stu-id="736ab-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="736ab-243">Asıl tür adı asıl özellik adıyla birleştirilmiş ve asıl özellik adı desenleri ile birleştirilmiş gezinme adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="736ab-244">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-244">For example:</span></span>

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

<span data-ttu-id="736ab-245">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-245">**Why**</span></span>

<span data-ttu-id="736ab-246">Bu değişikliği yanlışlıkla bir birincil anahtar özelliği sahip olunan türüne tanımlamamaya özen gösterin çalışıldı.</span><span class="sxs-lookup"><span data-stu-id="736ab-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="736ab-247">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-247">**Mitigations**</span></span>

<span data-ttu-id="736ab-248">Yabancı anahtar olması ve bu nedenle birincil anahtarın sonra açıkça bölüm özelliği isteniyorsa bu şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="736ab-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="736ab-249">Her bir özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır.</span><span class="sxs-lookup"><span data-stu-id="736ab-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="736ab-250">İzleme sorun #6872</span><span class="sxs-lookup"><span data-stu-id="736ab-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="736ab-251">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="736ab-252">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-252">**Old behavior**</span></span>

<span data-ttu-id="736ab-253">EF Core 3.0 önce tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="736ab-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="736ab-254">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-254">**New behavior**</span></span>

<span data-ttu-id="736ab-255">EF Core 3.0 ile başlayarak, her bir tamsayı anahtar özellik değer Oluşturucusu kendi bellek içi veritabanı kullanılırken alır.</span><span class="sxs-lookup"><span data-stu-id="736ab-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="736ab-256">Ayrıca, veritabanı silinirse, anahtar oluşturma için tüm tabloları sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="736ab-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="736ab-257">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-257">**Why**</span></span>

<span data-ttu-id="736ab-258">Bellek içi anahtar oluşturma gerçek veritabanı anahtar oluşturma için daha yakından Hizala ve bellek içi veritabanı kullanılırken testler birbirinden ayırma özelliğini artırmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="736ab-259">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-259">**Mitigations**</span></span>

<span data-ttu-id="736ab-260">Bunu ayarlamak için belirli bellek içi anahtar değerlerine bağlı olan bir uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="736ab-261">Göz önünde bulundurun yerine değil özel anahtar değerlerine bağlı olan veya yeni davranış eşleştirilecek güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="736ab-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="736ab-262">Varsayılan olarak kullanılan destekleyen alanlar</span><span class="sxs-lookup"><span data-stu-id="736ab-262">Backing fields are used by default</span></span>

[<span data-ttu-id="736ab-263">İzleme sorun #12430</span><span class="sxs-lookup"><span data-stu-id="736ab-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="736ab-264">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="736ab-265">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-265">**Old behavior**</span></span>

<span data-ttu-id="736ab-266">3.0 önce bir özellik için destek alanı biliniyordu olsa bile, EF Core yine de varsayılan olarak okuma ve özellik alıcı ve ayarlayıcı yöntemleri kullanarak özellik değeri yazma.</span><span class="sxs-lookup"><span data-stu-id="736ab-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="736ab-267">Bunun özel durumu burada destek alanı doğrudan biliniyorsa ayarlamak sorgu yürütme oluştu.</span><span class="sxs-lookup"><span data-stu-id="736ab-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="736ab-268">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-268">**New behavior**</span></span>

<span data-ttu-id="736ab-269">Bir özellik için destek alanı biliniyorsa, EF Core 3.0 ile başlayan sonra her zaman okuyabilmesini ve yazma yedekleme alanını kullanarak bu özelliği.</span><span class="sxs-lookup"><span data-stu-id="736ab-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="736ab-270">Uygulama, alıcı veya ayarlayıcı yöntemleri kodlanmış ek davranış bağlı değilse bu bir uygulama sonu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="736ab-271">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-271">**Why**</span></span>

<span data-ttu-id="736ab-272">Bu değişiklik iş mantığı deneyebileceğinizi olarak tetikleyen EF Core önlemek için varsayılan olarak varlıkları içeren bir veritabanı işlemleri gerçekleştirirken yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="736ab-273">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-273">**Mitigations**</span></span>

<span data-ttu-id="736ab-274">ModelBuilder fluent API'si özellik erişim modunda yapılandırmasını aracılığıyla öncesi 3.0 davranış geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="736ab-275">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="736ab-276">Birden çok uyumlu yedekleme alan bulunamazsa throw</span><span class="sxs-lookup"><span data-stu-id="736ab-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="736ab-277">İzleme sorun #12523</span><span class="sxs-lookup"><span data-stu-id="736ab-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="736ab-278">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="736ab-279">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-279">**Old behavior**</span></span>

<span data-ttu-id="736ab-280">Birden çok alan karşılarsa, EF Core 3.0 önce bazı öncelik sırası üzerinde bir alan seçilirdi sonra bir özelliğinin destek alanı bulma kurallarını temel.</span><span class="sxs-lookup"><span data-stu-id="736ab-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="736ab-281">Bu, belirsiz durumda kullanılacak yanlış alan neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="736ab-282">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-282">**New behavior**</span></span>

<span data-ttu-id="736ab-283">Birden çok alan aynı özelliğe eşleşirse EF Core 3.0 ile başlayarak, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="736ab-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="736ab-284">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-284">**Why**</span></span>

<span data-ttu-id="736ab-285">Yalnızca biri doğru olduğunda sessiz bir şekilde bir alanı başka bir kullanmaktan kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="736ab-286">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-286">**Mitigations**</span></span>

<span data-ttu-id="736ab-287">Belirsiz destekleyen alanlarla özellikleri, açıkça belirtilen kullanılacak bir alan olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="736ab-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="736ab-288">Örneğin, fluent API'sini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="736ab-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="736ab-289">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağırın</span><span class="sxs-lookup"><span data-stu-id="736ab-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="736ab-290">İzleme sorun #14756</span><span class="sxs-lookup"><span data-stu-id="736ab-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="736ab-291">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="736ab-292">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-292">**Old behavior**</span></span>

<span data-ttu-id="736ab-293">EF Core 3.0, çağırma önce `AddDbContext` veya `AddDbContextPool` de günlüğe kaydetme ve bellek D.I hizmetleriyle yapılan çağrılar aracılığıyla önbelleğe kaydetmek [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="736ab-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="736ab-294">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-294">**New behavior**</span></span>

<span data-ttu-id="736ab-295">EF Core 3.0 ile başlayan `AddDbContext` ve `AddDbContextPool` artık kayıt hizmetlerin bağımlılık ekleme (dı) sahip olur.</span><span class="sxs-lookup"><span data-stu-id="736ab-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="736ab-296">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-296">**Why**</span></span>

<span data-ttu-id="736ab-297">EF Core 3.0, bu hizmetler, uygulamanın DI cotainer olduğunu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="736ab-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="736ab-298">Ancak, varsa `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlı EF Core tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="736ab-299">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-299">**Mitigations**</span></span>

<span data-ttu-id="736ab-300">Uygulamanız bu hizmetlere ihtiyacı varsa, sonra bunları açıkça DI kullanarak kapsayıcı kayıt [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="736ab-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="736ab-301">DbContext.Entry artık yerel DetectChanges gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="736ab-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="736ab-302">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="736ab-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="736ab-303">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-304">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-304">**Old behavior**</span></span>

<span data-ttu-id="736ab-305">EF Core 3.0, çağırma önce `DbContext.Entry` izlenen tüm varlıklar için algılanabilir değişiklik neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="736ab-306">Bu durumu içinde kullanıma sunulan sağlamış `EntityEntry` güncel oluştu.</span><span class="sxs-lookup"><span data-stu-id="736ab-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="736ab-307">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-307">**New behavior**</span></span>

<span data-ttu-id="736ab-308">Çağırma EF Core ile 3.0, başlangıç `DbContext.Entry` ilgili asıl varlıkları yalnızca belirtilen varlık ve diğer değişikliklerini algılamak girişimi izlenen başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="736ab-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="736ab-309">Başka bir yerde değiştirir Bunun anlamı etkileri uygulama durumu olabilir. Bu yöntemi çağrılarak algılandı değil.</span><span class="sxs-lookup"><span data-stu-id="736ab-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="736ab-310">Unutmayın `ChangeTracker.AutoDetectChangesEnabled` ayarlanır `false` sonra bile bu yerel değişiklik algılama devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="736ab-311">Değişiklik algılama--örneğin neden diğer yöntemleri `ChangeTracker.Entries` ve `SaveChanges`--yine de tam neden `DetectChanges` tüm varlıkları izlenir.</span><span class="sxs-lookup"><span data-stu-id="736ab-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="736ab-312">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-312">**Why**</span></span>

<span data-ttu-id="736ab-313">Kullanmanın varsayılan performansını artırmak için bu değişiklik yapılmıştır `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="736ab-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="736ab-314">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-314">**Mitigations**</span></span>

<span data-ttu-id="736ab-315">Çağrı `ChgangeTracker.DetectChanges()` açıkça çağırmadan önce `Entry` öncesi 3.0 davranış sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="736ab-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="736ab-316">Dize ve bayt dizisi anahtarlar varsayılan olarak istemci tarafından oluşturulan değildir</span><span class="sxs-lookup"><span data-stu-id="736ab-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="736ab-317">İzleme sorun #14617</span><span class="sxs-lookup"><span data-stu-id="736ab-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="736ab-318">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="736ab-319">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-319">**Old behavior**</span></span>

<span data-ttu-id="736ab-320">EF Core 3.0 önce `string` ve `byte[]` açıkça null olmayan bir değer ayarlamadan anahtar özellikleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="736ab-321">Böyle bir durumda anahtar değeri istemcide bayt için seri hale getirilmiş bir GUID olarak oluşturulacak `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="736ab-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="736ab-322">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-322">**New behavior**</span></span>

<span data-ttu-id="736ab-323">Bir özel durum EF Core 3.0 ile başlayarak, anahtar değer kümesi gösteren oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="736ab-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="736ab-324">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-324">**Why**</span></span>

<span data-ttu-id="736ab-325">Bu değişiklik nedeniyle istemci tarafından oluşturulan yapılmıştır `string` / `byte[]` değerleri genellikle olmadığı kullanışlı ve varsayılan davranışını yaptığı sabit nedeni hakkında oluşturulan anahtar değerler için yaygın bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="736ab-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="736ab-326">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-326">**Mitigations**</span></span>

<span data-ttu-id="736ab-327">Başka bir null olmayan değer ayarlarsanız anahtar özellikleri oluşturulan değerler kullanması gerektiğini açıkça belirterek öncesi 3.0 davranış elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="736ab-328">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="736ab-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="736ab-329">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="736ab-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="736ab-330">Kapsamlı bir hizmet Iloggerfactory sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="736ab-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="736ab-331">İzleme sorun #14698</span><span class="sxs-lookup"><span data-stu-id="736ab-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="736ab-332">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-333">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-333">**Old behavior**</span></span>

<span data-ttu-id="736ab-334">EF Core 3.0 önce `ILoggerFactory` tek bir hizmet kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="736ab-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="736ab-335">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-335">**New behavior**</span></span>

<span data-ttu-id="736ab-336">EF Core 3.0 ile başlayan `ILoggerFactory` şimdi kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="736ab-337">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-337">**Why**</span></span>

<span data-ttu-id="736ab-338">Günlükçü ile ilişkisini izin vermek için bu değişiklik yapılmıştır bir `DbContext` diğer işlevleri sağlar ve bazı durumlarda pathological davranış gibi iç hizmet sağlayıcıları sayısındaki kaldıran örneği.</span><span class="sxs-lookup"><span data-stu-id="736ab-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="736ab-339">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-339">**Mitigations**</span></span>

<span data-ttu-id="736ab-340">Bu değişiklik, kaydetme ve EF Core iç hizmet sağlayıcısına özel hizmetler kullanarak sürece uygulama kodu etkilememesi.</span><span class="sxs-lookup"><span data-stu-id="736ab-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="736ab-341">Bu ortak değildir.</span><span class="sxs-lookup"><span data-stu-id="736ab-341">This isn't common.</span></span>
<span data-ttu-id="736ab-342">Bu durumlarda, bilgilerin çoğunu hala çalışır, ancak bağlı olarak herhangi bir tekil hizmeti olacak `ILoggerFactory` almak için değiştirilmesi gerekecektir `ILoggerFactory` farklı bir yolla.</span><span class="sxs-lookup"><span data-stu-id="736ab-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="736ab-343">Bu gibi durumlarda karşılaşırsanız lütfen sırasında bir sorun üzerinde dosya [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) nasıl kullandığınızı bize bildirmek `ILoggerFactory` sağlayacak şekilde biz bunu gelecekte kesmemesi nasıl daha iyi anlamak.</span><span class="sxs-lookup"><span data-stu-id="736ab-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="736ab-344">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="736ab-345">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="736ab-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="736ab-346">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-347">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-347">**Old behavior**</span></span>

<span data-ttu-id="736ab-348">`IDbContextOptionsExtensionWithDebugInfo` ek isteğe bağlı bir arabirim gelen genişletilmişse `IDbContextOptionsExtension` arabirimine 2.x sürüm döngüsü sırasında değiştirme bir hataya neden önlemek için.</span><span class="sxs-lookup"><span data-stu-id="736ab-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="736ab-349">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-349">**New behavior**</span></span>

<span data-ttu-id="736ab-350">Arabirimler artık birlikte birleştirilir `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="736ab-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="736ab-351">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-351">**Why**</span></span>

<span data-ttu-id="736ab-352">Arabirimler kavramsal olarak biri olduğundan, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="736ab-353">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-353">**Mitigations**</span></span>

<span data-ttu-id="736ab-354">Tüm uygulamaları `IDbContextOptionsExtension` yeni üye destekleyecek şekilde güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="736ab-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="736ab-355">Gezinti özellikleri tam olarak yüklü olduğundan yükleme Lazy proxy'leri artık varsayılır</span><span class="sxs-lookup"><span data-stu-id="736ab-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="736ab-356">İzleme sorun #12780</span><span class="sxs-lookup"><span data-stu-id="736ab-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="736ab-357">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="736ab-358">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-358">**Old behavior**</span></span>

<span data-ttu-id="736ab-359">EF Core 3.0, bir kez önce bir `DbContext` bırakıldı veya bir belirtilen gezinme özelliğinin bir varlığı söz konusu bağlamdan elde tam yüklü ise, olduğunu bilmesinin imkanı yoktu.</span><span class="sxs-lookup"><span data-stu-id="736ab-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="736ab-360">Proxy'leri bunun yerine null olmayan bir değer varsa bir başvuru Gezinti yüklenir ve boş değilse, bir koleksiyon Gezinti yüklenen varsayılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="736ab-361">Bu durumlarda, yavaş dışarıdan yüklemeyi deneyen bir İşlemsiz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="736ab-362">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-362">**New behavior**</span></span>

<span data-ttu-id="736ab-363">EF Core 3.0 ile başlayarak, proxy'leri bir gezinti özelliğinin yüklü olup olmadığını izler.</span><span class="sxs-lookup"><span data-stu-id="736ab-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="736ab-364">Bu yüklenen Gezinti boş veya null olduğunda bile bağlamı bırakıldıktan sonra yüklenen bir gezinti özelliği erişmeye çalışan her zaman bir İşlemsiz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="736ab-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="736ab-365">Buna karşılık, boş bir koleksiyon gezinme özelliği olsa bile bağlamı elden yüklü olmayan bir gezinti özelliği erişmeye çalışan bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="736ab-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="736ab-366">Bu durum ortaya çıkarsa, uygulama kodu, geçersiz bir zamanda yavaş yükleme kullanmak çalışıyor ve bunu yapmak için uygulama değiştirilmelidir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="736ab-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="736ab-367">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-367">**Why**</span></span>

<span data-ttu-id="736ab-368">Bu değişiklik, bırakılmış yükü yavaş çalışırken davranışını tutarlı ve doğru hale yapılmıştır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="736ab-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="736ab-369">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-369">**Mitigations**</span></span>

<span data-ttu-id="736ab-370">Uygulama kodunun yükleme yavaş bırakılmış bağlamla kullanmamanız güncelleştirin veya bu özel durum iletisini açıklandığı bir İşlemsiz olacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="736ab-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="736ab-371">İç hizmet sağlayıcılarının aşırı oluşturma artık varsayılan olarak bir hata</span><span class="sxs-lookup"><span data-stu-id="736ab-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="736ab-372">İzleme sorun #10236</span><span class="sxs-lookup"><span data-stu-id="736ab-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="736ab-373">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-374">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-374">**Old behavior**</span></span>

<span data-ttu-id="736ab-375">EF Core 3.0 önce bir uyarı iç hizmet sağlayıcıları pathological bir dizi oluşturmak için bir uygulama oturum açmış olmanız.</span><span class="sxs-lookup"><span data-stu-id="736ab-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="736ab-376">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-376">**New behavior**</span></span>

<span data-ttu-id="736ab-377">EF Core 3.0 ile başlayarak, bu uyarı artık olarak kabul edilir ve hata ve özel durum harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="736ab-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="736ab-378">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-378">**Why**</span></span>

<span data-ttu-id="736ab-379">Bu değişiklik, daha açık pathological bu durumda gösterme yoluyla uygulama kodu daha iyi desteklemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="736ab-380">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-380">**Mitigations**</span></span>

<span data-ttu-id="736ab-381">Bu hata ile karşılaşma eylemde en uygun nedeni, kök nedenini anlamak ve çok sayıda iç hizmet sağlayıcıları oluşturmayı durdur oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="736ab-382">Ancak, hata bir uyarı olarak geri dönüştürülür (veya yok sayıldı) aracılığıyla yapılandırma `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="736ab-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="736ab-383">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="736ab-384">Tek bir dize ile HasOne/çok ilişkin yeni davranış çağırılır</span><span class="sxs-lookup"><span data-stu-id="736ab-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="736ab-385">İzleme sorun #9171</span><span class="sxs-lookup"><span data-stu-id="736ab-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="736ab-386">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="736ab-387">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-387">**Old behavior**</span></span>

<span data-ttu-id="736ab-388">EF Core 3.0 önce kod arama `HasOne` veya `HasMany` tek bir dize ile yorumlandığı kafa karıştırıcı bir şekilde oluştu.</span><span class="sxs-lookup"><span data-stu-id="736ab-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="736ab-389">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="736ab-390">Kod ilgili Görülüyor `Samuri` bazı diğer varlık türü kullanarak `Entrance` özel gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="736ab-390">The code looks like it is relating `Samuri` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="736ab-391">Bazı varlık türü olarak adlandırılan bir ilişki oluşturmak bu kodu gerçekte çalışır `Entrance` ile gezinti özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="736ab-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="736ab-392">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-392">**New behavior**</span></span>

<span data-ttu-id="736ab-393">EF Core 3.0 ile başlayarak, yukarıdaki kod artık önce yapmakta gibi hangi Aranan yapar.</span><span class="sxs-lookup"><span data-stu-id="736ab-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="736ab-394">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-394">**Why**</span></span>

<span data-ttu-id="736ab-395">Eski davranışı özellikle yapılandırma kodu okurken ve hatalarını arayarak çok karmaşık.</span><span class="sxs-lookup"><span data-stu-id="736ab-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="736ab-396">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-396">**Mitigations**</span></span>

<span data-ttu-id="736ab-397">Bu, yalnızca tür adları ve gezinme özelliğini açıkça belirtmeden dizeleriyle ilişkileri açıkça yapılandırmakta olduğunuz uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="736ab-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="736ab-398">Bu yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="736ab-398">This is not common.</span></span>
<span data-ttu-id="736ab-399">Önceki davranışı açıkça geçirme aracılığıyla alınabilir `null` gezinme özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="736ab-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="736ab-400">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="736ab-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="736ab-401">İlişkisel: TypeMapping ek açıklama yalnızca TypeMapping sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="736ab-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="736ab-402">İzleme sorun #9913</span><span class="sxs-lookup"><span data-stu-id="736ab-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="736ab-403">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="736ab-404">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-404">**Old behavior**</span></span>

<span data-ttu-id="736ab-405">Tür eşlemesi ek açıklamaları için ek açıklama "İlişkisel: TypeMapping" adlandırılmıştı.</span><span class="sxs-lookup"><span data-stu-id="736ab-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="736ab-406">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-406">**New behavior**</span></span>

<span data-ttu-id="736ab-407">Ek açıklama türü eşleme ek açıklamalar için artık "TypeMapping" addır.</span><span class="sxs-lookup"><span data-stu-id="736ab-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="736ab-408">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-408">**Why**</span></span>

<span data-ttu-id="736ab-409">Türü eşlemeleri artık birden fazla yalnızca ilişkisel veritabanı sağlayıcıları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="736ab-410">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-410">**Mitigations**</span></span>

<span data-ttu-id="736ab-411">Bu, yalnızca tür eşlemesi ortak olmayan doğrudan bir ek açıklama, olarak erişen uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="736ab-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="736ab-412">API yüzeyi için ek açıklama doğrudan kullanmak yerine erişim türü eşlemeleri düzeltmek için en uygun eylemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="736ab-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="736ab-413">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="736ab-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="736ab-414">İzleme sorun #11811</span><span class="sxs-lookup"><span data-stu-id="736ab-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="736ab-415">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-416">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-416">**Old behavior**</span></span>

<span data-ttu-id="736ab-417">EF Core 3.0 önce `ToTable()` türetilmiş üzerinde adlı türü yoksayılan beri tek devralma eşleme stratejisi TPH burada bu geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="736ab-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="736ab-418">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-418">**New behavior**</span></span>

<span data-ttu-id="736ab-419">EF Core 3.0 ve sonraki bir sürümde TPT ve TPC desteği eklemek için hazırlık başlangıç `ToTable()` beklenmeyen eşleme değişiklik gelecekte önlemek için bir özel durum bir türetilmiş tür şimdi throw üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="736ab-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="736ab-420">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-420">**Why**</span></span>

<span data-ttu-id="736ab-421">Şu anda farklı bir tabloya türetilmiş bir tür eşlemek için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="736ab-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="736ab-422">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda bozucu önler.</span><span class="sxs-lookup"><span data-stu-id="736ab-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="736ab-423">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-423">**Mitigations**</span></span>

<span data-ttu-id="736ab-424">Türetilen türlerin diğer tablolara eşleme girişimleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="736ab-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="736ab-425">ForSqlServerHasIndex HasIndex ile değiştirildi</span><span class="sxs-lookup"><span data-stu-id="736ab-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="736ab-426">İzleme sorun #12366</span><span class="sxs-lookup"><span data-stu-id="736ab-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="736ab-427">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-428">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-428">**Old behavior**</span></span>

<span data-ttu-id="736ab-429">EF Core 3.0 önce `ForSqlServerHasIndex().ForSqlServerInclude()` ile kullanılan sütunları yapılandırmak üzere bir yöntem sağlayan `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="736ab-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="736ab-430">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-430">**New behavior**</span></span>

<span data-ttu-id="736ab-431">EF Core 3.0 ile başlayarak, kullanarak `Include` dizin ilişkisel düzeyinde artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="736ab-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="736ab-432">Kullanım `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="736ab-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="736ab-433">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-433">**Why**</span></span>

<span data-ttu-id="736ab-434">API ile dizin için birleştirmek için bu değişiklik yapılmıştır `Includes` tüm sağlayıcıları veritabanı için içine yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="736ab-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="736ab-435">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-435">**Mitigations**</span></span>

<span data-ttu-id="736ab-436">Yukarıda da gösterildiği gibi yeni API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="736ab-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="736ab-437">EF Core artık SQLite FK zorlama pragma gönderir</span><span class="sxs-lookup"><span data-stu-id="736ab-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="736ab-438">İzleme sorun #12151</span><span class="sxs-lookup"><span data-stu-id="736ab-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="736ab-439">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="736ab-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="736ab-440">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-440">**Old behavior**</span></span>

<span data-ttu-id="736ab-441">EF Core 3.0 önce EF Core gönderir gibi `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="736ab-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="736ab-442">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-442">**New behavior**</span></span>

<span data-ttu-id="736ab-443">EF Core 3.0 ile başlayarak, EF Core artık gönderir `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="736ab-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="736ab-444">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-444">**Why**</span></span>

<span data-ttu-id="736ab-445">EF Core kullandığından bu değişiklik yapılmıştır `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak, hangi sırayla anlamına gelir FK zorlama varsayılan olarak açık ve açıkça bir bağlantı her açıldığında etkinleştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="736ab-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="736ab-446">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-446">**Mitigations**</span></span>

<span data-ttu-id="736ab-447">Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="736ab-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="736ab-448">Diğer durumlarda, yabancı anahtarlar belirterek etkinleştirilebilir `Foreign Keys=True` bağlantı dizenizi içinde.</span><span class="sxs-lookup"><span data-stu-id="736ab-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="736ab-449">Microsoft.EntityFrameworkCore.Sqlite üzerinde SQLitePCLRaw.bundle_e_sqlite3 artık bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="736ab-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="736ab-450">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="736ab-450">**Old behavior**</span></span>

<span data-ttu-id="736ab-451">EF Core 3.0 önce EF Core kullanılan `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="736ab-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="736ab-452">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="736ab-452">**New behavior**</span></span>

<span data-ttu-id="736ab-453">EF Core 3.0 ile başlayarak, EF Core kullanan `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="736ab-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="736ab-454">**Neden**</span><span class="sxs-lookup"><span data-stu-id="736ab-454">**Why**</span></span>

<span data-ttu-id="736ab-455">Diğer platformlar ile tutarlı ios'ta kullanılan SQLite sürümü, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="736ab-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="736ab-456">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="736ab-456">**Mitigations**</span></span>

<span data-ttu-id="736ab-457">İos'ta yerel bir SQLite sürümü kullanmak için yapılandırma `Microsoft.Data.Sqlite` farklı bir kullanılacak `SQLitePCLRaw` paket.</span><span class="sxs-lookup"><span data-stu-id="736ab-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>
