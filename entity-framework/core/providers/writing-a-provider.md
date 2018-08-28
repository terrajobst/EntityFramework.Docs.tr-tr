---
title: EF Core - veritabanı sağlayıcısı yazma
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993672"
---
# <a name="writing-a-database-provider"></a>Veritabanı sağlayıcısı yazma

Bir Entity Framework Core veritabanı sağlayıcısı yazma hakkında daha fazla bilgi için bkz: [EF Core sağlayıcısı yazmak istediğiniz şekilde](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) tarafından [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Bu postaları EF Core 1.1 güncelleştirilmemiş ve o zamandan bu yana yapılan önemli değişiklikler olmuştur [sorunu 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) güncelleştirmeleri bu belge için izlemektir.

EF Core kod tabanının açık kaynaklıdır ve başvuru olarak kullanılan birkaç veritabanı sağlayıcısı içerir. Kaynak kodu bulabilirsiniz https://github.com/aspnet/EntityFrameworkCore. Ayrıca yaygın olarak kullanılan üçüncü taraf sağlayıcılar için gibi koda göz atmak yararlı olabilir [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), ve [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). Özellikle, bu projelerin gelen genişletmek ve NuGet üzerinde biz yayımlama işlevsel testleri çalıştırmak için kurulum değiştirilebilir. Bu tür bir kurulum önemle tavsiye edilir.

## <a name="keeping-up-to-date-with-provider-changes"></a>Sağlayıcı değişiklikleri takip etme

Oluşturduk 2.1 sürümünden sonra iş ile başlayarak, bir [değişiklik günlüğünü](provider-log.md) sağlayıcı kodu karşılık gelen değişiklikleri gerekebilir. Bu yeni bir sürümü EF Core ile çalışmak için mevcut bir sağlayıcı güncelleştirirken yardımcı olmayı hedefler.

2.1 önce kullandığımız [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) bizim GitHub sorunlar ve çekme istekleri benzer bir amaç için etiketleri. Bu etiketleri sorunları hangi iş öğelerinin belirli bir sürüm sağlayıcıları yapılacak işleri de gerektirebilir göstergesidir vermek için kullanılacak continiue yapacağız. A `providers-beware` etiket genellikle anlamına gelir uygulama bir iş öğesinin sağlayıcıları bozabilir ancak bir `providers-fyi` etiket genellikle anlamına gelir sağlayıcıları bozuk olmayacak, ancak yine de, örneğin değiştirilmesi, yeni işlevselliği etkinleştirmek için kod gerekebilir.

## <a name="suggested-naming-of-third-party-providers"></a>Önerilen adlandırma üçüncü taraf sağlayıcılar

NuGet paketleri için aşağıdaki adlandırma kullanmanızı öneririz. Bu, EF Core ekibi tarafından teslim edilen paket adları ile tutarlıdır.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Örneğin:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
