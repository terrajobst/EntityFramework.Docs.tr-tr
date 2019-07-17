---
title: EF Core 3.0 - EF Core yeni değişiklikler
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7cc0bd3946be2e63d9fb46a023bf6abe750ae0e3
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286489"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1afc9-102">EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil edilen değişiklikler</span><span class="sxs-lookup"><span data-stu-id="1afc9-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1afc9-103">Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1afc9-104">Aşağıdaki API ve davranışı değişiklikleri EF Core için geliştirilen uygulamaları ayırmak için olası sahip bunları 3.0.0 için yükseltirken 2.2.x.</span><span class="sxs-lookup"><span data-stu-id="1afc9-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1afc9-105">Veritabanı sağlayıcıları yalnızca etkileyen bekliyoruz değişiklikleri altında belgelenir [sağlayıcısı değişiklikleri](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1afc9-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1afc9-106">Bir 3.0 Önizlemesi'nden başka bir 3.0 Önizleme için kullanıma sunulan yeni özellikler sonlarını burada belgelenmemiş.</span><span class="sxs-lookup"><span data-stu-id="1afc9-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1afc9-107">LINQ sorguları, artık istemcide değerlendirilir</span><span class="sxs-lookup"><span data-stu-id="1afc9-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="1afc9-108">[Sorun #14935 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[sorun #12795 Ayrıca bkz:](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="1afc9-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="1afc9-109">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-110">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-110">**Old behavior**</span></span>

<span data-ttu-id="1afc9-111">EF Core, SQL veya bir parametre için bir sorgunun parçası olan bir ifade dönüştürülemedi, 3.0 önce bunu otomatik olarak istemcide ifadesi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1afc9-112">Varsayılan olarak, istemci değerlendirmesi yüksek maliyetlere neden olabilecek bir ifade yalnızca bir uyarı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1afc9-113">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-113">**New behavior**</span></span>

<span data-ttu-id="1afc9-114">3\.0 ile başlayarak, EF Core yalnızca ifadeleri üst düzey projeksiyon izin verir (son `Select()` sorguda çağrısı) istemcide değerlendirilecek.</span><span class="sxs-lookup"><span data-stu-id="1afc9-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1afc9-115">İfadeleri herhangi bir sorgunun parçası olarak SQL veya bir parametre olarak dönüştürülemez, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1afc9-116">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-116">**Why**</span></span>

<span data-ttu-id="1afc9-117">Otomatik istemci değerlendirme sorguların bile bunları önemli bölümleri çevrilemez yürütülecek çok sayıda sorgu sağlar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1afc9-118">Bu davranış yalnızca üretimde yetkisiz değiştirmeye karşı korumalı hale gelebilir beklenmeyen ve zararlı davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1afc9-119">Örneğin, bir koşulda bir `Where()` çevrilemez çağrısı, veritabanı sunucusu ve istemcide uygulanması için filtreyi aktarılacak tablosundan tüm satırları açabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1afc9-120">Tablo, geliştirme, ancak isabet sabit yalnızca birkaç satır içeriyorsa uygulama üretime burada tablo milyonlarca satır içerebilir taşındığında bu durum kolayca algılanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1afc9-121">İstemci değerlendirme uyarılar ayrıca geliştirme sırasında göz ardı edilmesi çok kolay kanıtladılar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1afc9-122">Belirli ifadeler için iyileştirme sorgu çevirisi sürümler arasında istenmeyen bozucu değişiklikleri nedeniyle oluşan sorunları bu yanı sıra, otomatik istemci değerlendirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1afc9-123">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-123">**Mitigations**</span></span>

<span data-ttu-id="1afc9-124">Bir sorgu tam olarak çevrilemez sonra ya da çevrilebilir veya kullanan bir form sorguda yeniden `AsEnumerable()`, `ToList()`, veya açıkça verileri Burada, ardından daha büyük olabilir istemciye geri getirmek için benzer LINQ nesneleri kullanılarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1afc9-125">Entity Framework Core artık ASP.NET Core paylaşılan framework'ün bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="1afc9-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1afc9-126">İzleme sorunu duyuruları #325</span><span class="sxs-lookup"><span data-stu-id="1afc9-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1afc9-127">Bu değişiklik, ASP.NET Core 3.0 Önizleme 1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1afc9-128">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-128">**Old behavior**</span></span>

<span data-ttu-id="1afc9-129">ASP.NET Core 3.0 Paketi başvurusu eklendiğinde önce `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`, EF Core içerir ve EF Core veri sağlayıcıları bazıları istediğiniz SQL Server sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1afc9-130">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-130">**New behavior**</span></span>

<span data-ttu-id="1afc9-131">3\. 0'dan başlayarak, ASP.NET Core paylaşılan çerçeve EF Core veya tüm EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="1afc9-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1afc9-132">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-132">**Why**</span></span>

<span data-ttu-id="1afc9-133">Bu değişiklikten önce EF Core alma olup uygulama ASP.NET Core ve SQL Server veya hedeflenen bağlı olarak farklı adımlar gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1afc9-134">Ayrıca, ASP.NET Core yükseltme EF Core ve SQL Server sağlayıcısı, her zaman tercih değildir yükseltmesini zorlandı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1afc9-135">Bu değişiklik, EF Core alma aynı tüm sağlayıcıları, desteklenen .NET uygulamaları ve uygulama türleri arasında deneyimidir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1afc9-136">Geliştiriciler, tam olarak EF Core ve EF Core veri sağlayıcıları yükseltildiğinde de denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1afc9-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1afc9-137">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-137">**Mitigations**</span></span>

<span data-ttu-id="1afc9-138">EF çekirdekli ASP.NET Core 3.0 uygulama veya desteklenen başka bir uygulama içinde kullanmak için açıkça uygulamanızın kullanacağı EF Core veritabanı sağlayıcısı için bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1afc9-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="1afc9-139">EF Core komut satırı aracı, dotnet ef artık .NET Core SDK'sının bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="1afc9-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="1afc9-140">İzleme sorun #14016</span><span class="sxs-lookup"><span data-stu-id="1afc9-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="1afc9-141">Bu değişiklik EF Core 3.0-preview 4 ve .NET Core SDK'sını ilgili sürümü kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="1afc9-142">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-142">**Old behavior**</span></span>

<span data-ttu-id="1afc9-143">3\.0 önce `dotnet ef` aracı, .NET Core SDK'yı eklenmiştir ve ek adımlar gerek kalmadan, herhangi bir projeyi komut satırından kullanmaya hazır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="1afc9-144">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-144">**New behavior**</span></span>

<span data-ttu-id="1afc9-145">3\. 0'dan başlayarak .NET SDK'sı yer almaz `dotnet ef` aracı kullanabilmeniz için önce açıkça yerel veya genel bir aracı yüklemek zorunda.</span><span class="sxs-lookup"><span data-stu-id="1afc9-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="1afc9-146">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-146">**Why**</span></span>

<span data-ttu-id="1afc9-147">Bu değişiklik dağıtmak ve güncelleştirmek sağlıyor `dotnet ef` bir düzenli .NET CLI aracı, NuGet üzerindeki EF Core 3.0 de her zaman bir NuGet paketi olarak dağıtılmış bir olgu ile tutarlı olarak.</span><span class="sxs-lookup"><span data-stu-id="1afc9-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="1afc9-148">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-148">**Mitigations**</span></span>

<span data-ttu-id="1afc9-149">Geçişleri veya iskele yönetebilmek için bir `DbContext`, yükleme `dotnet-ef` genel bir aracı olarak:</span><span class="sxs-lookup"><span data-stu-id="1afc9-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="1afc9-150">Aletle şekillerini kullanan bağımlılık olarak bildiren bir proje bağımlılıklarını geri yüklediğinizde de, yerel aracı edinebilirsiniz bir [aracı bildirim dosyası](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="1afc9-150">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="1afc9-151">SQL ve ExecuteSql ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="1afc9-151">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="1afc9-152">İzleme sorun #10996</span><span class="sxs-lookup"><span data-stu-id="1afc9-152">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="1afc9-153">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-153">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-154">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-154">**Old behavior**</span></span>

<span data-ttu-id="1afc9-155">EF Core 3.0 önce bu yöntem adlarının bir normal bir dize ya da SQL ve parametreleri ilişkilendirilmiş bir dize ile çalışmak için aşırı yüklenmiş.</span><span class="sxs-lookup"><span data-stu-id="1afc9-155">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="1afc9-156">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-156">**New behavior**</span></span>

<span data-ttu-id="1afc9-157">EF Core 3.0 ile başlayarak, kullanmaktadır `FromSqlRaw`, `ExecuteSqlRaw`, ve `ExecuteSqlRawAsync` burada parametreleri geçirilir ayrı olarak Sorgu dizesinden parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1afc9-157">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="1afc9-158">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-158">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="1afc9-159">Kullanım `FromSqlInterpolated`, `ExecuteSqlInterpolated`, ve `ExecuteSqlInterpolatedAsync` burada parametreleri geçirilir ilişkilendirilmiş sorgu dizesinin parçası olarak parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1afc9-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="1afc9-160">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-160">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="1afc9-161">Yukarıdaki sorgu her ikisi de aynı SQL parametrelere sahip aynı parametreli SQL üretecektir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-161">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="1afc9-162">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-162">**Why**</span></span>

<span data-ttu-id="1afc9-163">Yöntem aşırı yüklemeleri böyle güvenmelidir yanı sıra ilişkilendirilmiş dize yöntemini çağırmak için hedefi olduğu zaman yanlışlıkla ham dize yöntemini çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-163">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="1afc9-164">Bu, verilmiş olması, parametreli getirilemedi sorgularda neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-164">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="1afc9-165">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-165">**Mitigations**</span></span>

