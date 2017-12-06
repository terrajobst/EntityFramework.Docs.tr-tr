---
title: "Veritabanı sağlayıcısı - EF çekirdek yazma"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a>Veritabanı sağlayıcısı yazma

Bir Entity Framework Çekirdek veritabanı sağlayıcısı yazma hakkında daha fazla bilgi için bkz: [EF çekirdek sağlayıcısı yazmak istediğiniz şekilde](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) tarafından [Arthur Vickers](https://github.com/ajcvickers).

EF çekirdek kod tabanını açık kaynaklıdır ve başvuru olarak kullanılan birkaç veritabanı sağlayıcıları içerir. Kaynak kodu https://github.com/aspnet/EntityFramework bulabilirsiniz.

## <a name="the-providers-beware-label"></a>Etiket sağlayıcıları-kaybolacağını unutmayın

Bir sağlayıcı üzerinde çalışmaya sonra izlemesi [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) bizim GitHub sorunlar ve çekme istekleri etiketi. Bu etiket sağlayıcısı yazıcılarının etkileyebilecek değişiklikleri belirlemek için kullanırız.

## <a name="suggested-naming-of-third-party-providers"></a>Üçüncü taraf sağlayıcılar adlandırma önerilen

NuGet paketleri için aşağıdaki adlandırma kullanarak öneririz. Bu, EF çekirdek ekibi tarafından teslim edilen paket adları ile tutarlıdır.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Örneğin:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
