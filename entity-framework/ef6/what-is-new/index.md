---
title: -Yenilikler EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 9eb6a916d36ed41fcaea74564395695c048ab0f5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996341"
---
# <a name="whats-new-in-ef6"></a>EF6 yenilikler

En son özellikleri ve kararlılığı en yüksek alma emin olmak için Entity Framework ' son yayınlanan sürümünü kullanmanızı öneririz.
Ancak, önceki bir sürümü kullanmanız gerekebilir veya yeni yayın öncesi en son yenilikleri denemek isteyebilirsiniz unutmayın.
EF belirli sürümlerini yüklemek için bkz: [alma Entity Framework](~/ef6/fundamentals/install.md).

Bu sayfa, her yeni yayın üzerinde dahil özelliklerini içermektedir.

## <a name="recent-releases"></a>Yeni sürümler

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Visual Studio 2017 15.7 EF araçları güncelleştirme

Mayıs 2018'de Visual Studio 2017 15.7 bir parçası olarak EF Araçları'nın güncelleştirilmiş bir sürümünü çıkardık.
Sık karşılaşılan bazı sorunlu noktaları için geliştirmeler içerir:

- Birkaç kullanıcı arabirimi erişilebilirliği hata düzeltmeleri
- Geçici çözüm için mevcut veritabanlarından modelleri oluştururken, SQL Server performans regresyon [#4](https://github.com/aspnet/entityframework6/issues/4)
- SQL Server'da büyük modeller için modelleri güncelleştirme desteği [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Başka bir geliştirme bu EF Araçları'nın bu yeni sürümün bu EF 6.2 çalışma zamanı bir modeli içerisinde yeni bir proje oluştururken yüklemesidir. Visual Studio'nun eski sürümleriyle ilgili NuGet paketi sürümünü yükleyerek EF 6.2 çalışma zamanı (aynı zamanda EF'ın önceki bir sürümü) kullanmak da mümkündür.

### <a name="ef-62-runtime"></a>EF 6.2 çalışma zamanı

EF 6.2 çalışma zamanı için NuGet'ın Ekim 2017'de yayınlanmıştır.
Harika, teşekkür ederiz. topluluğumuza açık kaynağa katkıda bulunanlar, çalışmalarınızda kullanmanız için bölüm, çok sayıda EF 6.2 içerir [düzeltmeleri hataları](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) ve [Ürün geliştirmeleri](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

EF 6.2 çalışma zamanı etkileyen en önemli değişikliklerin kısa bir listesi aşağıda verilmiştir:

- Başlatma süresi yükleyerek azaltmak tamamlandı ilk kod modelleri kalıcı önbellekten [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Dizinleri tanımlamak için Fluent API'si [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- SQL BENZERİ Çevir LINQ sorguları yazma etkinleştirmek için DbFunctions.Like() [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe artık destekliyor - komut dosyası seçeneği [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 bir sırada bir SQL Server tarafından oluşturulan anahtar değerleri artık çalışabilir [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- SQL Azure yürütme stratejisi için geçici hataları güncelleştirme listesi [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Hata: "SqlParameter zaten başka bir SqlParameterCollection tarafından yer ile" sorguları veya SQL komutları yeniden deneme başarısız [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Hata: DbQuery.ToString() değerlendirmesini sık hata ayıklayıcıda zaman aşımına [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Gelecek sürümler

EF6'ın gelecek sürümünde hakkında daha fazla bilgi için lütfen bakmak bizim [yol haritası](roadmap.md).

## <a name="past-releases"></a>Eski sürümler

[Geçmiş sunumlar](past-releases.md) sayfası bir arşiv EF ve her bir yayının sunulan büyük özelliklerin tüm önceki sürümlerini içerir.