<span data-ttu-id="1afc9-166">Yeni yöntem adları kullanılacak anahtar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-166">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="1afc9-167">SQL yöntemleri yalnızca sorgu kökleri üzerinde belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="1afc9-167">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="1afc9-168">İzleme sorun #15704</span><span class="sxs-lookup"><span data-stu-id="1afc9-168">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="1afc9-169">Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-169">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1afc9-170">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-170">**Old behavior**</span></span>

<span data-ttu-id="1afc9-171">EF Core 3.0 önce `FromSql` yöntemi belirtilebilir herhangi bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-171">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="1afc9-172">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-172">**New behavior**</span></span>

<span data-ttu-id="1afc9-173">EF Core 3.0 ile başlayan yeni `FromSqlRaw` ve `FromSqlInterpolated` yöntemleri (hangi Değiştir `FromSql`) yalnızca sorgu kökleri üzerinde yani doğrudan belirtilebilir `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-173">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="1afc9-174">Başka bir yerde bunları denemek, bir derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-174">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="1afc9-175">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-175">**Why**</span></span>

<span data-ttu-id="1afc9-176">Belirtme `FromSql` herhangi bir yere dışında üzerinde bir `DbSet` hiçbir anlamı eklendi veya değer ve belirli senaryolarda karışıklığa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-176">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="1afc9-177">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-177">**Mitigations**</span></span>

<span data-ttu-id="1afc9-178">`FromSql` çağrıları doğrudan açık olmasını taşınması gereken `DbSet` hangi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-178">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="1afc9-179">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe~~ geri Dönüldü</span><span class="sxs-lookup"><span data-stu-id="1afc9-179">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="1afc9-180">İzleme sorun #14523</span><span class="sxs-lookup"><span data-stu-id="1afc9-180">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1afc9-181">Bu değişiklik, EF Core 3.0-Önizleme 7 döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1afc9-181">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1afc9-182">EF Core 3. 0'ı yeni yapılandırma günlük düzeyi için herhangi bir olay uygulama tarafından belirtilmesine izin verdiği için Biz bu değişiklik geri alınır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-182">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="1afc9-183">Örneğin, SQL günlüğü geçiş için `Debug`, açıkça düzeyinde yapılandırma `OnConfiguring` veya `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="1afc9-183">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1afc9-184">Geçici bir anahtar değere artık varlık örneklerini ayarlanır</span><span class="sxs-lookup"><span data-stu-id="1afc9-184">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1afc9-185">İzleme sorun #12378</span><span class="sxs-lookup"><span data-stu-id="1afc9-185">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1afc9-186">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-186">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1afc9-187">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-187">**Old behavior**</span></span>

<span data-ttu-id="1afc9-188">EF Core 3.0 önce veritabanı tarafından oluşturulan gerçek değer daha sonra sahip olabileceği tüm anahtar özellikleri geçici değerler atanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-188">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1afc9-189">Genellikle bu geçici değerleri büyük negatif sayılar yoktu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-189">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1afc9-190">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-190">**New behavior**</span></span>

<span data-ttu-id="1afc9-191">3\.0 ile başlayarak, EF Core, varlığın izleme bilgilerini bir parçası olarak geçici bir anahtar değeri depolar ve anahtar özelliği kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-191">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1afc9-192">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-192">**Why**</span></span>

<span data-ttu-id="1afc9-193">Geçici bir anahtar değere deneyebileceğinizi daha önce bazı tarafından izlenen bir varlık kalıcı hale gelmesini önlemek için bu değişiklik yapılmıştır `DbContext` örneği farklı bir taşınır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="1afc9-193">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1afc9-194">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-194">**Mitigations**</span></span>

<span data-ttu-id="1afc9-195">Varlıklar arasında ilişkilendirmeler form üzerine yabancı anahtarlar birincil anahtar değerlerini atamak uygulamaları birincil anahtarları depoda üretilmiş ve varlıklara ait eski davranışı bağımlı `Added` durumu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-195">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1afc9-196">Tarafından önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="1afc9-196">This can be avoided by:</span></span>
* <span data-ttu-id="1afc9-197">Depoda üretilmiş anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="1afc9-197">Not using store-generated keys.</span></span>
* <span data-ttu-id="1afc9-198">Yabancı anahtar değerleri ayarlamak yerine form ilişkiler için Gezinti özelliklerini ayarlama.</span><span class="sxs-lookup"><span data-stu-id="1afc9-198">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1afc9-199">Gerçek geçici anahtar değerlerinin varlığın izleme bilgileri edinin.</span><span class="sxs-lookup"><span data-stu-id="1afc9-199">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1afc9-200">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` geçici değer olsa bile iade `blog.Id` kendisini ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-200">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1afc9-201">Depoda üretilmiş anahtar değerlerini DetectChanges geliştirir</span><span class="sxs-lookup"><span data-stu-id="1afc9-201">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1afc9-202">İzleme sorun #14616</span><span class="sxs-lookup"><span data-stu-id="1afc9-202">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1afc9-203">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-203">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-204">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-204">**Old behavior**</span></span>

<span data-ttu-id="1afc9-205">EF Core 3.0 önce izlenmeyen bir varlık tarafından bulunan `DetectChanges` olarak izlenmesi `Added` durum ve eklenen yeni bir olarak ne zaman satır `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-205">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1afc9-206">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-206">**New behavior**</span></span>

<span data-ttu-id="1afc9-207">Oluşturulan anahtar değerlerini bir varlık kullanıyorsa EF Core 3.0 ile başlayan ve bazı anahtar değerini ayarlayın ve ardından varlığı olarak takip `Modified` durumu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-207">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1afc9-208">Bu varlık için bir satır var olduğu varsayılır ve bu anlamına gelir ne zaman güncelleştirilmiş `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-208">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1afc9-209">Anahtar değeri ayarlanmamış, veya varlık türü kullanmıyor oluşturulan anahtarları varsa yeni bir varlık olarak hala izlenecektir `Added` önceki sürümlerinde olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-209">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1afc9-210">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-210">**Why**</span></span>

<span data-ttu-id="1afc9-211">Bu değişiklik, daha kolay ve bağlantısı kesilmiş varlık grafiklerle depoda üretilmiş anahtarlar kullanılırken çalışmak için daha tutarlı hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-211">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1afc9-212">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-212">**Mitigations**</span></span>

<span data-ttu-id="1afc9-213">Bu değişiklik, bir varlık türü için üretilmiş anahtarlar yapılandırılır, ancak anahtar değerlerine açıkça yeni örnekleri için ayarlanan uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-213">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1afc9-214">Düzeltme üretilmiş değerleri anahtar özelliklerine açıkça yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-214">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1afc9-215">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="1afc9-215">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1afc9-216">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="1afc9-216">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1afc9-217">Art arda silme işlemi hemen hemen varsayılan olarak gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="1afc9-217">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1afc9-218">İzleme sorun #10114</span><span class="sxs-lookup"><span data-stu-id="1afc9-218">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1afc9-219">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-219">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-220">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-220">**Old behavior**</span></span>

<span data-ttu-id="1afc9-221">SaveChanges çağrıldı kadar EF Core uygulanan basamaklı eylemlerinin (gerekli sorumlu silindiğinde veya ilişki gerekli sorumlusuna yazıyordunuz bağımlı varlıkları silmek) 3.0 önce gerçekleşmedi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-221">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1afc9-222">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-222">**New behavior**</span></span>

<span data-ttu-id="1afc9-223">Tetikleme koşulu algılandığında hemen sonra 3.0 ile başlayarak, EF Core basamaklı eylemlerinin geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-223">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1afc9-224">Örneğin, çağırma `context.Remove()` asıl bir varlığı silme tüm izlenen sonucu ilgili gerekli bağımlılıklar da ayarlanacak şekilde `Deleted` hemen.</span><span class="sxs-lookup"><span data-stu-id="1afc9-224">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1afc9-225">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-225">**Why**</span></span>

<span data-ttu-id="1afc9-226">Veri bağlama ve hangi varlıkları silinecek anlamanız önemli olduğu senaryolar denetimi deneyimini geliştirmek için bu değişiklik yapılmıştır _önce_ `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-226">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1afc9-227">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-227">**Mitigations**</span></span>

<span data-ttu-id="1afc9-228">Davranış ayarları aracılığıyla geri yüklenebilir `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-228">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1afc9-229">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-229">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="1afc9-230">DeleteBehavior.Restrict temizleyici semantiğe sahip</span><span class="sxs-lookup"><span data-stu-id="1afc9-230">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="1afc9-231">İzleme sorun #12661</span><span class="sxs-lookup"><span data-stu-id="1afc9-231">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="1afc9-232">Bu değişiklik, EF Core 3.0-preview 5'teki sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-232">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="1afc9-233">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-233">**Old behavior**</span></span>

<span data-ttu-id="1afc9-234">3\.0 önce `DeleteBehavior.Restrict` ile veritabanındaki yabancı anahtarlar oluşturulan `Restrict` semantiği, aynı zamanda açık olmayan bir şekilde değiştirilmiş iç düzeltme.</span><span class="sxs-lookup"><span data-stu-id="1afc9-234">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="1afc9-235">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-235">**New behavior**</span></span>

<span data-ttu-id="1afc9-236">3\.0 ile başlayan `DeleteBehavior.Restrict` yabancı anahtarlar ile oluşturulan sağlar `Restrict` semantiği--diğer bir deyişle, hiçbir basamaklar; throw EF iç düzeltme de etkilemeden kısıtlama ihlali üzerinde--.</span><span class="sxs-lookup"><span data-stu-id="1afc9-236">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="1afc9-237">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-237">**Why**</span></span>

<span data-ttu-id="1afc9-238">Bu değişiklik deneyimini geliştirmek amacıyla kullanılarak yapıldığını `DeleteBehavior` beklenmeyen yan etkiler olmadan sezgisel bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="1afc9-238">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="1afc9-239">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-239">**Mitigations**</span></span>

<span data-ttu-id="1afc9-240">Kullanarak önceki davranış geri yüklenebilir `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-240">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1afc9-241">Sorgu türleri varlık türleri ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-241">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1afc9-242">İzleme sorun #14194</span><span class="sxs-lookup"><span data-stu-id="1afc9-242">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1afc9-243">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-243">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-244">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-244">**Old behavior**</span></span>

