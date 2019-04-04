---
title: EF Core 3.0 - EF Core yeni değişiklikler
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: fd593b2832a5a6ffe27cd4493127b5d405f684ba
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914133"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="35ec0-102">EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil edilen değişiklikler</span><span class="sxs-lookup"><span data-stu-id="35ec0-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="35ec0-103">Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="35ec0-104">Aşağıdaki API ve davranışı değişiklikleri EF Core için geliştirilen uygulamaları ayırmak için olası sahip bunları 3.0.0 için yükseltirken 2.2.x.</span><span class="sxs-lookup"><span data-stu-id="35ec0-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="35ec0-105">Veritabanı sağlayıcıları yalnızca etkileyen bekliyoruz değişiklikleri altında belgelenir [sağlayıcısı değişiklikleri](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="35ec0-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="35ec0-106">Bir 3.0 Önizlemesi'nden başka bir 3.0 Önizleme için kullanıma sunulan yeni özellikler sonlarını burada belgelenmemiş.</span><span class="sxs-lookup"><span data-stu-id="35ec0-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="35ec0-107">LINQ sorguları, artık istemcide değerlendirilir</span><span class="sxs-lookup"><span data-stu-id="35ec0-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="35ec0-108">[Sorun #14935 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[sorun #12795 Ayrıca bkz:](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="35ec0-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="35ec0-109">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-110">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-110">Old behavior</span></span>**

<span data-ttu-id="35ec0-111">EF Core, SQL veya bir parametre için bir sorgunun parçası olan bir ifade dönüştürülemedi, 3.0 önce bunu otomatik olarak istemcide ifadesi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="35ec0-112">Varsayılan olarak, istemci değerlendirmesi yüksek maliyetlere neden olabilecek bir ifade yalnızca bir uyarı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

**<span data-ttu-id="35ec0-113">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-113">New behavior</span></span>**

<span data-ttu-id="35ec0-114">3.0 ile başlayarak, EF Core yalnızca ifadeleri üst düzey projeksiyon izin verir (son `Select()` sorguda çağrısı) istemcide değerlendirilecek.</span><span class="sxs-lookup"><span data-stu-id="35ec0-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="35ec0-115">İfadeleri herhangi bir sorgunun parçası olarak SQL veya bir parametre olarak dönüştürülemez, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

**<span data-ttu-id="35ec0-116">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-116">Why</span></span>**

<span data-ttu-id="35ec0-117">Otomatik istemci değerlendirme sorguların bile bunları önemli bölümleri çevrilemez yürütülecek çok sayıda sorgu sağlar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="35ec0-118">Bu davranış yalnızca üretimde yetkisiz değiştirmeye karşı korumalı hale gelebilir beklenmeyen ve zararlı davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="35ec0-119">Örneğin, bir koşulda bir `Where()` çevrilemez çağrısı, veritabanı sunucusu ve istemcide uygulanması için filtreyi aktarılacak tablosundan tüm satırları açabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="35ec0-120">Tablo, geliştirme, ancak isabet sabit yalnızca birkaç satır içeriyorsa uygulama üretime burada tablo milyonlarca satır içerebilir taşındığında bu durum kolayca algılanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="35ec0-121">İstemci değerlendirme uyarılar ayrıca geliştirme sırasında göz ardı edilmesi çok kolay kanıtladılar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="35ec0-122">Belirli ifadeler için iyileştirme sorgu çevirisi sürümler arasında istenmeyen bozucu değişiklikleri nedeniyle oluşan sorunları bu yanı sıra, otomatik istemci değerlendirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

**<span data-ttu-id="35ec0-123">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-123">Mitigations</span></span>**

<span data-ttu-id="35ec0-124">Bir sorgu tam olarak çevrilemez sonra ya da çevrilebilir veya kullanan bir form sorguda yeniden `AsEnumerable()`, `ToList()`, veya açıkça verileri Burada, ardından daha büyük olabilir istemciye geri getirmek için benzer LINQ nesneleri kullanılarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="35ec0-125">Entity Framework Core artık ASP.NET Core paylaşılan framework'ün bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="35ec0-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="35ec0-126">İzleme sorunu duyuruları #325</span><span class="sxs-lookup"><span data-stu-id="35ec0-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="35ec0-127">Bu değişiklik, ASP.NET Core 3.0 Önizleme 1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

**<span data-ttu-id="35ec0-128">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-128">Old behavior</span></span>**

<span data-ttu-id="35ec0-129">ASP.NET Core 3.0 Paketi başvurusu eklendiğinde önce `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`, EF Core içerir ve EF Core veri sağlayıcıları bazıları istediğiniz SQL Server sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

**<span data-ttu-id="35ec0-130">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-130">New behavior</span></span>**

<span data-ttu-id="35ec0-131">3. 0'dan başlayarak, ASP.NET Core paylaşılan çerçeve EF Core veya tüm EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="35ec0-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

**<span data-ttu-id="35ec0-132">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-132">Why</span></span>**

<span data-ttu-id="35ec0-133">Bu değişiklikten önce EF Core alma olup uygulama ASP.NET Core ve SQL Server veya hedeflenen bağlı olarak farklı adımlar gereklidir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="35ec0-134">Ayrıca, ASP.NET Core yükseltme EF Core ve SQL Server sağlayıcısı, her zaman tercih değildir yükseltmesini zorlandı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="35ec0-135">Bu değişiklik, EF Core alma aynı tüm sağlayıcıları, desteklenen .NET uygulamaları ve uygulama türleri arasında deneyimidir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="35ec0-136">Geliştiriciler, tam olarak EF Core ve EF Core veri sağlayıcıları yükseltildiğinde de denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35ec0-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

**<span data-ttu-id="35ec0-137">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-137">Mitigations</span></span>**

<span data-ttu-id="35ec0-138">EF çekirdekli ASP.NET Core 3.0 uygulama veya desteklenen başka bir uygulama içinde kullanmak için açıkça uygulamanızın kullanacağı EF Core veritabanı sağlayıcısı için bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="35ec0-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="35ec0-139">SQL ve ExecuteSql ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="35ec0-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="35ec0-140">İzleme sorun #10996</span><span class="sxs-lookup"><span data-stu-id="35ec0-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="35ec0-141">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-142">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-142">Old behavior</span></span>**

<span data-ttu-id="35ec0-143">EF Core 3.0 önce bu yöntem adlarının bir normal bir dize ya da SQL ve parametreleri ilişkilendirilmiş bir dize ile çalışmak için aşırı yüklenmiş.</span><span class="sxs-lookup"><span data-stu-id="35ec0-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

**<span data-ttu-id="35ec0-144">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-144">New behavior</span></span>**

<span data-ttu-id="35ec0-145">EF Core 3.0 ile başlayarak, kullanmaktadır `FromSqlRaw`, `ExecuteSqlRaw`, ve `ExecuteSqlRawAsync` burada parametreleri geçirilir ayrı olarak Sorgu dizesinden parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="35ec0-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="35ec0-146">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="35ec0-147">Kullanım `FromSqlInterpolated`, `ExecuteSqlInterpolated`, ve `ExecuteSqlInterpolatedAsync` burada parametreleri geçirilir ilişkilendirilmiş sorgu dizesinin parçası olarak parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="35ec0-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="35ec0-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="35ec0-149">Yukarıdaki sorgu her ikisi de aynı SQL parametrelere sahip aynı parametreli SQL üretecektir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

**<span data-ttu-id="35ec0-150">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-150">Why</span></span>**

<span data-ttu-id="35ec0-151">Yöntem aşırı yüklemeleri böyle güvenmelidir yanı sıra ilişkilendirilmiş dize yöntemini çağırmak için hedefi olduğu zaman yanlışlıkla ham srting yöntemini çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="35ec0-152">Bu, verilmiş olması, parametreli getirilemedi sorgularda neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-152">This could result in queries not being parameterized when they should have been.</span></span>

**<span data-ttu-id="35ec0-153">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-153">Mitigations</span></span>**

<span data-ttu-id="35ec0-154">Yeni yöntem adları kullanılacak anahtar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="35ec0-155">Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir</span><span class="sxs-lookup"><span data-stu-id="35ec0-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="35ec0-156">İzleme sorun #14523</span><span class="sxs-lookup"><span data-stu-id="35ec0-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="35ec0-157">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-158">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-158">Old behavior</span></span>**

