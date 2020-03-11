---
title: Tasarım zamanı Hizmetleri-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416730"
---
# <a name="design-time-services"></a><span data-ttu-id="db652-102">Tasarım zamanı Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="db652-102">Design-time services</span></span>

<span data-ttu-id="db652-103">Araçlar tarafından kullanılan bazı hizmetler yalnızca tasarım zamanında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db652-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="db652-104">Bu hizmetler, EF Core çalışma zamanı hizmetlerinden ayrı olarak yönetilir ve bu uygulamaların uygulamanızla dağıtılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="db652-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="db652-105">Bu hizmetlerden birini geçersiz kılmak için (örneğin, geçiş dosyaları oluşturma hizmeti), başlangıç projenize `IDesignTimeServices` bir uygulamasını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="db652-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