<span data-ttu-id="1afc9-245">EF Core 3.0 önce [sorgu türü](xref:core/modeling/query-types) olan birincil anahtar, yapılandırılmış bir biçimde tanımlamıyor verileri sorgulamak için bir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-245">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1afc9-246">Diğer bir deyişle, bir sorgu türü kullanıldı sırasında normal bir varlık anahtarları (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) olmadan varlık türleriyle eşlemek için bir anahtar kullanılabilir (daha büyük olasılıkla bir tablodan ancak büyük olasılıkla bir görünümden) olduğunda türü kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-246">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1afc9-247">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-247">**New behavior**</span></span>

<span data-ttu-id="1afc9-248">Bir sorgu türü artık yalnızca birincil anahtarı olmayan bir varlık türü olur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-248">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1afc9-249">Anahtarsız varlık türleri, önceki sürümlerde sorgu türleri aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-249">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1afc9-250">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-250">**Why**</span></span>

<span data-ttu-id="1afc9-251">Sorgu türleri amacı etrafında karışıklık azaltmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-251">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1afc9-252">Özellikle, anahtarsız varlık türleridir ve bu nedenle kendiliğinden salt okunur olduklarından, ancak yalnızca bir varlık türü salt okunur olması gerekir çünkü bunlar kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-252">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1afc9-253">Benzer şekilde, genellikle görünümlerine eşlenmiş, ancak yalnızca görünümleri genellikle anahtarlar tanımlama yok olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-253">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1afc9-254">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-254">**Mitigations**</span></span>

