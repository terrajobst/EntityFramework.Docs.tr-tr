---
title: Günlüğe kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: e8adc39ec01ff75112b03446a488df6199cc7041
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416584"
---
# <a name="logging"></a><span data-ttu-id="e95a9-102">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="e95a9-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="e95a9-103">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e95a9-103">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="e95a9-104">ASP.NET Core uygulamalar</span><span class="sxs-lookup"><span data-stu-id="e95a9-104">ASP.NET Core applications</span></span>

<span data-ttu-id="e95a9-105">EF Core, `AddDbContext` veya `AddDbContextPool` kullanıldığı her seferinde ASP.NET Core günlük mekanizmalarıyla otomatik olarak tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="e95a9-106">Bu nedenle, ASP.NET Core kullanırken günlüğe kaydetme, [ASP.NET Core belgelerinde](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)açıklandığı şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e95a9-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="e95a9-107">Diğer uygulamalar</span><span class="sxs-lookup"><span data-stu-id="e95a9-107">Other applications</span></span>

<span data-ttu-id="e95a9-108">EF Core günlüğü, kendisi bir veya daha fazla günlüğe kaydetme sağlayıcısıyla yapılandırılmış bir ıloggerfactory gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="e95a9-109">Ortak sağlayıcılar aşağıdaki paketlere gönderilir:</span><span class="sxs-lookup"><span data-stu-id="e95a9-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="e95a9-110">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): basit konsol günlükçüsü.</span><span class="sxs-lookup"><span data-stu-id="e95a9-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="e95a9-111">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure Uygulama Hizmetleri ' tanılama günlükleri ' ve ' günlük akışı ' özelliklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="e95a9-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="e95a9-112">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System. Diagnostics. Debug. WriteLine () kullanılarak bir hata ayıklayıcı izleyicisine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="e95a9-113">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows olay günlüğü 'ne kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="e95a9-114">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener 'ı destekler.</span><span class="sxs-lookup"><span data-stu-id="e95a9-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="e95a9-115">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): `System.Diagnostics.TraceSource.TraceEvent()`kullanarak bir izleme dinleyicisine kaydeder.</span><span class="sxs-lookup"><span data-stu-id="e95a9-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="e95a9-116">Uygun paketleri yükledikten sonra, uygulamanın bir LoggerFactory 'nin tek/genel örneğini oluşturması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="e95a9-117">Örneğin, konsol günlükçüsü kullanarak:</span><span class="sxs-lookup"><span data-stu-id="e95a9-117">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="e95a9-118">Sürüm 3. x</span><span class="sxs-lookup"><span data-stu-id="e95a9-118">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="e95a9-119">Sürüm 2. x</span><span class="sxs-lookup"><span data-stu-id="e95a9-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="e95a9-120">Aşağıdaki kod örneği, sürüm 2,2 ' de kullanımdan kaldırılmış ve 3,0 ' de değiştirilen bir `ConsoleLoggerProvider` oluşturucusunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="e95a9-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="e95a9-121">2,2 kullanılırken uyarıları yoksaymak ve gizlemek güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="e95a9-122">Bu tek/genel örnek daha sonra `DbContextOptionsBuilder`EF Core kaydedilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="e95a9-123">Örnek:</span><span class="sxs-lookup"><span data-stu-id="e95a9-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="e95a9-124">Uygulamaların her bir bağlam örneği için yeni bir ıloggerfactory örneği oluşturmaması çok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="e95a9-125">Bunun yapılması bellek sızıntısı ve düşük performansa neden olur.</span><span class="sxs-lookup"><span data-stu-id="e95a9-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="e95a9-126">Günlüğe kaydedilen filtreleme</span><span class="sxs-lookup"><span data-stu-id="e95a9-126">Filtering what is logged</span></span>

<span data-ttu-id="e95a9-127">Uygulama, ıloggerprovider üzerinde bir filtre yapılandırarak günlüğe nelerin kaydedildiğini denetleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="e95a9-128">Örnek:</span><span class="sxs-lookup"><span data-stu-id="e95a9-128">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="e95a9-129">Sürüm 3. x</span><span class="sxs-lookup"><span data-stu-id="e95a9-129">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="e95a9-130">Sürüm 2. x</span><span class="sxs-lookup"><span data-stu-id="e95a9-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="e95a9-131">Aşağıdaki kod örneği, sürüm 2,2 ' de kullanımdan kaldırılmış ve 3,0 ' de değiştirilen bir `ConsoleLoggerProvider` oluşturucusunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="e95a9-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="e95a9-132">2,2 kullanılırken uyarıları yoksaymak ve gizlemek güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

<span data-ttu-id="e95a9-133">Bu örnekte, günlük yalnızca iletileri döndürecek şekilde filtrelenmiştir:</span><span class="sxs-lookup"><span data-stu-id="e95a9-133">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="e95a9-134">' Microsoft. EntityFrameworkCore. Database. Command ' kategorisinde</span><span class="sxs-lookup"><span data-stu-id="e95a9-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="e95a9-135">' bilgi ' düzeyinde</span><span class="sxs-lookup"><span data-stu-id="e95a9-135">at the 'Information' level</span></span>

<span data-ttu-id="e95a9-136">EF Core için, günlükçü kategorileri, kategorileri bulmayı kolaylaştırmak için `DbLoggerCategory` sınıfında tanımlanır, ancak bunlar basit dizelere çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="e95a9-137">Temel alınan günlük altyapısı hakkında daha fazla ayrıntı [ASP.NET Core günlük belgelerinde](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="e95a9-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
