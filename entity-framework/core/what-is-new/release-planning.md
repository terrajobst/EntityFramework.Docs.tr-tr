---
title: EF Core sürüm planlaması
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417338"
---
# <a name="release-planning-process"></a>Sürüm planlama süreci

Sık sık belirli bir sürümde belirli özellikleri nasıl seçtiğimiz hakkında sorular alabiliriz.
Bu belge, kullandığımız işlemi özetliyor.
Planlamak için daha iyi yollar buldukça süreç sürekli olarak gelişmektedir, ancak genel fikirler aynı kalır.

## <a name="different-kinds-of-releases"></a>Farklı sürüm türleri

Farklı türde sürümler farklı türde değişiklikler içerir.
Bu da sürüm planlaması serbest bırakma farklı türleri için farklı olduğu anlamına gelir.

### <a name="patch-releases"></a>Yama bültenleri

Yama sürümleri sürümün yalnızca "yama" kısmını değiştirir.
Örneğin, EF Core 3.1. **1,** EF Core 3.1'de bulunan sorunları yakan bir sürümdür. 0 ' dan **fazla.**

Yama sürümleri kritik müşteri hatalarını düzeltmek için tasarlanmıştır.
Bu, yama sürümlerinin yeni özellikler içermemesi anlamına gelir.
Özel durumlar dışında yama sürümlerinde API değişikliklerine izin verilmez.

Bir yama sürümünde değişiklik yapmak için çubuk çok yüksektir.
Bunun nedeni, yama sürümlerinin yeni hatalar getirmemesi açısından önemlidir.
Bu nedenle, karar süreci yüksek değer ve düşük risk vurgulamaktadır.

Biz daha fazla bir sorun yama olasılığı vardır:
  * Birden fazla müşteriyi etkiliyor
  * Bir önceki sürümden bir gerileme
  * Hata veri bozulmasına neden olur

Bir sorunu düzeltme olasılığımız daha düşüktür:
  * Makul geçici çözüm geçici çözüm
  * Düzeltme başka bir şey kırma riski yüksektür
  * Hata bir köşe çantasında.

