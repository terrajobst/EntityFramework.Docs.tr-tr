---
title: Günlüğü - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232142"
---
# <a name="logging"></a><span data-ttu-id="1ffa1-102">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="1ffa1-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="1ffa1-103">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) github'da.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="1ffa1-104">ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="1ffa1-104">ASP.NET Core applications</span></span>

<span data-ttu-id="1ffa1-105">EF çekirdek tümleştirir otomatik olarak ASP.NET Core günlük mekanizmalarıyla her `AddDbContext` veya `AddDbContextPool` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="1ffa1-106">Bu nedenle, ASP.NET Core kullanırken, günlük açıklandığı gibi yapılandırılmalıdır [ASP.NET Core belgeleri](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="1ffa1-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="1ffa1-107">Diğer uygulamalar</span><span class="sxs-lookup"><span data-stu-id="1ffa1-107">Other applications</span></span>

<span data-ttu-id="1ffa1-108">Şu anda oturum EF çekirdek kendisini bir veya daha fazla ILoggerProvider ile yapılandırılmış olan bir Iloggerfactory gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="1ffa1-109">Ortak sağlayıcıları aşağıdaki paketlerde gönderilir:</span><span class="sxs-lookup"><span data-stu-id="1ffa1-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="1ffa1-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): basit bir konsol Günlükçü.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="1ffa1-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): destekler Azure uygulama Hizmetleri 'Tanılama günlükleri' ve 'akış günlük' özellikleri.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="1ffa1-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.Diagnostics.Debug.WriteLine() kullanarak bir hata ayıklayıcı izleme günlükleri.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="1ffa1-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows olay günlüğüne günlükleri.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="1ffa1-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener destekler.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="1ffa1-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): günlükleri System.Diagnostics.TraceSource.TraceEvent() kullanarak bir izleme dinleyicisi.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="1ffa1-116">Uygun paketleri yükledikten sonra uygulamayı bir LoggerFactory singleton/genel bir örneğini oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="1ffa1-117">Örneğin, konsol Günlükçü kullanma:</span><span class="sxs-lookup"><span data-stu-id="1ffa1-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="1ffa1-118">Bu singleton/genel örnek sonra EF çekirdek ile kayıtlı olması `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="1ffa1-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ffa1-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="1ffa1-120">Uygulamalar her bağlam örneği için yeni bir Iloggerfactory örnek oluşturmayın çok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="1ffa1-121">Bunun yapılması, bir bellek sızıntısı ve performansın neden olur.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="1ffa1-122">Günlüğe kaydedilenler filtreleme</span><span class="sxs-lookup"><span data-stu-id="1ffa1-122">Filtering what is logged</span></span>

<span data-ttu-id="1ffa1-123">Günlüğe kaydedilenler filtre uygulamak için en kolay yolu ILoggerProvider kaydedilirken yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="1ffa1-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ffa1-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="1ffa1-125">Bu örnekte, yalnızca iletilerini döndürmek için günlük filtre uygulanır:</span><span class="sxs-lookup"><span data-stu-id="1ffa1-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="1ffa1-126">'Microsoft.EntityFrameworkCore.Database.Command' kategorisinde</span><span class="sxs-lookup"><span data-stu-id="1ffa1-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="1ffa1-127">'Bilgi' düzeyinde</span><span class="sxs-lookup"><span data-stu-id="1ffa1-127">at the 'Information' level</span></span>

<span data-ttu-id="1ffa1-128">İçinde tanımlanan EF çekirdek için Günlükçü kategorileri `DbLoggerCategory` kategorileri, ancak bunlar bulmayı kolaylaştırmak için sınıf çözümlemek için basit dizeleri.</span><span class="sxs-lookup"><span data-stu-id="1ffa1-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="1ffa1-129">Günlüğe kaydetme altyapının hakkında daha fazla ayrıntı bulunabilir [ASP.NET Core günlük belgelerine](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="1ffa1-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