<span data-ttu-id="35ec0-159">EF Core 3.0 önce sorgu ve diğer komutları yürütme sırasında günlüğe kaydedilen `Info` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

**<span data-ttu-id="35ec0-160">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-160">New behavior</span></span>**

<span data-ttu-id="35ec0-161">EF Core 3.0 ile başlayarak, komut/SQL yürütme günlüğe kaydetme sırasında işlemi `Debug` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

**<span data-ttu-id="35ec0-162">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-162">Why</span></span>**

<span data-ttu-id="35ec0-163">Konumundaki paraziti azaltmak için bu değişiklik yapılmıştır `Info` günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-163">This change was made to reduce the noise at the `Info` log level.</span></span>

**<span data-ttu-id="35ec0-164">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-164">Mitigations</span></span>**

<span data-ttu-id="35ec0-165">Bu günlük olayı tarafından tanımlanan `RelationalEventId.CommandExecuting` 20100 olay kimliği.</span><span class="sxs-lookup"><span data-stu-id="35ec0-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="35ec0-166">SQL oturum `Info` yeniden düzey, düzeyi açıkça yapılandırmanıza `OnConfiguring` veya `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="35ec0-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="35ec0-168">Geçici bir anahtar değere artık varlık örneklerini ayarlanır</span><span class="sxs-lookup"><span data-stu-id="35ec0-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="35ec0-169">İzleme sorun #12378</span><span class="sxs-lookup"><span data-stu-id="35ec0-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="35ec0-170">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="35ec0-171">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-171">Old behavior</span></span>**

<span data-ttu-id="35ec0-172">EF Core 3.0 önce veritabanı tarafından oluşturulan gerçek değer daha sonra sahip olabileceği tüm anahtar özellikleri geçici değerler atanmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="35ec0-173">Genellikle bu geçici değerleri büyük negatif sayılar yoktu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-173">Usually these temporary values were large negative numbers.</span></span>

**<span data-ttu-id="35ec0-174">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-174">New behavior</span></span>**

<span data-ttu-id="35ec0-175">3.0 ile başlayarak, EF Core, varlığın izleme bilgilerini bir parçası olarak geçici bir anahtar değeri depolar ve anahtar özelliği kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

**<span data-ttu-id="35ec0-176">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-176">Why</span></span>**

<span data-ttu-id="35ec0-177">Geçici bir anahtar değere deneyebileceğinizi daha önce bazı tarafından izlenen bir varlık kalıcı hale gelmesini önlemek için bu değişiklik yapılmıştır `DbContext` örneği farklı bir taşınır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="35ec0-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

**<span data-ttu-id="35ec0-178">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-178">Mitigations</span></span>**

<span data-ttu-id="35ec0-179">Varlıklar arasında ilişkilendirmeler form üzerine yabancı anahtarlar birincil anahtar değerlerini atamak uygulamaları birincil anahtarları depoda üretilmiş ve varlıklara ait eski davranışı bağımlı `Added` durumu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="35ec0-180">Tarafından önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="35ec0-180">This can be avoided by:</span></span>
* <span data-ttu-id="35ec0-181">Depoda üretilmiş anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="35ec0-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="35ec0-182">Yabancı anahtar değerleri ayarlamak yerine form ilişkiler için Gezinti özelliklerini ayarlama.</span><span class="sxs-lookup"><span data-stu-id="35ec0-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="35ec0-183">Gerçek geçici anahtar değerlerinin varlığın izleme bilgileri edinin.</span><span class="sxs-lookup"><span data-stu-id="35ec0-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="35ec0-184">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` geçici değer olsa bile iade `blog.Id` kendisini ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="35ec0-185">Depoda üretilmiş anahtar değerlerini DetectChanges geliştirir</span><span class="sxs-lookup"><span data-stu-id="35ec0-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="35ec0-186">İzleme sorun #14616</span><span class="sxs-lookup"><span data-stu-id="35ec0-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="35ec0-187">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-188">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-188">Old behavior</span></span>**

<span data-ttu-id="35ec0-189">EF Core 3.0 önce izlenmeyen bir varlık tarafından bulunan `DetectChanges` olarak izlenmesi `Added` durum ve eklenen yeni bir olarak ne zaman satır `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

**<span data-ttu-id="35ec0-190">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-190">New behavior</span></span>**

<span data-ttu-id="35ec0-191">Oluşturulan anahtar değerlerini bir varlık kullanıyorsa EF Core 3.0 ile başlayan ve bazı anahtar değerini ayarlayın ve ardından varlığı olarak takip `Modified` durumu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="35ec0-192">Bu varlık için bir satır var olduğu varsayılır ve bu anlamına gelir ne zaman güncelleştirilmiş `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="35ec0-193">Anahtar değeri ayarlanmamış, veya varlık türü kullanmıyor oluşturulan anahtarları varsa yeni bir varlık olarak hala izlenecektir `Added` önceki sürümlerinde olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

**<span data-ttu-id="35ec0-194">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-194">Why</span></span>**

<span data-ttu-id="35ec0-195">Bu değişiklik, daha kolay ve bağlantısı kesilmiş varlık grafiklerle depoda üretilmiş anahtarlar kullanılırken çalışmak için daha tutarlı hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

**<span data-ttu-id="35ec0-196">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-196">Mitigations</span></span>**

<span data-ttu-id="35ec0-197">Bu değişiklik, bir varlık türü için üretilmiş anahtarlar yapılandırılır, ancak anahtar değerlerine açıkça yeni örnekleri için ayarlanan uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="35ec0-198">Düzeltme üretilmiş değerleri anahtar özelliklerine açıkça yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="35ec0-199">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="35ec0-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="35ec0-200">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="35ec0-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="35ec0-201">Art arda silme işlemi hemen hemen varsayılan olarak gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="35ec0-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="35ec0-202">İzleme sorun #10114</span><span class="sxs-lookup"><span data-stu-id="35ec0-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="35ec0-203">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-204">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-204">Old behavior</span></span>**

<span data-ttu-id="35ec0-205">SaveChanges çağrıldı kadar EF Core uygulanan basamaklı eylemlerinin (gerekli sorumlu silindiğinde veya ilişki gerekli sorumlusuna yazıyordunuz bağımlı varlıkları silmek) 3.0 önce gerçekleşmedi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

**<span data-ttu-id="35ec0-206">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-206">New behavior</span></span>**

<span data-ttu-id="35ec0-207">Tetikleme koşulu algılandığında hemen sonra 3.0 ile başlayarak, EF Core basamaklı eylemlerinin geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="35ec0-208">Örneğin, çağırma `context.Remove()` asıl bir varlığı silme tüm izlenen sonucu ilgili gerekli bağımlılıklar da ayarlanacak şekilde `Deleted` hemen.</span><span class="sxs-lookup"><span data-stu-id="35ec0-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

**<span data-ttu-id="35ec0-209">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-209">Why</span></span>**

<span data-ttu-id="35ec0-210">Veri bağlama ve hangi varlıkları silinecek anlamanız önemli olduğu senaryolar denetimi deneyimini geliştirmek için bu değişiklik yapılmıştır _önce_ `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

**<span data-ttu-id="35ec0-211">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-211">Mitigations</span></span>**

<span data-ttu-id="35ec0-212">Davranış ayarları aracılığıyla geri yüklenebilir `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="35ec0-213">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="35ec0-214">Sorgu türleri varlık türleri ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-214">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="35ec0-215">İzleme sorun #14194</span><span class="sxs-lookup"><span data-stu-id="35ec0-215">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="35ec0-216">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-217">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-217">Old behavior</span></span>**

<span data-ttu-id="35ec0-218">EF Core 3.0 önce [sorgu türü](xref:core/modeling/query-types) olan birincil anahtar, yapılandırılmış bir biçimde tanımlamıyor verileri sorgulamak için bir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-218">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="35ec0-219">Diğer bir deyişle, bir sorgu türü kullanıldı sırasında normal bir varlık anahtarları (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) olmadan varlık türleriyle eşlemek için bir anahtar kullanılabilir (daha büyük olasılıkla bir tablodan ancak büyük olasılıkla bir görünümden) olduğunda türü kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-219">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

**<span data-ttu-id="35ec0-220">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-220">New behavior</span></span>**

