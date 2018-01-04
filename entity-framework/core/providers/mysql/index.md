---
title: "MySQL veritabanı sağlayıcısı - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="mysql-ef-core-database-provider"></a>MySQL EF çekirdek veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı MySQL ile kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı bir parçası olarak korunur [MySQL proje](http://dev.mysql.com).

> [!WARNING]  
> Yayın öncesi sağlayıcıdır.

> [!NOTE]  
> Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz. Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.

## <a name="install"></a>Yükleme

Yükleme [MySql.Data.EntityFrameworkCore NuGet paketi](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Başlarken

Bkz: [MySQL EF çekirdek sağlayıcısı ve Connector/Net 7.0.4 başlatarak](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* MySQL

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

MySQL belgeleri sürüm uyumluluk bilgilerini gözden geçirmeyi unutmayın [burada](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) ve [burada](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)
