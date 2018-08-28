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
<a name="design-time-services"></a>Tasarım zamanı Hizmetleri
====================
Araçları tarafından kullanılan bazı hizmetler, yalnızca tasarım zamanında kullanılır. Bu hizmetler, uygulamanızla dağıtılan önlemek için EF Core'nın çalışma zamanı hizmetlerinden ayrı olarak yönetilir. (Geçiş dosyaları oluşturmak için örneğin hizmeti) Bu hizmetlerden biri geçersiz kılmak için uygulaması Ekle `IDesignTimeServices` başlangıç projeniz için.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
