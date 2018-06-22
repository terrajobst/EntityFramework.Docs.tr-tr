---
title: Inmemory veritabanı sağlayıcısı - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678992"
---
# <a name="ef-core-in-memory-database-provider"></a>EF çekirdek bellek içi veritabanı sağlayıcısı

Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar. Bellek içi modunda SQLite sağlayıcısı test yerine ilişkisel veritabanları için daha uygun olabilir ancak bu test etmek için yararlı olabilir. Sağlayıcı bir parçası olarak korunur [Entity Framework Çekirdek proje](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Yükleme

Yükleme [Microsoft.EntityFrameworkCore.InMemory NuGet paketi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar bu sağlayıcı ile çalışmaya başlamanıza yardımcı olur.
* [InMemory ile test etme](../../miscellaneous/testing/in-memory.md)

* [UnicornStore örnek uygulaması testleri](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Veritabanı motoru desteklenen

* (Yalnızca sınama amacıyla tasarlanmış) yerleşik bellek içi veritabanı

## <a name="supported-platforms"></a>Desteklenen Platformlar

* .NET framework (4.5.1 veya sonraki sürümleri)

* .NET Core

* Mono (veya sonraki sürümleri 4.2.0)

* Evrensel Windows Platformu
