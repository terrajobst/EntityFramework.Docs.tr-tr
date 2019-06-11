---
title: EF Core 3.0 - EF Core yeni değişiklikler
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 9112d8d235237e68232aac54453d584af0edb524
ms.sourcegitcommit: b188194a1901f4d086d05765cbc5c9b8c9dc5eed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66829490"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1e5b9-102">EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil edilen değişiklikler</span><span class="sxs-lookup"><span data-stu-id="1e5b9-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e5b9-103">Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1e5b9-104">Aşağıdaki API ve davranışı değişiklikleri EF Core için geliştirilen uygulamaları ayırmak için olası sahip bunları 3.0.0 için yükseltirken 2.2.x.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1e5b9-105">Veritabanı sağlayıcıları yalnızca etkileyen bekliyoruz değişiklikleri altında belgelenir [sağlayıcısı değişiklikleri](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1e5b9-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1e5b9-106">Bir 3.0 Önizlemesi'nden başka bir 3.0 Önizleme için kullanıma sunulan yeni özellikler sonlarını burada belgelenmemiş.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1e5b9-107">LINQ sorguları, artık istemcide değerlendirilir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="1e5b9-108">[Sorun #14935 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[sorun #12795 Ayrıca bkz:](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="1e5b9-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="1e5b9-109">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-110">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-110">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-111">EF Core, SQL veya bir parametre için bir sorgunun parçası olan bir ifade dönüştürülemedi, 3.0 önce bunu otomatik olarak istemcide ifadesi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1e5b9-112">Varsayılan olarak, istemci değerlendirmesi yüksek maliyetlere neden olabilecek bir ifade yalnızca bir uyarı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1e5b9-113">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-113">**New behavior**</span></span>

<span data-ttu-id="1e5b9-114">3.0 ile başlayarak, EF Core yalnızca ifadeleri üst düzey projeksiyon izin verir (son `Select()` sorguda çağrısı) istemcide değerlendirilecek.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1e5b9-115">İfadeleri herhangi bir sorgunun parçası olarak SQL veya bir parametre olarak dönüştürülemez, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1e5b9-116">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-116">**Why**</span></span>

<span data-ttu-id="1e5b9-117">Otomatik istemci değerlendirme sorguların bile bunları önemli bölümleri çevrilemez yürütülecek çok sayıda sorgu sağlar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1e5b9-118">Bu davranış yalnızca üretimde yetkisiz değiştirmeye karşı korumalı hale gelebilir beklenmeyen ve zararlı davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1e5b9-119">Örneğin, bir koşulda bir `Where()` çevrilemez çağrısı, veritabanı sunucusu ve istemcide uygulanması için filtreyi aktarılacak tablosundan tüm satırları açabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1e5b9-120">Tablo, geliştirme, ancak isabet sabit yalnızca birkaç satır içeriyorsa uygulama üretime burada tablo milyonlarca satır içerebilir taşındığında bu durum kolayca algılanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1e5b9-121">İstemci değerlendirme uyarılar ayrıca geliştirme sırasında göz ardı edilmesi çok kolay kanıtladılar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1e5b9-122">Belirli ifadeler için iyileştirme sorgu çevirisi sürümler arasında istenmeyen bozucu değişiklikleri nedeniyle oluşan sorunları bu yanı sıra, otomatik istemci değerlendirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1e5b9-123">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-123">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-124">Bir sorgu tam olarak çevrilemez sonra ya da çevrilebilir veya kullanan bir form sorguda yeniden `AsEnumerable()`, `ToList()`, veya açıkça verileri Burada, ardından daha büyük olabilir istemciye geri getirmek için benzer LINQ nesneleri kullanılarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1e5b9-125">Entity Framework Core artık ASP.NET Core paylaşılan framework'ün bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1e5b9-126">İzleme sorunu duyuruları #325</span><span class="sxs-lookup"><span data-stu-id="1e5b9-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1e5b9-127">Bu değişiklik, ASP.NET Core 3.0 Önizleme 1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1e5b9-128">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-128">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-129">ASP.NET Core 3.0 Paketi başvurusu eklendiğinde önce `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`, EF Core içerir ve EF Core veri sağlayıcıları bazıları istediğiniz SQL Server sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1e5b9-130">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-130">**New behavior**</span></span>

<span data-ttu-id="1e5b9-131">3. 0'dan başlayarak, ASP.NET Core paylaşılan çerçeve EF Core veya tüm EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1e5b9-132">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-132">**Why**</span></span>

<span data-ttu-id="1e5b9-133">Bu değişiklikten önce EF Core alma olup uygulama ASP.NET Core ve SQL Server veya hedeflenen bağlı olarak farklı adımlar gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1e5b9-134">Ayrıca, ASP.NET Core yükseltme EF Core ve SQL Server sağlayıcısı, her zaman tercih değildir yükseltmesini zorlandı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1e5b9-135">Bu değişiklik, EF Core alma aynı tüm sağlayıcıları, desteklenen .NET uygulamaları ve uygulama türleri arasında deneyimidir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1e5b9-136">Geliştiriciler, tam olarak EF Core ve EF Core veri sağlayıcıları yükseltildiğinde de denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1e5b9-137">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-137">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-138">EF çekirdekli ASP.NET Core 3.0 uygulama veya desteklenen başka bir uygulama içinde kullanmak için açıkça uygulamanızın kullanacağı EF Core veritabanı sağlayıcısı için bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="1e5b9-139">EF Core komut satırı aracı, dotnet ef artık .NET Core SDK'sının bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="1e5b9-140">İzleme sorun #14016</span><span class="sxs-lookup"><span data-stu-id="1e5b9-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="1e5b9-141">Bu değişiklik EF Core 3.0-preview 4 ve .NET Core SDK'sını ilgili sürümü kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="1e5b9-142">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-142">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-143">3.0 önce `dotnet ef` aracı, .NET Core SDK'yı eklenmiştir ve ek adımlar gerek kalmadan, herhangi bir projeyi komut satırından kullanmaya hazır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="1e5b9-144">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-144">**New behavior**</span></span>

<span data-ttu-id="1e5b9-145">3. 0'dan başlayarak .NET SDK'sı yer almaz `dotnet ef` aracı kullanabilmeniz için önce açıkça yerel veya genel bir aracı yüklemek zorunda.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="1e5b9-146">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-146">**Why**</span></span>

<span data-ttu-id="1e5b9-147">Bu değişiklik dağıtmak ve güncelleştirmek sağlıyor `dotnet ef` bir düzenli .NET CLI aracı, NuGet üzerindeki EF Core 3.0 de her zaman bir NuGet paketi olarak dağıtılmış bir olgu ile tutarlı olarak.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="1e5b9-148">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-148">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-149">Geçişleri veya iskele yönetebilmek için bir `DbContext`, yükleme `dotnet-ef` kullanarak `dotnet tool install` komutu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="1e5b9-150">Örneğin, bir genel araç olarak yüklemek için şu komutu yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="1e5b9-151">Aletle şekillerini kullanan bağımlılık olarak bildiren bir proje bağımlılıklarını geri yüklediğinizde de, yerel aracı edinebilirsiniz bir [aracı bildirim dosyası](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="1e5b9-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="1e5b9-152">SQL ve ExecuteSql ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="1e5b9-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="1e5b9-153">İzleme sorun #10996</span><span class="sxs-lookup"><span data-stu-id="1e5b9-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="1e5b9-154">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-155">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-155">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-156">EF Core 3.0 önce bu yöntem adlarının bir normal bir dize ya da SQL ve parametreleri ilişkilendirilmiş bir dize ile çalışmak için aşırı yüklenmiş.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="1e5b9-157">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-157">**New behavior**</span></span>

<span data-ttu-id="1e5b9-158">EF Core 3.0 ile başlayarak, kullanmaktadır `FromSqlRaw`, `ExecuteSqlRaw`, ve `ExecuteSqlRawAsync` burada parametreleri geçirilir ayrı olarak Sorgu dizesinden parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="1e5b9-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="1e5b9-160">Kullanım `FromSqlInterpolated`, `ExecuteSqlInterpolated`, ve `ExecuteSqlInterpolatedAsync` burada parametreleri geçirilir ilişkilendirilmiş sorgu dizesinin parçası olarak parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="1e5b9-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="1e5b9-162">Yukarıdaki sorgu her ikisi de aynı SQL parametrelere sahip aynı parametreli SQL üretecektir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="1e5b9-163">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-163">**Why**</span></span>

<span data-ttu-id="1e5b9-164">Yöntem aşırı yüklemeleri böyle güvenmelidir yanı sıra ilişkilendirilmiş dize yöntemini çağırmak için hedefi olduğu zaman yanlışlıkla ham dize yöntemini çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="1e5b9-165">Bu, verilmiş olması, parametreli getirilemedi sorgularda neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="1e5b9-166">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-166">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-167">Yeni yöntem adları kullanılacak anahtar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="1e5b9-168">SQL yöntemleri yalnızca sorgu kökleri üzerinde belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="1e5b9-169">İzleme sorun #15704</span><span class="sxs-lookup"><span data-stu-id="1e5b9-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="1e5b9-170">Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1e5b9-171">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-171">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-172">EF Core 3.0 önce `FromSql` yöntemi belirtilebilir herhangi bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="1e5b9-173">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-173">**New behavior**</span></span>

<span data-ttu-id="1e5b9-174">EF Core 3.0 ile başlayan yeni `FromSqlRaw` ve `FromSqlInterpolated` yöntemleri (hangi Değiştir `FromSql`) yalnızca sorgu kökleri üzerinde yani doğrudan belirtilebilir `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="1e5b9-175">Başka bir yerde bunları denemek, bir derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="1e5b9-176">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-176">**Why**</span></span>

<span data-ttu-id="1e5b9-177">Belirtme `FromSql` herhangi bir yere dışında üzerinde bir `DbSet` hiçbir anlamı eklendi veya değer ve belirli senaryolarda karışıklığa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="1e5b9-178">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-178">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-179">`FromSql` çağrıları doğrudan açık olmasını taşınması gereken `DbSet` hangi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="1e5b9-180">Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-180">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="1e5b9-181">İzleme sorun #14523</span><span class="sxs-lookup"><span data-stu-id="1e5b9-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1e5b9-182">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-182">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-183">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-183">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-184">EF Core 3.0 önce sorgu ve diğer komutları yürütme sırasında günlüğe kaydedilen `Info` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-184">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="1e5b9-185">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-185">**New behavior**</span></span>

<span data-ttu-id="1e5b9-186">EF Core 3.0 ile başlayarak, komut/SQL yürütme günlüğe kaydetme sırasında işlemi `Debug` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-186">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="1e5b9-187">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-187">**Why**</span></span>

<span data-ttu-id="1e5b9-188">Konumundaki paraziti azaltmak için bu değişiklik yapılmıştır `Info` günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-188">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="1e5b9-189">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-189">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-190">Bu günlük olayı tarafından tanımlanan `RelationalEventId.CommandExecuting` 20100 olay kimliği.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-190">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="1e5b9-191">SQL oturum `Info` yeniden düzey, düzeyi açıkça yapılandırmanıza `OnConfiguring` veya `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-191">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="1e5b9-192">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-192">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1e5b9-193">Geçici bir anahtar değere artık varlık örneklerini ayarlanır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-193">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1e5b9-194">İzleme sorun #12378</span><span class="sxs-lookup"><span data-stu-id="1e5b9-194">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1e5b9-195">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-195">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1e5b9-196">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-196">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-197">EF Core 3.0 önce veritabanı tarafından oluşturulan gerçek değer daha sonra sahip olabileceği tüm anahtar özellikleri geçici değerler atanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-197">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1e5b9-198">Genellikle bu geçici değerleri büyük negatif sayılar yoktu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-198">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1e5b9-199">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-199">**New behavior**</span></span>

<span data-ttu-id="1e5b9-200">3.0 ile başlayarak, EF Core, varlığın izleme bilgilerini bir parçası olarak geçici bir anahtar değeri depolar ve anahtar özelliği kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-200">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1e5b9-201">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-201">**Why**</span></span>

<span data-ttu-id="1e5b9-202">Geçici bir anahtar değere deneyebileceğinizi daha önce bazı tarafından izlenen bir varlık kalıcı hale gelmesini önlemek için bu değişiklik yapılmıştır `DbContext` örneği farklı bir taşınır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-202">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1e5b9-203">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-203">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-204">Varlıklar arasında ilişkilendirmeler form üzerine yabancı anahtarlar birincil anahtar değerlerini atamak uygulamaları birincil anahtarları depoda üretilmiş ve varlıklara ait eski davranışı bağımlı `Added` durumu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-204">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1e5b9-205">Tarafından önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-205">This can be avoided by:</span></span>
* <span data-ttu-id="1e5b9-206">Depoda üretilmiş anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-206">Not using store-generated keys.</span></span>
* <span data-ttu-id="1e5b9-207">Yabancı anahtar değerleri ayarlamak yerine form ilişkiler için Gezinti özelliklerini ayarlama.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-207">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1e5b9-208">Gerçek geçici anahtar değerlerinin varlığın izleme bilgileri edinin.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-208">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1e5b9-209">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` geçici değer olsa bile iade `blog.Id` kendisini ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-209">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1e5b9-210">Depoda üretilmiş anahtar değerlerini DetectChanges geliştirir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-210">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1e5b9-211">İzleme sorun #14616</span><span class="sxs-lookup"><span data-stu-id="1e5b9-211">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1e5b9-212">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-212">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-213">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-213">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-214">EF Core 3.0 önce izlenmeyen bir varlık tarafından bulunan `DetectChanges` olarak izlenmesi `Added` durum ve eklenen yeni bir olarak ne zaman satır `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-214">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1e5b9-215">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-215">**New behavior**</span></span>

<span data-ttu-id="1e5b9-216">Oluşturulan anahtar değerlerini bir varlık kullanıyorsa EF Core 3.0 ile başlayan ve bazı anahtar değerini ayarlayın ve ardından varlığı olarak takip `Modified` durumu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-216">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1e5b9-217">Bu varlık için bir satır var olduğu varsayılır ve bu anlamına gelir ne zaman güncelleştirilmiş `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-217">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1e5b9-218">Anahtar değeri ayarlanmamış, veya varlık türü kullanmıyor oluşturulan anahtarları varsa yeni bir varlık olarak hala izlenecektir `Added` önceki sürümlerinde olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-218">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1e5b9-219">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-219">**Why**</span></span>

<span data-ttu-id="1e5b9-220">Bu değişiklik, daha kolay ve bağlantısı kesilmiş varlık grafiklerle depoda üretilmiş anahtarlar kullanılırken çalışmak için daha tutarlı hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-220">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1e5b9-221">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-221">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-222">Bu değişiklik, bir varlık türü için üretilmiş anahtarlar yapılandırılır, ancak anahtar değerlerine açıkça yeni örnekleri için ayarlanan uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-222">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1e5b9-223">Düzeltme üretilmiş değerleri anahtar özelliklerine açıkça yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-223">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1e5b9-224">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-224">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1e5b9-225">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-225">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1e5b9-226">Art arda silme işlemi hemen hemen varsayılan olarak gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-226">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1e5b9-227">İzleme sorun #10114</span><span class="sxs-lookup"><span data-stu-id="1e5b9-227">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1e5b9-228">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-228">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-229">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-229">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-230">SaveChanges çağrıldı kadar EF Core uygulanan basamaklı eylemlerinin (gerekli sorumlu silindiğinde veya ilişki gerekli sorumlusuna yazıyordunuz bağımlı varlıkları silmek) 3.0 önce gerçekleşmedi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-230">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1e5b9-231">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-231">**New behavior**</span></span>

<span data-ttu-id="1e5b9-232">Tetikleme koşulu algılandığında hemen sonra 3.0 ile başlayarak, EF Core basamaklı eylemlerinin geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-232">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1e5b9-233">Örneğin, çağırma `context.Remove()` asıl bir varlığı silme tüm izlenen sonucu ilgili gerekli bağımlılıklar da ayarlanacak şekilde `Deleted` hemen.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-233">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1e5b9-234">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-234">**Why**</span></span>

<span data-ttu-id="1e5b9-235">Veri bağlama ve hangi varlıkları silinecek anlamanız önemli olduğu senaryolar denetimi deneyimini geliştirmek için bu değişiklik yapılmıştır _önce_ `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-235">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1e5b9-236">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-236">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-237">Davranış ayarları aracılığıyla geri yüklenebilir `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-237">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1e5b9-238">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-238">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="1e5b9-239">DeleteBehavior.Restrict temizleyici semantiğe sahip</span><span class="sxs-lookup"><span data-stu-id="1e5b9-239">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="1e5b9-240">İzleme sorun #12661</span><span class="sxs-lookup"><span data-stu-id="1e5b9-240">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="1e5b9-241">Bu değişiklik, EF Core 3.0-preview 5'teki sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-241">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="1e5b9-242">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-242">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-243">3.0 önce `DeleteBehavior.Restrict` ile veritabanındaki yabancı anahtarlar oluşturulan `Restrict` semantiği, aynı zamanda açık olmayan bir şekilde değiştirilmiş iç düzeltme.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-243">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="1e5b9-244">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-244">**New behavior**</span></span>

<span data-ttu-id="1e5b9-245">3.0 ile başlayan `DeleteBehavior.Restrict` yabancı anahtarlar ile oluşturulan sağlar `Restrict` semantiği--diğer bir deyişle, hiçbir basamaklar; throw EF iç düzeltme de etkilemeden kısıtlama ihlali üzerinde--.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-245">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="1e5b9-246">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-246">**Why**</span></span>

<span data-ttu-id="1e5b9-247">Bu değişiklik deneyimini geliştirmek amacıyla kullanılarak yapıldığını `DeleteBehavior` beklenmeyen yan etkiler olmadan sezgisel bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-247">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="1e5b9-248">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-248">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-249">Kullanarak önceki davranış geri yüklenebilir `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-249">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1e5b9-250">Sorgu türleri varlık türleri ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-250">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1e5b9-251">İzleme sorun #14194</span><span class="sxs-lookup"><span data-stu-id="1e5b9-251">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1e5b9-252">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-252">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-253">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-253">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-254">EF Core 3.0 önce [sorgu türü](xref:core/modeling/query-types) olan birincil anahtar, yapılandırılmış bir biçimde tanımlamıyor verileri sorgulamak için bir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-254">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1e5b9-255">Diğer bir deyişle, bir sorgu türü kullanıldı sırasında normal bir varlık anahtarları (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) olmadan varlık türleriyle eşlemek için bir anahtar kullanılabilir (daha büyük olasılıkla bir tablodan ancak büyük olasılıkla bir görünümden) olduğunda türü kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-255">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1e5b9-256">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-256">**New behavior**</span></span>

<span data-ttu-id="1e5b9-257">Bir sorgu türü artık yalnızca birincil anahtarı olmayan bir varlık türü olur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-257">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1e5b9-258">Anahtarsız varlık türleri, önceki sürümlerde sorgu türleri aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-258">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1e5b9-259">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-259">**Why**</span></span>

<span data-ttu-id="1e5b9-260">Sorgu türleri amacı etrafında karışıklık azaltmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-260">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1e5b9-261">Özellikle, anahtarsız varlık türleridir ve bu nedenle kendiliğinden salt okunur olduklarından, ancak yalnızca bir varlık türü salt okunur olması gerekir çünkü bunlar kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-261">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1e5b9-262">Benzer şekilde, genellikle görünümlerine eşlenmiş, ancak yalnızca görünümleri genellikle anahtarlar tanımlama yok olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-262">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1e5b9-263">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-263">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-264">Aşağıdaki bölümleri API artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-264">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1e5b9-265">**`ModelBuilder.Query<>()`** -Bunun yerine `ModelBuilder.Entity<>().HasNoKey()` hiçbir anahtarına sahip bir varlık türü işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-265">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1e5b9-266">Bu yine de bir birincil anahtar bekleniyordu, ancak kuralı eşleşmiyor, yanlış yapılandırma önlemek için kural olarak yapılandırılamadığı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-266">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1e5b9-267">**`DbQuery<>`** -Bunun yerine `DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-267">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1e5b9-268">**`DbContext.Query<>()`** -Bunun yerine `DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-268">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1e5b9-269">Sahip olunan tür ilişkileri için API yapılandırması değişti</span><span class="sxs-lookup"><span data-stu-id="1e5b9-269">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1e5b9-270">[Sorun #12444 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sorun #9148 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sorun #14153 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1e5b9-270">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1e5b9-271">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-271">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-272">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-272">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-273">EF Core 3.0 önce hemen sonra yapılandırma sahibi ilişkinin gerçekleştirildi `OwnsOne` veya `OwnsMany` çağırın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-273">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1e5b9-274">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-274">**New behavior**</span></span>

<span data-ttu-id="1e5b9-275">EF Core 3.0 ile başlayarak, var. Şimdi fluent API'sini kullanarak sahibine bir gezinti özelliğini yapılandırmak için `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-275">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1e5b9-276">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-276">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1e5b9-277">Yapılandırma sahibi arasındaki ilişkiyi ilgili ve ait hemen sonra zincirlenmesi gereken `WithOwner()` diğer ilişkileri nasıl yapılandırılacağını benzer şekilde.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-277">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1e5b9-278">Sahip olunan türü hala sonra zincirlenmesine için yapılandırma while `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-278">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1e5b9-279">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-279">For example:</span></span>

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

<span data-ttu-id="1e5b9-280">Ayrıca çağırma `Entity()`, `HasOne()`, veya `Set()` sahip olunan bir türü ile hedef şimdi bir özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-280">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1e5b9-281">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-281">**Why**</span></span>

<span data-ttu-id="1e5b9-282">Sahip olunan türü yapılandırma arasında daha net bir ayrım oluşturmak için bu değişiklik yapılmıştır ve _ilişkisi_ ait türü.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-282">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1e5b9-283">Bu sırayla belirsizlik ve yöntemler gibi geçici karışıklık kaldırır `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-283">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1e5b9-284">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-284">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-285">Yukarıdaki örnekte gösterildiği gibi yeni bir API yüzeyi kullanmak için sahip olunan tür ilişkileri yapılandırmasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-285">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1e5b9-286">Bağımlı varlıkları tablo sorumlusuyla paylaşımı artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-286">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="1e5b9-287">İzleme sorun #9005</span><span class="sxs-lookup"><span data-stu-id="1e5b9-287">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="1e5b9-288">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-288">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-289">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-289">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-290">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-290">Consider the following model:</span></span>
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
<span data-ttu-id="1e5b9-291">3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya aynı tabloya açıkça eşleştirilmiş bir `OrderDetails` örneği her zaman gerekli yeni bir eklerken `Order`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="1e5b9-292">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-292">**New behavior**</span></span>

<span data-ttu-id="1e5b9-293">3.0 ile başlayarak, EF Core eklemek için sağlayan bir `Order` olmadan bir `OrderDetails` ve tüm eşler `OrderDetails` özellikleri hariç null yapılabilir sütunlar için birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-293">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="1e5b9-294">EF Core kümeleri sorgulanırken `OrderDetails` için `null` gerekli özelliklerinden birini yoksa, bir değer veya birincil anahtarı yanı sıra gereken özellikleri yoktur ve tüm özellikleri `null`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-294">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="1e5b9-295">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-295">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-296">İsteğe bağlı tüm sütunlar eklenmiş bağımlı paylaşımı tablo modelinizi varken işaret Gezinti olması beklenmiyor `null` Gezinti olduğunda durumlarında uygulama değiştirilmelidir sonra `null`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-296">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="1e5b9-297">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmesini veya en az bir özelliğine sahip olmayan bir`null` atanmış değer.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-297">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="1e5b9-298">Bir özelliğini eşleştirmek tüm varlıklar bir eşzamanlılık belirteci sütun içeren bir tablo paylaşımı sahip</span><span class="sxs-lookup"><span data-stu-id="1e5b9-298">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="1e5b9-299">İzleme sorun #14154</span><span class="sxs-lookup"><span data-stu-id="1e5b9-299">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="1e5b9-300">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-300">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-301">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-301">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-302">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-302">Consider the following model:</span></span>
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
<span data-ttu-id="1e5b9-303">3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya yalnızca güncelleştirme aynı tabloya açıkça eşlenen `OrderDetails` değil güncelleştirecek `Version` değeri istemcisi ve İleri güncelleştirmeyi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-303">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="1e5b9-304">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-304">**New behavior**</span></span>

<span data-ttu-id="1e5b9-305">EF Core 3.0 ile başlayarak, yeni yayınlar `Version` değerini `Order` bunu sahipse `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-305">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="1e5b9-306">Aksi takdirde model doğrulama sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-306">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="1e5b9-307">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-307">**Why**</span></span>

<span data-ttu-id="1e5b9-308">Eski eşzamanlılık belirteç değeri aynı tabloya eşlenen varlıkları yalnızca biri güncelleştirildiğinde önlemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-308">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="1e5b9-309">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-309">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-310">Eşzamanlılık belirteci sütuna eşlenmiş bir özellik içerir tabloda paylaşımı tüm varlıklar gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-310">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="1e5b9-311">Mümkündür oluşturma bir gölge durumu:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-311">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="1e5b9-312">Tüm türetilmiş türler için tek bir sütun eşlenmemiş türlerden devralınan özellikler artık eşlenir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-312">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="1e5b9-313">İzleme sorun #13998</span><span class="sxs-lookup"><span data-stu-id="1e5b9-313">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="1e5b9-314">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-314">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-315">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-315">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-316">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-316">Consider the following model:</span></span>
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

<span data-ttu-id="1e5b9-317">EF Core 3.0 önce `ShippingAddress` özelliği için sütunları ayırmak için eşlenebilir `BulkOrder` ve `Order` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-317">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="1e5b9-318">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-318">**New behavior**</span></span>

<span data-ttu-id="1e5b9-319">3.0 ile başlayarak, EF Core yalnızca bir sütun için oluşturur `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-319">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="1e5b9-320">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-320">**Why**</span></span>

<span data-ttu-id="1e5b9-321">Eski davranışı beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-321">The old behavoir was unexpected.</span></span>

<span data-ttu-id="1e5b9-322">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-322">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-323">Özellik sütunu türetilmiş türler üzerinde ayırmak için açıkça eşlenebilir:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-323">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1e5b9-324">Yabancı anahtar özellik kuralı asıl özelliğiyle aynı ada artık eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-324">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1e5b9-325">İzleme sorun #13274</span><span class="sxs-lookup"><span data-stu-id="1e5b9-325">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1e5b9-326">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-326">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-327">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-327">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-328">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-328">Consider the following model:</span></span>
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
<span data-ttu-id="1e5b9-329">EF Core 3.0 önce `CustomerId` özelliği kullanılan Kural gereği için yabancı anahtarı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-329">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1e5b9-330">Ancak, varsa `Order` bu da yapacağı sonra sahip olunan bir türü olan `CustomerId` birincil anahtar ve bu değilse genellikle beklentisi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-330">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1e5b9-331">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-331">**New behavior**</span></span>

<span data-ttu-id="1e5b9-332">3.0 ile başlayarak, EF Core asıl özelliğiyle aynı ada sahip oldukları özellikleri için yabancı anahtarlar kurala göre kullanırsanız dener.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-332">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1e5b9-333">Asıl tür adı asıl özellik adıyla birleştirilmiş ve asıl özellik adı desenleri ile birleştirilmiş gezinme adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-333">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1e5b9-334">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-334">For example:</span></span>

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

<span data-ttu-id="1e5b9-335">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-335">**Why**</span></span>

<span data-ttu-id="1e5b9-336">Bu değişikliği yanlışlıkla bir birincil anahtar özelliği sahip olunan türüne tanımlamamaya özen gösterin çalışıldı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-336">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1e5b9-337">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-337">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-338">Yabancı anahtar olması ve bu nedenle birincil anahtarın sonra açıkça bölüm özelliği isteniyorsa bu şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-338">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="1e5b9-339">Veritabanı bağlantısı artık kapalı kullanılmazsa süresi TransactionScope tamamlanmadan önce artık</span><span class="sxs-lookup"><span data-stu-id="1e5b9-339">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="1e5b9-340">İzleme sorun #14218</span><span class="sxs-lookup"><span data-stu-id="1e5b9-340">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="1e5b9-341">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-341">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-342">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-342">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-343">EF Core bağlam içinde bağlantısına açarsa, 3.0 önce bir `TransactionScope`, bağlantı sırasında geçerli açık kalır `TransactionScope` etkindir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-343">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="1e5b9-344">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-344">**New behavior**</span></span>

<span data-ttu-id="1e5b9-345">Kullanmaya tamamladıktan hemen sonra 3.0 ile başlayarak, EF Core bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-345">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="1e5b9-346">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-346">**Why**</span></span>

<span data-ttu-id="1e5b9-347">Birden fazla bağlamı aynı kullanmak için bu değişiklik sağlayan `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-347">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="1e5b9-348">Yeni davranış da EF6 eşleşir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-348">The new behavior also matches EF6.</span></span>

<span data-ttu-id="1e5b9-349">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-349">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-350">Bağlantı açık çağrı açık kalması gerekiyorsa `OpenConnection()` EF Core, erken kapatmadığına emin olun:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-350">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1e5b9-351">Her bir özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-351">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1e5b9-352">İzleme sorun #6872</span><span class="sxs-lookup"><span data-stu-id="1e5b9-352">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1e5b9-353">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-353">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-354">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-354">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-355">EF Core 3.0 önce tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-355">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1e5b9-356">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-356">**New behavior**</span></span>

<span data-ttu-id="1e5b9-357">EF Core 3.0 ile başlayarak, her bir tamsayı anahtar özellik değer Oluşturucusu kendi bellek içi veritabanı kullanılırken alır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-357">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1e5b9-358">Ayrıca, veritabanı silinirse, anahtar oluşturma için tüm tabloları sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-358">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1e5b9-359">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-359">**Why**</span></span>

<span data-ttu-id="1e5b9-360">Bellek içi anahtar oluşturma gerçek veritabanı anahtar oluşturma için daha yakından Hizala ve bellek içi veritabanı kullanılırken testler birbirinden ayırma özelliğini artırmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-360">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1e5b9-361">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-361">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-362">Bunu ayarlamak için belirli bellek içi anahtar değerlerine bağlı olan bir uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-362">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1e5b9-363">Göz önünde bulundurun yerine değil özel anahtar değerlerine bağlı olan veya yeni davranış eşleştirilecek güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-363">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1e5b9-364">Varsayılan olarak kullanılan destekleyen alanlar</span><span class="sxs-lookup"><span data-stu-id="1e5b9-364">Backing fields are used by default</span></span>

[<span data-ttu-id="1e5b9-365">İzleme sorun #12430</span><span class="sxs-lookup"><span data-stu-id="1e5b9-365">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1e5b9-366">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-366">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1e5b9-367">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-367">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-368">3.0 önce bir özellik için destek alanı biliniyordu olsa bile, EF Core yine de varsayılan olarak okuma ve özellik alıcı ve ayarlayıcı yöntemleri kullanarak özellik değeri yazma.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-368">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1e5b9-369">Bunun özel durumu burada destek alanı doğrudan biliniyorsa ayarlamak sorgu yürütme oluştu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-369">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1e5b9-370">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-370">**New behavior**</span></span>

<span data-ttu-id="1e5b9-371">Bir özellik için destek alanı biliniyorsa EF Core 3.0 ile başlayarak, ardından EF Core her zaman okuyabilmesini ve yazma yedekleme alanını kullanarak bu özelliği.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-371">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1e5b9-372">Uygulama, alıcı veya ayarlayıcı yöntemleri kodlanmış ek davranış bağlı değilse bu bir uygulama sonu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-372">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1e5b9-373">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-373">**Why**</span></span>

<span data-ttu-id="1e5b9-374">Bu değişiklik iş mantığı deneyebileceğinizi olarak tetikleyen EF Core önlemek için varsayılan olarak varlıkları içeren bir veritabanı işlemleri gerçekleştirirken yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-374">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1e5b9-375">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-375">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-376">Öncesi 3.0 davranışı özellik erişim modu konfigürasyonuyla geri yüklenebilir `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-376">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="1e5b9-377">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-377">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1e5b9-378">Birden çok uyumlu yedekleme alan bulunamazsa throw</span><span class="sxs-lookup"><span data-stu-id="1e5b9-378">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1e5b9-379">İzleme sorun #12523</span><span class="sxs-lookup"><span data-stu-id="1e5b9-379">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1e5b9-380">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-380">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-381">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-381">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-382">Birden çok alan karşılarsa, EF Core 3.0 önce bazı öncelik sırası üzerinde bir alan seçilirdi sonra bir özelliğinin destek alanı bulma kurallarını temel.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-382">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1e5b9-383">Bu, belirsiz durumda kullanılacak yanlış alan neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-383">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1e5b9-384">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-384">**New behavior**</span></span>

<span data-ttu-id="1e5b9-385">Birden çok alan aynı özelliğe eşleşirse EF Core 3.0 ile başlayarak, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-385">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1e5b9-386">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-386">**Why**</span></span>

<span data-ttu-id="1e5b9-387">Yalnızca biri doğru olduğunda sessiz bir şekilde bir alanı başka bir kullanmaktan kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-387">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1e5b9-388">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-388">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-389">Belirsiz destekleyen alanlarla özellikleri, açıkça belirtilen kullanılacak bir alan olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-389">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1e5b9-390">Örneğin, fluent API'sini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-390">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="1e5b9-391">Alan adı alanı-yalnızca özellik adlarının eşleşmesi gerekir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-391">Field-only property names should match the field name</span></span>

<span data-ttu-id="1e5b9-392">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-392">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-393">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-393">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-394">EF Core 3.0 önce bir özellik bir dize değeri belirtilebilir ve bu ada sahip hiçbir özellik CLR türüne bulunduysa EF Core kuralı kurallarını kullanarak bir alan için eşleşen deneyin.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-394">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="1e5b9-395">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-395">**New behavior**</span></span>

<span data-ttu-id="1e5b9-396">EF Core 3.0 ile başlayarak, bir alan özelliği salt okunur alan adı tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-396">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="1e5b9-397">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-397">**Why**</span></span>

<span data-ttu-id="1e5b9-398">Benzer şekilde adlı iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır, ayrıca eşleşen kuralları yalnızca alan özellikleri için CLR eşleniyor özellikleri aynıdır kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-398">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="1e5b9-399">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-399">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-400">Yalnızca alan özellikleri aynı eşleştirildikleri alan olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-400">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="1e5b9-401">EF Core 3.0 daha yeni bir önizleme özelliği adından farklı bir alan adı açıkça yapılandırma yeniden etkinleştirmek planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-401">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="1e5b9-402">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağırın</span><span class="sxs-lookup"><span data-stu-id="1e5b9-402">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="1e5b9-403">İzleme sorun #14756</span><span class="sxs-lookup"><span data-stu-id="1e5b9-403">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="1e5b9-404">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-404">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-405">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-405">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-406">EF Core 3.0, çağırma önce `AddDbContext` veya `AddDbContextPool` de günlüğe kaydetme ve bellek D.I hizmetleriyle yapılan çağrılar aracılığıyla önbelleğe kaydetmek [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1e5b9-406">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="1e5b9-407">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-407">**New behavior**</span></span>

<span data-ttu-id="1e5b9-408">EF Core 3.0 ile başlayan `AddDbContext` ve `AddDbContextPool` artık kayıt hizmetlerin bağımlılık ekleme (dı) sahip olur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-408">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="1e5b9-409">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-409">**Why**</span></span>

<span data-ttu-id="1e5b9-410">EF Core 3.0, bu hizmetler uygulamanın DI kapsayıcısında olduğunu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-410">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="1e5b9-411">Ancak, varsa `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlı EF Core tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-411">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="1e5b9-412">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-412">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-413">Uygulamanız bu hizmetlere ihtiyacı varsa, sonra bunları açıkça DI kullanarak kapsayıcı kayıt [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1e5b9-413">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1e5b9-414">DbContext.Entry artık yerel DetectChanges gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-414">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1e5b9-415">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="1e5b9-415">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1e5b9-416">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-416">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-417">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-417">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-418">EF Core 3.0, çağırma önce `DbContext.Entry` izlenen tüm varlıklar için algılanabilir değişiklik neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-418">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1e5b9-419">Bu durumu içinde kullanıma sunulan sağlamış `EntityEntry` güncel oluştu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-419">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1e5b9-420">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-420">**New behavior**</span></span>

<span data-ttu-id="1e5b9-421">Çağırma EF Core ile 3.0, başlangıç `DbContext.Entry` ilgili asıl varlıkları yalnızca belirtilen varlık ve diğer değişikliklerini algılamak girişimi izlenen başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-421">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1e5b9-422">Başka bir yerde değiştirir Bunun anlamı etkileri uygulama durumu olabilir. Bu yöntemi çağrılarak algılandı değil.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-422">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1e5b9-423">Unutmayın `ChangeTracker.AutoDetectChangesEnabled` ayarlanır `false` sonra bile bu yerel değişiklik algılama devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-423">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1e5b9-424">Değişiklik algılama--örneğin neden diğer yöntemleri `ChangeTracker.Entries` ve `SaveChanges`--yine de tam neden `DetectChanges` tüm varlıkları izlenir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-424">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1e5b9-425">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-425">**Why**</span></span>

<span data-ttu-id="1e5b9-426">Kullanmanın varsayılan performansını artırmak için bu değişiklik yapılmıştır `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-426">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1e5b9-427">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-427">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-428">Çağrı `ChgangeTracker.DetectChanges()` açıkça çağırmadan önce `Entry` öncesi 3.0 davranış sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-428">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1e5b9-429">Dize ve bayt dizisi anahtarlar varsayılan olarak istemci tarafından oluşturulan değildir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-429">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1e5b9-430">İzleme sorun #14617</span><span class="sxs-lookup"><span data-stu-id="1e5b9-430">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1e5b9-431">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-431">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-432">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-432">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-433">EF Core 3.0 önce `string` ve `byte[]` açıkça null olmayan bir değer ayarlamadan anahtar özellikleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-433">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1e5b9-434">Böyle bir durumda anahtar değeri istemcide bayt için seri hale getirilmiş bir GUID olarak oluşturulacak `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-434">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1e5b9-435">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-435">**New behavior**</span></span>

<span data-ttu-id="1e5b9-436">Bir özel durum EF Core 3.0 ile başlayarak, anahtar değer kümesi gösteren oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-436">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1e5b9-437">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-437">**Why**</span></span>

<span data-ttu-id="1e5b9-438">Bu değişiklik nedeniyle istemci tarafından oluşturulan yapılmıştır `string` / `byte[]` değerleri genellikle olmadığı kullanışlı ve varsayılan davranışını yaptığı sabit nedeni hakkında oluşturulan anahtar değerler için yaygın bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-438">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1e5b9-439">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-439">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-440">Başka bir null olmayan değer ayarlarsanız anahtar özellikleri oluşturulan değerler kullanması gerektiğini açıkça belirterek öncesi 3.0 davranış elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-440">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1e5b9-441">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-441">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1e5b9-442">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-442">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1e5b9-443">Kapsamlı bir hizmet Iloggerfactory sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="1e5b9-443">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1e5b9-444">İzleme sorun #14698</span><span class="sxs-lookup"><span data-stu-id="1e5b9-444">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1e5b9-445">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-445">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-446">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-446">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-447">EF Core 3.0 önce `ILoggerFactory` tek bir hizmet kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-447">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1e5b9-448">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-448">**New behavior**</span></span>

<span data-ttu-id="1e5b9-449">EF Core 3.0 ile başlayan `ILoggerFactory` şimdi kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-449">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1e5b9-450">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-450">**Why**</span></span>

<span data-ttu-id="1e5b9-451">Günlükçü ile ilişkisini izin vermek için bu değişiklik yapılmıştır bir `DbContext` diğer işlevleri sağlar ve bazı durumlarda pathological davranış gibi iç hizmet sağlayıcıları sayısındaki kaldıran örneği.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-451">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1e5b9-452">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-452">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-453">Bu değişiklik, kaydetme ve EF Core iç hizmet sağlayıcısına özel hizmetler kullanarak sürece uygulama kodu etkilememesi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-453">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1e5b9-454">Bu ortak değildir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-454">This isn't common.</span></span>
<span data-ttu-id="1e5b9-455">Bu durumlarda, bilgilerin çoğunu hala çalışır, ancak bağlı olarak herhangi bir tekil hizmeti olacak `ILoggerFactory` almak için değiştirilmesi gerekecektir `ILoggerFactory` farklı bir yolla.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-455">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1e5b9-456">Bu gibi durumlarda karşılaşırsanız lütfen sırasında bir sorun üzerinde dosya [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) nasıl kullandığınızı bize bildirmek `ILoggerFactory` sağlayacak şekilde biz bunu gelecekte kesmemesi nasıl daha iyi anlamak.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-456">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="1e5b9-457">IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-457">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1e5b9-458">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="1e5b9-458">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1e5b9-459">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-459">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-460">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-460">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-461">`IDbContextOptionsExtensionWithDebugInfo` ek isteğe bağlı bir arabirim gelen genişletilmişse `IDbContextOptionsExtension` arabirimine 2.x sürüm döngüsü sırasında değiştirme bir hataya neden önlemek için.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-461">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="1e5b9-462">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-462">**New behavior**</span></span>

<span data-ttu-id="1e5b9-463">Arabirimler artık birlikte birleştirilir `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-463">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="1e5b9-464">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-464">**Why**</span></span>

<span data-ttu-id="1e5b9-465">Arabirimler kavramsal olarak biri olduğundan, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-465">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="1e5b9-466">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-466">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-467">Tüm uygulamaları `IDbContextOptionsExtension` yeni üye destekleyecek şekilde güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-467">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1e5b9-468">Gezinti özellikleri tam olarak yüklü olduğundan yükleme Lazy proxy'leri artık varsayılır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-468">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1e5b9-469">İzleme sorun #12780</span><span class="sxs-lookup"><span data-stu-id="1e5b9-469">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1e5b9-470">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-470">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-471">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-471">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-472">EF Core 3.0, bir kez önce bir `DbContext` bırakıldı veya bir belirtilen gezinme özelliğinin bir varlığı söz konusu bağlamdan elde tam yüklü ise, olduğunu bilmesinin imkanı yoktu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-472">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1e5b9-473">Proxy'leri bunun yerine null olmayan bir değer varsa bir başvuru Gezinti yüklenir ve boş değilse, bir koleksiyon Gezinti yüklenen varsayılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-473">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1e5b9-474">Bu durumlarda, yavaş dışarıdan yüklemeyi deneyen bir İşlemsiz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-474">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1e5b9-475">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-475">**New behavior**</span></span>

<span data-ttu-id="1e5b9-476">EF Core 3.0 ile başlayarak, proxy'leri bir gezinti özelliğinin yüklü olup olmadığını izler.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-476">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1e5b9-477">Bu yüklenen Gezinti boş veya null olduğunda bile bağlamı bırakıldıktan sonra yüklenen bir gezinti özelliği erişmeye çalışan her zaman bir İşlemsiz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-477">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1e5b9-478">Buna karşılık, boş bir koleksiyon gezinme özelliği olsa bile bağlamı elden yüklü olmayan bir gezinti özelliği erişmeye çalışan bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-478">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1e5b9-479">Bu durum ortaya çıkarsa, uygulama kodu, geçersiz bir zamanda yavaş yükleme kullanmak çalışıyor ve bunu yapmak için uygulama değiştirilmelidir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-479">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1e5b9-480">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-480">**Why**</span></span>

<span data-ttu-id="1e5b9-481">Bu değişiklik, bırakılmış yükü yavaş çalışırken davranışını tutarlı ve doğru hale yapılmıştır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-481">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1e5b9-482">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-482">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-483">Uygulama kodunun yükleme yavaş bırakılmış bağlamla kullanmamanız güncelleştirin veya bu özel durum iletisini açıklandığı bir İşlemsiz olacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-483">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1e5b9-484">İç hizmet sağlayıcılarının aşırı oluşturma artık varsayılan olarak bir hata</span><span class="sxs-lookup"><span data-stu-id="1e5b9-484">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1e5b9-485">İzleme sorun #10236</span><span class="sxs-lookup"><span data-stu-id="1e5b9-485">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1e5b9-486">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-486">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-487">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-487">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-488">EF Core 3.0 önce bir uyarı iç hizmet sağlayıcıları pathological bir dizi oluşturmak için bir uygulama oturum açmış olmanız.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-488">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1e5b9-489">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-489">**New behavior**</span></span>

<span data-ttu-id="1e5b9-490">EF Core 3.0 ile başlayarak, bu uyarı artık olarak kabul edilir ve hata ve özel durum harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-490">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1e5b9-491">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-491">**Why**</span></span>

<span data-ttu-id="1e5b9-492">Bu değişiklik, daha açık pathological bu durumda gösterme yoluyla uygulama kodu daha iyi desteklemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-492">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1e5b9-493">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-493">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-494">Bu hata ile karşılaşma eylemde en uygun nedeni, kök nedenini anlamak ve çok sayıda iç hizmet sağlayıcıları oluşturmayı durdur oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-494">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1e5b9-495">Ancak, hata bir uyarı olarak geri dönüştürülür (veya yok sayıldı) aracılığıyla yapılandırma `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-495">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1e5b9-496">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-496">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="1e5b9-497">Tek bir dize ile HasOne/çok ilişkin yeni davranış çağırılır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-497">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="1e5b9-498">İzleme sorun #9171</span><span class="sxs-lookup"><span data-stu-id="1e5b9-498">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="1e5b9-499">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-499">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-500">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-500">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-501">EF Core 3.0 önce kod arama `HasOne` veya `HasMany` tek bir dize ile kafa karıştırıcı bir şekilde yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-501">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="1e5b9-502">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-502">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="1e5b9-503">Kod ilgili Görülüyor `Samurai` bazı diğer varlık türü kullanarak `Entrance` özel gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-503">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="1e5b9-504">Bazı varlık türü olarak adlandırılan bir ilişki oluşturmak bu kodu gerçekte çalışır `Entrance` ile gezinti özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-504">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="1e5b9-505">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-505">**New behavior**</span></span>

<span data-ttu-id="1e5b9-506">EF Core 3.0 ile başlayarak, yukarıdaki kod artık önce yapmakta gibi hangi Aranan yapar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-506">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="1e5b9-507">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-507">**Why**</span></span>

<span data-ttu-id="1e5b9-508">Eski davranışı özellikle yapılandırma kodu okurken ve hatalarını arayarak çok karmaşık.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-508">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="1e5b9-509">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-509">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-510">Bu, yalnızca tür adları ve gezinme özelliğini açıkça belirtmeden dizeleriyle ilişkileri açıkça yapılandırmakta olduğunuz uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-510">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="1e5b9-511">Bu yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-511">This is not common.</span></span>
<span data-ttu-id="1e5b9-512">Önceki davranışı açıkça geçirme aracılığıyla alınabilir `null` gezinme özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-512">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="1e5b9-513">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-513">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="1e5b9-514">Birden çok zaman uyumsuz yöntemler için dönüş türü için ValueTask görevden değiştirildi</span><span class="sxs-lookup"><span data-stu-id="1e5b9-514">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="1e5b9-515">İzleme sorun #15184</span><span class="sxs-lookup"><span data-stu-id="1e5b9-515">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="1e5b9-516">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-516">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-517">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-517">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-518">Aşağıdaki zaman uyumsuz yöntemler daha önce döndürülen bir `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-518">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="1e5b9-519">`ValueGenerator.NextValueAsync()` (ve türetilen sınıflar)</span><span class="sxs-lookup"><span data-stu-id="1e5b9-519">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="1e5b9-520">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-520">**New behavior**</span></span>

<span data-ttu-id="1e5b9-521">Yukarıda sözü edilen yöntemleri artık döndürür bir `ValueTask<T>` aynı üzerinden `T` önceki gibi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-521">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="1e5b9-522">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-522">**Why**</span></span>

<span data-ttu-id="1e5b9-523">Bu değişiklik, yığın ayırmaları genel performansını iyileştirme, bu yöntemleri çağrılırken tahakkuk sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-523">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="1e5b9-524">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-524">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-525">Yalnızca yukarıdaki API'leri yalnızca bekleyen gerekir derlenmesi için - uygulamalar kaynak değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-525">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="1e5b9-526">Daha karmaşık bir kullanım (örneğin döndürülen geçirme `Task` için `Task.WhenAny()`) genellikle gerektiren döndürülen `ValueTask<T>` dönüştürülmesi bir `Task<T>` çağırarak `AsTask()` üzerindeki.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-526">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="1e5b9-527">Bu bu değişiklik getirir ayırma azaltma verilerek unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-527">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1e5b9-528">İlişkisel: TypeMapping ek açıklama yalnızca TypeMapping sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="1e5b9-528">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1e5b9-529">İzleme sorun #9913</span><span class="sxs-lookup"><span data-stu-id="1e5b9-529">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1e5b9-530">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-530">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1e5b9-531">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-531">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-532">Tür eşlemesi ek açıklamaları için ek açıklama "İlişkisel: TypeMapping" adlandırılmıştı.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-532">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1e5b9-533">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-533">**New behavior**</span></span>

<span data-ttu-id="1e5b9-534">Ek açıklama türü eşleme ek açıklamalar için artık "TypeMapping" addır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-534">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1e5b9-535">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-535">**Why**</span></span>

<span data-ttu-id="1e5b9-536">Türü eşlemeleri artık birden fazla yalnızca ilişkisel veritabanı sağlayıcıları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-536">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1e5b9-537">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-537">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-538">Bu, yalnızca tür eşlemesi ortak olmayan doğrudan bir ek açıklama, olarak erişen uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-538">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1e5b9-539">API yüzeyi için ek açıklama doğrudan kullanmak yerine erişim türü eşlemeleri düzeltmek için en uygun eylemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-539">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1e5b9-540">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-540">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1e5b9-541">İzleme sorun #11811</span><span class="sxs-lookup"><span data-stu-id="1e5b9-541">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1e5b9-542">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-542">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-543">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-543">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-544">EF Core 3.0 önce `ToTable()` türetilmiş üzerinde adlı türü yoksayılan beri tek devralma eşleme stratejisi TPH burada bu geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-544">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1e5b9-545">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-545">**New behavior**</span></span>

<span data-ttu-id="1e5b9-546">EF Core 3.0 ve sonraki bir sürümde TPT ve TPC desteği eklemek için hazırlık başlangıç `ToTable()` beklenmeyen eşleme değişiklik gelecekte önlemek için bir özel durum bir türetilmiş tür şimdi throw üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-546">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1e5b9-547">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-547">**Why**</span></span>

<span data-ttu-id="1e5b9-548">Şu anda farklı bir tabloya türetilmiş bir tür eşlemek için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-548">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1e5b9-549">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda bozucu önler.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-549">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1e5b9-550">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-550">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-551">Türetilen türlerin diğer tablolara eşleme girişimleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-551">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1e5b9-552">ForSqlServerHasIndex HasIndex ile değiştirildi</span><span class="sxs-lookup"><span data-stu-id="1e5b9-552">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1e5b9-553">İzleme sorun #12366</span><span class="sxs-lookup"><span data-stu-id="1e5b9-553">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1e5b9-554">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-554">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-555">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-555">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-556">EF Core 3.0 önce `ForSqlServerHasIndex().ForSqlServerInclude()` ile kullanılan sütunları yapılandırmak üzere bir yöntem sağlayan `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-556">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1e5b9-557">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-557">**New behavior**</span></span>

<span data-ttu-id="1e5b9-558">EF Core 3.0 ile başlayarak, kullanarak `Include` dizin ilişkisel düzeyinde artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-558">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1e5b9-559">Kullanım `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-559">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1e5b9-560">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-560">**Why**</span></span>

<span data-ttu-id="1e5b9-561">API ile dizin için birleştirmek için bu değişiklik yapılmıştır `Include` tüm sağlayıcıları veritabanı için içine yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-561">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="1e5b9-562">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-562">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-563">Yukarıda da gösterildiği gibi yeni API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-563">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="1e5b9-564">Meta veri API'si değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="1e5b9-564">Metadata API changes</span></span>

[<span data-ttu-id="1e5b9-565">İzleme sorun #214</span><span class="sxs-lookup"><span data-stu-id="1e5b9-565">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1e5b9-566">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-566">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-567">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-567">**New behavior**</span></span>

<span data-ttu-id="1e5b9-568">Aşağıdaki özellikler için genişletme yöntemleri dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-568">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="1e5b9-569">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-569">**Why**</span></span>

<span data-ttu-id="1e5b9-570">Bu değişiklik, yukarıda sözü edilen arabirimleri uygulamasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-570">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="1e5b9-571">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-571">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-572">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-572">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="1e5b9-573">Sağlayıcıya özgü meta veriler API'SİNDEKİ değişiklikler</span><span class="sxs-lookup"><span data-stu-id="1e5b9-573">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="1e5b9-574">İzleme sorun #214</span><span class="sxs-lookup"><span data-stu-id="1e5b9-574">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1e5b9-575">Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-575">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1e5b9-576">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-576">**New behavior**</span></span>

<span data-ttu-id="1e5b9-577">Sağlayıcıya özgü genişletme yöntemlerini düzleştirilmiş:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-577">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="1e5b9-578">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-578">**Why**</span></span>

<span data-ttu-id="1e5b9-579">Bu değişiklik, yukarıda sözü edilen genişletme yöntemleri uygulamasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-579">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="1e5b9-580">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-580">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-581">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-581">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1e5b9-582">EF Core artık SQLite FK zorlama pragma gönderir</span><span class="sxs-lookup"><span data-stu-id="1e5b9-582">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1e5b9-583">İzleme sorun #12151</span><span class="sxs-lookup"><span data-stu-id="1e5b9-583">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1e5b9-584">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-584">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1e5b9-585">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-585">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-586">EF Core 3.0 önce EF Core gönderir gibi `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-586">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1e5b9-587">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-587">**New behavior**</span></span>

<span data-ttu-id="1e5b9-588">EF Core 3.0 ile başlayarak, EF Core artık gönderir `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-588">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1e5b9-589">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-589">**Why**</span></span>

<span data-ttu-id="1e5b9-590">EF Core kullandığından bu değişiklik yapılmıştır `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak, hangi sırayla anlamına gelir FK zorlama varsayılan olarak açık ve açıkça bir bağlantı her açıldığında etkinleştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-590">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1e5b9-591">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-591">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-592">Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-592">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1e5b9-593">Diğer durumlarda, yabancı anahtarlar belirterek etkinleştirilebilir `Foreign Keys=True` bağlantı dizenizi içinde.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-593">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="1e5b9-594">Microsoft.EntityFrameworkCore.Sqlite üzerinde SQLitePCLRaw.bundle_e_sqlite3 artık bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-594">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1e5b9-595">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-595">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-596">EF Core 3.0 önce EF Core kullanılan `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-596">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1e5b9-597">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-597">**New behavior**</span></span>

<span data-ttu-id="1e5b9-598">EF Core 3.0 ile başlayarak, EF Core kullanan `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-598">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1e5b9-599">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-599">**Why**</span></span>

<span data-ttu-id="1e5b9-600">Diğer platformlar ile tutarlı ios'ta kullanılan SQLite sürümü, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-600">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1e5b9-601">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-601">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-602">İos'ta yerel bir SQLite sürümü kullanmak için yapılandırma `Microsoft.Data.Sqlite` farklı bir kullanılacak `SQLitePCLRaw` paket.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-602">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1e5b9-603">GUID değerlerinin artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-603">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1e5b9-604">İzleme sorun #15078</span><span class="sxs-lookup"><span data-stu-id="1e5b9-604">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="1e5b9-605">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-605">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-606">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-606">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-607">GUID değerleri, daha önce SQLite değerlerine BLOB olarak sored.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-607">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="1e5b9-608">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-608">**New behavior**</span></span>

<span data-ttu-id="1e5b9-609">GUID değerleri, artık metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-609">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="1e5b9-610">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-610">**Why**</span></span>

<span data-ttu-id="1e5b9-611">İkili biçimi GUID'leri standartlaştırılmış değil.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-611">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="1e5b9-612">Değerlerin metin olarak depolanması veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-612">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1e5b9-613">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-613">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-614">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-614">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="1e5b9-615">EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-615">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="1e5b9-616">Microsoft.Data.Sqlite GUID değerlerinin hem BLOB hem de metin sütunları okuma özellikli kalır; Parametreler ve sabitleri için varsayılan biçimi değiştiğinden ancak büyük olasılıkla GUID'leri içeren çoğu senaryo için bir eylem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-616">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1e5b9-617">Char değerleri artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="1e5b9-617">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1e5b9-618">İzleme sorun #15020</span><span class="sxs-lookup"><span data-stu-id="1e5b9-618">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="1e5b9-619">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-619">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-620">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-620">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-621">Char değerleri daha önce SQLite tamsayı değerleri olarak sored.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-621">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="1e5b9-622">Örneğin, bir karakter değerini *A* 65 tamsayı değeri depolanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-622">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="1e5b9-623">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-623">**New behavior**</span></span>

<span data-ttu-id="1e5b9-624">Char değerleri artık metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-624">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="1e5b9-625">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-625">**Why**</span></span>

<span data-ttu-id="1e5b9-626">Değerlerin metin olarak depolanması daha doğal ve veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-626">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1e5b9-627">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-627">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-628">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-628">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="1e5b9-629">EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-629">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="1e5b9-630">Microsoft.Data.Sqlite Ayrıca belirli senaryoları herhangi bir eylem gerekli değil şekilde karakter değerleri hem tamsayı hem de metin sütunları, okuma özellikli kalır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-630">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="1e5b9-631">Geçiş kimlikleri, artık sabit kültürün takvimini kullanarak oluşturulur</span><span class="sxs-lookup"><span data-stu-id="1e5b9-631">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="1e5b9-632">İzleme sorun #12978</span><span class="sxs-lookup"><span data-stu-id="1e5b9-632">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="1e5b9-633">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-633">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-634">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-634">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-635">Geçiş kimlikleri, geçerli kültürün takvimini kullanarak yanlışlıkla üretildi.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-635">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="1e5b9-636">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-636">**New behavior**</span></span>

<span data-ttu-id="1e5b9-637">Geçiş kimlikleri artık her zaman sabit kültürün takvimini (Gregoryen) kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-637">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="1e5b9-638">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-638">**Why**</span></span>

<span data-ttu-id="1e5b9-639">Geçiş sırası önemlidir veritabanını güncelleştirmek ya da birleştirme çakışmalarını çözme.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-639">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="1e5b9-640">Sabit takvimi kullanılarak sıralama ekip üyelerinden farklı sistem takvimler sahip sonuçlanabilen sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-640">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="1e5b9-641">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-641">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-642">Bu değişiklik, olmayan-Gregoryen takvim yılı Gregoryen takvimini (içeren Thai Budist takvimi gibi) daha büyük olduğu kullanan herkesin etkiler.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-642">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="1e5b9-643">Mevcut geçiş kimlikleri böylece mevcut sonra yeni geçişleri sıralı güncelleştirilmesi gerekiyor geçişler.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-643">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="1e5b9-644">Geçiş kimliği geçiş özniteliğinde geçişler Tasarımcı dosyalarında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-644">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="1e5b9-645">Geçişleri geçmiş tablosu da güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-645">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="1e5b9-646">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="1e5b9-646">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="1e5b9-647">İzleme sorun #10985</span><span class="sxs-lookup"><span data-stu-id="1e5b9-647">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="1e5b9-648">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-648">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-649">**Değişiklik**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-649">**Change**</span></span>

<span data-ttu-id="1e5b9-650">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` yeniden adlandırıldı `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-650">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="1e5b9-651">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-651">**Why**</span></span>

<span data-ttu-id="1e5b9-652">Bu uyarı olayı tüm uyarı olayları ile adlandırma hizalar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-652">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="1e5b9-653">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-653">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-654">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-654">Use the new name.</span></span> <span data-ttu-id="1e5b9-655">(Olay kimliği numarasını değiştirilmedi unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="1e5b9-655">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="1e5b9-656">API açıklığa kavuşturmak için yabancı anahtar kısıtlaması adları</span><span class="sxs-lookup"><span data-stu-id="1e5b9-656">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="1e5b9-657">İzleme sorun #10730</span><span class="sxs-lookup"><span data-stu-id="1e5b9-657">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="1e5b9-658">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-658">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1e5b9-659">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-659">**Old behavior**</span></span>

<span data-ttu-id="1e5b9-660">EF Core 3.0 önce yabancı anahtar kısıtlaması adları için yalnızca "adı olarak" adı veriliyordu.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-660">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="1e5b9-661">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-661">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="1e5b9-662">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-662">**New behavior**</span></span>

<span data-ttu-id="1e5b9-663">EF Core 3.0 ile başlayarak, yabancı anahtar kısıtlaması adları şimdi de "kısıtlama adı" adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-663">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="1e5b9-664">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1e5b9-664">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="1e5b9-665">**Neden**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-665">**Why**</span></span>

<span data-ttu-id="1e5b9-666">Bu değişiklik, bu alanda adlandırma için tutarlılık getirir ve ayrıca bu yabancı anahtarı üzerinde tanımlanan yabancı anahtar kısıtlaması ve olmayan sütun veya özellik adı adı olduğunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-666">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="1e5b9-667">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="1e5b9-667">**Mitigations**</span></span>

<span data-ttu-id="1e5b9-668">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e5b9-668">Use the new name.</span></span>
