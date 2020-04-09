---
title: InMemory Veritabanı Sağlayıcısı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417221"
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core Bellek Veritabanı Sağlayıcısı

Bu veritabanı sağlayıcısı, Entity Framework Core'un bellek içi bir veritabanıyla kullanılmasına izin verir. Bellek modundaSQLite sağlayıcısı ilişkisel veritabanları için daha uygun bir test yerine olabilir, ancak bu sınama için yararlı olabilir. Sağlayıcı, Varlık Çerçeve Çekirdek [Projesi'nin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.

## <a name="install"></a>Yükleme

[Microsoft.EntityFrameworkCore.InMemory NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)yükleyin.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Başlarken

Aşağıdaki kaynaklar, bu sağlayıcıyla işe başlamanıza yardımcı olur.

* [InMemory ile test etme](../../miscellaneous/testing/in-memory.md)
* [UnicornStore Örnek Uygulama Testleri](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Desteklenen Veritabanı Motorları

İşlem içi bellek veritabanı (yalnızca sınama amacıyla tasarlanmıştır)
