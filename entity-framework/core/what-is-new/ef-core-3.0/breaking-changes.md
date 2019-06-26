---
title: EF Core 3.0 - EF Core yeni değişiklikler
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 96586808862c4373168dcd34a5f00c9f2f7563c3
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394823"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="6dd8e-102">EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil edilen değişiklikler</span><span class="sxs-lookup"><span data-stu-id="6dd8e-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dd8e-103">Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="6dd8e-104">Aşağıdaki API ve davranışı değişiklikleri EF Core için geliştirilen uygulamaları ayırmak için olası sahip bunları 3.0.0 için yükseltirken 2.2.x.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="6dd8e-105">Veritabanı sağlayıcıları yalnızca etkileyen bekliyoruz değişiklikleri altında belgelenir [sağlayıcısı değişiklikleri](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="6dd8e-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="6dd8e-106">Bir 3.0 Önizlemesi'nden başka bir 3.0 Önizleme için kullanıma sunulan yeni özellikler sonlarını burada belgelenmemiş.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="6dd8e-107">LINQ sorguları, artık istemcide değerlendirilir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="6dd8e-108">[Sorun #14935 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[sorun #12795 Ayrıca bkz:](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="6dd8e-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="6dd8e-109">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-110">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-110">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-111">EF Core, SQL veya bir parametre için bir sorgunun parçası olan bir ifade dönüştürülemedi, 3.0 önce bunu otomatik olarak istemcide ifadesi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="6dd8e-112">Varsayılan olarak, istemci değerlendirmesi yüksek maliyetlere neden olabilecek bir ifade yalnızca bir uyarı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="6dd8e-113">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-113">**New behavior**</span></span>

<span data-ttu-id="6dd8e-114">3.0 ile başlayarak, EF Core yalnızca ifadeleri üst düzey projeksiyon izin verir (son `Select()` sorguda çağrısı) istemcide değerlendirilecek.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="6dd8e-115">İfadeleri herhangi bir sorgunun parçası olarak SQL veya bir parametre olarak dönüştürülemez, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="6dd8e-116">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-116">**Why**</span></span>

<span data-ttu-id="6dd8e-117">Otomatik istemci değerlendirme sorguların bile bunları önemli bölümleri çevrilemez yürütülecek çok sayıda sorgu sağlar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="6dd8e-118">Bu davranış yalnızca üretimde yetkisiz değiştirmeye karşı korumalı hale gelebilir beklenmeyen ve zararlı davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="6dd8e-119">Örneğin, bir koşulda bir `Where()` çevrilemez çağrısı, veritabanı sunucusu ve istemcide uygulanması için filtreyi aktarılacak tablosundan tüm satırları açabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="6dd8e-120">Tablo, geliştirme, ancak isabet sabit yalnızca birkaç satır içeriyorsa uygulama üretime burada tablo milyonlarca satır içerebilir taşındığında bu durum kolayca algılanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="6dd8e-121">İstemci değerlendirme uyarılar ayrıca geliştirme sırasında göz ardı edilmesi çok kolay kanıtladılar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="6dd8e-122">Belirli ifadeler için iyileştirme sorgu çevirisi sürümler arasında istenmeyen bozucu değişiklikleri nedeniyle oluşan sorunları bu yanı sıra, otomatik istemci değerlendirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="6dd8e-123">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-123">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-124">Bir sorgu tam olarak çevrilemez sonra ya da çevrilebilir veya kullanan bir form sorguda yeniden `AsEnumerable()`, `ToList()`, veya açıkça verileri Burada, ardından daha büyük olabilir istemciye geri getirmek için benzer LINQ nesneleri kullanılarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="6dd8e-125">Entity Framework Core artık ASP.NET Core paylaşılan framework'ün bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="6dd8e-126">İzleme sorunu duyuruları #325</span><span class="sxs-lookup"><span data-stu-id="6dd8e-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="6dd8e-127">Bu değişiklik, ASP.NET Core 3.0 Önizleme 1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="6dd8e-128">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-128">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-129">ASP.NET Core 3.0 Paketi başvurusu eklendiğinde önce `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`, EF Core içerir ve EF Core veri sağlayıcıları bazıları istediğiniz SQL Server sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="6dd8e-130">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-130">**New behavior**</span></span>

<span data-ttu-id="6dd8e-131">3. 0'dan başlayarak, ASP.NET Core paylaşılan çerçeve EF Core veya tüm EF Core veri sağlayıcıları içermez.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="6dd8e-132">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-132">**Why**</span></span>

<span data-ttu-id="6dd8e-133">Bu değişiklikten önce EF Core alma olup uygulama ASP.NET Core ve SQL Server veya hedeflenen bağlı olarak farklı adımlar gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="6dd8e-134">Ayrıca, ASP.NET Core yükseltme EF Core ve SQL Server sağlayıcısı, her zaman tercih değildir yükseltmesini zorlandı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="6dd8e-135">Bu değişiklik, EF Core alma aynı tüm sağlayıcıları, desteklenen .NET uygulamaları ve uygulama türleri arasında deneyimidir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="6dd8e-136">Geliştiriciler, tam olarak EF Core ve EF Core veri sağlayıcıları yükseltildiğinde de denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="6dd8e-137">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-137">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-138">EF çekirdekli ASP.NET Core 3.0 uygulama veya desteklenen başka bir uygulama içinde kullanmak için açıkça uygulamanızın kullanacağı EF Core veritabanı sağlayıcısı için bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="6dd8e-139">EF Core komut satırı aracı, dotnet ef artık .NET Core SDK'sının bir parçasıdır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="6dd8e-140">İzleme sorun #14016</span><span class="sxs-lookup"><span data-stu-id="6dd8e-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="6dd8e-141">Bu değişiklik EF Core 3.0-preview 4 ve .NET Core SDK'sını ilgili sürümü kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="6dd8e-142">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-142">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-143">3.0 önce `dotnet ef` aracı, .NET Core SDK'yı eklenmiştir ve ek adımlar gerek kalmadan, herhangi bir projeyi komut satırından kullanmaya hazır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="6dd8e-144">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-144">**New behavior**</span></span>

<span data-ttu-id="6dd8e-145">3. 0'dan başlayarak .NET SDK'sı yer almaz `dotnet ef` aracı kullanabilmeniz için önce açıkça yerel veya genel bir aracı yüklemek zorunda.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="6dd8e-146">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-146">**Why**</span></span>

