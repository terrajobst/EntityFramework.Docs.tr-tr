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
# <a name="design-time-services"></a><span data-ttu-id="4f513-102">Tasarım zamanı Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="4f513-102">Design-time services</span></span>

<span data-ttu-id="4f513-103">Araçlar tarafından kullanılan bazı hizmetler yalnızca tasarım zamanında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4f513-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="4f513-104">Bu hizmetler, EF Core çalışma zamanı hizmetlerinden ayrı olarak yönetilir ve bu uygulamaların uygulamanızla dağıtılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="4f513-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="4f513-105">Bu hizmetlerden birini geçersiz kılmak için (örneğin, geçiş dosyaları oluşturma hizmeti), başlangıç projenize `IDesignTimeServices` bir uygulamasını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4f513-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
