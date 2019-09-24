---
title: Günlüğe kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 6a8499f9f0220087e76f2e0b3a75ce551c4ddb80
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197511"
---
# <a name="logging"></a>Günlüğe Kaydetme

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) GitHub üzerinde.

## <a name="aspnet-core-applications"></a>ASP.NET Core uygulamalar

EF Core, veya `AddDbContext` `AddDbContextPool` her kullanıldığında ASP.NET Core günlük mekanizmaları ile otomatik olarak tümleştirilir. Bu nedenle, ASP.NET Core kullanırken günlüğe kaydetme, [ASP.NET Core belgelerinde](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)açıklandığı şekilde yapılandırılmalıdır.

## <a name="other-applications"></a>Diğer uygulamalar

EF Core günlüğü, kendisi bir veya daha fazla günlüğe kaydetme sağlayıcısıyla yapılandırılmış bir ıloggerfactory gerektirir. Ortak sağlayıcılar aşağıdaki paketlere gönderilir:

* [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Basit konsol günlükçüsü.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Azure Uygulama Hizmetleri ' tanılama günlükleri ' ve ' günlük akışı ' özelliklerini destekler.
* [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): System. Diagnostics. Debug. WriteLine () kullanılarak bir hata ayıklayıcı izleyicisinde günlüğe kaydedilir.
* [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows olay günlüğü 'ne kaydedilir.
* [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener 'ı destekler.
* [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Kullanarak `System.Diagnostics.TraceSource.TraceEvent()`bir izleme dinleyicisine kaydeder.

Uygun paketleri yükledikten sonra, uygulamanın bir LoggerFactory 'nin tek/genel örneğini oluşturması gerekir. Örneğin, konsol günlükçüsü kullanarak:

# <a name="version-30tabv3"></a>[Sürüm 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[Sürüm 2. x](#tab/v2)

> [!NOTE]
> Aşağıdaki kod örneği, sürüm 2,2 `ConsoleLoggerProvider` ' de kullanımdan kaldırılmış ve 3,0 ' de değiştirilen bir oluşturucuyu kullanır. 2,2 kullanılırken uyarıları yoksaymak ve gizlemek güvenlidir.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

Bu tek başına/genel örnek, `DbContextOptionsBuilder`üzerinde EF Core ile kaydedilmelidir. Örneğin:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Uygulamaların her bir bağlam örneği için yeni bir ıloggerfactory örneği oluşturmaması çok önemlidir. Bunun yapılması bellek sızıntısı ve düşük performansa neden olur.

## <a name="filtering-what-is-logged"></a>Günlüğe kaydedilen filtreleme

Uygulama, ıloggerprovider üzerinde bir filtre yapılandırarak günlüğe nelerin kaydedildiğini denetleyebilir. Örneğin:

# <a name="version-30tabv3"></a>[Sürüm 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[Sürüm 2. x](#tab/v2)

> [!NOTE]
> Aşağıdaki kod örneği, sürüm 2,2 `ConsoleLoggerProvider` ' de kullanımdan kaldırılmış ve 3,0 ' de değiştirilen bir oluşturucuyu kullanır. 2,2 kullanılırken uyarıları yoksaymak ve gizlemek güvenlidir.

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

Bu örnekte, günlük yalnızca iletileri döndürecek şekilde filtrelenmiştir:
 * ' Microsoft. EntityFrameworkCore. Database. Command ' kategorisinde
 * ' bilgi ' düzeyinde

EF Core için, günlükçü kategorileri, kategorileri bulmayı kolaylaştırmak `DbLoggerCategory` için sınıfında tanımlanır, ancak bunlar basit dizelere çözümlenir.

Temel alınan günlük altyapısı hakkında daha fazla ayrıntı [ASP.NET Core günlük belgelerinde](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)bulunabilir.