<span data-ttu-id="6dd8e-147">Bu değişiklik dağıtmak ve güncelleştirmek sağlıyor `dotnet ef` bir düzenli .NET CLI aracı, NuGet üzerindeki EF Core 3.0 de her zaman bir NuGet paketi olarak dağıtılmış bir olgu ile tutarlı olarak.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="6dd8e-148">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-148">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-149">Geçişleri veya iskele yönetebilmek için bir `DbContext`, yükleme `dotnet-ef` kullanarak `dotnet tool install` komutu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="6dd8e-150">Örneğin, bir genel araç olarak yüklemek için şu komutu yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="6dd8e-151">Aletle şekillerini kullanan bağımlılık olarak bildiren bir proje bağımlılıklarını geri yüklediğinizde de, yerel aracı edinebilirsiniz bir [aracı bildirim dosyası](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="6dd8e-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="6dd8e-152">SQL ve ExecuteSql ExecuteSqlAsync yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="6dd8e-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="6dd8e-153">İzleme sorun #10996</span><span class="sxs-lookup"><span data-stu-id="6dd8e-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="6dd8e-154">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-155">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-155">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-156">EF Core 3.0 önce bu yöntem adlarının bir normal bir dize ya da SQL ve parametreleri ilişkilendirilmiş bir dize ile çalışmak için aşırı yüklenmiş.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="6dd8e-157">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-157">**New behavior**</span></span>

<span data-ttu-id="6dd8e-158">EF Core 3.0 ile başlayarak, kullanmaktadır `FromSqlRaw`, `ExecuteSqlRaw`, ve `ExecuteSqlRawAsync` burada parametreleri geçirilir ayrı olarak Sorgu dizesinden parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="6dd8e-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="6dd8e-160">Kullanım `FromSqlInterpolated`, `ExecuteSqlInterpolated`, ve `ExecuteSqlInterpolatedAsync` burada parametreleri geçirilir ilişkilendirilmiş sorgu dizesinin parçası olarak parametreli bir sorgu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="6dd8e-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="6dd8e-162">Yukarıdaki sorgu her ikisi de aynı SQL parametrelere sahip aynı parametreli SQL üretecektir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="6dd8e-163">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-163">**Why**</span></span>

<span data-ttu-id="6dd8e-164">Yöntem aşırı yüklemeleri böyle güvenmelidir yanı sıra ilişkilendirilmiş dize yöntemini çağırmak için hedefi olduğu zaman yanlışlıkla ham dize yöntemini çağırmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="6dd8e-165">Bu, verilmiş olması, parametreli getirilemedi sorgularda neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="6dd8e-166">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-166">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-167">Yeni yöntem adları kullanılacak anahtar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="6dd8e-168">SQL yöntemleri yalnızca sorgu kökleri üzerinde belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="6dd8e-169">İzleme sorun #15704</span><span class="sxs-lookup"><span data-stu-id="6dd8e-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="6dd8e-170">Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="6dd8e-171">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-171">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-172">EF Core 3.0 önce `FromSql` yöntemi belirtilebilir herhangi bir sorgu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="6dd8e-173">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-173">**New behavior**</span></span>

<span data-ttu-id="6dd8e-174">EF Core 3.0 ile başlayan yeni `FromSqlRaw` ve `FromSqlInterpolated` yöntemleri (hangi Değiştir `FromSql`) yalnızca sorgu kökleri üzerinde yani doğrudan belirtilebilir `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="6dd8e-175">Başka bir yerde bunları denemek, bir derleme hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="6dd8e-176">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-176">**Why**</span></span>

<span data-ttu-id="6dd8e-177">Belirtme `FromSql` herhangi bir yere dışında üzerinde bir `DbSet` hiçbir anlamı eklendi veya değer ve belirli senaryolarda karışıklığa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="6dd8e-178">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-178">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-179">`FromSql` çağrıları doğrudan açık olmasını taşınması gereken `DbSet` hangi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="6dd8e-180">~~Sorgu yürütme hata ayıklama düzeyinde günlüğe~~ geri Dönüldü</span><span class="sxs-lookup"><span data-stu-id="6dd8e-180">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="6dd8e-181">İzleme sorun #14523</span><span class="sxs-lookup"><span data-stu-id="6dd8e-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="6dd8e-182">Bu değişiklik, EF Core 3.0-Önizleme 7 döndürülür.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-182">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="6dd8e-183">EF Core 3. 0'ı yeni yapılandırma günlük düzeyi için herhangi bir olay uygulama tarafından belirtilmesine izin verdiği için Biz bu değişiklik geri alınır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-183">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="6dd8e-184">Örneğin, SQL günlüğü geçiş için `Debug`, açıkça düzeyinde yapılandırma `OnConfiguring` veya `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-184">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="6dd8e-185">Geçici bir anahtar değere artık varlık örneklerini ayarlanır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-185">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="6dd8e-186">İzleme sorun #12378</span><span class="sxs-lookup"><span data-stu-id="6dd8e-186">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="6dd8e-187">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-187">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="6dd8e-188">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-188">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-189">EF Core 3.0 önce veritabanı tarafından oluşturulan gerçek değer daha sonra sahip olabileceği tüm anahtar özellikleri geçici değerler atanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-189">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="6dd8e-190">Genellikle bu geçici değerleri büyük negatif sayılar yoktu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-190">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="6dd8e-191">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-191">**New behavior**</span></span>

<span data-ttu-id="6dd8e-192">3.0 ile başlayarak, EF Core, varlığın izleme bilgilerini bir parçası olarak geçici bir anahtar değeri depolar ve anahtar özelliği kendisini değiştirmeden bırakır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-192">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="6dd8e-193">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-193">**Why**</span></span>

<span data-ttu-id="6dd8e-194">Geçici bir anahtar değere deneyebileceğinizi daha önce bazı tarafından izlenen bir varlık kalıcı hale gelmesini önlemek için bu değişiklik yapılmıştır `DbContext` örneği farklı bir taşınır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-194">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="6dd8e-195">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-195">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-196">Varlıklar arasında ilişkilendirmeler form üzerine yabancı anahtarlar birincil anahtar değerlerini atamak uygulamaları birincil anahtarları depoda üretilmiş ve varlıklara ait eski davranışı bağımlı `Added` durumu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-196">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="6dd8e-197">Tarafından önlenebilir:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-197">This can be avoided by:</span></span>
* <span data-ttu-id="6dd8e-198">Depoda üretilmiş anahtarlar kullanmıyor.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-198">Not using store-generated keys.</span></span>
* <span data-ttu-id="6dd8e-199">Yabancı anahtar değerleri ayarlamak yerine form ilişkiler için Gezinti özelliklerini ayarlama.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-199">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="6dd8e-200">Gerçek geçici anahtar değerlerinin varlığın izleme bilgileri edinin.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-200">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="6dd8e-201">Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` geçici değer olsa bile iade `blog.Id` kendisini ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-201">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="6dd8e-202">Depoda üretilmiş anahtar değerlerini DetectChanges geliştirir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-202">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="6dd8e-203">İzleme sorun #14616</span><span class="sxs-lookup"><span data-stu-id="6dd8e-203">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="6dd8e-204">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-204">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-205">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-205">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-206">EF Core 3.0 önce izlenmeyen bir varlık tarafından bulunan `DetectChanges` olarak izlenmesi `Added` durum ve eklenen yeni bir olarak ne zaman satır `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-206">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="6dd8e-207">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-207">**New behavior**</span></span>

<span data-ttu-id="6dd8e-208">Oluşturulan anahtar değerlerini bir varlık kullanıyorsa EF Core 3.0 ile başlayan ve bazı anahtar değerini ayarlayın ve ardından varlığı olarak takip `Modified` durumu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-208">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="6dd8e-209">Bu varlık için bir satır var olduğu varsayılır ve bu anlamına gelir ne zaman güncelleştirilmiş `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-209">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="6dd8e-210">Anahtar değeri ayarlanmamış, veya varlık türü kullanmıyor oluşturulan anahtarları varsa yeni bir varlık olarak hala izlenecektir `Added` önceki sürümlerinde olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-210">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="6dd8e-211">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-211">**Why**</span></span>

<span data-ttu-id="6dd8e-212">Bu değişiklik, daha kolay ve bağlantısı kesilmiş varlık grafiklerle depoda üretilmiş anahtarlar kullanılırken çalışmak için daha tutarlı hale getirmek için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-212">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="6dd8e-213">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-213">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-214">Bu değişiklik, bir varlık türü için üretilmiş anahtarlar yapılandırılır, ancak anahtar değerlerine açıkça yeni örnekleri için ayarlanan uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-214">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="6dd8e-215">Düzeltme üretilmiş değerleri anahtar özelliklerine açıkça yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-215">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="6dd8e-216">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-216">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="6dd8e-217">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-217">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="6dd8e-218">Art arda silme işlemi hemen hemen varsayılan olarak gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-218">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="6dd8e-219">İzleme sorun #10114</span><span class="sxs-lookup"><span data-stu-id="6dd8e-219">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="6dd8e-220">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-220">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-221">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-221">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-222">SaveChanges çağrıldı kadar EF Core uygulanan basamaklı eylemlerinin (gerekli sorumlu silindiğinde veya ilişki gerekli sorumlusuna yazıyordunuz bağımlı varlıkları silmek) 3.0 önce gerçekleşmedi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-222">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="6dd8e-223">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-223">**New behavior**</span></span>

<span data-ttu-id="6dd8e-224">Tetikleme koşulu algılandığında hemen sonra 3.0 ile başlayarak, EF Core basamaklı eylemlerinin geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-224">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="6dd8e-225">Örneğin, çağırma `context.Remove()` asıl bir varlığı silme tüm izlenen sonucu ilgili gerekli bağımlılıklar da ayarlanacak şekilde `Deleted` hemen.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-225">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="6dd8e-226">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-226">**Why**</span></span>

<span data-ttu-id="6dd8e-227">Veri bağlama ve hangi varlıkları silinecek anlamanız önemli olduğu senaryolar denetimi deneyimini geliştirmek için bu değişiklik yapılmıştır _önce_ `SaveChanges` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-227">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="6dd8e-228">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-228">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-229">Davranış ayarları aracılığıyla geri yüklenebilir `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-229">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="6dd8e-230">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-230">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="6dd8e-231">DeleteBehavior.Restrict temizleyici semantiğe sahip</span><span class="sxs-lookup"><span data-stu-id="6dd8e-231">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="6dd8e-232">İzleme sorun #12661</span><span class="sxs-lookup"><span data-stu-id="6dd8e-232">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="6dd8e-233">Bu değişiklik, EF Core 3.0-preview 5'teki sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-233">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="6dd8e-234">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-234">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-235">3.0 önce `DeleteBehavior.Restrict` ile veritabanındaki yabancı anahtarlar oluşturulan `Restrict` semantiği, aynı zamanda açık olmayan bir şekilde değiştirilmiş iç düzeltme.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-235">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="6dd8e-236">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-236">**New behavior**</span></span>

<span data-ttu-id="6dd8e-237">3.0 ile başlayan `DeleteBehavior.Restrict` yabancı anahtarlar ile oluşturulan sağlar `Restrict` semantiği--diğer bir deyişle, hiçbir basamaklar; throw EF iç düzeltme de etkilemeden kısıtlama ihlali üzerinde--.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-237">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="6dd8e-238">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-238">**Why**</span></span>

<span data-ttu-id="6dd8e-239">Bu değişiklik deneyimini geliştirmek amacıyla kullanılarak yapıldığını `DeleteBehavior` beklenmeyen yan etkiler olmadan sezgisel bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-239">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="6dd8e-240">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-240">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-241">Kullanarak önceki davranış geri yüklenebilir `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-241">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="6dd8e-242">Sorgu türleri varlık türleri ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-242">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="6dd8e-243">İzleme sorun #14194</span><span class="sxs-lookup"><span data-stu-id="6dd8e-243">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="6dd8e-244">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-244">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-245">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-245">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-246">EF Core 3.0 önce [sorgu türü](xref:core/modeling/query-types) olan birincil anahtar, yapılandırılmış bir biçimde tanımlamıyor verileri sorgulamak için bir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-246">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="6dd8e-247">Diğer bir deyişle, bir sorgu türü kullanıldı sırasında normal bir varlık anahtarları (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) olmadan varlık türleriyle eşlemek için bir anahtar kullanılabilir (daha büyük olasılıkla bir tablodan ancak büyük olasılıkla bir görünümden) olduğunda türü kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-247">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="6dd8e-248">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-248">**New behavior**</span></span>

<span data-ttu-id="6dd8e-249">Bir sorgu türü artık yalnızca birincil anahtarı olmayan bir varlık türü olur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-249">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="6dd8e-250">Anahtarsız varlık türleri, önceki sürümlerde sorgu türleri aynı işlevselliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-250">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="6dd8e-251">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-251">**Why**</span></span>

<span data-ttu-id="6dd8e-252">Sorgu türleri amacı etrafında karışıklık azaltmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-252">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="6dd8e-253">Özellikle, anahtarsız varlık türleridir ve bu nedenle kendiliğinden salt okunur olduklarından, ancak yalnızca bir varlık türü salt okunur olması gerekir çünkü bunlar kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-253">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="6dd8e-254">Benzer şekilde, genellikle görünümlerine eşlenmiş, ancak yalnızca görünümleri genellikle anahtarlar tanımlama yok olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-254">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="6dd8e-255">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-255">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-256">Aşağıdaki bölümleri API artık kullanılmıyor:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-256">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="6dd8e-257">**`ModelBuilder.Query<>()`** -Bunun yerine `ModelBuilder.Entity<>().HasNoKey()` hiçbir anahtarına sahip bir varlık türü işaretlemek için çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-257">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="6dd8e-258">Bu yine de bir birincil anahtar bekleniyordu, ancak kuralı eşleşmiyor, yanlış yapılandırma önlemek için kural olarak yapılandırılamadığı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-258">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="6dd8e-259">**`DbQuery<>`** -Bunun yerine `DbSet<>` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-259">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="6dd8e-260">**`DbContext.Query<>()`** -Bunun yerine `DbContext.Set<>()` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-260">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="6dd8e-261">Sahip olunan tür ilişkileri için API yapılandırması değişti</span><span class="sxs-lookup"><span data-stu-id="6dd8e-261">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="6dd8e-262">[Sorun #12444 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sorun #9148 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sorun #14153 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="6dd8e-262">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="6dd8e-263">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-263">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-264">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-264">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-265">EF Core 3.0 önce hemen sonra yapılandırma sahibi ilişkinin gerçekleştirildi `OwnsOne` veya `OwnsMany` çağırın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-265">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="6dd8e-266">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-266">**New behavior**</span></span>

<span data-ttu-id="6dd8e-267">EF Core 3.0 ile başlayarak, var. Şimdi fluent API'sini kullanarak sahibine bir gezinti özelliğini yapılandırmak için `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-267">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="6dd8e-268">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-268">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="6dd8e-269">Yapılandırma sahibi arasındaki ilişkiyi ilgili ve ait hemen sonra zincirlenmesi gereken `WithOwner()` diğer ilişkileri nasıl yapılandırılacağını benzer şekilde.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-269">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="6dd8e-270">Sahip olunan türü hala sonra zincirlenmesine için yapılandırma while `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-270">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="6dd8e-271">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-271">For example:</span></span>

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

<span data-ttu-id="6dd8e-272">Ayrıca çağırma `Entity()`, `HasOne()`, veya `Set()` sahip olunan bir türü ile hedef şimdi bir özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-272">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="6dd8e-273">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-273">**Why**</span></span>

<span data-ttu-id="6dd8e-274">Sahip olunan türü yapılandırma arasında daha net bir ayrım oluşturmak için bu değişiklik yapılmıştır ve _ilişkisi_ ait türü.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-274">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="6dd8e-275">Bu sırayla belirsizlik ve yöntemler gibi geçici karışıklık kaldırır `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-275">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="6dd8e-276">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-276">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-277">Yukarıdaki örnekte gösterildiği gibi yeni bir API yüzeyi kullanmak için sahip olunan tür ilişkileri yapılandırmasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-277">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="6dd8e-278">Bağımlı varlıkları tablo sorumlusuyla paylaşımı artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-278">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="6dd8e-279">İzleme sorun #9005</span><span class="sxs-lookup"><span data-stu-id="6dd8e-279">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="6dd8e-280">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-280">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-281">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-281">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-282">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-282">Consider the following model:</span></span>
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
<span data-ttu-id="6dd8e-283">3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya aynı tabloya açıkça eşleştirilmiş bir `OrderDetails` örneği her zaman gerekli yeni bir eklerken `Order`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-283">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="6dd8e-284">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-284">**New behavior**</span></span>

<span data-ttu-id="6dd8e-285">3.0 ile başlayarak, EF Core eklemek için sağlayan bir `Order` olmadan bir `OrderDetails` ve tüm eşler `OrderDetails` özellikleri hariç null yapılabilir sütunlar için birincil anahtarı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-285">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="6dd8e-286">EF Core kümeleri sorgulanırken `OrderDetails` için `null` gerekli özelliklerinden birini yoksa, bir değer veya birincil anahtarı yanı sıra gereken özellikleri yoktur ve tüm özellikleri `null`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-286">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="6dd8e-287">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-287">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-288">İsteğe bağlı tüm sütunlar eklenmiş bağımlı paylaşımı tablo modelinizi varken işaret Gezinti olması beklenmiyor `null` Gezinti olduğunda durumlarında uygulama değiştirilmelidir sonra `null`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-288">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="6dd8e-289">Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmesini veya en az bir özelliğine sahip olmayan bir`null` atanmış değer.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-289">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="6dd8e-290">Bir özelliğini eşleştirmek tüm varlıklar bir eşzamanlılık belirteci sütun içeren bir tablo paylaşımı sahip</span><span class="sxs-lookup"><span data-stu-id="6dd8e-290">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="6dd8e-291">İzleme sorun #14154</span><span class="sxs-lookup"><span data-stu-id="6dd8e-291">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="6dd8e-292">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-292">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-293">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-293">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-294">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-294">Consider the following model:</span></span>
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
<span data-ttu-id="6dd8e-295">3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya yalnızca güncelleştirme aynı tabloya açıkça eşlenen `OrderDetails` değil güncelleştirecek `Version` değeri istemcisi ve İleri güncelleştirmeyi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-295">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="6dd8e-296">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-296">**New behavior**</span></span>

<span data-ttu-id="6dd8e-297">EF Core 3.0 ile başlayarak, yeni yayınlar `Version` değerini `Order` bunu sahipse `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-297">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="6dd8e-298">Aksi takdirde model doğrulama sırasında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-298">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="6dd8e-299">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-299">**Why**</span></span>

<span data-ttu-id="6dd8e-300">Eski eşzamanlılık belirteç değeri aynı tabloya eşlenen varlıkları yalnızca biri güncelleştirildiğinde önlemek için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-300">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="6dd8e-301">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-301">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-302">Eşzamanlılık belirteci sütuna eşlenmiş bir özellik içerir tabloda paylaşımı tüm varlıklar gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-302">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="6dd8e-303">Mümkündür oluşturma bir gölge durumu:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-303">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="6dd8e-304">Tüm türetilmiş türler için tek bir sütun eşlenmemiş türlerden devralınan özellikler artık eşlenir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-304">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="6dd8e-305">İzleme sorun #13998</span><span class="sxs-lookup"><span data-stu-id="6dd8e-305">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="6dd8e-306">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-306">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-307">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-307">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-308">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-308">Consider the following model:</span></span>
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

<span data-ttu-id="6dd8e-309">EF Core 3.0 önce `ShippingAddress` özelliği için sütunları ayırmak için eşlenebilir `BulkOrder` ve `Order` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-309">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="6dd8e-310">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-310">**New behavior**</span></span>

<span data-ttu-id="6dd8e-311">3.0 ile başlayarak, EF Core yalnızca bir sütun için oluşturur `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-311">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="6dd8e-312">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-312">**Why**</span></span>

<span data-ttu-id="6dd8e-313">Eski davranışı beklenmiyordu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-313">The old behavoir was unexpected.</span></span>

<span data-ttu-id="6dd8e-314">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-314">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-315">Özellik sütunu türetilmiş türler üzerinde ayırmak için açıkça eşlenebilir:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-315">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="6dd8e-316">Yabancı anahtar özellik kuralı asıl özelliğiyle aynı ada artık eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-316">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="6dd8e-317">İzleme sorun #13274</span><span class="sxs-lookup"><span data-stu-id="6dd8e-317">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="6dd8e-318">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-318">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-319">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-319">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-320">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-320">Consider the following model:</span></span>
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
<span data-ttu-id="6dd8e-321">EF Core 3.0 önce `CustomerId` özelliği kullanılan Kural gereği için yabancı anahtarı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-321">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="6dd8e-322">Ancak, varsa `Order` bu da yapacağı sonra sahip olunan bir türü olan `CustomerId` birincil anahtar ve bu değilse genellikle beklentisi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-322">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="6dd8e-323">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-323">**New behavior**</span></span>

<span data-ttu-id="6dd8e-324">3.0 ile başlayarak, EF Core asıl özelliğiyle aynı ada sahip oldukları özellikleri için yabancı anahtarlar kurala göre kullanırsanız dener.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-324">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="6dd8e-325">Asıl tür adı asıl özellik adıyla birleştirilmiş ve asıl özellik adı desenleri ile birleştirilmiş gezinme adı hala eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-325">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="6dd8e-326">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-326">For example:</span></span>

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

<span data-ttu-id="6dd8e-327">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-327">**Why**</span></span>

<span data-ttu-id="6dd8e-328">Bu değişikliği yanlışlıkla bir birincil anahtar özelliği sahip olunan türüne tanımlamamaya özen gösterin çalışıldı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-328">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="6dd8e-329">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-329">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-330">Yabancı anahtar olması ve bu nedenle birincil anahtarın sonra açıkça bölüm özelliği isteniyorsa bu şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-330">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="6dd8e-331">Veritabanı bağlantısı artık kapalı kullanılmazsa süresi TransactionScope tamamlanmadan önce artık</span><span class="sxs-lookup"><span data-stu-id="6dd8e-331">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="6dd8e-332">İzleme sorun #14218</span><span class="sxs-lookup"><span data-stu-id="6dd8e-332">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="6dd8e-333">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-333">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-334">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-334">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-335">EF Core bağlam içinde bağlantısına açarsa, 3.0 önce bir `TransactionScope`, bağlantı sırasında geçerli açık kalır `TransactionScope` etkindir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-335">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="6dd8e-336">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-336">**New behavior**</span></span>

<span data-ttu-id="6dd8e-337">Kullanmaya tamamladıktan hemen sonra 3.0 ile başlayarak, EF Core bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-337">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="6dd8e-338">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-338">**Why**</span></span>

<span data-ttu-id="6dd8e-339">Birden fazla bağlamı aynı kullanmak için bu değişiklik sağlayan `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-339">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="6dd8e-340">Yeni davranış da EF6 eşleşir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-340">The new behavior also matches EF6.</span></span>

<span data-ttu-id="6dd8e-341">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-341">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-342">Bağlantı açık çağrı açık kalması gerekiyorsa `OpenConnection()` EF Core, erken kapatmadığına emin olun:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-342">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="6dd8e-343">Her bir özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-343">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="6dd8e-344">İzleme sorun #6872</span><span class="sxs-lookup"><span data-stu-id="6dd8e-344">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="6dd8e-345">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-345">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-346">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-346">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-347">EF Core 3.0 önce tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-347">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="6dd8e-348">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-348">**New behavior**</span></span>

<span data-ttu-id="6dd8e-349">EF Core 3.0 ile başlayarak, her bir tamsayı anahtar özellik değer Oluşturucusu kendi bellek içi veritabanı kullanılırken alır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-349">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="6dd8e-350">Ayrıca, veritabanı silinirse, anahtar oluşturma için tüm tabloları sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-350">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="6dd8e-351">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-351">**Why**</span></span>

<span data-ttu-id="6dd8e-352">Bellek içi anahtar oluşturma gerçek veritabanı anahtar oluşturma için daha yakından Hizala ve bellek içi veritabanı kullanılırken testler birbirinden ayırma özelliğini artırmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-352">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="6dd8e-353">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-353">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-354">Bunu ayarlamak için belirli bellek içi anahtar değerlerine bağlı olan bir uygulama bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-354">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="6dd8e-355">Göz önünde bulundurun yerine değil özel anahtar değerlerine bağlı olan veya yeni davranış eşleştirilecek güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-355">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="6dd8e-356">Varsayılan olarak kullanılan destekleyen alanlar</span><span class="sxs-lookup"><span data-stu-id="6dd8e-356">Backing fields are used by default</span></span>

[<span data-ttu-id="6dd8e-357">İzleme sorun #12430</span><span class="sxs-lookup"><span data-stu-id="6dd8e-357">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="6dd8e-358">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-358">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="6dd8e-359">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-359">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-360">3.0 önce bir özellik için destek alanı biliniyordu olsa bile, EF Core yine de varsayılan olarak okuma ve özellik alıcı ve ayarlayıcı yöntemleri kullanarak özellik değeri yazma.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-360">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="6dd8e-361">Bunun özel durumu burada destek alanı doğrudan biliniyorsa ayarlamak sorgu yürütme oluştu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-361">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="6dd8e-362">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-362">**New behavior**</span></span>

<span data-ttu-id="6dd8e-363">Bir özellik için destek alanı biliniyorsa EF Core 3.0 ile başlayarak, ardından EF Core her zaman okuyabilmesini ve yazma yedekleme alanını kullanarak bu özelliği.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-363">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="6dd8e-364">Uygulama, alıcı veya ayarlayıcı yöntemleri kodlanmış ek davranış bağlı değilse bu bir uygulama sonu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-364">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="6dd8e-365">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-365">**Why**</span></span>

<span data-ttu-id="6dd8e-366">Bu değişiklik iş mantığı deneyebileceğinizi olarak tetikleyen EF Core önlemek için varsayılan olarak varlıkları içeren bir veritabanı işlemleri gerçekleştirirken yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-366">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="6dd8e-367">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-367">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-368">Öncesi 3.0 davranışı özellik erişim modu konfigürasyonuyla geri yüklenebilir `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-368">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="6dd8e-369">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-369">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="6dd8e-370">Birden çok uyumlu yedekleme alan bulunamazsa throw</span><span class="sxs-lookup"><span data-stu-id="6dd8e-370">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="6dd8e-371">İzleme sorun #12523</span><span class="sxs-lookup"><span data-stu-id="6dd8e-371">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="6dd8e-372">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-372">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-373">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-373">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-374">Birden çok alan karşılarsa, EF Core 3.0 önce bazı öncelik sırası üzerinde bir alan seçilirdi sonra bir özelliğinin destek alanı bulma kurallarını temel.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-374">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="6dd8e-375">Bu, belirsiz durumda kullanılacak yanlış alan neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-375">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="6dd8e-376">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-376">**New behavior**</span></span>

<span data-ttu-id="6dd8e-377">Birden çok alan aynı özelliğe eşleşirse EF Core 3.0 ile başlayarak, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-377">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="6dd8e-378">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-378">**Why**</span></span>

<span data-ttu-id="6dd8e-379">Yalnızca biri doğru olduğunda sessiz bir şekilde bir alanı başka bir kullanmaktan kaçınmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-379">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="6dd8e-380">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-380">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-381">Belirsiz destekleyen alanlarla özellikleri, açıkça belirtilen kullanılacak bir alan olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-381">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="6dd8e-382">Örneğin, fluent API'sini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-382">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="6dd8e-383">Alan adı alanı-yalnızca özellik adlarının eşleşmesi gerekir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-383">Field-only property names should match the field name</span></span>

<span data-ttu-id="6dd8e-384">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-384">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-385">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-385">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-386">EF Core 3.0 önce bir özellik bir dize değeri belirtilebilir ve bu ada sahip hiçbir özellik CLR türüne bulunduysa EF Core kuralı kurallarını kullanarak bir alan için eşleşen deneyin.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-386">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="6dd8e-387">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-387">**New behavior**</span></span>

<span data-ttu-id="6dd8e-388">EF Core 3.0 ile başlayarak, bir alan özelliği salt okunur alan adı tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-388">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="6dd8e-389">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-389">**Why**</span></span>

<span data-ttu-id="6dd8e-390">Benzer şekilde adlı iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır, ayrıca eşleşen kuralları yalnızca alan özellikleri için CLR eşleniyor özellikleri aynıdır kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-390">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="6dd8e-391">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-391">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-392">Yalnızca alan özellikleri aynı eşleştirildikleri alan olarak adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-392">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="6dd8e-393">EF Core 3.0 daha yeni bir önizleme özelliği adından farklı bir alan adı açıkça yapılandırma yeniden etkinleştirmek planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-393">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="6dd8e-394">AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağırın</span><span class="sxs-lookup"><span data-stu-id="6dd8e-394">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="6dd8e-395">İzleme sorun #14756</span><span class="sxs-lookup"><span data-stu-id="6dd8e-395">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="6dd8e-396">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-396">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-397">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-397">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-398">EF Core 3.0, çağırma önce `AddDbContext` veya `AddDbContextPool` de günlüğe kaydetme ve bellek D.I hizmetleriyle yapılan çağrılar aracılığıyla önbelleğe kaydetmek [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="6dd8e-398">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="6dd8e-399">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-399">**New behavior**</span></span>

<span data-ttu-id="6dd8e-400">EF Core 3.0 ile başlayan `AddDbContext` ve `AddDbContextPool` artık kayıt hizmetlerin bağımlılık ekleme (dı) sahip olur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-400">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="6dd8e-401">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-401">**Why**</span></span>

<span data-ttu-id="6dd8e-402">EF Core 3.0, bu hizmetler uygulamanın DI kapsayıcısında olduğunu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-402">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="6dd8e-403">Ancak, varsa `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlı EF Core tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-403">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="6dd8e-404">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-404">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-405">Uygulamanız bu hizmetlere ihtiyacı varsa, sonra bunları açıkça DI kullanarak kapsayıcı kayıt [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="6dd8e-405">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="6dd8e-406">DbContext.Entry artık yerel DetectChanges gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-406">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="6dd8e-407">İzleme sorun #13552</span><span class="sxs-lookup"><span data-stu-id="6dd8e-407">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="6dd8e-408">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-408">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-409">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-409">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-410">EF Core 3.0, çağırma önce `DbContext.Entry` izlenen tüm varlıklar için algılanabilir değişiklik neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-410">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="6dd8e-411">Bu durumu içinde kullanıma sunulan sağlamış `EntityEntry` güncel oluştu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-411">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="6dd8e-412">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-412">**New behavior**</span></span>

<span data-ttu-id="6dd8e-413">Çağırma EF Core ile 3.0, başlangıç `DbContext.Entry` ilgili asıl varlıkları yalnızca belirtilen varlık ve diğer değişikliklerini algılamak girişimi izlenen başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-413">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="6dd8e-414">Başka bir yerde değiştirir Bunun anlamı etkileri uygulama durumu olabilir. Bu yöntemi çağrılarak algılandı değil.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-414">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="6dd8e-415">Unutmayın `ChangeTracker.AutoDetectChangesEnabled` ayarlanır `false` sonra bile bu yerel değişiklik algılama devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-415">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="6dd8e-416">Değişiklik algılama--örneğin neden diğer yöntemleri `ChangeTracker.Entries` ve `SaveChanges`--yine de tam neden `DetectChanges` tüm varlıkları izlenir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-416">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="6dd8e-417">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-417">**Why**</span></span>

<span data-ttu-id="6dd8e-418">Kullanmanın varsayılan performansını artırmak için bu değişiklik yapılmıştır `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-418">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="6dd8e-419">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-419">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-420">Çağrı `ChgangeTracker.DetectChanges()` açıkça çağırmadan önce `Entry` öncesi 3.0 davranış sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-420">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="6dd8e-421">Dize ve bayt dizisi anahtarlar varsayılan olarak istemci tarafından oluşturulan değildir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-421">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="6dd8e-422">İzleme sorun #14617</span><span class="sxs-lookup"><span data-stu-id="6dd8e-422">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="6dd8e-423">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-424">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-424">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-425">EF Core 3.0 önce `string` ve `byte[]` açıkça null olmayan bir değer ayarlamadan anahtar özellikleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-425">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="6dd8e-426">Böyle bir durumda anahtar değeri istemcide bayt için seri hale getirilmiş bir GUID olarak oluşturulacak `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-426">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="6dd8e-427">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-427">**New behavior**</span></span>

<span data-ttu-id="6dd8e-428">Bir özel durum EF Core 3.0 ile başlayarak, anahtar değer kümesi gösteren oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-428">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="6dd8e-429">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-429">**Why**</span></span>

<span data-ttu-id="6dd8e-430">Bu değişiklik nedeniyle istemci tarafından oluşturulan yapılmıştır `string` / `byte[]` değerleri genellikle olmadığı kullanışlı ve varsayılan davranışını yaptığı sabit nedeni hakkında oluşturulan anahtar değerler için yaygın bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-430">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="6dd8e-431">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-431">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-432">Başka bir null olmayan değer ayarlarsanız anahtar özellikleri oluşturulan değerler kullanması gerektiğini açıkça belirterek öncesi 3.0 davranış elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-432">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="6dd8e-433">Örneğin, fluent API'si ile:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-433">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="6dd8e-434">Veya veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-434">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="6dd8e-435">Kapsamlı bir hizmet Iloggerfactory sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="6dd8e-435">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="6dd8e-436">İzleme sorun #14698</span><span class="sxs-lookup"><span data-stu-id="6dd8e-436">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="6dd8e-437">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-437">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-438">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-438">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-439">EF Core 3.0 önce `ILoggerFactory` tek bir hizmet kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-439">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="6dd8e-440">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-440">**New behavior**</span></span>

<span data-ttu-id="6dd8e-441">EF Core 3.0 ile başlayan `ILoggerFactory` şimdi kapsamlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-441">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="6dd8e-442">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-442">**Why**</span></span>

<span data-ttu-id="6dd8e-443">Günlükçü ile ilişkisini izin vermek için bu değişiklik yapılmıştır bir `DbContext` diğer işlevleri sağlar ve bazı durumlarda pathological davranış gibi iç hizmet sağlayıcıları sayısındaki kaldıran örneği.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-443">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="6dd8e-444">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-444">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-445">Bu değişiklik, kaydetme ve EF Core iç hizmet sağlayıcısına özel hizmetler kullanarak sürece uygulama kodu etkilememesi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-445">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="6dd8e-446">Bu ortak değildir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-446">This isn't common.</span></span>
<span data-ttu-id="6dd8e-447">Bu durumlarda, bilgilerin çoğunu hala çalışır, ancak bağlı olarak herhangi bir tekil hizmeti olacak `ILoggerFactory` almak için değiştirilmesi gerekecektir `ILoggerFactory` farklı bir yolla.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-447">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="6dd8e-448">Bu gibi durumlarda karşılaşırsanız lütfen sırasında bir sorun üzerinde dosya [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) nasıl kullandığınızı bize bildirmek `ILoggerFactory` sağlayacak şekilde biz bunu gelecekte kesmemesi nasıl daha iyi anlamak.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-448">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="6dd8e-449">Gezinti özellikleri tam olarak yüklü olduğundan yükleme Lazy proxy'leri artık varsayılır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-449">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="6dd8e-450">İzleme sorun #12780</span><span class="sxs-lookup"><span data-stu-id="6dd8e-450">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="6dd8e-451">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-451">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-452">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-452">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-453">EF Core 3.0, bir kez önce bir `DbContext` bırakıldı veya bir belirtilen gezinme özelliğinin bir varlığı söz konusu bağlamdan elde tam yüklü ise, olduğunu bilmesinin imkanı yoktu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-453">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="6dd8e-454">Proxy'leri bunun yerine null olmayan bir değer varsa bir başvuru Gezinti yüklenir ve boş değilse, bir koleksiyon Gezinti yüklenen varsayılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-454">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="6dd8e-455">Bu durumlarda, yavaş dışarıdan yüklemeyi deneyen bir İşlemsiz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-455">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="6dd8e-456">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-456">**New behavior**</span></span>

<span data-ttu-id="6dd8e-457">EF Core 3.0 ile başlayarak, proxy'leri bir gezinti özelliğinin yüklü olup olmadığını izler.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-457">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="6dd8e-458">Bu yüklenen Gezinti boş veya null olduğunda bile bağlamı bırakıldıktan sonra yüklenen bir gezinti özelliği erişmeye çalışan her zaman bir İşlemsiz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-458">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="6dd8e-459">Buna karşılık, boş bir koleksiyon gezinme özelliği olsa bile bağlamı elden yüklü olmayan bir gezinti özelliği erişmeye çalışan bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-459">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="6dd8e-460">Bu durum ortaya çıkarsa, uygulama kodu, geçersiz bir zamanda yavaş yükleme kullanmak çalışıyor ve bunu yapmak için uygulama değiştirilmelidir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-460">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="6dd8e-461">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-461">**Why**</span></span>

<span data-ttu-id="6dd8e-462">Bu değişiklik, bırakılmış yükü yavaş çalışırken davranışını tutarlı ve doğru hale yapılmıştır `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-462">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="6dd8e-463">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-463">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-464">Uygulama kodunun yükleme yavaş bırakılmış bağlamla kullanmamanız güncelleştirin veya bu özel durum iletisini açıklandığı bir İşlemsiz olacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-464">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="6dd8e-465">İç hizmet sağlayıcılarının aşırı oluşturma artık varsayılan olarak bir hata</span><span class="sxs-lookup"><span data-stu-id="6dd8e-465">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="6dd8e-466">İzleme sorun #10236</span><span class="sxs-lookup"><span data-stu-id="6dd8e-466">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="6dd8e-467">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-467">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-468">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-468">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-469">EF Core 3.0 önce bir uyarı iç hizmet sağlayıcıları pathological bir dizi oluşturmak için bir uygulama oturum açmış olmanız.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-469">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="6dd8e-470">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-470">**New behavior**</span></span>

<span data-ttu-id="6dd8e-471">EF Core 3.0 ile başlayarak, bu uyarı artık olarak kabul edilir ve hata ve özel durum harekete geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-471">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="6dd8e-472">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-472">**Why**</span></span>

<span data-ttu-id="6dd8e-473">Bu değişiklik, daha açık pathological bu durumda gösterme yoluyla uygulama kodu daha iyi desteklemek üzere yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-473">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="6dd8e-474">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-474">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-475">Bu hata ile karşılaşma eylemde en uygun nedeni, kök nedenini anlamak ve çok sayıda iç hizmet sağlayıcıları oluşturmayı durdur oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-475">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="6dd8e-476">Ancak, hata bir uyarı olarak geri dönüştürülür (veya yok sayıldı) aracılığıyla yapılandırma `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-476">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="6dd8e-477">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-477">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="6dd8e-478">Tek bir dize ile HasOne/çok ilişkin yeni davranış çağırılır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-478">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="6dd8e-479">İzleme sorun #9171</span><span class="sxs-lookup"><span data-stu-id="6dd8e-479">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="6dd8e-480">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-481">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-481">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-482">EF Core 3.0 önce kod arama `HasOne` veya `HasMany` tek bir dize ile kafa karıştırıcı bir şekilde yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-482">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="6dd8e-483">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-483">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="6dd8e-484">Kod ilgili Görülüyor `Samurai` bazı diğer varlık türü kullanarak `Entrance` özel gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-484">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="6dd8e-485">Bazı varlık türü olarak adlandırılan bir ilişki oluşturmak bu kodu gerçekte çalışır `Entrance` ile gezinti özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-485">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="6dd8e-486">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-486">**New behavior**</span></span>

<span data-ttu-id="6dd8e-487">EF Core 3.0 ile başlayarak, yukarıdaki kod artık önce yapmakta gibi hangi Aranan yapar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-487">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="6dd8e-488">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-488">**Why**</span></span>

<span data-ttu-id="6dd8e-489">Eski davranışı özellikle yapılandırma kodu okurken ve hatalarını arayarak çok karmaşık.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-489">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="6dd8e-490">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-490">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-491">Bu, yalnızca tür adları ve gezinme özelliğini açıkça belirtmeden dizeleriyle ilişkileri açıkça yapılandırmakta olduğunuz uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-491">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="6dd8e-492">Bu yaygın değildir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-492">This is not common.</span></span>
<span data-ttu-id="6dd8e-493">Önceki davranışı açıkça geçirme aracılığıyla alınabilir `null` gezinme özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-493">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="6dd8e-494">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-494">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="6dd8e-495">Birden çok zaman uyumsuz yöntemler için dönüş türü için ValueTask görevden değiştirildi</span><span class="sxs-lookup"><span data-stu-id="6dd8e-495">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="6dd8e-496">İzleme sorun #15184</span><span class="sxs-lookup"><span data-stu-id="6dd8e-496">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="6dd8e-497">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-497">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-498">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-498">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-499">Aşağıdaki zaman uyumsuz yöntemler daha önce döndürülen bir `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-499">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="6dd8e-500">`ValueGenerator.NextValueAsync()` (ve türetilen sınıflar)</span><span class="sxs-lookup"><span data-stu-id="6dd8e-500">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="6dd8e-501">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-501">**New behavior**</span></span>

<span data-ttu-id="6dd8e-502">Yukarıda sözü edilen yöntemleri artık döndürür bir `ValueTask<T>` aynı üzerinden `T` önceki gibi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-502">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="6dd8e-503">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-503">**Why**</span></span>

<span data-ttu-id="6dd8e-504">Bu değişiklik, yığın ayırmaları genel performansını iyileştirme, bu yöntemleri çağrılırken tahakkuk sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-504">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="6dd8e-505">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-505">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-506">Yalnızca yukarıdaki API'leri yalnızca bekleyen gerekir derlenmesi için - uygulamalar kaynak değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-506">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="6dd8e-507">Daha karmaşık bir kullanım (örneğin döndürülen geçirme `Task` için `Task.WhenAny()`) genellikle gerektiren döndürülen `ValueTask<T>` dönüştürülmesi bir `Task<T>` çağırarak `AsTask()` üzerindeki.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-507">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="6dd8e-508">Bu bu değişiklik getirir ayırma azaltma verilerek unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-508">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="6dd8e-509">İlişkisel: TypeMapping ek açıklama yalnızca TypeMapping sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="6dd8e-509">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="6dd8e-510">İzleme sorun #9913</span><span class="sxs-lookup"><span data-stu-id="6dd8e-510">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="6dd8e-511">Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-511">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="6dd8e-512">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-512">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-513">Tür eşlemesi ek açıklamaları için ek açıklama "İlişkisel: TypeMapping" adlandırılmıştı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-513">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="6dd8e-514">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-514">**New behavior**</span></span>

<span data-ttu-id="6dd8e-515">Ek açıklama türü eşleme ek açıklamalar için artık "TypeMapping" addır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-515">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="6dd8e-516">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-516">**Why**</span></span>

<span data-ttu-id="6dd8e-517">Türü eşlemeleri artık birden fazla yalnızca ilişkisel veritabanı sağlayıcıları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-517">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="6dd8e-518">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-518">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-519">Bu, yalnızca tür eşlemesi ortak olmayan doğrudan bir ek açıklama, olarak erişen uygulamalar keser.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-519">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="6dd8e-520">API yüzeyi için ek açıklama doğrudan kullanmak yerine erişim türü eşlemeleri düzeltmek için en uygun eylemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-520">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="6dd8e-521">Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-521">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="6dd8e-522">İzleme sorun #11811</span><span class="sxs-lookup"><span data-stu-id="6dd8e-522">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="6dd8e-523">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-523">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-524">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-524">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-525">EF Core 3.0 önce `ToTable()` türetilmiş üzerinde adlı türü yoksayılan beri tek devralma eşleme stratejisi TPH burada bu geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-525">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="6dd8e-526">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-526">**New behavior**</span></span>

<span data-ttu-id="6dd8e-527">EF Core 3.0 ve sonraki bir sürümde TPT ve TPC desteği eklemek için hazırlık başlangıç `ToTable()` beklenmeyen eşleme değişiklik gelecekte önlemek için bir özel durum bir türetilmiş tür şimdi throw üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-527">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="6dd8e-528">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-528">**Why**</span></span>

<span data-ttu-id="6dd8e-529">Şu anda farklı bir tabloya türetilmiş bir tür eşlemek için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-529">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="6dd8e-530">Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda bozucu önler.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-530">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="6dd8e-531">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-531">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-532">Türetilen türlerin diğer tablolara eşleme girişimleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-532">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="6dd8e-533">ForSqlServerHasIndex HasIndex ile değiştirildi</span><span class="sxs-lookup"><span data-stu-id="6dd8e-533">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="6dd8e-534">İzleme sorun #12366</span><span class="sxs-lookup"><span data-stu-id="6dd8e-534">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="6dd8e-535">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-535">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-536">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-536">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-537">EF Core 3.0 önce `ForSqlServerHasIndex().ForSqlServerInclude()` ile kullanılan sütunları yapılandırmak üzere bir yöntem sağlayan `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-537">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="6dd8e-538">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-538">**New behavior**</span></span>

<span data-ttu-id="6dd8e-539">EF Core 3.0 ile başlayarak, kullanarak `Include` dizin ilişkisel düzeyinde artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-539">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="6dd8e-540">Kullanım `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-540">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="6dd8e-541">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-541">**Why**</span></span>

<span data-ttu-id="6dd8e-542">API ile dizin için birleştirmek için bu değişiklik yapılmıştır `Include` tüm sağlayıcıları veritabanı için içine yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-542">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="6dd8e-543">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-543">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-544">Yukarıda da gösterildiği gibi yeni API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-544">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="6dd8e-545">Meta veri API'si değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="6dd8e-545">Metadata API changes</span></span>

[<span data-ttu-id="6dd8e-546">İzleme sorun #214</span><span class="sxs-lookup"><span data-stu-id="6dd8e-546">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="6dd8e-547">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-548">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-548">**New behavior**</span></span>

<span data-ttu-id="6dd8e-549">Aşağıdaki özellikler için genişletme yöntemleri dönüştürüldü:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-549">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="6dd8e-550">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-550">**Why**</span></span>

<span data-ttu-id="6dd8e-551">Bu değişiklik, yukarıda sözü edilen arabirimleri uygulamasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-551">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="6dd8e-552">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-552">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-553">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-553">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="6dd8e-554">Sağlayıcıya özgü meta veriler API'SİNDEKİ değişiklikler</span><span class="sxs-lookup"><span data-stu-id="6dd8e-554">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="6dd8e-555">İzleme sorun #214</span><span class="sxs-lookup"><span data-stu-id="6dd8e-555">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="6dd8e-556">Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-556">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="6dd8e-557">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-557">**New behavior**</span></span>

<span data-ttu-id="6dd8e-558">Sağlayıcıya özgü genişletme yöntemlerini düzleştirilmiş:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-558">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="6dd8e-559">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-559">**Why**</span></span>

<span data-ttu-id="6dd8e-560">Bu değişiklik, yukarıda sözü edilen genişletme yöntemleri uygulamasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-560">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="6dd8e-561">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-561">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-562">Yeni uzantı yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-562">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="6dd8e-563">EF Core artık SQLite FK zorlama pragma gönderir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-563">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="6dd8e-564">İzleme sorun #12151</span><span class="sxs-lookup"><span data-stu-id="6dd8e-564">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="6dd8e-565">Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-565">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="6dd8e-566">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-566">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-567">EF Core 3.0 önce EF Core gönderir gibi `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-567">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="6dd8e-568">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-568">**New behavior**</span></span>

<span data-ttu-id="6dd8e-569">EF Core 3.0 ile başlayarak, EF Core artık gönderir `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-569">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="6dd8e-570">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-570">**Why**</span></span>

<span data-ttu-id="6dd8e-571">EF Core kullandığından bu değişiklik yapılmıştır `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak, hangi sırayla anlamına gelir FK zorlama varsayılan olarak açık ve açıkça bir bağlantı her açıldığında etkinleştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-571">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="6dd8e-572">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-572">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-573">Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-573">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="6dd8e-574">Diğer durumlarda, yabancı anahtarlar belirterek etkinleştirilebilir `Foreign Keys=True` bağlantı dizenizi içinde.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-574">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="6dd8e-575">Microsoft.EntityFrameworkCore.Sqlite üzerinde SQLitePCLRaw.bundle_e_sqlite3 artık bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-575">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="6dd8e-576">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-576">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-577">EF Core 3.0 önce EF Core kullanılan `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-577">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="6dd8e-578">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-578">**New behavior**</span></span>

<span data-ttu-id="6dd8e-579">EF Core 3.0 ile başlayarak, EF Core kullanan `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-579">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="6dd8e-580">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-580">**Why**</span></span>

<span data-ttu-id="6dd8e-581">Diğer platformlar ile tutarlı ios'ta kullanılan SQLite sürümü, bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-581">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="6dd8e-582">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-582">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-583">İos'ta yerel bir SQLite sürümü kullanmak için yapılandırma `Microsoft.Data.Sqlite` farklı bir kullanılacak `SQLitePCLRaw` paket.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-583">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="6dd8e-584">GUID değerlerinin artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-584">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="6dd8e-585">İzleme sorun #15078</span><span class="sxs-lookup"><span data-stu-id="6dd8e-585">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="6dd8e-586">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-586">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-587">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-587">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-588">GUID değerleri, daha önce SQLite değerlerine BLOB olarak sored.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-588">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="6dd8e-589">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-589">**New behavior**</span></span>

<span data-ttu-id="6dd8e-590">GUID değerleri, artık metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-590">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="6dd8e-591">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-591">**Why**</span></span>

<span data-ttu-id="6dd8e-592">İkili biçimi GUID'leri standartlaştırılmış değil.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-592">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="6dd8e-593">Değerlerin metin olarak depolanması veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-593">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="6dd8e-594">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-594">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-595">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-595">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="6dd8e-596">EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-596">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="6dd8e-597">Microsoft.Data.Sqlite GUID değerlerinin hem BLOB hem de metin sütunları okuma özellikli kalır; Parametreler ve sabitleri için varsayılan biçimi değiştiğinden ancak büyük olasılıkla GUID'leri içeren çoğu senaryo için bir eylem yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-597">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="6dd8e-598">Char değerleri artık SQLite metin olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="6dd8e-598">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="6dd8e-599">İzleme sorun #15020</span><span class="sxs-lookup"><span data-stu-id="6dd8e-599">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="6dd8e-600">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-600">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-601">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-601">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-602">Char değerleri daha önce SQLite tamsayı değerleri olarak sored.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-602">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="6dd8e-603">Örneğin, bir karakter değerini *A* 65 tamsayı değeri depolanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-603">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="6dd8e-604">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-604">**New behavior**</span></span>

<span data-ttu-id="6dd8e-605">Char değerleri artık metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-605">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="6dd8e-606">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-606">**Why**</span></span>

<span data-ttu-id="6dd8e-607">Değerlerin metin olarak depolanması daha doğal ve veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-607">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="6dd8e-608">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-608">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-609">Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-609">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="6dd8e-610">EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-610">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="6dd8e-611">Microsoft.Data.Sqlite Ayrıca belirli senaryoları herhangi bir eylem gerekli değil şekilde karakter değerleri hem tamsayı hem de metin sütunları, okuma özellikli kalır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-611">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="6dd8e-612">Geçiş kimlikleri, artık sabit kültürün takvimini kullanarak oluşturulur</span><span class="sxs-lookup"><span data-stu-id="6dd8e-612">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="6dd8e-613">İzleme sorun #12978</span><span class="sxs-lookup"><span data-stu-id="6dd8e-613">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="6dd8e-614">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-614">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-615">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-615">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-616">Geçiş kimlikleri, geçerli kültürün takvimini kullanarak yanlışlıkla üretildi.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-616">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="6dd8e-617">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-617">**New behavior**</span></span>

<span data-ttu-id="6dd8e-618">Geçiş kimlikleri artık her zaman sabit kültürün takvimini (Gregoryen) kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-618">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="6dd8e-619">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-619">**Why**</span></span>

<span data-ttu-id="6dd8e-620">Geçiş sırası önemlidir veritabanını güncelleştirmek ya da birleştirme çakışmalarını çözme.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-620">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="6dd8e-621">Sabit takvimi kullanılarak sıralama ekip üyelerinden farklı sistem takvimler sahip sonuçlanabilen sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-621">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="6dd8e-622">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-622">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-623">Bu değişiklik, olmayan-Gregoryen takvim yılı Gregoryen takvimini (içeren Thai Budist takvimi gibi) daha büyük olduğu kullanan herkesin etkiler.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-623">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="6dd8e-624">Mevcut geçiş kimlikleri böylece mevcut sonra yeni geçişleri sıralı güncelleştirilmesi gerekiyor geçişler.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-624">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="6dd8e-625">Geçiş kimliği geçiş özniteliğinde geçişler Tasarımcı dosyalarında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-625">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="6dd8e-626">Geçişleri geçmiş tablosu da güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-626">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="6dd8e-627">Uzantı bilgileri/meta verileri IDbContextOptionsExtension kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="6dd8e-627">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="6dd8e-628">İzleme sorun #16119</span><span class="sxs-lookup"><span data-stu-id="6dd8e-628">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="6dd8e-629">Bu değişiklik, EF Core 3.0-Önizleme 7 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-629">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="6dd8e-630">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-630">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-631">`IDbContextOptionsExtension` uzantı hakkında meta veri sağlamak için yöntemleri içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-631">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="6dd8e-632">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-632">**New behavior**</span></span>

<span data-ttu-id="6dd8e-633">Bu yöntemler, yeni bir taşınmış `DbContextOptionsExtensionInfo` soyut temel sınıf yeni bir döndürülen `IDbContextOptionsExtension.Info` özelliği.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-633">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="6dd8e-634">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-634">**Why**</span></span>

<span data-ttu-id="6dd8e-635">2.0 sürümlerinden 3.0 üzerinden ekleyin veya bu yöntemleri birkaç kez değiştirmek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-635">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="6dd8e-636">Yeni bir soyut temel sınıf bozucu mevcut uzantılar bozup olmadan bu tür değişiklikler yapmaya kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-636">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="6dd8e-637">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-637">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-638">Yeni desende uzantıları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-638">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="6dd8e-639">Örnekler birçok uygulamalarında bulunan `IDbContextOptionsExtension` EF Core uzantılarında farklı türleri için kaynak kodu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-639">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="6dd8e-640">LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="6dd8e-640">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="6dd8e-641">İzleme sorun #10985</span><span class="sxs-lookup"><span data-stu-id="6dd8e-641">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="6dd8e-642">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-642">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-643">**Değişiklik**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-643">**Change**</span></span>

<span data-ttu-id="6dd8e-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` yeniden adlandırıldı `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="6dd8e-645">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-645">**Why**</span></span>

<span data-ttu-id="6dd8e-646">Bu uyarı olayı tüm uyarı olayları ile adlandırma hizalar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-646">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="6dd8e-647">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-647">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-648">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-648">Use the new name.</span></span> <span data-ttu-id="6dd8e-649">(Olay kimliği numarasını değiştirilmedi unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="6dd8e-649">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="6dd8e-650">API açıklığa kavuşturmak için yabancı anahtar kısıtlaması adları</span><span class="sxs-lookup"><span data-stu-id="6dd8e-650">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="6dd8e-651">İzleme sorun #10730</span><span class="sxs-lookup"><span data-stu-id="6dd8e-651">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="6dd8e-652">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-652">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-653">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-653">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-654">EF Core 3.0 önce yabancı anahtar kısıtlaması adları için yalnızca "adı olarak" adı veriliyordu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-654">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="6dd8e-655">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-655">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="6dd8e-656">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-656">**New behavior**</span></span>

<span data-ttu-id="6dd8e-657">EF Core 3.0 ile başlayarak, yabancı anahtar kısıtlaması adları şimdi de "kısıtlama adı" adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-657">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="6dd8e-658">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6dd8e-658">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="6dd8e-659">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-659">**Why**</span></span>

<span data-ttu-id="6dd8e-660">Bu değişiklik, bu alanda adlandırma için tutarlılık getirir ve ayrıca bu yabancı anahtarı üzerinde tanımlanan yabancı anahtar kısıtlaması ve olmayan sütun veya özellik adı adı olduğunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-660">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="6dd8e-661">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-661">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-662">Yeni bir ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-662">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="6dd8e-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync yapıldı genel</span><span class="sxs-lookup"><span data-stu-id="6dd8e-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="6dd8e-664">İzleme sorun #15997</span><span class="sxs-lookup"><span data-stu-id="6dd8e-664">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="6dd8e-665">Bu değişiklik, EF Core 3.0-Önizleme 7 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-665">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="6dd8e-666">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-666">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-667">EF Core 3.0 önce bu yöntemleri korunmuş.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-667">Before EF Core 3.0, these methods were protected.</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="6dd8e-668">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-668">**New behavior**</span></span>

<span data-ttu-id="6dd8e-669">EF Core 3.0 ile başlayarak, bu yöntemleri herkese açık.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-669">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="6dd8e-670">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-670">**Why**</span></span>

<span data-ttu-id="6dd8e-671">Bu yöntemler, bir veritabanı oluşturuldu ancak boş olup olmadığını belirlemek için EF tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-671">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="6dd8e-672">Ayrıca geçişleri uygulanacağını belirleyen olduğunda bu EF dışında yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-672">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="6dd8e-673">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-673">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-674">Herhangi bir geçersiz kılma erişilebilirliğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-674">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="6dd8e-675">Microsoft.EntityFrameworkCore.Design artık DevelopmentDependency pakettir</span><span class="sxs-lookup"><span data-stu-id="6dd8e-675">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="6dd8e-676">İzleme sorun #11506</span><span class="sxs-lookup"><span data-stu-id="6dd8e-676">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="6dd8e-677">Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6dd8e-678">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-678">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-679">EF Core 3.0 önce derleme, ona bağımlı olan projeler tarafından başvurulan, normal bir NuGet paketi Microsoft.EntityFrameworkCore.Design oluştu.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-679">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="6dd8e-680">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-680">**New behavior**</span></span>

<span data-ttu-id="6dd8e-681">EF Core 3.0 ile başlayarak, bunu bir DevelopmentDependency paketidir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-681">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="6dd8e-682">Bağımlılık geçişli diğer projelere akış olmaz ve, varsayılan olarak, artık anlamına gelir, bütünleştirilmiş koda başvurun.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-682">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="6dd8e-683">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-683">**Why**</span></span>

<span data-ttu-id="6dd8e-684">Bu paket, yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-684">This package is only intended to be used at design time.</span></span> <span data-ttu-id="6dd8e-685">Dağıtılan uygulamalar ona başvuran olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-685">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="6dd8e-686">Paket bir DevelopmentDependency yaparak bu öneriyi çalışacak şekilde güçlendirir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-686">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="6dd8e-687">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-687">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-688">Bu paketi, EF Core'nın tasarım zamanı davranışını geçersiz kılmak için başvurmanız gerekiyorsa, güncelleştirme güncelleştirebilirsiniz PackageReference öğe meta verileri, projenizdeki.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-688">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="6dd8e-689">Paketi Microsoft.EntityFrameworkCore.Tools geçişli başvuruluyor, açık bir PackageReference meta verilerini değiştirmek için paketi eklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-689">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="6dd8e-690">SQLitePCL.raw 2.0.0 sürümüne güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="6dd8e-690">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="6dd8e-691">İzleme sorun #14824</span><span class="sxs-lookup"><span data-stu-id="6dd8e-691">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="6dd8e-692">Bu değişiklik, EF Core 3.0-Önizleme 7 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-692">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="6dd8e-693">**Eski davranışı**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-693">**Old behavior**</span></span>

<span data-ttu-id="6dd8e-694">Microsoft.EntityFrameworkCore.Sqlite önceden SQLitePCL.raw sürüm 1.1.12 bağlıydı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-694">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="6dd8e-695">**Yeni davranış**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-695">**New behavior**</span></span>

<span data-ttu-id="6dd8e-696">Olduğunuz güncelleştiriyoruz bizim paketini 2.0.0 sürümüne bağlı.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-696">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="6dd8e-697">**Neden**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-697">**Why**</span></span>

<span data-ttu-id="6dd8e-698">.NET Standard 2.0 SQLitePCL.raw 2.0.0 sürümünü hedefler.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-698">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="6dd8e-699">Daha önce .NET standart, geçişli paket çalışmak için büyük bir kapanış gerekli 1.1 hedeflenen.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-699">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="6dd8e-700">**Risk azaltma işlemleri**</span><span class="sxs-lookup"><span data-stu-id="6dd8e-700">**Mitigations**</span></span>

<span data-ttu-id="6dd8e-701">SQLitePCL.raw sürüm 2.0.0 bazı önemli değişiklikler içerir.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-701">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="6dd8e-702">Bkz: [sürüm notları](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="6dd8e-702">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>