Bu çubuk, [uzun vadeli bir destek (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) sürümü ömrü boyunca kademeli olarak yükselir. Bunun nedeni LTS sürümlerinin kararlılığı vurgulamasıdır.

Bir sorunu düzeltmeme konusundaki son karar Microsoft'taki .NET Directors tarafından verilir.

### <a name="minor-releases"></a>Küçük sürümler

Küçük sürümler, sürümün yalnızca "küçük" kısmını değiştirir.
Örneğin, EF Core 3. **1**.0, EF Core 3'te geliştiren bir sürümdür. **0**.0.

Küçük sürümler:
* Önceki sürümün kalitesini ve özelliklerini geliştirmeyi amaçlamaktadır
* Genellikle hata düzeltmeleri ve yeni özellikler içerir
* Kasıtlı kırma değişikliklerini eklemeyin
* NuGet'e itilen birkaç ön sürüm önizlemesi var

### <a name="major-releases"></a>Önemli sürümler

Büyük sürümler EF "büyük" sürüm numarasını değiştirir.
Örneğin, EF Core **3**.0.0, EF Core 2.2.x üzerinde büyük bir adım atan önemli bir sürümdür.

Başlıca sürümler:
* Önceki sürümün kalitesini ve özelliklerini geliştirmeyi amaçlamaktadır
* Genellikle hata düzeltmeleri ve yeni özellikler içerir
  * Yeni özelliklerden bazıları, EF Core'un çalışma şeklindeki temel değişiklikler olabilir
* Genellikle kasıtlı kesme değişiklikleri içerir
  * Son dakika değişiklikleri, öğrendiğimiz gibi gelişen EF Core'un gerekli bir parçasıdır
  * Ancak, potansiyel müşteri etkisi nedeniyle herhangi bir kırılma değişiklik yapma konusunda çok dikkatli düşünüyorum. Geçmişteki değişiklikleri kırmak konusunda çok agresif olabiliriz. İleriye dönük olarak, uygulamaları bozan değişiklikleri en aza indirmek ve veritabanı sağlayıcılarını ve uzantılarını kıran değişiklikleri azaltmak için çalışacağız.
* NuGet'e itilen birçok ön sürüm önizlemesi var

## <a name="planning-for-majorminor-releases"></a>Büyük/küçük sürümler için planlama

### <a name="github-issue-tracking"></a>GitHub sorun izleme

GitHub[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)( ) tüm EF Core planlaması için gerçeğin kaynağıdır.

GitHub'da sorunlar:

* Bir durum
  * [Açık](https://github.com/dotnet/efcore/issues) sorunlar ele alınmadı.
  * [Kapalı](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) sorunlar ele alındı.
    * Düzeltilen tüm sorunlar [kapalı sabit ile etiketlenir.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed) Kapalı sabit etiketli bir sorun düzeltilir ve birleştirilir, ancak yayımlanmamış olabilir.
    * Diğer `closed-` etiketler, bir sorunu kapatmak için başka nedenler gösterir. Örneğin, yinelenenler kapalı yinelenen olarak etiketlenir.
* A türü
  * [Hatalar](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) hataları temsil ediyor.
  * [Geliştirmeler,](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) varolan özelliklerdeki yeni özellikleri veya daha iyi işlevselliği temsil eder.
* Bir kilometre taşı
  * [Kilometre taşı olmayan sorunlar](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) takım tarafından değerlendirilmektedir. Konuyla ilgili ne yapılacağına ilişkin karar henüz verilmedi veya kararda bir değişiklik düşünüldü.
  * [Biriktirme Listesi kilometre taşındaki sorunlar,](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) EF ekibinin gelecekteki bir sürümde üzerinde çalışmayı düşüneceği öğelerdir
    * Biriktirme listesindeki sorunlar, bu çalışma öğesinin bir sonraki sürüm listesinde üst sıralarda olduğunu belirten [bir sonraki sürüm için düşününle etiketlenebilir.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release)
  * Sürümlenmiş bir kilometre taşındaki açık sorunlar, takımın bu sürümde üzerinde çalışmayı planladığı öğelerdir. Örneğin, [bunlar EF Core 5.0 için çalışmayı planladığımız konulardır.](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0)
  * Sürümlenmiş bir kilometre taşındaki kapalı sorunlar, bu sürüm için tamamlanan sorunlardır. Sürümün henüz yayımlanmamış olabileceğini unutmayın. Örneğin, [bunlar EF Core 3.0 için tamamlanan sorunlardır.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed)
* Oy!
  * Oylama, bir sorunun sizin için önemli olduğunu göstermenin en iyi yoludur.
  * Oy vermek için, konuya bir "thumbs-up" 👍 eklemeniz gerekiyor. Örneğin, [bunlar en çok oylanan konulardır](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * Değer kattığını düşünüyorsanız, lütfen özelliğe ihtiyacınız olan belirli nedenleri açıklayan yorum da yapın. "+1" veya benzeri yorum lama değer katmaz.

### <a name="the-planning-process"></a>Planlama süreci

Planlama işlemi, biriktirme listesinin en çok istenen özelliklerini almaktan daha fazla ilgilidir.
Bunun nedeni, birden çok paydaştan birden çok şekilde geri bildirim toplamamızdır.
Daha sonra bir sürümü şu temele göre şekillendiriz:

* Müşterilerden giriş
* Diğer paydaşlardan giriş
* Stratejik yön
* Kullanılabilir kaynaklar
* Zamanlama

Sorduğumuz sorulardan bazıları şunlardır:

1. **Kaç geliştiriciler bu özelliği kullanacağını ve ne kadar daha iyi kendi uygulamaları veya deneyim yapacak düşünüyorum?** Bu soruyu cevaplamak için, birçok kaynaktan geri bildirim toplarız — Yorumlar ve oylar bu kaynaklardan biridir. Önemli müşterilerle belirli etkileşimler de başka bir şeydir.

2. **Bu özelliği henüz uygulamazsak, kişilerin kullanabileceği geçici çözüm nedir?** Örneğin, birçok geliştirici, yerel çok-çok destek eksikliğini aşmak için bir birleştirme tablosunu eşleyebilir. Açıkçası, tüm geliştiriciler bunu yapmak istiyorum, ama birçok olabilir, ve bu bizim kararda bir faktör olarak sayar.

3. **Bu özelliğin uygulanması, bizi diğer özellikleri uygulamaya yaklaştıracak şekilde EF Core mimarisini geliştirir mi?** Biz diğer özellikler için yapı taşları olarak hareket özellikleri lehine eğilimindedir. Örneğin, özellik torbası varlıkları çok-çok destek doğru hareket yardımcı olabilir ve varlık yapıcılar bizim tembel yükleme desteği sağladı.

4. **Özellik genişletilebilirlik noktası mı?** Geliştiricilerin kendi davranışlarını kancaya takmalarını ve eksik işlevleri telafi etmelerini sağladığından, genişletilebilirlik noktalarını normal özelliklere göre tercih etme eğilimindeyizdir.

5. **Diğer ürünlerle birlikte kullanıldığında özelliğin sinerjisi nedir?** EF Core'u Visual Studio'nun en son sürümü, Microsoft Azure gibi diğer ürünlerle birlikte kullanma deneyimini etkinleştiren veya önemli ölçüde iyileştiren özelliklerden yanayız.

6. **Bir özellik üzerinde çalışabilecek kişilerin becerileri nelerdir ve bu kaynaklardan en iyi şekilde nasıl yararlanabiliriz?** EF ekibinin her üyesi ve topluluk katılımcılarımız farklı alanlarda farklı deneyim seviyelerine sahiptir, bu nedenle buna göre planlamayapmak zorundayız. GroupBy çevirileri veya çok-çok gibi belirli bir özellik üzerinde çalışmak için "tüm eller güvertede" olmasını istesek bile, bu pratik olmaz.
