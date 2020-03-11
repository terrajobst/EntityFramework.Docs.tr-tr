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
# <a name="design-time-services"></a>Tasarım zamanı Hizmetleri

Araçlar tarafından kullanılan bazı hizmetler yalnızca tasarım zamanında kullanılır. Bu hizmetler, EF Core çalışma zamanı hizmetlerinden ayrı olarak yönetilir ve bu uygulamaların uygulamanızla dağıtılmasını önler. Bu hizmetlerden birini geçersiz kılmak için (örneğin, geçiş dosyaları oluşturma hizmeti), başlangıç projenize `IDesignTimeServices` bir uygulamasını ekleyin.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
