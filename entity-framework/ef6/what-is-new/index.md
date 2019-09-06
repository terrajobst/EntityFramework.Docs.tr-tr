---
title: Yenilikler-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271439"
---
# <a name="whats-new-in-ef6"></a>EF6 'deki yenilikler

En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.
Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.
EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).

Bu sayfa, her yeni sürümde bulunan özellikleri belgeler.

## <a name="recent-releases"></a>Son yayınlar

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Visual Studio 'da EF araçları güncelleştirmesi 2017 15,7

Mayıs 2018 ' de, Visual Studio 2017 15,7 kapsamında EF araçlarının güncelleştirilmiş bir sürümünü yayımladık.
Bu, bazı yaygın sorun noktaları için iyileştirmeler içerir:

- Birkaç kullanıcı arabirimi erişilebilirlik hatası düzeltmeleri
- Mevcut veritabanlarından model oluştururken SQL Server performans gerileme için geçici çözüm [#4](https://github.com/aspnet/entityframework6/issues/4)
- SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185) daha büyük modellerin modellerini güncelleştirme desteği

EF araçları 'nın bu yeni sürümündeki diğer bir geliştirme, yeni bir projede model oluştururken EF 6,2 çalışma zamanını yüklemektedir. Visual Studio 'nun eski sürümlerinde, NuGet paketinin karşılık gelen sürümünü yükleyerek EF 6,2 çalışma zamanının (EF 'in geçmiş sürümleri) kullanılması mümkündür.

### <a name="ef-62-runtime"></a>EF 6,2 çalışma zamanı

EF 6,2 çalışma zamanı, Ekim 2017 ' de NuGet 'e yayımlandı.
Açık kaynak katkıda bulunanlar topluluğumuza yönelik harika bir bölümde Teşekkür ederiz, EF 6,2 birçok [hata](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) düzeltmesi ve [ürün geliştirmesi](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)içerir.

EF 6,2 çalışma zamanını etkileyen en önemli değişikliklerin kısa bir listesi aşağıdadır:

- Kalıcı bir önbellekten tamamlanan kod ilk modellerini yükleyerek başlangıç süresini düşürün [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Dizinleri tanımlamak için akıcı API [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241) gıbı çeviren LINQ sorguları yazmayı etkinleştirmek Için dbfunctions. like ()
- Migrate. exe artık-Script seçeneğini destekliyor [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6, artık SQL Server bir sıra tarafından oluşturulan anahtar değerleriyle çalışabilir [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- SQL Azure yürütme stratejisi için geçici hataların listesini güncelleştirin [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Tiva Sorgular veya SQL komutlarının yeniden denenme işlemi "SqlParameter zaten başka bir SqlParameterCollection tarafından bulunuyor" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Tiva DbQuery. ToString (), hata ayıklayıcıda sıklıkla zaman aşımına uğruyor [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Gelecek yayınlar

EF6 'in gelecek sürümü hakkında daha fazla bilgi için lütfen [yol haritası](roadmap.md)' na bakın.

## <a name="past-releases"></a>Geçmiş yayınlar

[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.
