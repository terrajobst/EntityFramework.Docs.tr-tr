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
<a name="design-time-services"></a><span data-ttu-id="53c47-102">Tasarım zamanı Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="53c47-102">Design-time services</span></span>
====================
<span data-ttu-id="53c47-103">Araçları tarafından kullanılan bazı hizmetler yalnızca tasarım zamanında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53c47-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="53c47-104">Bu hizmetler ile uygulamanızı dağıtılan önlemek için EF çekirdek'ın çalışma zamanı hizmetlerinden ayrı olarak yönetilir.</span><span class="sxs-lookup"><span data-stu-id="53c47-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="53c47-105">Bu hizmetlerden (geçiş dosyaları oluşturmak için örneğin hizmeti) biri geçersiz kılmak için uygulaması eklemek `IDesignTimeServices` başlangıç projenize.</span><span class="sxs-lookup"><span data-stu-id="53c47-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
