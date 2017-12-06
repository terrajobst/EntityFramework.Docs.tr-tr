---
title: "Tasarım zamanı Hizmetleri - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a>Tasarım zamanı Hizmetleri
====================
Araçları tarafından kullanılan bazı hizmetler yalnızca tasarım zamanında kullanılır. Bu hizmetler ile uygulamanızı dağıtılan önlemek için EF çekirdek'ın çalışma zamanı hizmetlerinden ayrı olarak yönetilir. Bu hizmetlerden (geçiş dosyaları oluşturmak için örneğin hizmeti) biri geçersiz kılmak için uygulaması eklemek `IDesignTimeServices` başlangıç projenize.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
