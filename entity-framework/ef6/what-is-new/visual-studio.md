---
title: Visual Studio sürümlerine - EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: d104236ac5c8877da421ba10de9827f17937a9ec
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489927"
---
# <a name="visual-studio-releases"></a>Visual Studio sürümleri

Visual Studio'nun en son sürümü, .NET, NuGet ve Entity Framework için en yeni araçlara içerdiği için her zaman kullanılması önerilir.
Aslında, çeşitli örnekleri ve izlenecek yollar için Entity Framework belgelerine arasında Visual Studio'nun yeni bir sürümü kullandığınız varsayılır.

İçine almak sürece kullanmak için Visual Studio'nun eski sürümlerini Entity Framework'ün farklı sürümlerini bazı farklılıklar hesap ancak mümkündür:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 ve üzeri

- Visual Studio'nun bu sürümü, Entity Framework Araçları ve EF 6.2 çalışma zamanı en son sürümünü içerir ve ek kurulum adımı gerektirmez.
Bkz: [yenilikler](~/ef6/what-is-new/index.md) bu sürümler hakkında daha fazla ayrıntı için.
- Entity Framework EF araçları kullanarak yeni projelere ekleme EF 6.2 NuGet paketini otomatik olarak ekler.
El ile yüklemek veya yükseltme çevrimiçi herhangi EF NuGet paketine kullanılabilir.
- Varsayılan olarak, Visual Studio'nun bu sürümü ile kullanılabilir SQL Server örneği ifadesini MSSQLLocalDB adlı bir LocalDB örneğidir.
Bağlantı dizesi kullanması gereken sunucu bölümü "(localdb)\\ifadesini MSSQLLocalDB".
Ön ekine sahip bir verbatim dizesi kullanmayı unutmayın `@` veya çift ters eğik çizgi "\\\\" C# kodunda belirtmek için bir bağlantı dizesi.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 için Visual Studio 2017 15.6

- Visual Studio'nun bu sürümleri, Entity Framework Araçları ve çalışma zamanı 6.1.3 içerir.
Bkz: [son sürümleri](~/ef6/what-is-new/past-releases.md#ef-613) bu sürümler hakkında daha fazla ayrıntı için.
- Entity Framework EF araçları kullanarak yeni projelere ekleme 6.1.3 EF otomatik olarak eklenir NuGet paketi.
El ile yüklemek veya yükseltme çevrimiçi herhangi EF NuGet paketine kullanılabilir.
- Varsayılan olarak, Visual Studio'nun bu sürümü ile kullanılabilir SQL Server örneği ifadesini MSSQLLocalDB adlı bir LocalDB örneğidir.
Bağlantı dizesi kullanması gereken sunucu bölümü "(localdb)\\ifadesini MSSQLLocalDB".
Ön ekine sahip bir verbatim dizesi kullanmayı unutmayın `@` veya çift ters eğik çizgi "\\\\" C# kodunda belirtmek için bir bağlantı dizesi.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Visual Studio'nun bu sürümü, içerir ve Entity Framework Araçları ve çalışma zamanı sürümünü daha eski.
Entity Framework Araçları 6.1.3, yükseltmeniz kesinlikle önerilir kullanarak [yükleyici](https://www.microsoft.com/en-us/download/details.aspx?id=40762) Microsoft Download Center'daki kullanılabilir.
Bkz: [son sürümleri](~/ef6/what-is-new/past-releases.md#ef-613) bu sürümler hakkında daha fazla ayrıntı için.
- Entity Framework yükseltilen EF araçları kullanarak yeni projelere ekleme 6.1.3 EF otomatik olarak eklenir NuGet paketi.
El ile yüklemek veya yükseltme çevrimiçi herhangi EF NuGet paketine kullanılabilir.
- Varsayılan olarak, Visual Studio'nun bu sürümü ile kullanılabilir SQL Server örneği ifadesini MSSQLLocalDB adlı bir LocalDB örneğidir.
Bağlantı dizesi kullanması gereken sunucu bölümü "(localdb)\\ifadesini MSSQLLocalDB".
Ön ekine sahip bir verbatim dizesi kullanmayı unutmayın `@` veya çift ters eğik çizgi "\\\\" C# kodunda belirtmek için bir bağlantı dizesi.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Visual Studio'nun bu sürümü, içerir ve Entity Framework Araçları ve çalışma zamanı sürümünü daha eski.
Entity Framework Araçları 6.1.3, yükseltmeniz kesinlikle önerilir kullanarak [yükleyici](https://www.microsoft.com/en-us/download/details.aspx?id=40762) Microsoft Download Center'daki kullanılabilir.
Bkz: [son sürümleri](~/ef6/what-is-new/past-releases.md#ef-613) bu sürümler hakkında daha fazla ayrıntı için.
- Entity Framework yükseltilen EF araçları kullanarak yeni projelere ekleme 6.1.3 EF otomatik olarak eklenir NuGet paketi.
El ile yüklemek veya yükseltme çevrimiçi herhangi EF NuGet paketine kullanılabilir.
- Varsayılan olarak, Visual Studio'nun bu sürümü ile kullanılabilir SQL Server örneği v11.0 adlı bir LocalDB örneğidir.
Bağlantı dizesi kullanması gereken sunucu bölümü "(localdb)\\v11.0".
Ön ekine sahip bir verbatim dizesi kullanmayı unutmayın `@` veya çift ters eğik çizgi "\\\\" C# kodunda belirtmek için bir bağlantı dizesi.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Entity Framework Tools sürümü Visual Studio'nun bu sürümü ile Entity Framework 6 çalışma zamanı ile uyumlu değildir ve yükseltilemez.
- Varsayılan olarak Entity Framework Araçları Entity Framework 4.0 projelerinize ekler.
Daha yeni sürümlerinden hiçbirini EF kullanan uygulamalar oluşturmak için önce yüklemeniz gerekir [NuGet paket yöneticisini uzantısı](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Varsayılan olarak, tüm kod oluşturma EF araçları sürümünde EntityObject ve Entity Framework 4'te temel alır.
DbContext ve Entity Framework 5 DbContext kod oluşturma şablonlar yükleyerek dayalı için kod oluşturma geçiş öneririz [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) veya [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- NuGet Paket Yöneticisi Uzantıları yükledikten sonra el ile yükleyebilir veya çevrimiçi tüm kullanılabilir EF NuGet paketini yükseltin ve EF6 kod bir tasarımcı gerektirmeyen ilk ile kullanın.
- Varsayılan olarak, SQL Server, Visual Studio'nun bu sürümü ile kullanılabilir SQLEXPRESS adlı SQL Server Express örneğidir.
Bağlantı dizesi kullanması gereken sunucu bölümü ". \\SQLEXPRESS ".
Ön ekine sahip bir verbatim dizesi kullanmayı unutmayın `@` veya çift ters eğik çizgi "\\\\" C# kodunda belirtmek için bir bağlantı dizesi.
