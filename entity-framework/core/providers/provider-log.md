---
title: Sağlayıcı günlüğü-etkileyen değişiklikler-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: b911a2da493e20c4e4ce6f1e25024bd0efd38b44
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417824"
---
# <a name="provider-impacting-changes"></a>Sağlayıcıları etkileyen değişiklikler

Bu sayfa, diğer veritabanı sağlayıcılarının yazarlarının tepki vermesini gerektirebilecek EF Core depo üzerinde yapılan çekme isteklerinin bağlantılarını içerir. Amaç, sağlayıcıları yeni bir sürüme güncelleştirirken var olan üçüncü taraf veritabanı sağlayıcılarının yazarları için bir başlangıç noktası sağlamaktır.

Bu günlüğü 2,1 ile 2,2 arasındaki değişikliklerle başlatıyoruz. 2,1 ' den önce, sorunlarımızda ve çekme isteklerimizde [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etiketlerini kullandık.

## <a name="22-----30"></a>2,2---> 3,0

[Uygulama düzeyindeki](../what-is-new/ef-core-3.0/breaking-changes.md) en büyük değişikliklerin çoğunun sağlayıcıları da etkileyeceğini unutmayın.

* <https://github.com/aspnet/EntityFrameworkCore/pull/14022>
  * Kullanımdan kaldırılan API 'Ler ve daraltılmış isteğe bağlı parametre aşırı yüklemeleri kaldırıldı
  * DatabaseColumn. GetUnderlyingStoreType () kaldırıldı
* <https://github.com/aspnet/EntityFrameworkCore/pull/14589>
  * Kullanımdan kaldırılan API 'Ler kaldırıldı
* <https://github.com/aspnet/EntityFrameworkCore/pull/15044>
  * Temel uygulamadaki birkaç hatayı düzeltmek için gereken davranış değişikliklerinden dolayı CharTypeMapping alt sınıfları bozulmuş olabilir.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15090>
  * Idatabasemodelfactory için bir temel sınıf eklendi ve sonraki kesmeleri azaltmak için bir parametre nesnesi kullanmak üzere güncelleştirildi.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15123>
  * Sonraki kesmeleri azaltmak için Migrationsssıngenerator içinde kullanılan parametre nesneleri.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14972>
  * Günlük düzeylerinin açık yapılandırması, sağlayıcıların kullandığı API 'Lerde bazı değişiklikler gerektirdi. Özellikle, sağlayıcılar günlüğe kaydetme altyapısını doğrudan kullanıyorsa bu değişiklik, bu değişikliği kesintiye uğratır. Ayrıca, altyapıyı kullanan (genel olacak) bir sağlayıcının `LoggingDefinitions` veya `RelationalLoggingDefinitions`türetmesine gerek olacaktır. Örnekler için SQL Server ve bellek içi sağlayıcılara bakın.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15091>
  * Çekirdek, Ilişkisel ve soyutlamalar kaynak dizeleri artık geneldir.
  * `CoreLoggerExtensions` ve `RelationalLoggerExtensions` artık genel. Sağlayıcılar, çekirdek veya ilişkisel düzeyde tanımlanan olayları günlüğe kaydederken bu API 'Leri kullanmalıdır. Günlük kaynaklarına doğrudan erişmeyin; Bunlar hala iç.
  * `IRawSqlCommandBuilder`, tek bir hizmetten kapsamlı bir hizmete değiştirilmiştir
  * `IMigrationsSqlGenerator`, tek bir hizmetten kapsamlı bir hizmete değiştirilmiştir
* <https://github.com/aspnet/EntityFrameworkCore/pull/14706>
  * İlişkisel komutlar oluşturmaya yönelik altyapı, sağlayıcılar tarafından güvenli bir şekilde kullanılabilmesi ve yeniden düzenlenmiş çok az olması için genel kullanıma açıldı.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14733>
  * `ILazyLoader`, kapsamlı bir hizmetten geçici bir hizmete değiştirilmiştir