<span data-ttu-id="35ec0-221">Bir sorgu türü artık yalnızca birincil anahtarı olmayan bir varlık türü olur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-221">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="35ec0-222">Anahtarsız varlık türleri, önceki sürümlerde sorgu türleri aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-222">Keyless entity types have the same functionality as query types in previous versions.</span></span>

**<span data-ttu-id="35ec0-223">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-223">Why</span></span>**

<span data-ttu-id="35ec0-224">Sorgu türleri amacı etrafında karışıklık azaltmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-224">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="35ec0-225">Özellikle, anahtarsız varlık türleridir ve bu nedenle kendiliğinden salt okunur olduklarından, ancak yalnızca bir varlık türü salt okunur olması gerekir çünkü bunlar kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-225">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="35ec0-226">Benzer şekilde, genellikle görünümlerine eşlenmiş, ancak yalnızca görünümleri genellikle anahtarlar tanımlama yok olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-226">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

**<span data-ttu-id="35ec0-227">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-227">Mitigations</span></span>**

<span data-ttu-id="35ec0-228">Aşağıdaki bölümleri API artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="35ec0-228">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="35ec0-229">**`ModelBuilder.Query<>()`** -Bunun yerine `ModelBuilder.Entity<>().HasNoKey()` hiçbir anahtarına sahip bir varlık türü işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-229">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="35ec0-230">Bu yine de bir birincil anahtar bekleniyordu, ancak kuralı eşleşmiyor, yanlış yapılandırma önlemek için kural olarak yapılandırılamadığı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-230">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="35ec0-231">**`DbQuery<>`** -Bunun yerine `DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-231">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="35ec0-232">**`DbContext.Query<>()`** -Bunun yerine `DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-232">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="35ec0-233">Sahip olunan tür ilişkileri için API yapılandırması değişti</span><span class="sxs-lookup"><span data-stu-id="35ec0-233">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="35ec0-234">[Sorun #12444 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sorun #9148 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sorun #14153 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="35ec0-234">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="35ec0-235">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-235">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-236">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-236">Old behavior</span></span>**

<span data-ttu-id="35ec0-237">EF Core 3.0 önce hemen sonra yapılandırma sahibi ilişkinin gerçekleştirildi `OwnsOne` veya `OwnsMany` çağırın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-237">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

**<span data-ttu-id="35ec0-238">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-238">New behavior</span></span>**

<span data-ttu-id="35ec0-239">EF Core 3.0 ile başlayarak, var. Şimdi fluent API'sini kullanarak sahibine bir gezinti özelliğini yapılandırmak için `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-239">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="35ec0-240">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-240">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="35ec0-241">Yapılandırma sahibi arasındaki ilişkiyi ilgili ve ait hemen sonra zincirlenmesi gereken `WithOwner()` diğer ilişkileri nasıl yapılandırılacağını benzer şekilde.</span><span class="sxs-lookup"><span data-stu-id="35ec0-241">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="35ec0-242">Sahip olunan türü hala sonra zincirlenmesine için yapılandırma while `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-242">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="35ec0-243">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-243">For example:</span></span>

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

<span data-ttu-id="35ec0-244">Ayrıca çağırma `Entity()`, `HasOne()`, veya `Set()` sahip olunan bir türü ile hedef şimdi bir özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="35ec0-244">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

**<span data-ttu-id="35ec0-245">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-245">Why</span></span>**

<span data-ttu-id="35ec0-246">Sahip olunan türü yapılandırma arasında daha net bir ayrım oluşturmak için bu değişiklik yapılmıştır ve _ilişkisi_ ait türü.</span><span class="sxs-lookup"><span data-stu-id="35ec0-246">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="35ec0-247">Bu sırayla belirsizlik ve yöntemler gibi geçici karışıklık kaldırır `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-247">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

**<span data-ttu-id="35ec0-248">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-248">Mitigations</span></span>**

<span data-ttu-id="35ec0-249">Yukarıdaki örnekte gösterildiği gibi yeni bir API yüzeyi kullanmak için sahip olunan tür ilişkileri yapılandırmasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="35ec0-249">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="35ec0-250">Bağımlı varlıkları tablo sorumlusuyla paylaşımı artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="35ec0-250">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="35ec0-251">İzleme sorun #9005</span><span class="sxs-lookup"><span data-stu-id="35ec0-251">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="35ec0-252">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-252">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-253">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-253">Old behavior</span></span>**

<span data-ttu-id="35ec0-254">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="35ec0-254">Consider the following model:</span></span>
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
<span data-ttu-id="35ec0-255">3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya aynı tabloya açıkça eşleştirilmiş bir `OrderDetails` örneği her zaman gerekli yeni bir eklerken `Order`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-255">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


**<span data-ttu-id="35ec0-256">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-256">New behavior</span></span>**

<span data-ttu-id="35ec0-257">3.0 ile başlayarak, EF Core eklemek için sağlayan bir `Order` olmadan bir `OrderDetails` ve tüm eşler `OrderDetails` özellikleri hariç null yapılabilir sütunlar için birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-257">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="35ec0-258">EF Core kümeleri sorgulanırken `OrderDetails` için `null` gerekli özelliklerinden birini yoksa, bir değer veya birincil anahtarı yanı sıra gereken özellikleri yoktur ve tüm özellikleri `null`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-258">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

**<span data-ttu-id="35ec0-259">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-259">Mitigations</span></span>**

<span data-ttu-id="35ec0-260">İsteğe bağlı tüm sütunlar eklenmiş bağımlı paylaşımı tablo modelinizi varken işaret Gezinti olması beklenmiyor `null` Gezinti olduğunda durumlarında uygulama değiştirilmelidir sonra `null`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-260">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="35ec0-261">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmesini veya en az bir özelliğine sahip olmayan bir`null` atanmış değer.</span><span class="sxs-lookup"><span data-stu-id="35ec0-261">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="35ec0-262">Bir özelliğini eşleştirmek tüm varlıklar bir eşzamanlılık belirteci sütun içeren bir tablo paylaşımı sahip</span><span class="sxs-lookup"><span data-stu-id="35ec0-262">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="35ec0-263">İzleme sorun #14154</span><span class="sxs-lookup"><span data-stu-id="35ec0-263">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="35ec0-264">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-264">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-265">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-265">Old behavior</span></span>**

<span data-ttu-id="35ec0-266">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="35ec0-266">Consider the following model:</span></span>
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
<span data-ttu-id="35ec0-267">3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya yalnızca güncelleştirme aynı tabloya açıkça eşlenen `OrderDetails` değil güncelleştirecek `Version` değeri istemcisi ve İleri güncelleştirmeyi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-267">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


**<span data-ttu-id="35ec0-268">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-268">New behavior</span></span>**

<span data-ttu-id="35ec0-269">EF Core 3.0 ile başlayarak, yeni yayınlar `Version` değerini `Order` bunu sahipse `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-269">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="35ec0-270">Aksi takdirde model doğrulama sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-270">Otherwise an exception is thrown during model validation.</span></span>

**<span data-ttu-id="35ec0-271">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-271">Why</span></span>**

<span data-ttu-id="35ec0-272">Eski eşzamanlılık belirteç değeri aynı tabloya eşlenen varlıkları yalnızca biri güncelleştirildiğinde önlemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-272">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

**<span data-ttu-id="35ec0-273">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-273">Mitigations</span></span>**

<span data-ttu-id="35ec0-274">Eşzamanlılık belirteci sütuna eşlenmiş bir özellik içerir tabloda paylaşımı tüm varlıklar gerekir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-274">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="35ec0-275">Mümkündür oluşturma bir gölge durumu:</span><span class="sxs-lookup"><span data-stu-id="35ec0-275">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="35ec0-276">Tüm türetilmiş türler için tek bir sütun eşlenmemiş türlerden devralınan özellikler artık eşlenir</span><span class="sxs-lookup"><span data-stu-id="35ec0-276">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="35ec0-277">İzleme sorun #13998</span><span class="sxs-lookup"><span data-stu-id="35ec0-277">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="35ec0-278">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-279">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-279">Old behavior</span></span>**

<span data-ttu-id="35ec0-280">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="35ec0-280">Consider the following model:</span></span>
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

<span data-ttu-id="35ec0-281">EF Core 3.0 önce `ShippingAddress` özelliği için sütunları ayırmak için eşlenebilir `BulkOrder` ve `Order` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="35ec0-281">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

