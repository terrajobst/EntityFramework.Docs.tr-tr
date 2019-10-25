---
title: Tasarım zamanı Hizmetleri-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811942"
---
# <a name="design-time-services"></a>Tasarım zamanı Hizmetleri

Araçlar tarafından kullanılan bazı hizmetler yalnızca tasarım zamanında kullanılır. Bu hizmetler, EF Core çalışma zamanı hizmetlerinden ayrı olarak yönetilir ve bu uygulamaların uygulamanızla dağıtılmasını önler. Bu hizmetlerden birini geçersiz kılmak için (örneğin, geçiş dosyaları oluşturma hizmeti), başlangıç projenize `IDesignTimeServices` bir uygulamasını ekleyin.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
