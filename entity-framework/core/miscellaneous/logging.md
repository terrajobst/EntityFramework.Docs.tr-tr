---
title: Oturum açma - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985580"
---
# <a name="logging"></a><span data-ttu-id="5e8eb-102">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="5e8eb-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="5e8eb-103">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="5e8eb-104">ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="5e8eb-104">ASP.NET Core applications</span></span>

<span data-ttu-id="5e8eb-105">EF Core ile ASP.NET Core günlüğe kaydetme sistemleri otomatik olarak tümleştirilir her `AddDbContext` veya `AddDbContextPool` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="5e8eb-106">Bu nedenle, ASP.NET Core kullanırken, günlüğe kaydetme bölümünde anlatıldığı gibi yapılandırılmalıdır [ASP.NET Core belgeleri](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="5e8eb-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="5e8eb-107">Diğer uygulamalar</span><span class="sxs-lookup"><span data-stu-id="5e8eb-107">Other applications</span></span>

<span data-ttu-id="5e8eb-108">Şu anda oturum EF Core kendisi ile bir veya daha fazla ILoggerProvider yapılandırılmış olan bir Iloggerfactory gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="5e8eb-109">Ortak sağlayıcıları aşağıdaki paketlerde gönderilir:</span><span class="sxs-lookup"><span data-stu-id="5e8eb-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="5e8eb-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Basit bir konsol Günlükçü.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="5e8eb-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure uygulama Hizmetleri 'Tanılama günlükleri' ve 'akış oturum' özelliklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="5e8eb-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Bir hata ayıklayıcı günlükleri System.Diagnostics.Debug.WriteLine() kullanarak izleyin.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="5e8eb-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows olay günlüğüne günlükleri.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="5e8eb-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener destekler.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="5e8eb-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Günlükleri System.Diagnostics.TraceSource.TraceEvent() kullanarak bir izleme dinleyicisi.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="5e8eb-116">Aşağıdaki kod örneği kullanan bir `ConsoleLoggerProvider` 2.2 sürümünde geçersiz Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="5e8eb-117">Eski günlük API'leri için doğru değiştirmeler 3.0 sürümünde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="5e8eb-118">Bu arada, yoksaymak ve uyarıların görünmemesi güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="5e8eb-119">Uygun paket yüklendikten sonra uygulamayı bir LoggerFactory tekil/genel bir örneğini oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="5e8eb-120">Örneğin, konsol günlüğe kullanarak:</span><span class="sxs-lookup"><span data-stu-id="5e8eb-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="5e8eb-121">Bu singleton/genel örnek ardından EF Core ile şirket kaydedilmelidir `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="5e8eb-122">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e8eb-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="5e8eb-123">Uygulamalar her bağlam örneği için yeni bir Iloggerfactory örnek oluşturmayın çok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="5e8eb-124">Bunun yapılması, bir bellek sızıntısı ve performansın düşmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="5e8eb-125">Günlüğe kaydedilenler filtreleme</span><span class="sxs-lookup"><span data-stu-id="5e8eb-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="5e8eb-126">Aşağıdaki kod örneği kullanan bir `ConsoleLoggerProvider` 2.2 sürümünde geçersiz Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="5e8eb-127">Eski günlük API'leri için doğru değiştirmeler 3.0 sürümünde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="5e8eb-128">Bu arada, yoksaymak ve uyarıların görünmemesi güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="5e8eb-129">Günlüğe kaydedilenler filtrelemek için en kolay yolu ILoggerProvider kaydederken yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="5e8eb-130">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e8eb-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="5e8eb-131">Bu örnekte, günlük, yalnızca iletileri döndürmek için filtrelenmiştir:</span><span class="sxs-lookup"><span data-stu-id="5e8eb-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="5e8eb-132">'Microsoft.EntityFrameworkCore.Database.Command' kategorisinde</span><span class="sxs-lookup"><span data-stu-id="5e8eb-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="5e8eb-133">'Bilgi' düzeyinde</span><span class="sxs-lookup"><span data-stu-id="5e8eb-133">at the 'Information' level</span></span>

<span data-ttu-id="5e8eb-134">EF Core için Günlükçü kategorileri içinde tanımlanan `DbLoggerCategory` kategorileri, ancak bunlar bulmayı kolaylaştırmak için sınıf çözümlemek için basit dizeler.</span><span class="sxs-lookup"><span data-stu-id="5e8eb-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="5e8eb-135">Altyapının günlüğe kaydetme hakkında daha fazla ayrıntı bulunabilir [ASP.NET Core günlük belgeleri](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="5e8eb-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
