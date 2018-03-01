---
title: "Pomelo MySQL veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a>MySQL için pomelo EF çekirdek veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı MySQL ile kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı bir parçası olarak korunur [Pomelo Foundation proje](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).

> [!NOTE]  
>
> Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz. Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.

## <a name="install"></a>Yükleme

Yükleme [Pomelo.EntityFrameworkCore.MySql NuGet paketi](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.
* [Başlatılan belgeleri alma](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [Yuuko Blog örnek uygulama](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* MySQL
* MariaDB

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

* Mono (veya sonraki sürümleri 4.2.0)
