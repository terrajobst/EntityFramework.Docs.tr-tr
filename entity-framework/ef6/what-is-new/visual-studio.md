---
title: Visual Studio yayınları-EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416942"
---
# <a name="visual-studio-releases"></a>Visual Studio Yayınları

.NET, NuGet ve Entity Framework için en son Araçları içerdiğinden, her zaman Visual Studio 'nun en son sürümünü kullanmanızı öneririz.
Aslında Entity Framework belgelerindeki çeşitli örnekler ve izlenecek yollar, Visual Studio 'nun yeni bir sürümünü kullandığınızı varsayar.

Ancak bazı farklılıklar dikkate aldığından, farklı Entity Framework sürümleriyle Visual Studio 'nun eski sürümlerini kullanmak mümkündür:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15,7 ve üzeri

- Visual Studio 'nun bu sürümü Entity Framework araçları ve EF 6,2 çalışma zamanının en son sürümünü içerir ve ek kurulum adımları gerektirmez.
Bu sürümler hakkında daha fazla [ayrıntı için bkz. yenilikler.](~/ef6/what-is-new/index.md)
- EF araçlarını kullanarak yeni projelere Entity Framework eklemek, EF 6,2 NuGet paketini otomatik olarak ekler.
Çevrimiçi kullanılabilir olan herhangi bir EF NuGet paketini el ile yükleyebilir veya yükseltebilirsiniz.
- Varsayılan olarak, Visual Studio 'nun bu sürümüyle birlikte sunulan SQL Server örneği, MSSQLLocalDB adlı bir LocalDB örneğidir.
Kullanmanız gereken bağlantı dizesinin sunucu bölümü "(LocalDB)\\MSSQLLocalDB" dir.
C# Kodda bir bağlantı dizesi belirtirken `@` veya çift ters eğik çizgi "\\\\" önekli bir tam dize kullanmayı unutmayın.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015-Visual Studio 2017 15,6

- Visual Studio 'nun bu sürümleri Entity Framework araçları ve çalışma zamanı 6.1.3 içerir.
Bu sürümler hakkında daha fazla bilgi için bkz. [Geçmiş yayınlar](~/ef6/what-is-new/past-releases.md#ef-613) .
- EF araçlarını kullanarak yeni projelere Entity Framework eklemek, EF 6.1.3 NuGet paketini otomatik olarak ekler.
Çevrimiçi kullanılabilir olan herhangi bir EF NuGet paketini el ile yükleyebilir veya yükseltebilirsiniz.
- Varsayılan olarak, Visual Studio 'nun bu sürümüyle birlikte sunulan SQL Server örneği, MSSQLLocalDB adlı bir LocalDB örneğidir.
Kullanmanız gereken bağlantı dizesinin sunucu bölümü "(LocalDB)\\MSSQLLocalDB" dir.
C# Kodda bir bağlantı dizesi belirtirken `@` veya çift ters eğik çizgi "\\\\" önekli bir tam dize kullanmayı unutmayın.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Visual Studio 'nun bu sürümü, Entity Framework araçları ve çalışma zamanının daha eski bir sürümünü içerir.
Microsoft Indirme merkezi 'nde bulunan [yükleyiciyi](https://www.microsoft.com/download/details.aspx?id=40762) kullanarak Entity Framework Tools 6.1.3 sürümüne yükseltmeniz önerilir.
Bu sürümler hakkında daha fazla bilgi için bkz. [Geçmiş yayınlar](~/ef6/what-is-new/past-releases.md#ef-613) .
- Yükseltilen EF araçlarını kullanarak yeni projelere Entity Framework eklemek, EF 6.1.3 NuGet paketini otomatik olarak ekler.
Çevrimiçi kullanılabilir olan herhangi bir EF NuGet paketini el ile yükleyebilir veya yükseltebilirsiniz.
- Varsayılan olarak, Visual Studio 'nun bu sürümüyle birlikte sunulan SQL Server örneği, MSSQLLocalDB adlı bir LocalDB örneğidir.
Kullanmanız gereken bağlantı dizesinin sunucu bölümü "(LocalDB)\\MSSQLLocalDB" dir.
C# Kodda bir bağlantı dizesi belirtirken `@` veya çift ters eğik çizgi "\\\\" önekli bir tam dize kullanmayı unutmayın.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Visual Studio 'nun bu sürümü, Entity Framework araçları ve çalışma zamanının daha eski bir sürümünü içerir.
Microsoft Indirme merkezi 'nde bulunan [yükleyiciyi](https://www.microsoft.com/download/details.aspx?id=40762) kullanarak Entity Framework Tools 6.1.3 sürümüne yükseltmeniz önerilir.
Bu sürümler hakkında daha fazla bilgi için bkz. [Geçmiş yayınlar](~/ef6/what-is-new/past-releases.md#ef-613) .
- Yükseltilen EF araçlarını kullanarak yeni projelere Entity Framework eklemek, EF 6.1.3 NuGet paketini otomatik olarak ekler.
Çevrimiçi kullanılabilir olan herhangi bir EF NuGet paketini el ile yükleyebilir veya yükseltebilirsiniz.
- Varsayılan olarak, Visual Studio 'nun bu sürümüyle birlikte sunulan SQL Server örneği, v 11.0 adlı bir LocalDB örneğidir.
Kullanmanız gereken bağlantı dizesinin sunucu bölümü "(LocalDB)\\v 11.0" dir.
C# Kodda bir bağlantı dizesi belirtirken `@` veya çift ters eğik çizgi "\\\\" önekli bir tam dize kullanmayı unutmayın.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Visual Studio 'nun bu sürümü ile kullanılabilen Entity Framework Tools sürümü Entity Framework 6 çalışma zamanı ile uyumlu değildir ve yükseltilemez.
- Varsayılan olarak, Entity Framework araçları projelerinize Entity Framework 4,0 ekler.
EF 'in daha yeni sürümlerini kullanan uygulamalar oluşturmak için önce [NuGet Paket Yöneticisi uzantısını](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager)yüklemeniz gerekir.
- Varsayılan olarak, EF araçları sürümündeki tüm kod üretimi EntityObject ve Entity Framework 4 ' ü temel alır.
Veya [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET)için DbContext kod oluşturma şablonlarını yükleyerek, kod oluşturma 'yı DbContext ve Entity Framework 5 ' e dayandırarak geçiş yapmanızı öneririz.
- NuGet Paket Yöneticisi uzantıları 'nı yükledikten sonra çevrimiçi kullanılabilir olan herhangi bir EF NuGet paketini el ile yükleyebilir veya yükseltebilirsiniz ve tasarımcı gerektirmeyen Code First ile EF6 kullanabilirsiniz.
- Varsayılan olarak, Visual Studio 'nun bu sürümüyle birlikte sunulan SQL Server örneği SQLEXPRESS adlı SQL Server Express.
Bağlantı dizesinin kullanmanız gereken sunucu bölümü ".\\SQLEXPRESS ".
C# Kodda bir bağlantı dizesi belirtirken `@` veya çift ters eğik çizgi "\\\\" önekli bir tam dize kullanmayı unutmayın.