* <https://github.com/aspnet/EntityFrameworkCore/pull/14610>
  * `IUpdateSqlGenerator`, kapsamlı bir hizmetten tek bir hizmete değiştirilmiştir
  * Ayrıca, `ISingletonUpdateSqlGenerator` kaldırılmıştır
* <https://github.com/aspnet/EntityFrameworkCore/pull/15067>
  * Sağlayıcılar tarafından kullanılmakta olan çok sayıda iç kod, genel kullanıma sunuldu
  * Bu, kullanıma sunulan yerlerden bir şekilde katlandığından `IndentedStringBuilder` başvurulmayacak.
  * `NonCapturingLazyInitializer` kullanımları BCL `LazyInitializer` ile değiştirilmelidir
* <https://github.com/aspnet/EntityFrameworkCore/pull/14608>
  * Bu değişiklik, uygulama bölünmesi değişiklikleri belgesinde tam olarak ele alınmıştır. Sağlayıcılar için bu durum daha fazla etkilenebilir çünkü EF Core test genellikle bu soruna yol açabilir, bu nedenle test altyapısı bu sorunu daha düşük hale getirmek üzere değiştirilmiştir.
* <https://github.com/aspnet/EntityFrameworkCore/issues/13961>
  * `EntityMaterializerSource` basitleştirildi
* <https://github.com/aspnet/EntityFrameworkCore/pull/14895>
  * StartsWith çevirisi, sağlayıcıların istediği/tepki vermesini gerektirebilecek bir şekilde değiştirilmiştir
* <https://github.com/aspnet/EntityFrameworkCore/pull/15168>
  * Kural kümesi Hizmetleri değişti. Sağlayıcılar bundan böyle "ProviderConventionSet" veya "RelationalConventionSet" öğesinden kalıtımla almalıdır.
  * Özelleştirmeler `IConventionSetCustomizer` Hizmetleri aracılığıyla eklenebilir, ancak bu, sağlayıcılar değil diğer uzantılar tarafından kullanılmak üzere tasarlanmıştır.
  * Çalışma zamanında kullanılan kurallar `IConventionSetBuilder`çözümlenmelidir.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15288>
  * Veri dengeli dağıtımı, iç türleri kullanma gereksinimini ortadan kaldırmak için genel bir API 'ye yeniden düzenlenmiş. Bu, yalnızca ilişkisel olmayan sağlayıcıları etkilemelidir, çünkü dengeli dağıtım, tüm ilişkisel sağlayıcılar için temel ilişkisel sınıf tarafından işlenir.

## <a name="21-----22"></a>2,1---> 2,2

### <a name="test-only-changes"></a>Yalnızca test değişiklikleri

* <https://github.com/aspnet/EntityFrameworkCore/pull/12057>-testlerde özelleştirilebilir SQL ile ölçümlere Izin ver
  * Buıldatatypestestbase 'de katı olmayan kayan nokta karşılaştırmaları sağlayan test değişiklikleri
  * Sorgu testlerinin farklı SQL farklıölçerler ile yeniden kullanılmasına izin veren test değişiklikleri
* <https://github.com/aspnet/EntityFrameworkCore/pull/12072>-ilişkisel belirtim testlerine DbFunction testleri ekleme
  * Böylece, bu testlerin tüm veritabanı sağlayıcılarına karşı çalıştırılabilmesi için
* <https://github.com/aspnet/EntityFrameworkCore/pull/12362>-zaman uyumsuz test Temizleme
  * `Wait` çağrılarını, gereksiz zaman uyumsuz ve yeniden adlandırılmış bazı test yöntemlerini kaldırın
* <https://github.com/aspnet/EntityFrameworkCore/pull/12666>-günlük kaydı test altyapısını bütünleştirme
  * `CreateListLoggerFactory` eklendi ve önceki bir günlüğe kaydetme altyapısı kaldırıldı, bu da bu testleri kullanan sağlayıcıların tepki vermesini gerektirir
