---
title: Oturum açma - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 65501b5ac03ae544c51b7fc1a07fa9eea849f1e3
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022151"
---
# <a name="logging"></a><span data-ttu-id="98398-102">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="98398-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="98398-103">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="98398-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="98398-104">ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="98398-104">ASP.NET Core applications</span></span>

<span data-ttu-id="98398-105">EF Core ile ASP.NET Core günlüğe kaydetme sistemleri otomatik olarak tümleştirilir her `AddDbContext` veya `AddDbContextPool` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98398-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="98398-106">Bu nedenle, ASP.NET Core kullanırken, günlüğe kaydetme bölümünde anlatıldığı gibi yapılandırılmalıdır [ASP.NET Core belgeleri](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="98398-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="98398-107">Diğer uygulamalar</span><span class="sxs-lookup"><span data-stu-id="98398-107">Other applications</span></span>

<span data-ttu-id="98398-108">Şu anda oturum EF Core kendisi ile bir veya daha fazla ILoggerProvider yapılandırılmış olan bir Iloggerfactory gerektirir.</span><span class="sxs-lookup"><span data-stu-id="98398-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="98398-109">Ortak sağlayıcıları aşağıdaki paketlerde gönderilir:</span><span class="sxs-lookup"><span data-stu-id="98398-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="98398-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): basit bir konsol Günlükçü.</span><span class="sxs-lookup"><span data-stu-id="98398-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="98398-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): destekleyen Azure uygulama Hizmetleri 'Tanılama günlükleri' ve 'akış oturum' özellikleri.</span><span class="sxs-lookup"><span data-stu-id="98398-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="98398-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.Diagnostics.Debug.WriteLine() kullanarak bir hata ayıklayıcı izleme günlükleri.</span><span class="sxs-lookup"><span data-stu-id="98398-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="98398-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows olay günlüğüne günlükleri.</span><span class="sxs-lookup"><span data-stu-id="98398-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="98398-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener destekler.</span><span class="sxs-lookup"><span data-stu-id="98398-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="98398-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): System.Diagnostics.TraceSource.TraceEvent() kullanarak bir izleme dinleyicisi için günlükleri.</span><span class="sxs-lookup"><span data-stu-id="98398-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="98398-116">Uygun paket yüklendikten sonra uygulamayı bir LoggerFactory tekil/genel bir örneğini oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="98398-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="98398-117">Örneğin, konsol günlüğe kullanarak:</span><span class="sxs-lookup"><span data-stu-id="98398-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="98398-118">Bu singleton/genel örnek ardından EF Core ile şirket kaydedilmelidir `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="98398-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="98398-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="98398-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="98398-120">Uygulamalar her bağlam örneği için yeni bir Iloggerfactory örnek oluşturmayın çok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="98398-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="98398-121">Bunun yapılması, bir bellek sızıntısı ve performansın düşmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="98398-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="98398-122">Günlüğe kaydedilenler filtreleme</span><span class="sxs-lookup"><span data-stu-id="98398-122">Filtering what is logged</span></span>

<span data-ttu-id="98398-123">Günlüğe kaydedilenler filtrelemek için en kolay yolu ILoggerProvider kaydederken yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="98398-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="98398-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="98398-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="98398-125">Bu örnekte, günlük, yalnızca iletileri döndürmek için filtrelenmiştir:</span><span class="sxs-lookup"><span data-stu-id="98398-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="98398-126">'Microsoft.EntityFrameworkCore.Database.Command' kategorisinde</span><span class="sxs-lookup"><span data-stu-id="98398-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="98398-127">'Bilgi' düzeyinde</span><span class="sxs-lookup"><span data-stu-id="98398-127">at the 'Information' level</span></span>

<span data-ttu-id="98398-128">EF Core için Günlükçü kategorileri içinde tanımlanan `DbLoggerCategory` kategorileri, ancak bunlar bulmayı kolaylaştırmak için sınıf çözümlemek için basit dizeler.</span><span class="sxs-lookup"><span data-stu-id="98398-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="98398-129">Altyapının günlüğe kaydetme hakkında daha fazla ayrıntı bulunabilir [ASP.NET Core günlük belgeleri](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="98398-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
