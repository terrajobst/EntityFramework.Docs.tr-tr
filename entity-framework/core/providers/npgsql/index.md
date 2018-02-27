---
title: "PostgreSQL - EF çekirdek için Npgsql veritabanı sağlayıcısı"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a>PostgreSQL için Npgsql EF çekirdek veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı ile PostgreSQL kullanılacak Entity Framework Çekirdek sağlar. Sağlayıcı bir parçası olarak korunur [Npgsql proje](http://www.npgsql.org).

> [!NOTE]  
> Bu sağlayıcının Entity Framework Çekirdek projenin bir parçası olarak korunmaz. Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.

## <a name="install"></a>Yükleme

Yükleme [Npgsql.EntityFrameworkCore.PostgreSQL NuGet paketi](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a>Başlarken

Bkz: [Npgsql belgelerine](http://www.npgsql.org/efcore/index.html) başlamak için.

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* PostgreSQL

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

* Mono (veya sonraki sürümleri 4.2.0)