**<span data-ttu-id="35ec0-282">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-282">New behavior</span></span>**

<span data-ttu-id="35ec0-283">3.0 ile başlayarak, EF Core yalnızca bir sütun için oluşturur `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-283">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

**<span data-ttu-id="35ec0-284">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-284">Why</span></span>**

<span data-ttu-id="35ec0-285">Eski davranışı beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-285">The old behavoir was unexpected.</span></span>

**<span data-ttu-id="35ec0-286">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-286">Mitigations</span></span>**

<span data-ttu-id="35ec0-287">Özellik sütunu türetilmiş türler üzerinde ayırmak için açıkça eşlenebilir:</span><span class="sxs-lookup"><span data-stu-id="35ec0-287">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="35ec0-288">Yabancı anahtar özellik kuralı asıl özelliğiyle aynı ada artık eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="35ec0-288">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="35ec0-289">İzleme sorun #13274</span><span class="sxs-lookup"><span data-stu-id="35ec0-289">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="35ec0-290">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-290">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-291">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-291">Old behavior</span></span>**

<span data-ttu-id="35ec0-292">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="35ec0-292">Consider the following model:</span></span>
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
<span data-ttu-id="35ec0-293">EF Core 3.0 önce `CustomerId` özelliği kullanılan Kural gereği için yabancı anahtarı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-293">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="35ec0-294">Ancak, varsa `Order` bu da yapacağı sonra sahip olunan bir türü olan `CustomerId` birincil anahtar ve bu değilse genellikle beklentisi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-294">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

**<span data-ttu-id="35ec0-295">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-295">New behavior</span></span>**

<span data-ttu-id="35ec0-296">3.0 ile başlayarak, EF Core asıl özelliğiyle aynı ada sahip oldukları özellikleri için yabancı anahtarlar kurala göre kullanırsanız dener.</span><span class="sxs-lookup"><span data-stu-id="35ec0-296">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="35ec0-297">Asıl tür adı asıl özellik adıyla birleştirilmiş ve asıl özellik adı desenleri ile birleştirilmiş gezinme adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-297">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="35ec0-298">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-298">For example:</span></span>

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

**<span data-ttu-id="35ec0-299">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-299">Why</span></span>**

<span data-ttu-id="35ec0-300">Bu değişikliği yanlışlıkla bir birincil anahtar özelliği sahip olunan türüne tanımlamamaya özen gösterin çalışıldı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-300">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

**<span data-ttu-id="35ec0-301">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-301">Mitigations</span></span>**

<span data-ttu-id="35ec0-302">Yabancı anahtar olması ve bu nedenle birincil anahtarın sonra açıkça bölüm özelliği isteniyorsa bu şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-302">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="35ec0-303">Veritabanı bağlantısı artık kapalı kullanılmazsa süresi TransactionScope tamamlanmadan önce artık</span><span class="sxs-lookup"><span data-stu-id="35ec0-303">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="35ec0-304">İzleme sorun #14218</span><span class="sxs-lookup"><span data-stu-id="35ec0-304">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="35ec0-305">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-305">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-306">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-306">Old behavior</span></span>**

<span data-ttu-id="35ec0-307">EF Core bağlam içinde bağlantısına açarsa, 3.0 önce bir `TransactionScope`, bağlantı sırasında geçerli açık kalır `TransactionScope` etkindir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-307">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

**<span data-ttu-id="35ec0-308">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-308">New behavior</span></span>**

<span data-ttu-id="35ec0-309">Kullanmaya tamamladıktan hemen sonra 3.0 ile başlayarak, EF Core bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-309">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

**<span data-ttu-id="35ec0-310">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-310">Why</span></span>**

<span data-ttu-id="35ec0-311">Birden fazla bağlamı aynı kullanmak için bu değişiklik sağlayan `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-311">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="35ec0-312">Yeni davranış alose EF6 eşleşir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-312">The new behavior alose matches EF6.</span></span>

**<span data-ttu-id="35ec0-313">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-313">Mitigations</span></span>**

<span data-ttu-id="35ec0-314">Bağlantı açık çağrı açık kalması gerekiyorsa `OpenConnection()` EF Core, erken kapatmadığına emin olun:</span><span class="sxs-lookup"><span data-stu-id="35ec0-314">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="35ec0-315">Her bir özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-315">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="35ec0-316">İzleme sorun #6872</span><span class="sxs-lookup"><span data-stu-id="35ec0-316">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="35ec0-317">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-317">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-318">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-318">Old behavior</span></span>**

<span data-ttu-id="35ec0-319">EF Core 3.0 önce tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-319">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

**<span data-ttu-id="35ec0-320">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-320">New behavior</span></span>**

<span data-ttu-id="35ec0-321">EF Core 3.0 ile başlayarak, her bir tamsayı anahtar özellik değer Oluşturucusu kendi bellek içi veritabanı kullanılırken alır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-321">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="35ec0-322">Ayrıca, veritabanı silinirse, anahtar oluşturma için tüm tabloları sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-322">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

**<span data-ttu-id="35ec0-323">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-323">Why</span></span>**

<span data-ttu-id="35ec0-324">Bellek içi anahtar oluşturma gerçek veritabanı anahtar oluşturma için daha yakından Hizala ve bellek içi veritabanı kullanılırken testler birbirinden ayırma özelliğini artırmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-324">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

**<span data-ttu-id="35ec0-325">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-325">Mitigations</span></span>**

<span data-ttu-id="35ec0-326">Bunu ayarlamak için belirli bellek içi anahtar değerlerine bağlı olan bir uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-326">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="35ec0-327">Göz önünde bulundurun yerine değil özel anahtar değerlerine bağlı olan veya yeni davranış eşleştirilecek güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="35ec0-327">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="35ec0-328">Varsayılan olarak kullanılan destekleyen alanlar</span><span class="sxs-lookup"><span data-stu-id="35ec0-328">Backing fields are used by default</span></span>

