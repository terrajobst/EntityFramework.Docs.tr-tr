---
title: Günlük sağlayıcısı etkileyen değişiklikler - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998364"
---
# <a name="provider-impacting-changes"></a>Sağlayıcı etkileyen değişiklikler

Bu sayfa, çekme isteklerine tepki vermek için diğer veritabanı sağlayıcıları yazarları gerektirebilecek EF Core depoda yapılan bağlantılar içerir. Amaç bir başlangıç noktası mevcut üçüncü taraf veritabanı sağlayıcıları yazarları için sağlayıcının yeni bir sürüme güncelleştirirken sağlamaktır.

2.2 2.1 değişikliklerle bu günlük başlıyoruz. 2.1 önce kullandığımız [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) bizim sorunlar ve çekme istekleri etiketleri.

### <a name="21-----22"></a>2.1 2.2--->

#### <a name="test-only-changes"></a>Yalnızca test değişiklikleri

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Özelleştirilebilir SQL sınırlayıcı testlerinde izin ver
  * Katı olmayan kayan nokta karşılaştırmalar BuiltInDataTypesTestBase içinde izin değişiklikleri test etmek
  * Farklı bir SQL sınırlayıcı ile yeniden kullanılmasının gerekmesi sorgu testlerine izin vermek test değişiklikleri
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -İlişkisel belirtimi test DbFunction testleri ekleyin.
  * Örneğin bu testleri tüm veritabanı sağlayıcıları karşı çalıştırabilirsiniz
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Zaman uyumsuz test temizleme
  * Kaldırma `Wait` çağrıları, zaman uyumsuz kapatın ve bazı test yöntemlerini yeniden adlandırıldı
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Günlük kaydı ve test altyapısı birleştirin
  * Eklenen `CreateListLoggerFactory` ve tepki vermek için bu testleri kullanarak sağlayıcılarının gerektiren bazı önceki günlük altyapısı kaldırıldı
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Zaman uyumlu ve zaman uyumsuz olarak daha fazla sorgu testleri çalıştırma
  * Test adları ve hesaba katacak şekilde değişti, hangi tepki vermek için bu testleri kullanarak sağlayıcılarının gerektirir
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Gezintiler ComplexNavigations modelinde yeniden adlandırma
  * Bu testleri kullanarak sağlayıcılarının react gerekebilir
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Laboratuvardaki işlevsel testlere disposing yerine havuza bağlamını döndürür
  * Bu değişiklik, bazı test tepki vermek sağlayıcıları gerektirebilecek yeniden düzenleme içerir


#### <a name="test-and-product-code-changes"></a>Test ve ürün kodu değişiklikleri

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -RelationalTypeMapping.Clone yöntemleri birleştirebilirsiniz.
  * 2.1 bir basitleştirme türetilmiş sınıflarda için izin verilen RelationalTypeMapping için değişiklikler. İnanıyoruz yok sağlayıcıları bu bozucu, ancak sağlayıcıları yararlanabilir bu değişiklik, türetilen türde de eşleme sınıfları.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Etiketli veya adlandırılmış sorguları
  * LINQ sorguları etiketleme ve bu etiketleri SQL açıklamaları olarak göstermek zorunda için altyapı ekler. Bu SQL oluşturma tepki vermek sağlayıcıları gerektirebilir.
