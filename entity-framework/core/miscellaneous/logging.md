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
# <a name="logging"></a>Günlüğe Kaydetme

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) github'da.

## <a name="aspnet-core-applications"></a>ASP.NET Core uygulamaları

EF çekirdek tümleştirir otomatik olarak ASP.NET Core günlük mekanizmalarıyla her `AddDbContext` veya `AddDbContextPool` kullanılır. Bu nedenle, ASP.NET Core kullanırken, günlük açıklandığı gibi yapılandırılmalıdır [ASP.NET Core belgeleri](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Diğer uygulamalar

Şu anda oturum EF çekirdek kendisini bir veya daha fazla ILoggerProvider ile yapılandırılmış olan bir Iloggerfactory gerektirir. Ortak sağlayıcıları aşağıdaki paketlerde gönderilir:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): basit bir konsol Günlükçü.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): destekler Azure uygulama Hizmetleri 'Tanılama günlükleri' ve 'akış günlük' özellikleri.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System.Diagnostics.Debug.WriteLine() kullanarak bir hata ayıklayıcı izleme günlükleri.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows olay günlüğüne günlükleri.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener destekler.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): günlükleri System.Diagnostics.TraceSource.TraceEvent() kullanarak bir izleme dinleyicisi.

Uygun paketleri yükledikten sonra uygulamayı bir LoggerFactory singleton/genel bir örneğini oluşturmanız gerekir. Örneğin, konsol Günlükçü kullanma:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Bu singleton/genel örnek sonra EF çekirdek ile kayıtlı olması `DbContextOptionsBuilder`. Örneğin:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Uygulamalar her bağlam örneği için yeni bir Iloggerfactory örnek oluşturmayın çok önemlidir. Bunun yapılması, bir bellek sızıntısı ve performansın neden olur.

## <a name="filtering-what-is-logged"></a>Günlüğe kaydedilenler filtreleme

Günlüğe kaydedilenler filtre uygulamak için en kolay yolu ILoggerProvider kaydedilirken yapılandırmaktır. Örneğin:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

Bu örnekte, yalnızca iletilerini döndürmek için günlük filtre uygulanır:
 * 'Microsoft.EntityFrameworkCore.Database.Command' kategorisinde
 * 'Bilgi' düzeyinde

İçinde tanımlanan EF çekirdek için Günlükçü kategorileri `DbLoggerCategory` kategorileri, ancak bunlar bulmayı kolaylaştırmak için sınıf çözümlemek için basit dizeleri.

Günlüğe kaydetme altyapının hakkında daha fazla ayrıntı bulunabilir [ASP.NET Core günlük belgelerine](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