<span data-ttu-id="1afc9-255">Aşağıdaki bölümleri API artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="1afc9-255">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1afc9-256">**`ModelBuilder.Query<>()`** -Bunun yerine `ModelBuilder.Entity<>().HasNoKey()` hiçbir anahtarına sahip bir varlık türü işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-256">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1afc9-257">Bu yine de bir birincil anahtar bekleniyordu, ancak kuralı eşleşmiyor, yanlış yapılandırma önlemek için kural olarak yapılandırılamadığı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-257">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1afc9-258">**`DbQuery<>`** -Bunun yerine `DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-258">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1afc9-259">**`DbContext.Query<>()`** -Bunun yerine `DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-259">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1afc9-260">Sahip olunan tür ilişkileri için API yapılandırması değişti</span><span class="sxs-lookup"><span data-stu-id="1afc9-260">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1afc9-261">[Sorun #12444 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sorun #9148 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sorun #14153 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1afc9-261">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1afc9-262">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-262">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-263">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-263">**Old behavior**</span></span>

<span data-ttu-id="1afc9-264">EF Core 3.0 önce hemen sonra yapılandırma sahibi ilişkinin gerçekleştirildi `OwnsOne` veya `OwnsMany` çağırın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-264">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1afc9-265">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-265">**New behavior**</span></span>

<span data-ttu-id="1afc9-266">EF Core 3.0 ile başlayarak, var. Şimdi fluent API'sini kullanarak sahibine bir gezinti özelliğini yapılandırmak için `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-266">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1afc9-267">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-267">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1afc9-268">Yapılandırma sahibi arasındaki ilişkiyi ilgili ve ait hemen sonra zincirlenmesi gereken `WithOwner()` diğer ilişkileri nasıl yapılandırılacağını benzer şekilde.</span><span class="sxs-lookup"><span data-stu-id="1afc9-268">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1afc9-269">Sahip olunan türü hala sonra zincirlenmesine için yapılandırma while `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-269">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1afc9-270">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-270">For example:</span></span>

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

<span data-ttu-id="1afc9-271">Ayrıca çağırma `Entity()`, `HasOne()`, veya `Set()` sahip olunan bir türü ile hedef şimdi bir özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="1afc9-271">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1afc9-272">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-272">**Why**</span></span>

<span data-ttu-id="1afc9-273">Sahip olunan türü yapılandırma arasında daha net bir ayrım oluşturmak için bu değişiklik yapılmıştır ve _ilişkisi_ ait türü.</span><span class="sxs-lookup"><span data-stu-id="1afc9-273">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1afc9-274">Bu sırayla belirsizlik ve yöntemler gibi geçici karışıklık kaldırır `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-274">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1afc9-275">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-275">**Mitigations**</span></span>

<span data-ttu-id="1afc9-276">Yukarıdaki örnekte gösterildiği gibi yeni bir API yüzeyi kullanmak için sahip olunan tür ilişkileri yapılandırmasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1afc9-276">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1afc9-277">Bağımlı varlıkları tablo sorumlusuyla paylaşımı artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="1afc9-277">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="1afc9-278">İzleme sorun #9005</span><span class="sxs-lookup"><span data-stu-id="1afc9-278">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="1afc9-279">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-279">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-280">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-280">**Old behavior**</span></span>

<span data-ttu-id="1afc9-281">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1afc9-281">Consider the following model:</span></span>
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
<span data-ttu-id="1afc9-282">3\.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya aynı tabloya açıkça eşleştirilmiş bir `OrderDetails` örneği her zaman gerekli yeni bir eklerken `Order`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-282">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="1afc9-283">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-283">**New behavior**</span></span>

<span data-ttu-id="1afc9-284">3\.0 ile başlayarak, EF Core eklemek için sağlayan bir `Order` olmadan bir `OrderDetails` ve tüm eşler `OrderDetails` özellikleri hariç null yapılabilir sütunlar için birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-284">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="1afc9-285">EF Core kümeleri sorgulanırken `OrderDetails` için `null` gerekli özelliklerinden birini yoksa, bir değer veya birincil anahtarı yanı sıra gereken özellikleri yoktur ve tüm özellikleri `null`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-285">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="1afc9-286">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-286">**Mitigations**</span></span>

<span data-ttu-id="1afc9-287">İsteğe bağlı tüm sütunlar eklenmiş bağımlı paylaşımı tablo modelinizi varken işaret Gezinti olması beklenmiyor `null` Gezinti olduğunda durumlarında uygulama değiştirilmelidir sonra `null`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-287">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="1afc9-288">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmesini veya en az bir özelliğine sahip olmayan bir`null` atanmış değer.</span><span class="sxs-lookup"><span data-stu-id="1afc9-288">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="1afc9-289">Bir özelliğini eşleştirmek tüm varlıklar bir eşzamanlılık belirteci sütun içeren bir tablo paylaşımı sahip</span><span class="sxs-lookup"><span data-stu-id="1afc9-289">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="1afc9-290">İzleme sorun #14154</span><span class="sxs-lookup"><span data-stu-id="1afc9-290">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="1afc9-291">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-291">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-292">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-292">**Old behavior**</span></span>

<span data-ttu-id="1afc9-293">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1afc9-293">Consider the following model:</span></span>
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
<span data-ttu-id="1afc9-294">3\.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya yalnızca güncelleştirme aynı tabloya açıkça eşlenen `OrderDetails` değil güncelleştirecek `Version` değeri istemcisi ve İleri güncelleştirmeyi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-294">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="1afc9-295">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-295">**New behavior**</span></span>

<span data-ttu-id="1afc9-296">EF Core 3.0 ile başlayarak, yeni yayınlar `Version` değerini `Order` bunu sahipse `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-296">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="1afc9-297">Aksi takdirde model doğrulama sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-297">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="1afc9-298">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-298">**Why**</span></span>

<span data-ttu-id="1afc9-299">Eski eşzamanlılık belirteç değeri aynı tabloya eşlenen varlıkları yalnızca biri güncelleştirildiğinde önlemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-299">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="1afc9-300">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-300">**Mitigations**</span></span>

<span data-ttu-id="1afc9-301">Eşzamanlılık belirteci sütuna eşlenmiş bir özellik içerir tabloda paylaşımı tüm varlıklar gerekir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-301">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="1afc9-302">Mümkündür oluşturma bir gölge durumu:</span><span class="sxs-lookup"><span data-stu-id="1afc9-302">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="1afc9-303">Tüm türetilmiş türler için tek bir sütun eşlenmemiş türlerden devralınan özellikler artık eşlenir</span><span class="sxs-lookup"><span data-stu-id="1afc9-303">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="1afc9-304">İzleme sorun #13998</span><span class="sxs-lookup"><span data-stu-id="1afc9-304">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="1afc9-305">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-305">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-306">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-306">**Old behavior**</span></span>

<span data-ttu-id="1afc9-307">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1afc9-307">Consider the following model:</span></span>
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

<span data-ttu-id="1afc9-308">EF Core 3.0 önce `ShippingAddress` özelliği için sütunları ayırmak için eşlenebilir `BulkOrder` ve `Order` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="1afc9-308">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="1afc9-309">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-309">**New behavior**</span></span>

<span data-ttu-id="1afc9-310">3\.0 ile başlayarak, EF Core yalnızca bir sütun için oluşturur `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-310">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="1afc9-311">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-311">**Why**</span></span>

<span data-ttu-id="1afc9-312">Eski davranışı beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-312">The old behavoir was unexpected.</span></span>

<span data-ttu-id="1afc9-313">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-313">**Mitigations**</span></span>

<span data-ttu-id="1afc9-314">Özellik sütunu türetilmiş türler üzerinde ayırmak için açıkça eşlenebilir:</span><span class="sxs-lookup"><span data-stu-id="1afc9-314">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1afc9-315">Yabancı anahtar özellik kuralı asıl özelliğiyle aynı ada artık eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="1afc9-315">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1afc9-316">İzleme sorun #13274</span><span class="sxs-lookup"><span data-stu-id="1afc9-316">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1afc9-317">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-317">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-318">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-318">**Old behavior**</span></span>

<span data-ttu-id="1afc9-319">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1afc9-319">Consider the following model:</span></span>
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
<span data-ttu-id="1afc9-320">EF Core 3.0 önce `CustomerId` özelliği kullanılan Kural gereği için yabancı anahtarı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-320">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1afc9-321">Ancak, varsa `Order` bu da yapacağı sonra sahip olunan bir türü olan `CustomerId` birincil anahtar ve bu değilse genellikle beklentisi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-321">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1afc9-322">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-322">**New behavior**</span></span>

<span data-ttu-id="1afc9-323">3\.0 ile başlayarak, EF Core asıl özelliğiyle aynı ada sahip oldukları özellikleri için yabancı anahtarlar kurala göre kullanırsanız dener.</span><span class="sxs-lookup"><span data-stu-id="1afc9-323">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1afc9-324">Asıl tür adı asıl özellik adıyla birleştirilmiş ve asıl özellik adı desenleri ile birleştirilmiş gezinme adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-324">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1afc9-325">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-325">For example:</span></span>

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

<span data-ttu-id="1afc9-326">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-326">**Why**</span></span>

<span data-ttu-id="1afc9-327">Bu değişikliği yanlışlıkla bir birincil anahtar özelliği sahip olunan türüne tanımlamamaya özen gösterin çalışıldı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-327">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1afc9-328">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-328">**Mitigations**</span></span>

<span data-ttu-id="1afc9-329">Yabancı anahtar olması ve bu nedenle birincil anahtarın sonra açıkça bölüm özelliği isteniyorsa bu şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-329">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="1afc9-330">Veritabanı bağlantısı artık kapalı kullanılmazsa süresi TransactionScope tamamlanmadan önce artık</span><span class="sxs-lookup"><span data-stu-id="1afc9-330">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="1afc9-331">İzleme sorun #14218</span><span class="sxs-lookup"><span data-stu-id="1afc9-331">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="1afc9-332">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-332">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-333">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-333">**Old behavior**</span></span>

<span data-ttu-id="1afc9-334">EF Core bağlam içinde bağlantısına açarsa, 3.0 önce bir `TransactionScope`, bağlantı sırasında geçerli açık kalır `TransactionScope` etkindir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-334">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="1afc9-335">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-335">**New behavior**</span></span>

<span data-ttu-id="1afc9-336">Kullanmaya tamamladıktan hemen sonra 3.0 ile başlayarak, EF Core bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-336">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="1afc9-337">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-337">**Why**</span></span>

<span data-ttu-id="1afc9-338">Birden fazla bağlamı aynı kullanmak için bu değişiklik sağlayan `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-338">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="1afc9-339">Yeni davranış da EF6 eşleşir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-339">The new behavior also matches EF6.</span></span>

<span data-ttu-id="1afc9-340">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-340">**Mitigations**</span></span>

<span data-ttu-id="1afc9-341">Bağlantı açık çağrı açık kalması gerekiyorsa `OpenConnection()` EF Core, erken kapatmadığına emin olun:</span><span class="sxs-lookup"><span data-stu-id="1afc9-341">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1afc9-342">Her bir özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-342">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1afc9-343">İzleme sorun #6872</span><span class="sxs-lookup"><span data-stu-id="1afc9-343">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1afc9-344">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-344">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-345">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-345">**Old behavior**</span></span>

<span data-ttu-id="1afc9-346">EF Core 3.0 önce tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-346">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1afc9-347">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-347">**New behavior**</span></span>

<span data-ttu-id="1afc9-348">EF Core 3.0 ile başlayarak, her bir tamsayı anahtar özellik değer Oluşturucusu kendi bellek içi veritabanı kullanılırken alır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-348">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1afc9-349">Ayrıca, veritabanı silinirse, anahtar oluşturma için tüm tabloları sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-349">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1afc9-350">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-350">**Why**</span></span>

<span data-ttu-id="1afc9-351">Bellek içi anahtar oluşturma gerçek veritabanı anahtar oluşturma için daha yakından Hizala ve bellek içi veritabanı kullanılırken testler birbirinden ayırma özelliğini artırmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-351">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1afc9-352">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-352">**Mitigations**</span></span>

<span data-ttu-id="1afc9-353">Bunu ayarlamak için belirli bellek içi anahtar değerlerine bağlı olan bir uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-353">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1afc9-354">Göz önünde bulundurun yerine değil özel anahtar değerlerine bağlı olan veya yeni davranış eşleştirilecek güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="1afc9-354">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1afc9-355">Varsayılan olarak kullanılan destekleyen alanlar</span><span class="sxs-lookup"><span data-stu-id="1afc9-355">Backing fields are used by default</span></span>

[<span data-ttu-id="1afc9-356">İzleme sorun #12430</span><span class="sxs-lookup"><span data-stu-id="1afc9-356">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1afc9-357">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-357">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1afc9-358">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-358">**Old behavior**</span></span>

<span data-ttu-id="1afc9-359">3\.0 önce bir özellik için destek alanı biliniyordu olsa bile, EF Core yine de varsayılan olarak okuma ve özellik alıcı ve ayarlayıcı yöntemleri kullanarak özellik değeri yazma.</span><span class="sxs-lookup"><span data-stu-id="1afc9-359">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1afc9-360">Bunun özel durumu burada destek alanı doğrudan biliniyorsa ayarlamak sorgu yürütme oluştu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-360">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1afc9-361">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-361">**New behavior**</span></span>

<span data-ttu-id="1afc9-362">Bir özellik için destek alanı biliniyorsa EF Core 3.0 ile başlayarak, ardından EF Core her zaman okuyabilmesini ve yazma yedekleme alanını kullanarak bu özelliği.</span><span class="sxs-lookup"><span data-stu-id="1afc9-362">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1afc9-363">Uygulama, alıcı veya ayarlayıcı yöntemleri kodlanmış ek davranış bağlı değilse bu bir uygulama sonu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-363">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1afc9-364">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-364">**Why**</span></span>

<span data-ttu-id="1afc9-365">Bu değişiklik iş mantığı deneyebileceğinizi olarak tetikleyen EF Core önlemek için varsayılan olarak varlıkları içeren bir veritabanı işlemleri gerçekleştirirken yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-365">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1afc9-366">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-366">**Mitigations**</span></span>

<span data-ttu-id="1afc9-367">Öncesi 3.0 davranışı özellik erişim modu konfigürasyonuyla geri yüklenebilir `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-367">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="1afc9-368">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-368">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1afc9-369">Birden çok uyumlu yedekleme alan bulunamazsa throw</span><span class="sxs-lookup"><span data-stu-id="1afc9-369">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1afc9-370">İzleme sorun #12523</span><span class="sxs-lookup"><span data-stu-id="1afc9-370">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1afc9-371">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-371">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-372">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-372">**Old behavior**</span></span>

<span data-ttu-id="1afc9-373">Birden çok alan karşılarsa, EF Core 3.0 önce bazı öncelik sırası üzerinde bir alan seçilirdi sonra bir özelliğinin destek alanı bulma kurallarını temel.</span><span class="sxs-lookup"><span data-stu-id="1afc9-373">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1afc9-374">Bu, belirsiz durumda kullanılacak yanlış alan neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-374">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1afc9-375">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-375">**New behavior**</span></span>

<span data-ttu-id="1afc9-376">Birden çok alan aynı özelliğe eşleşirse EF Core 3.0 ile başlayarak, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-376">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1afc9-377">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-377">**Why**</span></span>

<span data-ttu-id="1afc9-378">Yalnızca biri doğru olduğunda sessiz bir şekilde bir alanı başka bir kullanmaktan kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-378">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1afc9-379">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-379">**Mitigations**</span></span>

<span data-ttu-id="1afc9-380">Belirsiz destekleyen alanlarla özellikleri, açıkça belirtilen kullanılacak bir alan olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-380">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1afc9-381">Örneğin, fluent API'sini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="1afc9-381">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="1afc9-382">Alan adı alanı-yalnızca özellik adlarının eşleşmesi gerekir</span><span class="sxs-lookup"><span data-stu-id="1afc9-382">Field-only property names should match the field name</span></span>

<span data-ttu-id="1afc9-383">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-383">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-384">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-384">**Old behavior**</span></span>

<span data-ttu-id="1afc9-385">EF Core 3.0 önce bir özellik bir dize değeri belirtilebilir ve bu ada sahip hiçbir özellik CLR türüne bulunduysa EF Core kuralı kurallarını kullanarak bir alan için eşleşen deneyin.</span><span class="sxs-lookup"><span data-stu-id="1afc9-385">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="1afc9-386">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-386">**New behavior**</span></span>

<span data-ttu-id="1afc9-387">EF Core 3.0 ile başlayarak, bir alan özelliği salt okunur alan adı tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-387">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="1afc9-388">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-388">**Why**</span></span>

<span data-ttu-id="1afc9-389">Benzer şekilde adlı iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır, ayrıca eşleşen kuralları yalnızca alan özellikleri için CLR eşleniyor özellikleri aynıdır kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-389">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="1afc9-390">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-390">**Mitigations**</span></span>

<span data-ttu-id="1afc9-391">Yalnızca alan özellikleri aynı eşleştirildikleri alan olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-391">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="1afc9-392">EF Core 3.0 daha yeni bir önizleme özelliği adından farklı bir alan adı açıkça yapılandırma yeniden etkinleştirmek planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="1afc9-392">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="1afc9-393">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağırın</span><span class="sxs-lookup"><span data-stu-id="1afc9-393">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="1afc9-394">İzleme sorun #14756</span><span class="sxs-lookup"><span data-stu-id="1afc9-394">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="1afc9-395">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-395">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-396">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-396">**Old behavior**</span></span>

<span data-ttu-id="1afc9-397">EF Core 3.0, çağırma önce `AddDbContext` veya `AddDbContextPool` de günlüğe kaydetme ve bellek D.I hizmetleriyle yapılan çağrılar aracılığıyla önbelleğe kaydetmek [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1afc9-397">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="1afc9-398">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-398">**New behavior**</span></span>

<span data-ttu-id="1afc9-399">EF Core 3.0 ile başlayan `AddDbContext` ve `AddDbContextPool` artık kayıt hizmetlerin bağımlılık ekleme (dı) sahip olur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-399">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="1afc9-400">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-400">**Why**</span></span>

<span data-ttu-id="1afc9-401">EF Core 3.0, bu hizmetler uygulamanın DI kapsayıcısında olduğunu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="1afc9-401">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="1afc9-402">Ancak, varsa `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlı EF Core tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-402">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="1afc9-403">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-403">**Mitigations**</span></span>

<span data-ttu-id="1afc9-404">Uygulamanız bu hizmetlere ihtiyacı varsa, sonra bunları açıkça DI kullanarak kapsayıcı kayıt [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1afc9-404">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1afc9-405">DbContext.Entry artık yerel DetectChanges gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="1afc9-405">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1afc9-406">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="1afc9-406">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1afc9-407">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-407">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-408">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-408">**Old behavior**</span></span>

<span data-ttu-id="1afc9-409">EF Core 3.0, çağırma önce `DbContext.Entry` izlenen tüm varlıklar için algılanabilir değişiklik neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-409">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1afc9-410">Bu durumu içinde kullanıma sunulan sağlamış `EntityEntry` güncel oluştu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-410">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1afc9-411">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-411">**New behavior**</span></span>

<span data-ttu-id="1afc9-412">Çağırma EF Core ile 3.0, başlangıç `DbContext.Entry` ilgili asıl varlıkları yalnızca belirtilen varlık ve diğer değişikliklerini algılamak girişimi izlenen başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="1afc9-412">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1afc9-413">Başka bir yerde değiştirir Bunun anlamı etkileri uygulama durumu olabilir. Bu yöntemi çağrılarak algılandı değil.</span><span class="sxs-lookup"><span data-stu-id="1afc9-413">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1afc9-414">Unutmayın `ChangeTracker.AutoDetectChangesEnabled` ayarlanır `false` sonra bile bu yerel değişiklik algılama devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-414">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1afc9-415">Değişiklik algılama--örneğin neden diğer yöntemleri `ChangeTracker.Entries` ve `SaveChanges`--yine de tam neden `DetectChanges` tüm varlıkları izlenir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-415">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1afc9-416">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-416">**Why**</span></span>

<span data-ttu-id="1afc9-417">Kullanmanın varsayılan performansını artırmak için bu değişiklik yapılmıştır `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-417">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1afc9-418">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-418">**Mitigations**</span></span>

<span data-ttu-id="1afc9-419">Çağrı `ChgangeTracker.DetectChanges()` açıkça çağırmadan önce `Entry` öncesi 3.0 davranış sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="1afc9-419">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1afc9-420">Dize ve bayt dizisi anahtarlar varsayılan olarak istemci tarafından oluşturulan değildir</span><span class="sxs-lookup"><span data-stu-id="1afc9-420">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1afc9-421">İzleme sorun #14617</span><span class="sxs-lookup"><span data-stu-id="1afc9-421">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1afc9-422">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-423">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-423">**Old behavior**</span></span>

<span data-ttu-id="1afc9-424">EF Core 3.0 önce `string` ve `byte[]` açıkça null olmayan bir değer ayarlamadan anahtar özellikleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-424">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1afc9-425">Böyle bir durumda anahtar değeri istemcide bayt için seri hale getirilmiş bir GUID olarak oluşturulacak `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-425">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1afc9-426">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-426">**New behavior**</span></span>

<span data-ttu-id="1afc9-427">Bir özel durum EF Core 3.0 ile başlayarak, anahtar değer kümesi gösteren oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-427">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1afc9-428">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-428">**Why**</span></span>

<span data-ttu-id="1afc9-429">Bu değişiklik nedeniyle istemci tarafından oluşturulan yapılmıştır `string` / `byte[]` değerleri genellikle olmadığı kullanışlı ve varsayılan davranışını yaptığı sabit nedeni hakkında oluşturulan anahtar değerler için yaygın bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="1afc9-429">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1afc9-430">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-430">**Mitigations**</span></span>

<span data-ttu-id="1afc9-431">Başka bir null olmayan değer ayarlarsanız anahtar özellikleri oluşturulan değerler kullanması gerektiğini açıkça belirterek öncesi 3.0 davranış elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-431">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1afc9-432">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="1afc9-432">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1afc9-433">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="1afc9-433">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1afc9-434">Kapsamlı bir hizmet Iloggerfactory sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="1afc9-434">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1afc9-435">İzleme sorun #14698</span><span class="sxs-lookup"><span data-stu-id="1afc9-435">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1afc9-436">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-436">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-437">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-437">**Old behavior**</span></span>

<span data-ttu-id="1afc9-438">EF Core 3.0 önce `ILoggerFactory` tek bir hizmet kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-438">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1afc9-439">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-439">**New behavior**</span></span>

<span data-ttu-id="1afc9-440">EF Core 3.0 ile başlayan `ILoggerFactory` şimdi kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-440">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1afc9-441">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-441">**Why**</span></span>

<span data-ttu-id="1afc9-442">Günlükçü ile ilişkisini izin vermek için bu değişiklik yapılmıştır bir `DbContext` diğer işlevleri sağlar ve bazı durumlarda pathological davranış gibi iç hizmet sağlayıcıları sayısındaki kaldıran örneği.</span><span class="sxs-lookup"><span data-stu-id="1afc9-442">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1afc9-443">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-443">**Mitigations**</span></span>

<span data-ttu-id="1afc9-444">Bu değişiklik, kaydetme ve EF Core iç hizmet sağlayıcısına özel hizmetler kullanarak sürece uygulama kodu etkilememesi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-444">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1afc9-445">Bu ortak değildir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-445">This isn't common.</span></span>
<span data-ttu-id="1afc9-446">Bu durumlarda, bilgilerin çoğunu hala çalışır, ancak bağlı olarak herhangi bir tekil hizmeti olacak `ILoggerFactory` almak için değiştirilmesi gerekecektir `ILoggerFactory` farklı bir yolla.</span><span class="sxs-lookup"><span data-stu-id="1afc9-446">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1afc9-447">Bu gibi durumlarda karşılaşırsanız lütfen sırasında bir sorun üzerinde dosya [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) nasıl kullandığınızı bize bildirmek `ILoggerFactory` sağlayacak şekilde biz bunu gelecekte kesmemesi nasıl daha iyi anlamak.</span><span class="sxs-lookup"><span data-stu-id="1afc9-447">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1afc9-448">Gezinti özellikleri tam olarak yüklü olduğundan yükleme Lazy proxy'leri artık varsayılır</span><span class="sxs-lookup"><span data-stu-id="1afc9-448">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1afc9-449">İzleme sorun #12780</span><span class="sxs-lookup"><span data-stu-id="1afc9-449">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1afc9-450">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-450">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-451">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-451">**Old behavior**</span></span>

<span data-ttu-id="1afc9-452">EF Core 3.0, bir kez önce bir `DbContext` bırakıldı veya bir belirtilen gezinme özelliğinin bir varlığı söz konusu bağlamdan elde tam yüklü ise, olduğunu bilmesinin imkanı yoktu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-452">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1afc9-453">Proxy'leri bunun yerine null olmayan bir değer varsa bir başvuru Gezinti yüklenir ve boş değilse, bir koleksiyon Gezinti yüklenen varsayılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-453">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1afc9-454">Bu durumlarda, yavaş dışarıdan yüklemeyi deneyen bir İşlemsiz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-454">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1afc9-455">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-455">**New behavior**</span></span>

<span data-ttu-id="1afc9-456">EF Core 3.0 ile başlayarak, proxy'leri bir gezinti özelliğinin yüklü olup olmadığını izler.</span><span class="sxs-lookup"><span data-stu-id="1afc9-456">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1afc9-457">Bu yüklenen Gezinti boş veya null olduğunda bile bağlamı bırakıldıktan sonra yüklenen bir gezinti özelliği erişmeye çalışan her zaman bir İşlemsiz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-457">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1afc9-458">Buna karşılık, boş bir koleksiyon gezinme özelliği olsa bile bağlamı elden yüklü olmayan bir gezinti özelliği erişmeye çalışan bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-458">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1afc9-459">Bu durum ortaya çıkarsa, uygulama kodu, geçersiz bir zamanda yavaş yükleme kullanmak çalışıyor ve bunu yapmak için uygulama değiştirilmelidir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-459">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1afc9-460">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-460">**Why**</span></span>

<span data-ttu-id="1afc9-461">Bu değişiklik, bırakılmış yükü yavaş çalışırken davranışını tutarlı ve doğru hale yapılmıştır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="1afc9-461">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1afc9-462">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-462">**Mitigations**</span></span>

<span data-ttu-id="1afc9-463">Uygulama kodunun yükleme yavaş bırakılmış bağlamla kullanmamanız güncelleştirin veya bu özel durum iletisini açıklandığı bir İşlemsiz olacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-463">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1afc9-464">İç hizmet sağlayıcılarının aşırı oluşturma artık varsayılan olarak bir hata</span><span class="sxs-lookup"><span data-stu-id="1afc9-464">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1afc9-465">İzleme sorun #10236</span><span class="sxs-lookup"><span data-stu-id="1afc9-465">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1afc9-466">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-466">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-467">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-467">**Old behavior**</span></span>

<span data-ttu-id="1afc9-468">EF Core 3.0 önce bir uyarı iç hizmet sağlayıcıları pathological bir dizi oluşturmak için bir uygulama oturum açmış olmanız.</span><span class="sxs-lookup"><span data-stu-id="1afc9-468">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1afc9-469">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-469">**New behavior**</span></span>

<span data-ttu-id="1afc9-470">EF Core 3.0 ile başlayarak, bu uyarı artık olarak kabul edilir ve hata ve özel durum harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-470">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1afc9-471">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-471">**Why**</span></span>

<span data-ttu-id="1afc9-472">Bu değişiklik, daha açık pathological bu durumda gösterme yoluyla uygulama kodu daha iyi desteklemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-472">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1afc9-473">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-473">**Mitigations**</span></span>

<span data-ttu-id="1afc9-474">Bu hata ile karşılaşma eylemde en uygun nedeni, kök nedenini anlamak ve çok sayıda iç hizmet sağlayıcıları oluşturmayı durdur oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-474">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1afc9-475">Ancak, hata bir uyarı olarak geri dönüştürülür (veya yok sayıldı) aracılığıyla yapılandırma `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-475">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1afc9-476">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-476">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="1afc9-477">Tek bir dize ile HasOne/çok ilişkin yeni davranış çağırılır</span><span class="sxs-lookup"><span data-stu-id="1afc9-477">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="1afc9-478">İzleme sorun #9171</span><span class="sxs-lookup"><span data-stu-id="1afc9-478">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="1afc9-479">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-479">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-480">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-480">**Old behavior**</span></span>

<span data-ttu-id="1afc9-481">EF Core 3.0 önce kod arama `HasOne` veya `HasMany` tek bir dize ile kafa karıştırıcı bir şekilde yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-481">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="1afc9-482">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-482">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="1afc9-483">Kod ilgili Görülüyor `Samurai` bazı diğer varlık türü kullanarak `Entrance` özel gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="1afc9-483">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="1afc9-484">Bazı varlık türü olarak adlandırılan bir ilişki oluşturmak bu kodu gerçekte çalışır `Entrance` ile gezinti özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="1afc9-484">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="1afc9-485">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-485">**New behavior**</span></span>

<span data-ttu-id="1afc9-486">EF Core 3.0 ile başlayarak, yukarıdaki kod artık önce yapmakta gibi hangi Aranan yapar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-486">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="1afc9-487">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-487">**Why**</span></span>

<span data-ttu-id="1afc9-488">Eski davranışı özellikle yapılandırma kodu okurken ve hatalarını arayarak çok karmaşık.</span><span class="sxs-lookup"><span data-stu-id="1afc9-488">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="1afc9-489">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-489">**Mitigations**</span></span>

<span data-ttu-id="1afc9-490">Bu, yalnızca tür adları ve gezinme özelliğini açıkça belirtmeden dizeleriyle ilişkileri açıkça yapılandırmakta olduğunuz uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="1afc9-490">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="1afc9-491">Bu yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-491">This is not common.</span></span>
<span data-ttu-id="1afc9-492">Önceki davranışı açıkça geçirme aracılığıyla alınabilir `null` gezinme özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-492">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="1afc9-493">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-493">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="1afc9-494">Birden çok zaman uyumsuz yöntemler için dönüş türü için ValueTask görevden değiştirildi</span><span class="sxs-lookup"><span data-stu-id="1afc9-494">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="1afc9-495">İzleme sorun #15184</span><span class="sxs-lookup"><span data-stu-id="1afc9-495">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="1afc9-496">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-496">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-497">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-497">**Old behavior**</span></span>

<span data-ttu-id="1afc9-498">Aşağıdaki zaman uyumsuz yöntemler daha önce döndürülen bir `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="1afc9-498">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="1afc9-499">`ValueGenerator.NextValueAsync()` (ve türetilen sınıflar)</span><span class="sxs-lookup"><span data-stu-id="1afc9-499">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="1afc9-500">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-500">**New behavior**</span></span>

<span data-ttu-id="1afc9-501">Yukarıda sözü edilen yöntemleri artık döndürür bir `ValueTask<T>` aynı üzerinden `T` önceki gibi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-501">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="1afc9-502">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-502">**Why**</span></span>

<span data-ttu-id="1afc9-503">Bu değişiklik, yığın ayırmaları genel performansını iyileştirme, bu yöntemleri çağrılırken tahakkuk sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-503">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="1afc9-504">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-504">**Mitigations**</span></span>

<span data-ttu-id="1afc9-505">Yalnızca yukarıdaki API'leri yalnızca bekleyen gerekir derlenmesi için - uygulamalar kaynak değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-505">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="1afc9-506">Daha karmaşık bir kullanım (örneğin döndürülen geçirme `Task` için `Task.WhenAny()`) genellikle gerektiren döndürülen `ValueTask<T>` dönüştürülmesi bir `Task<T>` çağırarak `AsTask()` üzerindeki.</span><span class="sxs-lookup"><span data-stu-id="1afc9-506">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="1afc9-507">Bu bu değişiklik getirir ayırma azaltma verilerek unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-507">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1afc9-508">İlişkisel: TypeMapping ek açıklama yalnızca TypeMapping sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="1afc9-508">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1afc9-509">İzleme sorun #9913</span><span class="sxs-lookup"><span data-stu-id="1afc9-509">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1afc9-510">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-510">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1afc9-511">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-511">**Old behavior**</span></span>

<span data-ttu-id="1afc9-512">Tür eşlemesi ek açıklamaları için ek açıklama "İlişkisel: TypeMapping" adlandırılmıştı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-512">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1afc9-513">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-513">**New behavior**</span></span>

<span data-ttu-id="1afc9-514">Ek açıklama türü eşleme ek açıklamalar için artık "TypeMapping" addır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-514">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1afc9-515">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-515">**Why**</span></span>

<span data-ttu-id="1afc9-516">Türü eşlemeleri artık birden fazla yalnızca ilişkisel veritabanı sağlayıcıları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-516">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1afc9-517">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-517">**Mitigations**</span></span>

<span data-ttu-id="1afc9-518">Bu, yalnızca tür eşlemesi ortak olmayan doğrudan bir ek açıklama, olarak erişen uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="1afc9-518">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1afc9-519">API yüzeyi için ek açıklama doğrudan kullanmak yerine erişim türü eşlemeleri düzeltmek için en uygun eylemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-519">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1afc9-520">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-520">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1afc9-521">İzleme sorun #11811</span><span class="sxs-lookup"><span data-stu-id="1afc9-521">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1afc9-522">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-522">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-523">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-523">**Old behavior**</span></span>

<span data-ttu-id="1afc9-524">EF Core 3.0 önce `ToTable()` türetilmiş üzerinde adlı türü yoksayılan beri tek devralma eşleme stratejisi TPH burada bu geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="1afc9-524">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1afc9-525">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-525">**New behavior**</span></span>

<span data-ttu-id="1afc9-526">EF Core 3.0 ve sonraki bir sürümde TPT ve TPC desteği eklemek için hazırlık başlangıç `ToTable()` beklenmeyen eşleme değişiklik gelecekte önlemek için bir özel durum bir türetilmiş tür şimdi throw üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-526">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1afc9-527">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-527">**Why**</span></span>

<span data-ttu-id="1afc9-528">Şu anda farklı bir tabloya türetilmiş bir tür eşlemek için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="1afc9-528">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1afc9-529">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda bozucu önler.</span><span class="sxs-lookup"><span data-stu-id="1afc9-529">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1afc9-530">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-530">**Mitigations**</span></span>

<span data-ttu-id="1afc9-531">Türetilen türlerin diğer tablolara eşleme girişimleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-531">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1afc9-532">ForSqlServerHasIndex HasIndex ile değiştirildi</span><span class="sxs-lookup"><span data-stu-id="1afc9-532">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1afc9-533">İzleme sorun #12366</span><span class="sxs-lookup"><span data-stu-id="1afc9-533">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1afc9-534">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-534">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-535">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-535">**Old behavior**</span></span>

<span data-ttu-id="1afc9-536">EF Core 3.0 önce `ForSqlServerHasIndex().ForSqlServerInclude()` ile kullanılan sütunları yapılandırmak üzere bir yöntem sağlayan `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-536">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1afc9-537">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-537">**New behavior**</span></span>

<span data-ttu-id="1afc9-538">EF Core 3.0 ile başlayarak, kullanarak `Include` dizin ilişkisel düzeyinde artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-538">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1afc9-539">Kullanım `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-539">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1afc9-540">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-540">**Why**</span></span>

<span data-ttu-id="1afc9-541">API ile dizin için birleştirmek için bu değişiklik yapılmıştır `Include` tüm sağlayıcıları veritabanı için içine yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1afc9-541">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="1afc9-542">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-542">**Mitigations**</span></span>

<span data-ttu-id="1afc9-543">Yukarıda da gösterildiği gibi yeni API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-543">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="1afc9-544">Meta veri API'si değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="1afc9-544">Metadata API changes</span></span>

[<span data-ttu-id="1afc9-545">İzleme sorun #214</span><span class="sxs-lookup"><span data-stu-id="1afc9-545">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1afc9-546">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-546">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-547">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-547">**New behavior**</span></span>

<span data-ttu-id="1afc9-548">Aşağıdaki özellikler için genişletme yöntemleri dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="1afc9-548">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="1afc9-549">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-549">**Why**</span></span>

<span data-ttu-id="1afc9-550">Bu değişiklik, yukarıda sözü edilen arabirimleri uygulamasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-550">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="1afc9-551">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-551">**Mitigations**</span></span>

<span data-ttu-id="1afc9-552">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-552">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="1afc9-553">Sağlayıcıya özgü meta veriler API'SİNDEKİ değişiklikler</span><span class="sxs-lookup"><span data-stu-id="1afc9-553">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="1afc9-554">İzleme sorun #214</span><span class="sxs-lookup"><span data-stu-id="1afc9-554">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1afc9-555">Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-555">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1afc9-556">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-556">**New behavior**</span></span>

<span data-ttu-id="1afc9-557">Sağlayıcıya özgü genişletme yöntemlerini düzleştirilmiş:</span><span class="sxs-lookup"><span data-stu-id="1afc9-557">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="1afc9-558">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-558">**Why**</span></span>

<span data-ttu-id="1afc9-559">Bu değişiklik, yukarıda sözü edilen genişletme yöntemleri uygulamasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-559">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="1afc9-560">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-560">**Mitigations**</span></span>

<span data-ttu-id="1afc9-561">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-561">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1afc9-562">EF Core artık SQLite FK zorlama pragma gönderir</span><span class="sxs-lookup"><span data-stu-id="1afc9-562">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1afc9-563">İzleme sorun #12151</span><span class="sxs-lookup"><span data-stu-id="1afc9-563">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1afc9-564">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-564">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1afc9-565">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-565">**Old behavior**</span></span>

<span data-ttu-id="1afc9-566">EF Core 3.0 önce EF Core gönderir gibi `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="1afc9-566">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1afc9-567">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-567">**New behavior**</span></span>

<span data-ttu-id="1afc9-568">EF Core 3.0 ile başlayarak, EF Core artık gönderir `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="1afc9-568">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1afc9-569">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-569">**Why**</span></span>

<span data-ttu-id="1afc9-570">EF Core kullandığından bu değişiklik yapılmıştır `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak, hangi sırayla anlamına gelir FK zorlama varsayılan olarak açık ve açıkça bir bağlantı her açıldığında etkinleştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1afc9-570">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1afc9-571">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-571">**Mitigations**</span></span>

<span data-ttu-id="1afc9-572">Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-572">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1afc9-573">Diğer durumlarda, yabancı anahtarlar belirterek etkinleştirilebilir `Foreign Keys=True` bağlantı dizenizi içinde.</span><span class="sxs-lookup"><span data-stu-id="1afc9-573">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="1afc9-574">Microsoft.EntityFrameworkCore.Sqlite üzerinde SQLitePCLRaw.bundle_e_sqlite3 artık bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-574">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1afc9-575">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-575">**Old behavior**</span></span>

<span data-ttu-id="1afc9-576">EF Core 3.0 önce EF Core kullanılan `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-576">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1afc9-577">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-577">**New behavior**</span></span>

<span data-ttu-id="1afc9-578">EF Core 3.0 ile başlayarak, EF Core kullanan `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-578">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1afc9-579">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-579">**Why**</span></span>

<span data-ttu-id="1afc9-580">Diğer platformlar ile tutarlı ios'ta kullanılan SQLite sürümü, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-580">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1afc9-581">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-581">**Mitigations**</span></span>

<span data-ttu-id="1afc9-582">İos'ta yerel bir SQLite sürümü kullanmak için yapılandırma `Microsoft.Data.Sqlite` farklı bir kullanılacak `SQLitePCLRaw` paket.</span><span class="sxs-lookup"><span data-stu-id="1afc9-582">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1afc9-583">GUID değerlerinin artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="1afc9-583">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1afc9-584">İzleme sorun #15078</span><span class="sxs-lookup"><span data-stu-id="1afc9-584">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="1afc9-585">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-585">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-586">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-586">**Old behavior**</span></span>

<span data-ttu-id="1afc9-587">GUID değerleri, daha önce SQLite değerlerine BLOB olarak sored.</span><span class="sxs-lookup"><span data-stu-id="1afc9-587">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="1afc9-588">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-588">**New behavior**</span></span>

<span data-ttu-id="1afc9-589">GUID değerleri, artık metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-589">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="1afc9-590">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-590">**Why**</span></span>

<span data-ttu-id="1afc9-591">İkili biçimi GUID'leri standartlaştırılmış değil.</span><span class="sxs-lookup"><span data-stu-id="1afc9-591">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="1afc9-592">Değerlerin metin olarak depolanması veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-592">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1afc9-593">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-593">**Mitigations**</span></span>

<span data-ttu-id="1afc9-594">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1afc9-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="1afc9-595">EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-595">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="1afc9-596">Microsoft.Data.Sqlite GUID değerlerinin hem BLOB hem de metin sütunları okuma özellikli kalır; Parametreler ve sabitleri için varsayılan biçimi değiştiğinden ancak büyük olasılıkla GUID'leri içeren çoğu senaryo için bir eylem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-596">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1afc9-597">Char değerleri artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="1afc9-597">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1afc9-598">İzleme sorun #15020</span><span class="sxs-lookup"><span data-stu-id="1afc9-598">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="1afc9-599">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-599">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-600">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-600">**Old behavior**</span></span>

<span data-ttu-id="1afc9-601">Char değerleri daha önce SQLite tamsayı değerleri olarak sored.</span><span class="sxs-lookup"><span data-stu-id="1afc9-601">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="1afc9-602">Örneğin, bir karakter değerini *A* 65 tamsayı değeri depolanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-602">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="1afc9-603">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-603">**New behavior**</span></span>

<span data-ttu-id="1afc9-604">Char değerleri artık metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-604">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="1afc9-605">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-605">**Why**</span></span>

<span data-ttu-id="1afc9-606">Değerlerin metin olarak depolanması daha doğal ve veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-606">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1afc9-607">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-607">**Mitigations**</span></span>

<span data-ttu-id="1afc9-608">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1afc9-608">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="1afc9-609">EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-609">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="1afc9-610">Microsoft.Data.Sqlite Ayrıca belirli senaryoları herhangi bir eylem gerekli değil şekilde karakter değerleri hem tamsayı hem de metin sütunları, okuma özellikli kalır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-610">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="1afc9-611">Geçiş kimlikleri, artık sabit kültürün takvimini kullanarak oluşturulur</span><span class="sxs-lookup"><span data-stu-id="1afc9-611">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="1afc9-612">İzleme sorun #12978</span><span class="sxs-lookup"><span data-stu-id="1afc9-612">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="1afc9-613">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-613">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-614">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-614">**Old behavior**</span></span>

<span data-ttu-id="1afc9-615">Geçiş kimlikleri, geçerli kültürün takvimini kullanarak yanlışlıkla üretildi.</span><span class="sxs-lookup"><span data-stu-id="1afc9-615">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="1afc9-616">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-616">**New behavior**</span></span>

<span data-ttu-id="1afc9-617">Geçiş kimlikleri artık her zaman sabit kültürün takvimini (Gregoryen) kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-617">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="1afc9-618">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-618">**Why**</span></span>

<span data-ttu-id="1afc9-619">Geçiş sırası önemlidir veritabanını güncelleştirmek ya da birleştirme çakışmalarını çözme.</span><span class="sxs-lookup"><span data-stu-id="1afc9-619">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="1afc9-620">Sabit takvimi kullanılarak sıralama ekip üyelerinden farklı sistem takvimler sahip sonuçlanabilen sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="1afc9-620">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="1afc9-621">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-621">**Mitigations**</span></span>

<span data-ttu-id="1afc9-622">Bu değişiklik, olmayan-Gregoryen takvim yılı Gregoryen takvimini (içeren Thai Budist takvimi gibi) daha büyük olduğu kullanan herkesin etkiler.</span><span class="sxs-lookup"><span data-stu-id="1afc9-622">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="1afc9-623">Mevcut geçiş kimlikleri böylece mevcut sonra yeni geçişleri sıralı güncelleştirilmesi gerekiyor geçişler.</span><span class="sxs-lookup"><span data-stu-id="1afc9-623">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="1afc9-624">Geçiş kimliği geçiş özniteliğinde geçişler Tasarımcı dosyalarında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-624">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="1afc9-625">Geçişleri geçmiş tablosu da güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="1afc9-625">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="1afc9-626">UseRowNumberForPaging kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="1afc9-626">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="1afc9-627">İzleme sorun #16400</span><span class="sxs-lookup"><span data-stu-id="1afc9-627">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="1afc9-628">Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-628">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1afc9-629">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-629">**Old behavior**</span></span>

<span data-ttu-id="1afc9-630">EF Core 3.0 önce `UseRowNumberForPaging` SQL Server 2008 ile uyumlu sayfalama SQL oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-630">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="1afc9-631">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-631">**New behavior**</span></span>

<span data-ttu-id="1afc9-632">EF Core 3.0 ile başlayarak, EF yalnızca SQL yalnızca sonraki SQL Server sürümleriyle uyumludur sayfalama oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-632">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="1afc9-633">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-633">**Why**</span></span>

<span data-ttu-id="1afc9-634">Çünkü bu değişiklik yapıyoruz [SQL Server 2008 olup artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) ve EF Core 3. 0'yaptığınız sorgu değişikliklerini çalışmak için bu özelliği güncelleştirmek, önemli iş kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-634">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="1afc9-635">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-635">**Mitigations**</span></span>

<span data-ttu-id="1afc9-636">Böylece oluşturulan SQL desteklenen SQL Server'ın daha yeni bir sürüme güncelleştirmek ya da daha yüksek bir uyumluluk düzeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="1afc9-636">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="1afc9-637">Bununla, bunu yapmanız mümkün değilse lütfen [izleme soruna açıklama](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ayrıntılarla.</span><span class="sxs-lookup"><span data-stu-id="1afc9-637">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="1afc9-638">Biz geri bildirimine göre bu kararı yeniden ziyaret.</span><span class="sxs-lookup"><span data-stu-id="1afc9-638">We may revisit this decision based on feedback.</span></span>

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="1afc9-639">Uzantı bilgileri/meta verileri IDbContextOptionsExtension kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="1afc9-639">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1afc9-640">İzleme sorun #16119</span><span class="sxs-lookup"><span data-stu-id="1afc9-640">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="1afc9-641">Bu değişiklik, EF Core 3.0-Önizleme 7 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-641">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1afc9-642">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-642">**Old behavior**</span></span>

<span data-ttu-id="1afc9-643">`IDbContextOptionsExtension` uzantı hakkında meta veri sağlamak için yöntemleri içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-643">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="1afc9-644">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-644">**New behavior**</span></span>

<span data-ttu-id="1afc9-645">Bu yöntemler, yeni bir taşınmış `DbContextOptionsExtensionInfo` soyut temel sınıf yeni bir döndürülen `IDbContextOptionsExtension.Info` özelliği.</span><span class="sxs-lookup"><span data-stu-id="1afc9-645">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="1afc9-646">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-646">**Why**</span></span>

<span data-ttu-id="1afc9-647">2\.0 sürümlerinden 3.0 üzerinden ekleyin veya bu yöntemleri birkaç kez değiştirmek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="1afc9-647">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="1afc9-648">Yeni bir soyut temel sınıf bozucu mevcut uzantılar bozup olmadan bu tür değişiklikler yapmaya kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-648">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="1afc9-649">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-649">**Mitigations**</span></span>

<span data-ttu-id="1afc9-650">Yeni desende uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1afc9-650">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="1afc9-651">Örnekler birçok uygulamalarında bulunan `IDbContextOptionsExtension` EF Core uzantılarında farklı türleri için kaynak kodu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-651">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="1afc9-652">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="1afc9-652">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="1afc9-653">İzleme sorun #10985</span><span class="sxs-lookup"><span data-stu-id="1afc9-653">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="1afc9-654">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-654">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-655">**Değişiklik**</span><span class="sxs-lookup"><span data-stu-id="1afc9-655">**Change**</span></span>

<span data-ttu-id="1afc9-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` yeniden adlandırıldı `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="1afc9-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="1afc9-657">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-657">**Why**</span></span>

<span data-ttu-id="1afc9-658">Bu uyarı olayı tüm uyarı olayları ile adlandırma hizalar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-658">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="1afc9-659">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-659">**Mitigations**</span></span>

<span data-ttu-id="1afc9-660">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-660">Use the new name.</span></span> <span data-ttu-id="1afc9-661">(Olay kimliği numarasını değiştirilmedi unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="1afc9-661">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="1afc9-662">API açıklığa kavuşturmak için yabancı anahtar kısıtlaması adları</span><span class="sxs-lookup"><span data-stu-id="1afc9-662">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="1afc9-663">İzleme sorun #10730</span><span class="sxs-lookup"><span data-stu-id="1afc9-663">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="1afc9-664">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-664">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-665">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-665">**Old behavior**</span></span>

<span data-ttu-id="1afc9-666">EF Core 3.0 önce yabancı anahtar kısıtlaması adları için yalnızca "adı olarak" adı veriliyordu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-666">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="1afc9-667">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-667">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="1afc9-668">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-668">**New behavior**</span></span>

<span data-ttu-id="1afc9-669">EF Core 3.0 ile başlayarak, yabancı anahtar kısıtlaması adları şimdi de "kısıtlama adı" adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-669">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="1afc9-670">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1afc9-670">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="1afc9-671">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-671">**Why**</span></span>

<span data-ttu-id="1afc9-672">Bu değişiklik, bu alanda adlandırma için tutarlılık getirir ve ayrıca bu yabancı anahtarı üzerinde tanımlanan yabancı anahtar kısıtlaması ve olmayan sütun veya özellik adı adı olduğunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-672">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="1afc9-673">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-673">**Mitigations**</span></span>

<span data-ttu-id="1afc9-674">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="1afc9-674">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="1afc9-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync yapıldı genel</span><span class="sxs-lookup"><span data-stu-id="1afc9-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="1afc9-676">İzleme sorun #15997</span><span class="sxs-lookup"><span data-stu-id="1afc9-676">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="1afc9-677">Bu değişiklik, EF Core 3.0-Önizleme 7 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-677">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1afc9-678">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-678">**Old behavior**</span></span>

<span data-ttu-id="1afc9-679">EF Core 3.0 önce bu yöntemleri korunmuş.</span><span class="sxs-lookup"><span data-stu-id="1afc9-679">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="1afc9-680">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-680">**New behavior**</span></span>

<span data-ttu-id="1afc9-681">EF Core 3.0 ile başlayarak, bu yöntemleri herkese açık.</span><span class="sxs-lookup"><span data-stu-id="1afc9-681">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="1afc9-682">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-682">**Why**</span></span>

<span data-ttu-id="1afc9-683">Bu yöntemler, bir veritabanı oluşturuldu ancak boş olup olmadığını belirlemek için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-683">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="1afc9-684">Ayrıca geçişleri uygulanacağını belirleyen olduğunda bu EF dışında yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-684">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="1afc9-685">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-685">**Mitigations**</span></span>

<span data-ttu-id="1afc9-686">Herhangi bir geçersiz kılma erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1afc9-686">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="1afc9-687">Microsoft.EntityFrameworkCore.Design artık DevelopmentDependency pakettir</span><span class="sxs-lookup"><span data-stu-id="1afc9-687">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="1afc9-688">İzleme sorun #11506</span><span class="sxs-lookup"><span data-stu-id="1afc9-688">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="1afc9-689">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-689">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1afc9-690">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-690">**Old behavior**</span></span>

<span data-ttu-id="1afc9-691">EF Core 3.0 önce derleme, ona bağımlı olan projeler tarafından başvurulan, normal bir NuGet paketi Microsoft.EntityFrameworkCore.Design oluştu.</span><span class="sxs-lookup"><span data-stu-id="1afc9-691">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="1afc9-692">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-692">**New behavior**</span></span>

<span data-ttu-id="1afc9-693">EF Core 3.0 ile başlayarak, bunu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-693">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="1afc9-694">Bağımlılık geçişli diğer projelere akış olmaz ve, varsayılan olarak, artık anlamına gelir, bütünleştirilmiş koda başvurun.</span><span class="sxs-lookup"><span data-stu-id="1afc9-694">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="1afc9-695">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-695">**Why**</span></span>

<span data-ttu-id="1afc9-696">Bu paket, yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-696">This package is only intended to be used at design time.</span></span> <span data-ttu-id="1afc9-697">Dağıtılan uygulamalar ona başvuran olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="1afc9-697">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="1afc9-698">Paket bir DevelopmentDependency yaparak bu öneriyi çalışacak şekilde güçlendirir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-698">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="1afc9-699">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-699">**Mitigations**</span></span>

<span data-ttu-id="1afc9-700">Bu paketi, EF Core'nın tasarım zamanı davranışını geçersiz kılmak için başvurmanız gerekiyorsa, güncelleştirme güncelleştirebilirsiniz PackageReference öğe meta verileri, projenizdeki.</span><span class="sxs-lookup"><span data-stu-id="1afc9-700">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="1afc9-701">Paketi Microsoft.EntityFrameworkCore.Tools geçişli başvuruluyor, açık bir PackageReference meta verilerini değiştirmek için paketi eklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-701">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="1afc9-702">SQLitePCL.raw 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="1afc9-702">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="1afc9-703">İzleme sorun #14824</span><span class="sxs-lookup"><span data-stu-id="1afc9-703">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="1afc9-704">Bu değişiklik, EF Core 3.0-Önizleme 7 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-704">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1afc9-705">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-705">**Old behavior**</span></span>

<span data-ttu-id="1afc9-706">Microsoft.EntityFrameworkCore.Sqlite önceden SQLitePCL.raw sürüm 1.1.12 bağlıydı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-706">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="1afc9-707">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-707">**New behavior**</span></span>

<span data-ttu-id="1afc9-708">Olduğunuz güncelleştiriyoruz bizim paketini 2.0.0 sürümüne bağlı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-708">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="1afc9-709">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-709">**Why**</span></span>

<span data-ttu-id="1afc9-710">.NET Standard 2.0 SQLitePCL.raw 2.0.0 sürümünü hedefler.</span><span class="sxs-lookup"><span data-stu-id="1afc9-710">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="1afc9-711">Daha önce .NET standart, geçişli paket çalışmak için büyük bir kapanış gerekli 1.1 hedeflenen.</span><span class="sxs-lookup"><span data-stu-id="1afc9-711">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="1afc9-712">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-712">**Mitigations**</span></span>

<span data-ttu-id="1afc9-713">SQLitePCL.raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-713">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="1afc9-714">Bkz: [sürüm notları](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="1afc9-714">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="1afc9-715">NetTopologySuite 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="1afc9-715">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="1afc9-716">İzleme sorun #14825</span><span class="sxs-lookup"><span data-stu-id="1afc9-716">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="1afc9-717">Bu değişiklik, EF Core 3.0-Önizleme 7 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1afc9-717">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1afc9-718">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1afc9-718">**Old behavior**</span></span>

<span data-ttu-id="1afc9-719">Uzamsal paketleri, daha önce NetTopologySuite sürüm 1.15.1 bağlıydı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-719">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="1afc9-720">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1afc9-720">**New behavior**</span></span>

<span data-ttu-id="1afc9-721">Olduğunuz güncelleştiriyoruz bizim paketini 2.0.0 sürümüne bağlı.</span><span class="sxs-lookup"><span data-stu-id="1afc9-721">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="1afc9-722">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1afc9-722">**Why**</span></span>

<span data-ttu-id="1afc9-723">EF Core kullanıcılar tarafından karşılaşılan birkaç kullanılabilirlik sorunlarını gidermek için NetTopologySuite 2.0.0 sürümü amaçlar.</span><span class="sxs-lookup"><span data-stu-id="1afc9-723">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="1afc9-724">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1afc9-724">**Mitigations**</span></span>

<span data-ttu-id="1afc9-725">Sürüm 2.0.0 NetTopologySuite bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="1afc9-725">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="1afc9-726">Bkz: [sürüm notları](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="1afc9-726">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