[<span data-ttu-id="35ec0-329">İzleme sorun #12430</span><span class="sxs-lookup"><span data-stu-id="35ec0-329">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="35ec0-330">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-330">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="35ec0-331">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-331">Old behavior</span></span>**

<span data-ttu-id="35ec0-332">3.0 önce bir özellik için destek alanı biliniyordu olsa bile, EF Core yine de varsayılan olarak okuma ve özellik alıcı ve ayarlayıcı yöntemleri kullanarak özellik değeri yazma.</span><span class="sxs-lookup"><span data-stu-id="35ec0-332">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="35ec0-333">Bunun özel durumu burada destek alanı doğrudan biliniyorsa ayarlamak sorgu yürütme oluştu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-333">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

**<span data-ttu-id="35ec0-334">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-334">New behavior</span></span>**

<span data-ttu-id="35ec0-335">Bir özellik için destek alanı biliniyorsa, EF Core 3.0 ile başlayan sonra her zaman okuyabilmesini ve yazma yedekleme alanını kullanarak bu özelliği.</span><span class="sxs-lookup"><span data-stu-id="35ec0-335">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="35ec0-336">Uygulama, alıcı veya ayarlayıcı yöntemleri kodlanmış ek davranış bağlı değilse bu bir uygulama sonu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-336">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

**<span data-ttu-id="35ec0-337">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-337">Why</span></span>**

<span data-ttu-id="35ec0-338">Bu değişiklik iş mantığı deneyebileceğinizi olarak tetikleyen EF Core önlemek için varsayılan olarak varlıkları içeren bir veritabanı işlemleri gerçekleştirirken yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-338">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

**<span data-ttu-id="35ec0-339">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-339">Mitigations</span></span>**

<span data-ttu-id="35ec0-340">ModelBuilder fluent API'si özellik erişim modunda yapılandırmasını aracılığıyla öncesi 3.0 davranış geri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-340">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="35ec0-341">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-341">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="35ec0-342">Birden çok uyumlu yedekleme alan bulunamazsa throw</span><span class="sxs-lookup"><span data-stu-id="35ec0-342">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="35ec0-343">İzleme sorun #12523</span><span class="sxs-lookup"><span data-stu-id="35ec0-343">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="35ec0-344">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-344">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-345">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-345">Old behavior</span></span>**

<span data-ttu-id="35ec0-346">Birden çok alan karşılarsa, EF Core 3.0 önce bazı öncelik sırası üzerinde bir alan seçilirdi sonra bir özelliğinin destek alanı bulma kurallarını temel.</span><span class="sxs-lookup"><span data-stu-id="35ec0-346">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="35ec0-347">Bu, belirsiz durumda kullanılacak yanlış alan neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-347">This could cause the wrong field to be used in ambiguous cases.</span></span>

**<span data-ttu-id="35ec0-348">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-348">New behavior</span></span>**

<span data-ttu-id="35ec0-349">Birden çok alan aynı özelliğe eşleşirse EF Core 3.0 ile başlayarak, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-349">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

**<span data-ttu-id="35ec0-350">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-350">Why</span></span>**

<span data-ttu-id="35ec0-351">Yalnızca biri doğru olduğunda sessiz bir şekilde bir alanı başka bir kullanmaktan kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-351">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

**<span data-ttu-id="35ec0-352">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-352">Mitigations</span></span>**

<span data-ttu-id="35ec0-353">Belirsiz destekleyen alanlarla özellikleri, açıkça belirtilen kullanılacak bir alan olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-353">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="35ec0-354">Örneğin, fluent API'sini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="35ec0-354">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="35ec0-355">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağırın</span><span class="sxs-lookup"><span data-stu-id="35ec0-355">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="35ec0-356">İzleme sorun #14756</span><span class="sxs-lookup"><span data-stu-id="35ec0-356">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="35ec0-357">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-358">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-358">Old behavior</span></span>**

<span data-ttu-id="35ec0-359">EF Core 3.0, çağırma önce `AddDbContext` veya `AddDbContextPool` de günlüğe kaydetme ve bellek D.I hizmetleriyle yapılan çağrılar aracılığıyla önbelleğe kaydetmek [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="35ec0-359">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

**<span data-ttu-id="35ec0-360">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-360">New behavior</span></span>**

<span data-ttu-id="35ec0-361">EF Core 3.0 ile başlayan `AddDbContext` ve `AddDbContextPool` artık kayıt hizmetlerin bağımlılık ekleme (dı) sahip olur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-361">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

**<span data-ttu-id="35ec0-362">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-362">Why</span></span>**

<span data-ttu-id="35ec0-363">EF Core 3.0, bu hizmetler, uygulamanın DI cotainer olduğunu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="35ec0-363">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="35ec0-364">Ancak, varsa `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlı EF Core tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-364">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

**<span data-ttu-id="35ec0-365">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-365">Mitigations</span></span>**

<span data-ttu-id="35ec0-366">Uygulamanız bu hizmetlere ihtiyacı varsa, sonra bunları açıkça DI kullanarak kapsayıcı kayıt [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="35ec0-366">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="35ec0-367">DbContext.Entry artık yerel DetectChanges gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="35ec0-367">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="35ec0-368">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="35ec0-368">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="35ec0-369">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-369">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-370">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-370">Old behavior</span></span>**

<span data-ttu-id="35ec0-371">EF Core 3.0, çağırma önce `DbContext.Entry` izlenen tüm varlıklar için algılanabilir değişiklik neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-371">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="35ec0-372">Bu durumu içinde kullanıma sunulan sağlamış `EntityEntry` güncel oluştu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-372">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

**<span data-ttu-id="35ec0-373">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-373">New behavior</span></span>**

<span data-ttu-id="35ec0-374">Çağırma EF Core ile 3.0, başlangıç `DbContext.Entry` ilgili asıl varlıkları yalnızca belirtilen varlık ve diğer değişikliklerini algılamak girişimi izlenen başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="35ec0-374">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="35ec0-375">Başka bir yerde değiştirir Bunun anlamı etkileri uygulama durumu olabilir. Bu yöntemi çağrılarak algılandı değil.</span><span class="sxs-lookup"><span data-stu-id="35ec0-375">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="35ec0-376">Unutmayın `ChangeTracker.AutoDetectChangesEnabled` ayarlanır `false` sonra bile bu yerel değişiklik algılama devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-376">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="35ec0-377">Değişiklik algılama--örneğin neden diğer yöntemleri `ChangeTracker.Entries` ve `SaveChanges`--yine de tam neden `DetectChanges` tüm varlıkları izlenir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-377">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

**<span data-ttu-id="35ec0-378">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-378">Why</span></span>**

<span data-ttu-id="35ec0-379">Kullanmanın varsayılan performansını artırmak için bu değişiklik yapılmıştır `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-379">This change was made to improve the default performance of using `context.Entry`.</span></span>

**<span data-ttu-id="35ec0-380">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-380">Mitigations</span></span>**

<span data-ttu-id="35ec0-381">Çağrı `ChgangeTracker.DetectChanges()` açıkça çağırmadan önce `Entry` öncesi 3.0 davranış sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="35ec0-381">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="35ec0-382">Dize ve bayt dizisi anahtarlar varsayılan olarak istemci tarafından oluşturulan değildir</span><span class="sxs-lookup"><span data-stu-id="35ec0-382">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="35ec0-383">İzleme sorun #14617</span><span class="sxs-lookup"><span data-stu-id="35ec0-383">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="35ec0-384">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-384">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-385">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-385">Old behavior</span></span>**

<span data-ttu-id="35ec0-386">EF Core 3.0 önce `string` ve `byte[]` açıkça null olmayan bir değer ayarlamadan anahtar özellikleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-386">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="35ec0-387">Böyle bir durumda anahtar değeri istemcide bayt için seri hale getirilmiş bir GUID olarak oluşturulacak `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-387">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

**<span data-ttu-id="35ec0-388">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-388">New behavior</span></span>**

<span data-ttu-id="35ec0-389">Bir özel durum EF Core 3.0 ile başlayarak, anahtar değer kümesi gösteren oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-389">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

**<span data-ttu-id="35ec0-390">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-390">Why</span></span>**

<span data-ttu-id="35ec0-391">Bu değişiklik nedeniyle istemci tarafından oluşturulan yapılmıştır `string` / `byte[]` değerleri genellikle olmadığı kullanışlı ve varsayılan davranışını yaptığı sabit nedeni hakkında oluşturulan anahtar değerler için yaygın bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="35ec0-391">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

**<span data-ttu-id="35ec0-392">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-392">Mitigations</span></span>**

<span data-ttu-id="35ec0-393">Başka bir null olmayan değer ayarlarsanız anahtar özellikleri oluşturulan değerler kullanması gerektiğini açıkça belirterek öncesi 3.0 davranış elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-393">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="35ec0-394">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="35ec0-394">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="35ec0-395">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="35ec0-395">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="35ec0-396">Kapsamlı bir hizmet Iloggerfactory sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="35ec0-396">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="35ec0-397">İzleme sorun #14698</span><span class="sxs-lookup"><span data-stu-id="35ec0-397">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="35ec0-398">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-399">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-399">Old behavior</span></span>**

<span data-ttu-id="35ec0-400">EF Core 3.0 önce `ILoggerFactory` tek bir hizmet kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-400">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

**<span data-ttu-id="35ec0-401">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-401">New behavior</span></span>**

<span data-ttu-id="35ec0-402">EF Core 3.0 ile başlayan `ILoggerFactory` şimdi kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-402">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

**<span data-ttu-id="35ec0-403">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-403">Why</span></span>**

<span data-ttu-id="35ec0-404">Günlükçü ile ilişkisini izin vermek için bu değişiklik yapılmıştır bir `DbContext` diğer işlevleri sağlar ve bazı durumlarda pathological davranış gibi iç hizmet sağlayıcıları sayısındaki kaldıran örneği.</span><span class="sxs-lookup"><span data-stu-id="35ec0-404">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

**<span data-ttu-id="35ec0-405">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-405">Mitigations</span></span>**

<span data-ttu-id="35ec0-406">Bu değişiklik, kaydetme ve EF Core iç hizmet sağlayıcısına özel hizmetler kullanarak sürece uygulama kodu etkilememesi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-406">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="35ec0-407">Bu ortak değildir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-407">This isn't common.</span></span>
<span data-ttu-id="35ec0-408">Bu durumlarda, bilgilerin çoğunu hala çalışır, ancak bağlı olarak herhangi bir tekil hizmeti olacak `ILoggerFactory` almak için değiştirilmesi gerekecektir `ILoggerFactory` farklı bir yolla.</span><span class="sxs-lookup"><span data-stu-id="35ec0-408">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="35ec0-409">Bu gibi durumlarda karşılaşırsanız lütfen sırasında bir sorun üzerinde dosya [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) nasıl kullandığınızı bize bildirmek `ILoggerFactory` sağlayacak şekilde biz bunu gelecekte kesmemesi nasıl daha iyi anlamak.</span><span class="sxs-lookup"><span data-stu-id="35ec0-409">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="35ec0-410">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-410">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="35ec0-411">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="35ec0-411">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="35ec0-412">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-412">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-413">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-413">Old behavior</span></span>**

`IDbContextOptionsExtensionWithDebugInfo` <span data-ttu-id="35ec0-414">ek isteğe bağlı bir arabirim gelen genişletilmişse `IDbContextOptionsExtension` arabirimine 2.x sürüm döngüsü sırasında değiştirme bir hataya neden önlemek için.</span><span class="sxs-lookup"><span data-stu-id="35ec0-414">was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

**<span data-ttu-id="35ec0-415">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-415">New behavior</span></span>**

<span data-ttu-id="35ec0-416">Arabirimler artık birlikte birleştirilir `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-416">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

**<span data-ttu-id="35ec0-417">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-417">Why</span></span>**

<span data-ttu-id="35ec0-418">Arabirimler kavramsal olarak biri olduğundan, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-418">This change was made because the interfaces are conceptually one.</span></span>

**<span data-ttu-id="35ec0-419">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-419">Mitigations</span></span>**

<span data-ttu-id="35ec0-420">Tüm uygulamaları `IDbContextOptionsExtension` yeni üye destekleyecek şekilde güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-420">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="35ec0-421">Gezinti özellikleri tam olarak yüklü olduğundan yükleme Lazy proxy'leri artık varsayılır</span><span class="sxs-lookup"><span data-stu-id="35ec0-421">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="35ec0-422">İzleme sorun #12780</span><span class="sxs-lookup"><span data-stu-id="35ec0-422">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="35ec0-423">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-423">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-424">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-424">Old behavior</span></span>**

<span data-ttu-id="35ec0-425">EF Core 3.0, bir kez önce bir `DbContext` bırakıldı veya bir belirtilen gezinme özelliğinin bir varlığı söz konusu bağlamdan elde tam yüklü ise, olduğunu bilmesinin imkanı yoktu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-425">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="35ec0-426">Proxy'leri bunun yerine null olmayan bir değer varsa bir başvuru Gezinti yüklenir ve boş değilse, bir koleksiyon Gezinti yüklenen varsayılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-426">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="35ec0-427">Bu durumlarda, yavaş dışarıdan yüklemeyi deneyen bir İşlemsiz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-427">In these cases, attempting to lazy-load would be a no-op.</span></span>

**<span data-ttu-id="35ec0-428">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-428">New behavior</span></span>**

<span data-ttu-id="35ec0-429">EF Core 3.0 ile başlayarak, proxy'leri bir gezinti özelliğinin yüklü olup olmadığını izler.</span><span class="sxs-lookup"><span data-stu-id="35ec0-429">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="35ec0-430">Bu yüklenen Gezinti boş veya null olduğunda bile bağlamı bırakıldıktan sonra yüklenen bir gezinti özelliği erişmeye çalışan her zaman bir İşlemsiz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-430">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="35ec0-431">Buna karşılık, boş bir koleksiyon gezinme özelliği olsa bile bağlamı elden yüklü olmayan bir gezinti özelliği erişmeye çalışan bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-431">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="35ec0-432">Bu durum ortaya çıkarsa, uygulama kodu, geçersiz bir zamanda yavaş yükleme kullanmak çalışıyor ve bunu yapmak için uygulama değiştirilmelidir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-432">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

**<span data-ttu-id="35ec0-433">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-433">Why</span></span>**

<span data-ttu-id="35ec0-434">Bu değişiklik, bırakılmış yükü yavaş çalışırken davranışını tutarlı ve doğru hale yapılmıştır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="35ec0-434">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

**<span data-ttu-id="35ec0-435">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-435">Mitigations</span></span>**

<span data-ttu-id="35ec0-436">Uygulama kodunun yükleme yavaş bırakılmış bağlamla kullanmamanız güncelleştirin veya bu özel durum iletisini açıklandığı bir İşlemsiz olacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-436">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="35ec0-437">İç hizmet sağlayıcılarının aşırı oluşturma artık varsayılan olarak bir hata</span><span class="sxs-lookup"><span data-stu-id="35ec0-437">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="35ec0-438">İzleme sorun #10236</span><span class="sxs-lookup"><span data-stu-id="35ec0-438">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="35ec0-439">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-440">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-440">Old behavior</span></span>**

<span data-ttu-id="35ec0-441">EF Core 3.0 önce bir uyarı iç hizmet sağlayıcıları pathological bir dizi oluşturmak için bir uygulama oturum açmış olmanız.</span><span class="sxs-lookup"><span data-stu-id="35ec0-441">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

**<span data-ttu-id="35ec0-442">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-442">New behavior</span></span>**

<span data-ttu-id="35ec0-443">EF Core 3.0 ile başlayarak, bu uyarı artık olarak kabul edilir ve hata ve özel durum harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-443">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

**<span data-ttu-id="35ec0-444">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-444">Why</span></span>**

<span data-ttu-id="35ec0-445">Bu değişiklik, daha açık pathological bu durumda gösterme yoluyla uygulama kodu daha iyi desteklemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-445">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

**<span data-ttu-id="35ec0-446">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-446">Mitigations</span></span>**

<span data-ttu-id="35ec0-447">Bu hata ile karşılaşma eylemde en uygun nedeni, kök nedenini anlamak ve çok sayıda iç hizmet sağlayıcıları oluşturmayı durdur oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-447">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="35ec0-448">Ancak, hata bir uyarı olarak geri dönüştürülür (veya yok sayıldı) aracılığıyla yapılandırma `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-448">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="35ec0-449">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-449">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="35ec0-450">Tek bir dize ile HasOne/çok ilişkin yeni davranış çağırılır</span><span class="sxs-lookup"><span data-stu-id="35ec0-450">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="35ec0-451">İzleme sorun #9171</span><span class="sxs-lookup"><span data-stu-id="35ec0-451">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="35ec0-452">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-452">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-453">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-453">Old behavior</span></span>**

<span data-ttu-id="35ec0-454">EF Core 3.0 önce kod arama `HasOne` veya `HasMany` tek bir dize ile yorumlandığı kafa karıştırıcı bir şekilde oluştu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-454">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="35ec0-455">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-455">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="35ec0-456">Kod ilgili Görülüyor `Samurai` bazı diğer varlık türü kullanarak `Entrance` özel gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="35ec0-456">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="35ec0-457">Bazı varlık türü olarak adlandırılan bir ilişki oluşturmak bu kodu gerçekte çalışır `Entrance` ile gezinti özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="35ec0-457">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

**<span data-ttu-id="35ec0-458">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-458">New behavior</span></span>**

<span data-ttu-id="35ec0-459">EF Core 3.0 ile başlayarak, yukarıdaki kod artık önce yapmakta gibi hangi Aranan yapar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-459">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

**<span data-ttu-id="35ec0-460">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-460">Why</span></span>**

<span data-ttu-id="35ec0-461">Eski davranışı özellikle yapılandırma kodu okurken ve hatalarını arayarak çok karmaşık.</span><span class="sxs-lookup"><span data-stu-id="35ec0-461">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

**<span data-ttu-id="35ec0-462">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-462">Mitigations</span></span>**

<span data-ttu-id="35ec0-463">Bu, yalnızca tür adları ve gezinme özelliğini açıkça belirtmeden dizeleriyle ilişkileri açıkça yapılandırmakta olduğunuz uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="35ec0-463">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="35ec0-464">Bu yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-464">This is not common.</span></span>
<span data-ttu-id="35ec0-465">Önceki davranışı açıkça geçirme aracılığıyla alınabilir `null` gezinme özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-465">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="35ec0-466">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-466">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="35ec0-467">Birden çok zaman uyumsuz yöntemler için dönüş türü için ValueTask görevden değiştirildi</span><span class="sxs-lookup"><span data-stu-id="35ec0-467">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="35ec0-468">İzleme sorun #15184</span><span class="sxs-lookup"><span data-stu-id="35ec0-468">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="35ec0-469">Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-469">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-470">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-470">Old behavior</span></span>**

<span data-ttu-id="35ec0-471">Aşağıdaki zaman uyumsuz yöntemler daha önce döndürülen bir `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="35ec0-471">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` <span data-ttu-id="35ec0-472">(ve türetilen sınıflar)</span><span class="sxs-lookup"><span data-stu-id="35ec0-472">(and deriving classes)</span></span>

**<span data-ttu-id="35ec0-473">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-473">New behavior</span></span>**

<span data-ttu-id="35ec0-474">Yukarıda sözü edilen yöntemleri artık döndürür bir `ValueTask<T>` aynı üzerinden `T` önceki gibi.</span><span class="sxs-lookup"><span data-stu-id="35ec0-474">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

**<span data-ttu-id="35ec0-475">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-475">Why</span></span>**

<span data-ttu-id="35ec0-476">Bu değişiklik, yığın ayırmaları genel performansını iyileştirme, bu yöntemleri çağrılırken tahakkuk sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-476">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

**<span data-ttu-id="35ec0-477">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-477">Mitigations</span></span>**

<span data-ttu-id="35ec0-478">Yalnızca yukarıdaki API'leri yalnızca bekleyen gerekir derlenmesi için - uygulamalar kaynak değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-478">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="35ec0-479">Daha karmaşık bir kullanım (örneğin döndürülen geçirme `Task` için `Task.WhenAny()`) genellikle gerektiren döndürülen `ValueTask<T>` dönüştürülmesi bir `Task<T>` çağırarak `AsTask()` üzerindeki.</span><span class="sxs-lookup"><span data-stu-id="35ec0-479">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="35ec0-480">Bu bu değişiklik getirir ayırma azaltma verilerek unutmayın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-480">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="35ec0-481">İlişkisel: TypeMapping ek açıklama yalnızca TypeMapping sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="35ec0-481">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="35ec0-482">İzleme sorun #9913</span><span class="sxs-lookup"><span data-stu-id="35ec0-482">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="35ec0-483">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-483">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="35ec0-484">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-484">Old behavior</span></span>**

<span data-ttu-id="35ec0-485">Tür eşlemesi ek açıklamaları için ek açıklama "İlişkisel: TypeMapping" adlandırılmıştı.</span><span class="sxs-lookup"><span data-stu-id="35ec0-485">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

**<span data-ttu-id="35ec0-486">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-486">New behavior</span></span>**

<span data-ttu-id="35ec0-487">Ek açıklama türü eşleme ek açıklamalar için artık "TypeMapping" addır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-487">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

**<span data-ttu-id="35ec0-488">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-488">Why</span></span>**

<span data-ttu-id="35ec0-489">Türü eşlemeleri artık birden fazla yalnızca ilişkisel veritabanı sağlayıcıları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-489">Type mappings are now used for more than just relational database providers.</span></span>

**<span data-ttu-id="35ec0-490">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-490">Mitigations</span></span>**

<span data-ttu-id="35ec0-491">Bu, yalnızca tür eşlemesi ortak olmayan doğrudan bir ek açıklama, olarak erişen uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="35ec0-491">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="35ec0-492">API yüzeyi için ek açıklama doğrudan kullanmak yerine erişim türü eşlemeleri düzeltmek için en uygun eylemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-492">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="35ec0-493">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-493">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="35ec0-494">İzleme sorun #11811</span><span class="sxs-lookup"><span data-stu-id="35ec0-494">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="35ec0-495">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-495">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-496">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-496">Old behavior</span></span>**

<span data-ttu-id="35ec0-497">EF Core 3.0 önce `ToTable()` türetilmiş üzerinde adlı türü yoksayılan beri tek devralma eşleme stratejisi TPH burada bu geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="35ec0-497">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

**<span data-ttu-id="35ec0-498">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-498">New behavior</span></span>**

<span data-ttu-id="35ec0-499">EF Core 3.0 ve sonraki bir sürümde TPT ve TPC desteği eklemek için hazırlık başlangıç `ToTable()` beklenmeyen eşleme değişiklik gelecekte önlemek için bir özel durum bir türetilmiş tür şimdi throw üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-499">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

**<span data-ttu-id="35ec0-500">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-500">Why</span></span>**

<span data-ttu-id="35ec0-501">Şu anda farklı bir tabloya türetilmiş bir tür eşlemek için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="35ec0-501">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="35ec0-502">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda bozucu önler.</span><span class="sxs-lookup"><span data-stu-id="35ec0-502">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

**<span data-ttu-id="35ec0-503">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-503">Mitigations</span></span>**

<span data-ttu-id="35ec0-504">Türetilen türlerin diğer tablolara eşleme girişimleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-504">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="35ec0-505">ForSqlServerHasIndex HasIndex ile değiştirildi</span><span class="sxs-lookup"><span data-stu-id="35ec0-505">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="35ec0-506">İzleme sorun #12366</span><span class="sxs-lookup"><span data-stu-id="35ec0-506">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="35ec0-507">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-507">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-508">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-508">Old behavior</span></span>**

<span data-ttu-id="35ec0-509">EF Core 3.0 önce `ForSqlServerHasIndex().ForSqlServerInclude()` ile kullanılan sütunları yapılandırmak üzere bir yöntem sağlayan `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-509">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

**<span data-ttu-id="35ec0-510">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-510">New behavior</span></span>**

<span data-ttu-id="35ec0-511">EF Core 3.0 ile başlayarak, kullanarak `Include` dizin ilişkisel düzeyinde artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-511">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="35ec0-512">Kullanım `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-512">Use `HasIndex().ForSqlServerInclude()`.</span></span>

**<span data-ttu-id="35ec0-513">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-513">Why</span></span>**

<span data-ttu-id="35ec0-514">API ile dizin için birleştirmek için bu değişiklik yapılmıştır `Includes` tüm sağlayıcıları veritabanı için içine yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="35ec0-514">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

**<span data-ttu-id="35ec0-515">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-515">Mitigations</span></span>**

<span data-ttu-id="35ec0-516">Yukarıda da gösterildiği gibi yeni API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-516">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="35ec0-517">EF Core artık SQLite FK zorlama pragma gönderir</span><span class="sxs-lookup"><span data-stu-id="35ec0-517">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="35ec0-518">İzleme sorun #12151</span><span class="sxs-lookup"><span data-stu-id="35ec0-518">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="35ec0-519">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-519">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="35ec0-520">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-520">Old behavior</span></span>**

<span data-ttu-id="35ec0-521">EF Core 3.0 önce EF Core gönderir gibi `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="35ec0-521">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="35ec0-522">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-522">New behavior</span></span>**

<span data-ttu-id="35ec0-523">EF Core 3.0 ile başlayarak, EF Core artık gönderir `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="35ec0-523">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="35ec0-524">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-524">Why</span></span>**

<span data-ttu-id="35ec0-525">EF Core kullandığından bu değişiklik yapılmıştır `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak, hangi sırayla anlamına gelir FK zorlama varsayılan olarak açık ve açıkça bir bağlantı her açıldığında etkinleştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="35ec0-525">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

**<span data-ttu-id="35ec0-526">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-526">Mitigations</span></span>**

<span data-ttu-id="35ec0-527">Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-527">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="35ec0-528">Diğer durumlarda, yabancı anahtarlar belirterek etkinleştirilebilir `Foreign Keys=True` bağlantı dizenizi içinde.</span><span class="sxs-lookup"><span data-stu-id="35ec0-528">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="35ec0-529">Microsoft.EntityFrameworkCore.Sqlite üzerinde SQLitePCLRaw.bundle_e_sqlite3 artık bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-529">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

**<span data-ttu-id="35ec0-530">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-530">Old behavior</span></span>**

<span data-ttu-id="35ec0-531">EF Core 3.0 önce EF Core kullanılan `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-531">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

**<span data-ttu-id="35ec0-532">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-532">New behavior</span></span>**

<span data-ttu-id="35ec0-533">EF Core 3.0 ile başlayarak, EF Core kullanan `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-533">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

**<span data-ttu-id="35ec0-534">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-534">Why</span></span>**

<span data-ttu-id="35ec0-535">Diğer platformlar ile tutarlı ios'ta kullanılan SQLite sürümü, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-535">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

**<span data-ttu-id="35ec0-536">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-536">Mitigations</span></span>**

<span data-ttu-id="35ec0-537">İos'ta yerel bir SQLite sürümü kullanmak için yapılandırma `Microsoft.Data.Sqlite` farklı bir kullanılacak `SQLitePCLRaw` paket.</span><span class="sxs-lookup"><span data-stu-id="35ec0-537">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="35ec0-538">GUID değerlerinin artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="35ec0-538">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="35ec0-539">İzleme sorun #15078</span><span class="sxs-lookup"><span data-stu-id="35ec0-539">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="35ec0-540">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-540">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-541">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-541">Old behavior</span></span>**

<span data-ttu-id="35ec0-542">GUID değerleri, daha önce SQLite değerlerine BLOB olarak sored.</span><span class="sxs-lookup"><span data-stu-id="35ec0-542">Guid values were previously sored as BLOB values on SQLite.</span></span>

**<span data-ttu-id="35ec0-543">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-543">New behavior</span></span>**

<span data-ttu-id="35ec0-544">GUID değerlerinin sotred metin olarak sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-544">Guid values are now sotred as TEXT.</span></span>

**<span data-ttu-id="35ec0-545">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-545">Why</span></span>**

<span data-ttu-id="35ec0-546">İkili biçimi GUID'leri standartlaştırılmış değil.</span><span class="sxs-lookup"><span data-stu-id="35ec0-546">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="35ec0-547">Değerlerin metin olarak depolanması veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-547">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="35ec0-548">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-548">Mitigations</span></span>**

<span data-ttu-id="35ec0-549">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35ec0-549">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="35ec0-550">EF Core de önceki davranışı configuirng bir değer dönüştürücü bu özellikleri kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35ec0-550">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="35ec0-551">Microsoft.Data.Sqlite GUID değerlerinin hem BLOB hem de metin sütunları okuma özellikli kalır; Parametreler ve sabitleri için varsayılan biçimi değiştiğinden ancak büyük olasılıkla GUID'leri içeren çoğu senaryo için bir eylem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-551">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="35ec0-552">Char değerleri artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="35ec0-552">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="35ec0-553">İzleme sorun #15020</span><span class="sxs-lookup"><span data-stu-id="35ec0-553">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="35ec0-554">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-554">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-555">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-555">Old behavior</span></span>**

<span data-ttu-id="35ec0-556">Char değerleri daha önce SQLite tamsayı değerleri olarak sored.</span><span class="sxs-lookup"><span data-stu-id="35ec0-556">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="35ec0-557">Örneğin, bir karakter değerini *A* 65 tamsayı değeri depolanmıştır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-557">For example, a char value of *A* was stored as the integer value 65.</span></span>

**<span data-ttu-id="35ec0-558">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-558">New behavior</span></span>**

<span data-ttu-id="35ec0-559">Char değerleri sotred metin olarak sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-559">Char values are now sotred as TEXT.</span></span>

**<span data-ttu-id="35ec0-560">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-560">Why</span></span>**

<span data-ttu-id="35ec0-561">Değerlerin metin olarak depolanması daha doğal ve veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-561">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="35ec0-562">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-562">Mitigations</span></span>**

<span data-ttu-id="35ec0-563">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35ec0-563">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="35ec0-564">EF Core de önceki davranışı configuirng bir değer dönüştürücü bu özellikleri kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35ec0-564">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="35ec0-565">Microsoft.Data.Sqlite Ayrıca belirli senaryoları herhangi bir eylem gerekli değil şekilde karakter değerleri hem tamsayı hem de metin sütunları, okuma özellikli kalır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-565">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="35ec0-566">Geçiş kimlikleri, artık sabit kültürün takvimini kullanarak oluşturulur</span><span class="sxs-lookup"><span data-stu-id="35ec0-566">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="35ec0-567">İzleme sorun #12978</span><span class="sxs-lookup"><span data-stu-id="35ec0-567">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="35ec0-568">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-568">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-569">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-569">Old behavior</span></span>**

<span data-ttu-id="35ec0-570">Geçiş kimlikleri currret kültürün takvimini kullanarak oluşturulan inadvertantly yoktu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-570">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

**<span data-ttu-id="35ec0-571">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-571">New behavior</span></span>**

<span data-ttu-id="35ec0-572">Geçiş kimlikleri artık her zaman sabit kültürün takvimini (Gregoryen) kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-572">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

**<span data-ttu-id="35ec0-573">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-573">Why</span></span>**

<span data-ttu-id="35ec0-574">Geçiş sırası önemlidir veritabanını güncelleştirmek ya da birleştirme çakışmalarını çözme.</span><span class="sxs-lookup"><span data-stu-id="35ec0-574">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="35ec0-575">Sabit takvimi kullanılarak sıralama ekip üyelerinden farklı sistem takvimler sahip sonuçlanabilen sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="35ec0-575">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

**<span data-ttu-id="35ec0-576">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-576">Mitigations</span></span>**

<span data-ttu-id="35ec0-577">Bu değişiklik olmayan Gregoryen takvimdeki bir yılın Gregoryen takvimini (içeren Thai Budist takvimi gibi) daha büyük olduğu kullanan herkesin etkiler.</span><span class="sxs-lookup"><span data-stu-id="35ec0-577">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="35ec0-578">Mevcut geçiş kimlikleri böylece mevcut sonra yeni geçişleri sıralı güncelleştirilmesi gerekiyor geçişler.</span><span class="sxs-lookup"><span data-stu-id="35ec0-578">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="35ec0-579">Geçiş kimliği geçiş özniteliğinde geçişler Tasarımcı dosyalarında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="35ec0-579">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="35ec0-580">Geçişleri geçmiş tablosu da güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="35ec0-580">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="35ec0-581">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="35ec0-581">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="35ec0-582">İzleme sorun #10985</span><span class="sxs-lookup"><span data-stu-id="35ec0-582">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="35ec0-583">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-583">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-584">Değiştir</span><span class="sxs-lookup"><span data-stu-id="35ec0-584">Change</span></span>**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` <span data-ttu-id="35ec0-585">yeniden adlandırıldı `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="35ec0-585">has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

**<span data-ttu-id="35ec0-586">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-586">Why</span></span>**

<span data-ttu-id="35ec0-587">Bu uyarı olayı tüm uyarı olayları ile adlandırma hizalar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-587">Aligns the naming of this warning event with all other warning events.</span></span>

**<span data-ttu-id="35ec0-588">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-588">Mitigations</span></span>**

<span data-ttu-id="35ec0-589">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-589">Use the new name.</span></span> <span data-ttu-id="35ec0-590">(Olay kimliği numarasını değiştirilmedi unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="35ec0-590">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="35ec0-591">API açıklığa kavuşturmak için yabancı anahtar kısıtlaması adları</span><span class="sxs-lookup"><span data-stu-id="35ec0-591">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="35ec0-592">İzleme sorun #10730</span><span class="sxs-lookup"><span data-stu-id="35ec0-592">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="35ec0-593">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="35ec0-593">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="35ec0-594">Eski davranışı</span><span class="sxs-lookup"><span data-stu-id="35ec0-594">Old behavior</span></span>**

<span data-ttu-id="35ec0-595">EF Core 3.0 önce yabancı anahtar kısıtlaması adları için yalnızca "adı olarak" adı veriliyordu.</span><span class="sxs-lookup"><span data-stu-id="35ec0-595">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="35ec0-596">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-596">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

**<span data-ttu-id="35ec0-597">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="35ec0-597">New behavior</span></span>**

<span data-ttu-id="35ec0-598">EF Core 3.0 ile başlayarak, yabancı anahtar kısıtlaması adları şimdi de "kısıtlaması adı" olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="35ec0-598">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="35ec0-599">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35ec0-599">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

**<span data-ttu-id="35ec0-600">Neden</span><span class="sxs-lookup"><span data-stu-id="35ec0-600">Why</span></span>**

<span data-ttu-id="35ec0-601">Bu değişiklik, bu alanda adlandırma için tutarlılık getirir ve ayrıca bu yabancı anahtarı üzerinde tanımlanan yabancı anahtar kısıtlaması ve olmayan sütun veya özellik adı adı olduğunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="35ec0-601">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

**<span data-ttu-id="35ec0-602">Risk azaltma işlemleri</span><span class="sxs-lookup"><span data-stu-id="35ec0-602">Mitigations</span></span>**

<span data-ttu-id="35ec0-603">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="35ec0-603">Use the new name.</span></span>
