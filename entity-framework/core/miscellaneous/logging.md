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
# <a name="logging"></a>Günlüğe Kaydetme

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) GitHub üzerinde.

## <a name="aspnet-core-applications"></a>ASP.NET Core uygulamaları

EF Core ile ASP.NET Core günlüğe kaydetme sistemleri otomatik olarak tümleştirilir her `AddDbContext` veya `AddDbContextPool` kullanılır. Bu nedenle, ASP.NET Core kullanırken, günlüğe kaydetme bölümünde anlatıldığı gibi yapılandırılmalıdır [ASP.NET Core belgeleri](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Diğer uygulamalar

Şu anda oturum EF Core kendisi ile bir veya daha fazla ILoggerProvider yapılandırılmış olan bir Iloggerfactory gerektirir. Ortak sağlayıcıları aşağıdaki paketlerde gönderilir:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Basit bir konsol Günlükçü.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure uygulama Hizmetleri 'Tanılama günlükleri' ve 'akış oturum' özelliklerini destekler.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Bir hata ayıklayıcı günlükleri System.Diagnostics.Debug.WriteLine() kullanarak izleyin.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows olay günlüğüne günlükleri.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener destekler.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Günlükleri System.Diagnostics.TraceSource.TraceEvent() kullanarak bir izleme dinleyicisi.

> [!NOTE]
> Aşağıdaki kod örneği kullanan bir `ConsoleLoggerProvider` 2.2 sürümünde geçersiz Oluşturucusu. Eski günlük API'leri için doğru değiştirmeler 3.0 sürümünde kullanılabilir. Bu arada, yoksaymak ve uyarıların görünmemesi güvenlidir.

Uygun paket yüklendikten sonra uygulamayı bir LoggerFactory tekil/genel bir örneğini oluşturmanız gerekir. Örneğin, konsol günlüğe kullanarak:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Bu singleton/genel örnek ardından EF Core ile şirket kaydedilmelidir `DbContextOptionsBuilder`. Örneğin:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Uygulamalar her bağlam örneği için yeni bir Iloggerfactory örnek oluşturmayın çok önemlidir. Bunun yapılması, bir bellek sızıntısı ve performansın düşmesine neden olur.

## <a name="filtering-what-is-logged"></a>Günlüğe kaydedilenler filtreleme

> [!NOTE]
> Aşağıdaki kod örneği kullanan bir `ConsoleLoggerProvider` 2.2 sürümünde geçersiz Oluşturucusu. Eski günlük API'leri için doğru değiştirmeler 3.0 sürümünde kullanılabilir. Bu arada, yoksaymak ve uyarıların görünmemesi güvenlidir.

Günlüğe kaydedilenler filtrelemek için en kolay yolu ILoggerProvider kaydederken yapılandırmaktır. Örneğin:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

Bu örnekte, günlük, yalnızca iletileri döndürmek için filtrelenmiştir:
 * 'Microsoft.EntityFrameworkCore.Database.Command' kategorisinde
 * 'Bilgi' düzeyinde

EF Core için Günlükçü kategorileri içinde tanımlanan `DbLoggerCategory` kategorileri, ancak bunlar bulmayı kolaylaştırmak için sınıf çözümlemek için basit dizeler.

Altyapının günlüğe kaydetme hakkında daha fazla ayrıntı bulunabilir [ASP.NET Core günlük belgeleri](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
