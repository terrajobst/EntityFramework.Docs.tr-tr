---
title: Günlük sağlayıcısı etkileyen değişiklikler - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 229c15ec0402e1706318593a099236f723d80595
ms.sourcegitcommit: ab847dd881d51122e695b7cd8c025fcf3a5a9033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58678387"
---
# <a name="provider-impacting-changes"></a>Sağlayıcı etkileyen değişiklikler

Bu sayfa, çekme isteklerine tepki vermek için diğer veritabanı sağlayıcıları yazarları gerektirebilecek EF Core depoda yapılan bağlantılar içerir. Amaç bir başlangıç noktası mevcut üçüncü taraf veritabanı sağlayıcıları yazarları için sağlayıcının yeni bir sürüme güncelleştirirken sağlamaktır.

2.2 2.1 değişikliklerle bu günlük başlıyoruz. 2.1 önce kullandığımız [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) bizim sorunlar ve çekme istekleri etiketleri.

## <a name="22-----30"></a>2.2 ---> 3.0

Birçoğu Not [uygulama düzeyi bozucu değişiklikler](../what-is-new/ef-core-3.0/breaking-changes.md) sağlayıcıları de etkiler.

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * Kaldırılan eski API'ler ve daraltılmış isteğe bağlı parametresi aşırı yüklemeler
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * Eski API'ler kaldırıldı
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * Adornerset'in alt CharTypeMapping temel uygulamada birkaç hataları düzeltmek için gereken davranış değişiklikleri nedeniyle kesilmiş olabilir.
* https://github.com/aspnet/EntityFrameworkCore/pull/15090
  * Bir temel sınıf için IDatabaseModelFactory eklenir ve gelecekteki sonları azaltmak için bir sayıdır nesnesi kullanacak şekilde güncelleştirildi.
* https://github.com/aspnet/EntityFrameworkCore/pull/15123
  * Parametresi nesnelerini MigrationsSqlGenerator gelecekteki sonları azaltmak için kullanılır.
* https://github.com/aspnet/EntityFrameworkCore/pull/14972
  * Açık günlük düzeyleri yapılandırılmasını bazı değişiklikler sağlayıcılarını kullanarak API'leri için gereklidir. Özellikle, sağlayıcıları günlük altyapısı doğrudan kullanıyorsanız, bu değişiklik kullanan kesilebilir. Ayrıca, (olacağı ortak) altyapısını kullanan sağlayıcıları ileriye dönük türetmek gerekir `LoggingDefinitions` veya `RelationalLoggingDefinitions`. SQL Server ve bellek içi sağlayıcıları örnekler için bkz.
* https://github.com/aspnet/EntityFrameworkCore/pull/15091
  * Çekirdek, ilişkisel ve Soyutlamalarına kaynak dizeleri herkese açık.
  * `CoreLoggerExtensions` ve `RelationalLoggerExtensions` genel sunulmuştur. Sağlayıcılar, çekirdek veya ilişkisel düzeyinde tanımlanan olayları günlüğe kaydedilirken bu API'leri kullanmanız gerekir. Günlüğe kaydetme kaynaklara doğrudan erişemez; Bunlar yine de iç.
  * `IRawSqlCommandBuilder` kapsamlı bir hizmet için bir singleton hizmetinden değişti
  * `IMigrationsSqlGenerator` kapsamlı bir hizmet için bir singleton hizmetinden değişti
* https://github.com/aspnet/EntityFrameworkCore/pull/14706
  * Böylece onu güvenli bir şekilde sağlayıcıları tarafından kullanılan ve biraz yeniden düzenlenen ilişkisel komutlarını oluşturmak için altyapı genel yapıldı.
  * `IRelationalCommandBuilderFactory`kapsamlı bir hizmet için singleton hizmetinden değişti
  * `IShaperCommandContextFactory` kapsamlı bir hizmet için singleton hizmetinden değişti
  * `ISelectExpressionFactory` kapsamlı bir hizmet için singleton hizmetinden değişti
* https://github.com/aspnet/EntityFrameworkCore/pull/14733
  * `ILazyLoader` kapsamlı bir hizmet için geçici bir hizmet olarak değiştirildi
* https://github.com/aspnet/EntityFrameworkCore/pull/14610
  * `IUpdateSqlGenerator` tek bir hizmet kapsamlı bir hizmetten değişti
  * Ayrıca, `ISingletonUpdateSqlGenerator` kaldırıldı
* https://github.com/aspnet/EntityFrameworkCore/pull/15067
  * Sağlayıcıları tarafından kullanılan iç kod artık genel yapıldı
  * Artık başvurmak için necssary olmalıdır `IndentedStringBuilder` olduğundan, kullanıma sunulan basamak dışında belirlenen faktörleri
  * Açık olan `NonCapturingLazyInitializer` ile değiştirilmelidir `LazyInitializer` BCL gelen
* https://github.com/aspnet/EntityFrameworkCore/pull/14608
  * Bu değişiklik, bozucu değişiklikleri belge uygulamada tam olarak ele alınmıştır. Sağlayıcılar için test EF core genellikle bu sorunu basmak için büyük olasılıkla daha az oluşturan ve test altyapısı değiştirildi sağladığından daha fazla etkiliyor olabilir.
* https://github.com/aspnet/EntityFrameworkCore/issues/13961
  * `EntityMaterializerSource` basitleştirildi
