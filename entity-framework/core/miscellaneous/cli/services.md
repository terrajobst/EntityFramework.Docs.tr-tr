---
title: Tasarım zamanı Hizmetleri - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997537"
---
<a name="design-time-services"></a><span data-ttu-id="8bbdd-102">Tasarım zamanı Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="8bbdd-102">Design-time services</span></span>
====================
<span data-ttu-id="8bbdd-103">Araçları tarafından kullanılan bazı hizmetler, yalnızca tasarım zamanında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8bbdd-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="8bbdd-104">Bu hizmetler, uygulamanızla dağıtılan önlemek için EF Core'nın çalışma zamanı hizmetlerinden ayrı olarak yönetilir.</span><span class="sxs-lookup"><span data-stu-id="8bbdd-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="8bbdd-105">(Geçiş dosyaları oluşturmak için örneğin hizmeti) Bu hizmetlerden biri geçersiz kılmak için uygulaması Ekle `IDesignTimeServices` başlangıç projeniz için.</span><span class="sxs-lookup"><span data-stu-id="8bbdd-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