* <https://github.com/aspnet/EntityFrameworkCore/pull/12500>-eşzamanlı ve zaman uyumsuz olarak daha fazla sorgu testi çalıştırın
  * Test adları ve düzenleme değiştirildi, bu da bu testleri kullanan sağlayıcıların tepki vermesini gerektirir
* <https://github.com/aspnet/EntityFrameworkCore/pull/12766>-Complexgezginler modelinde gezinmeleri yeniden adlandırma
  * Bu testlerin kullanıldığı sağlayıcıların işlem yapması gerekebilir
* <https://github.com/aspnet/EntityFrameworkCore/pull/12141>-işlev testlerinde elden atılıyor yerine bağlamı havuza döndürün
  * Bu değişiklik, sağlayıcıların tepki vermesini gerektirebilecek bazı test yeniden düzenlemesi içerir

### <a name="test-and-product-code-changes"></a>Test ve ürün kodu değişiklikleri

* <https://github.com/aspnet/EntityFrameworkCore/pull/12109>-RelationalTypeMapping. Clone yöntemlerini birleştirme
  * Türetilmiş sınıflarda bir basitleştirme için izin verilen RelationalTypeMapping 'a 2,1 değişiklik. Bunun sağlayıcılara yönelik olduğunu düşünmedik, ancak sağlayıcılar türetilmiş tür eşleme sınıflarında bu değişiklikten faydalanabilir.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12069> etiketli veya adlandırılmış sorgular
  * LINQ sorgularını etiketlemek için altyapı ekler ve bu etiketlerin SQL 'de açıklama olarak görünmesini sağlar. Bu, sağlayıcıların SQL Generation 'e tepki vermesini gerektirebilir.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13115>-Sal aracılığıyla uzamsal verileri destekleme
  * Tür eşlemelerinin ve üye çeviricilerin sağlayıcının dışına kaydedilmesini sağlar
    * Sağlayıcılar temel çağırmalıdır. Çalışması için ITypeInfo kaynak uygulamasında FindMapping ()
  * Sağlayıcılarınızda sağlayıcılar arasında tutarlı olan uzamsal destek eklemek için bu kalıbı izleyin.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13199>-hizmet sağlayıcısı oluşturmak için gelişmiş hata ayıklama ekleme
  * DbContextOptionsExtensions 'in, iç hizmet sağlayıcısının neden yeniden derlenmekte olduğunu anlamalarına yardımcı olabilecek yeni bir arabirim uygulamasına olanak tanır
* <https://github.com/aspnet/EntityFrameworkCore/pull/13289>-sistem durumu denetimleri tarafından kullanılmak üzere CanConnect API 'SI ekler
  * Bu PR, veritabanının kullanılabilir olup olmadığını anlamak için ASP.NET Core sistem durumu denetimleri tarafından kullanılacak `CanConnect` kavramını ekler. Varsayılan olarak, ilişkisel uygulama yalnızca `Exist`çağırır, ancak sağlayıcılar gerektiğinde farklı bir şekilde uygulayabilir. İlişkisel olmayan sağlayıcıların, sistem durumu denetiminin kullanılabilir olması için yeni API 'yi uygulaması gerekir.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13306>-DbParameter boyutunu ayarlayamayan temel RelationalTypeMapping 'ı güncelleştirme
  * Kesmeye neden olabileceği için varsayılan olarak ayar boyutunu durdurun. Boyut ayarlanması gerekiyorsa, sağlayıcıların kendi mantığını eklemesi gerekebilir.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13372>-RevEng: her zaman ondalık sütunlar için sütun türü belirt
  * Kural tarafından yapılandırmak yerine, her zaman yapı iskelesi kodundaki ondalık sütunlar için sütun türünü yapılandırın.
  * Sağlayıcılar, sonunda herhangi bir değişiklik gerektirmemelidir.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13469>-SQL CASE ifadeleri oluşturmak için CaseExpression ekler
* <https://github.com/aspnet/EntityFrameworkCore/pull/13648>-bağımsız değişkenlerin ve sonuçların depolama türü çıkarımını geliştirmek için SqlFunctionExpression üzerinde tür eşlemelerini belirtme özelliği ekler.
