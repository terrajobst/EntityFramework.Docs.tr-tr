---
title: Veritabanı sağlayıcısı yazma-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d52a8581772cc5405e94966fa7ebdff4128c252
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654773"
---
# <a name="writing-a-database-provider"></a>Veritabanı Sağlayıcısı Yazma

Entity Framework Core veritabanı sağlayıcısı yazma hakkında daha fazla bilgi için, bkz. [Arthur Vicranlar](https://github.com/ajcvickers)tarafından [bir EF Core sağlayıcısı yazmak istiyor](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) .

> [!NOTE]
> Bu gönderimler EF Core 1,1 ' den beri güncelleştirilmemiş ve bu süre [681 sorunu](https://github.com/aspnet/EntityFramework.Docs/issues/681) bu belgelerde yapılan güncelleştirmeleri izlemediğinden önemli değişiklikler yapıldı.

EF Core kod temeli açık kaynaktır ve başvuru olarak kullanılabilecek çeşitli veritabanı sağlayıcıları içerir. Kaynak kodunu <https://github.com/aspnet/EntityFrameworkCore>' de bulabilirsiniz. Ayrıca, [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)ve [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)gibi yaygın olarak kullanılan üçüncü taraf sağlayıcılarının koduna bakmak da yararlı olabilir. Özellikle, bu projeler, NuGet üzerinde yayımladığımız işlev testlerini genişletmek ve çalıştırmak için kurulumlardır. Bu tür bir kurulum kesinlikle önerilir.

## <a name="keeping-up-to-date-with-provider-changes"></a>Sağlayıcı değişiklikleriyle güncel tutma

2,1 sürümünden sonra çalışmaya başladıktan sonra, sağlayıcı kodunda ilgili değişikliklere ihtiyacı olabilecek bir [değişiklikler günlüğü](provider-log.md) oluşturduk. Bu, mevcut bir sağlayıcıyı EF Core yeni bir sürümüyle çalışacak şekilde güncelleştirirken yardımcı olmaya yöneliktir.

2,1 ' dan önce, GitHub sorunlarımızda [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etiketleri ve benzer bir amaç için çekme isteklerimize kullandık. Bu etiketleri sorunlarını, belirli bir sürümdeki iş öğelerinin, sağlayıcılarda hangi iş öğelerinin yapılmasını gerektirdiğini belirten bir belirtiye vermek için de kullanacağız. Bir `providers-beware` etiketi genellikle bir iş öğesi uygulamasının sağlayıcıları bozabileceğinden, bir `providers-fyi` etiketinin genellikle sağlayıcıların kesilmeyeceği, ancak örneğin yeni işlevselliği etkinleştirmek için de kodun değiştirilmesi gerekebilecek anlamına gelir.

## <a name="suggested-naming-of-third-party-providers"></a>Üçüncü taraf sağlayıcıların önerilen adlandırma

NuGet paketleri için aşağıdaki adlandırmanın kullanılması önerilir. Bu, EF Core ekibi tarafından teslim edilen paketlerin adlarıyla tutarlıdır.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Örneğin:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