* https://github.com/aspnet/EntityFrameworkCore/pull/14895
  * StartsWith çeviri, sağlayıcıları istediğiniz/react gerek şekilde değiştirildi
* https://github.com/aspnet/EntityFrameworkCore/pull/15168
  * Kural kümesi Hizmetleri değişti. Sağlayıcıları artık "ProviderConventionSet" veya "RelationalConventionSet" devralır.
  * Özelleştirmeleri aracılığıyla eklenebilir `IConventionSetCustomizer` Hizmetleri, ancak bu yok sağlayıcıları diğer uzantıları tarafından kullanılmak üzere tasarlanmıştır.
  * Çalışma zamanında kullanılan kuralları gelen çözümlenen `IConventionSetBuilder`.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Yalnızca test değişiklikleri

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) -Özelleştirilebilir SQL sınırlayıcı testlerinde izin ver
  * Katı olmayan kayan nokta karşılaştırmalar BuiltInDataTypesTestBase içinde izin değişiklikleri test etmek
  * Farklı bir SQL sınırlayıcı ile yeniden kullanılmasının gerekmesi sorgu testlerine izin vermek test değişiklikleri
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -İlişkisel belirtimi test DbFunction testleri ekleyin.
  * Örneğin bu testleri tüm veritabanı sağlayıcıları karşı çalıştırabilirsiniz
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Zaman uyumsuz test temizleme
  * Kaldırma `Wait` çağrıları, zaman uyumsuz kapatın ve bazı test yöntemlerini yeniden adlandırıldı
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Günlük kaydı ve test altyapısı birleştirin
  * Eklenen `CreateListLoggerFactory` ve tepki vermek için bu testleri kullanarak sağlayıcılarının gerektiren bazı önceki günlük altyapısı kaldırıldı
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) -Zaman uyumlu ve zaman uyumsuz olarak daha fazla sorgu testleri çalıştırma
  * Test adları ve hesaba katacak şekilde değişti, hangi tepki vermek için bu testleri kullanarak sağlayıcılarının gerektirir
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) -Gezintiler ComplexNavigations modelinde yeniden adlandırma
  * Bu testleri kullanarak sağlayıcılarının react gerekebilir
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) -Laboratuvardaki işlevsel testlere disposing yerine havuza bağlamını döndürür
  * Bu değişiklik, bazı test tepki vermek sağlayıcıları gerektirebilecek yeniden düzenleme içerir


### <a name="test-and-product-code-changes"></a>Test ve ürün kodu değişiklikleri

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -RelationalTypeMapping.Clone yöntemleri birleştirebilirsiniz.
  * 2.1 bir basitleştirme türetilmiş sınıflarda için izin verilen RelationalTypeMapping için değişiklikler. İnanıyoruz yok sağlayıcıları bu bozucu, ancak sağlayıcıları yararlanabilir bu değişiklik, türetilen türde de eşleme sınıfları.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Etiketli veya adlandırılmış sorguları
  * LINQ sorguları etiketleme ve bu etiketleri SQL açıklamaları olarak göstermek zorunda için altyapı ekler. Bu SQL oluşturma tepki vermek sağlayıcıları gerektirebilir.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) -NTS aracılığıyla uzamsal veri desteği
  * Tür eşlemeleri ve üye sağlayıcısı dışında kaydedilecek çevirmenler izin verir.
    * Sağlayıcıları temel çağırmanız gerekir. Bunun çalışması kendi ITypeMappingSource uygulamasında FindMapping()
  * Uzamsal desteği sağlayıcıları arasında tutarlıdır sağlayıcınız eklemek için bu desen izleyin.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) -Hizmet sağlayıcısı oluşturmak için Gelişmiş hata ayıklama ekleme
  * Kişiler neden iç hizmet sağlayıcısı yeniden oluşturulmuş anlamanıza yardımcı olabilecek yeni bir arabirim uygulamak DbContextOptionsExtensions sağlar
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) -CanConnect API sistem durumu denetimleri tarafından kullanılmak üzere ekler
  * Bu çekme isteği kavramını ekler `CanConnect` ASP.NET Core sistem tarafından kullanılacak veritabanını kullanılabilir olup olmadığını belirlemek için denetler. Varsayılan olarak, yalnızca ilişkisel uygulama çağrıları `Exist`, ancak sağlayıcıları uygulayabilirsiniz farklı bir şey gerekirse. İlişkisel olmayan sağlayıcıları kullanılabilir olması sistem durumu denetimi için sırayla yeni API uygulamanız gerekir.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) -DbParameter boyutu ayarlamadığınızdan temel RelationalTypeMapping güncelleştirme
  * Kesme neden olabilir, varsayılan olarak boyutunu ayarlama durdurun. Sağlayıcıları boyutunu ayarlamak gerekiyorsa kendi mantığınızı eklemek gerekebilir.
* (https://github.com/aspnet/EntityFrameworkCore/pull/13372) -RevEng: Her zaman ondalık sütunlar için sütun türü belirtin
  * Kural gereği yapılandırma yerine iskele kurulan kodu ondalık sütunlar için sütun türü her zaman yapılandırın.
  * Sağlayıcıları kendi tarafında herhangi bir değişiklik yapılması gerekmez.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) -SQL çalışması ifadeleri oluşturmak için CaseExpression ekler
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) -Depolama tür çıkarımı bağımsız değişkenleri ve sonuçları geliştirmek için SqlFunctionExpression türü eşlemeleri belirtme olanağı ekler.
