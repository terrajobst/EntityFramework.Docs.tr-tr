---
title: "IBM veri sunucu (DB2) veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>IBM veri sunucu (DB2) EF çekirdek veritabanı sağlayıcıları

Bu veritabanı sağlayıcısı IBM veri sunucusu ile kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı IBM tarafından korunur.

> [!NOTE]  
> Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz. Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.

## <a name="install"></a>Yükleme

IBM veri sunucusuyla Windows üzerinde çalışacak şekilde yükleme [IBM. EntityFrameworkCore NuGet paketi](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

IBM veri sunucu Linux üzerinde çalışmak için yükleme [IBM. EntityFrameworkCore lnx NuGet paketi](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Başlarken

[.NET Core için IBM .NET sağlayıcısını kullanmaya başlama](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* zOS
* IBM dashDB dahil olmak üzere LUW
* IBM EDİYORUM
* Informix

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (veya sonraki sürümleri 4.6)
* .NET Core
