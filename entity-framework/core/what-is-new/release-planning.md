---
title: Yayın planlamasını EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888063"
---
# <a name="release-planning-process"></a>Sürüm planlama süreci

Genellikle belirli bir yayına gitmek için belirli özellikleri nasıl seçtiğimiz hakkında sorular sunuyoruz.
Bu belge, kullandığımız süreci özetler.
Planlamada daha iyi yollar bulduğumuz sürece süreç sürekli gelişiyor, ancak genel fikirler aynı kalır.

## <a name="different-kinds-of-releases"></a>Farklı sürüm türleri

Farklı sürüm türleri farklı türlerde değişiklikler içerir.
Buna karşılık, yayın planlaması farklı sürüm türlerine göre farklılık gösterir.

### <a name="patch-releases"></a>Düzeltme Eki sürümleri

Düzeltme Eki sürümleri yalnızca sürümün "Patch" bölümünü değiştirir.
Örneğin, 3,1 EF Core. **1** EF Core 3,1 ' de bulunan düzeltme eki eklenen bir sürümdür. **0**.

Düzeltme Eki sürümleri, kritik müşteri hatalarını gidermeye yöneliktir.
Bu, düzeltme sürümlerinin yeni özellikler içermediği anlamına gelir.
Özel durumlar dışında, yama yayımlarıyla API değişikliklerine izin verilmez.

Bir yama sürümünde değişiklik yapmak için çubuk çok yüksektir.
Bunun nedeni, düzeltme sürümlerinin yeni hatalar sunmaz önemli öneme sahiptir.
Bu nedenle, karar işlemi yüksek değeri ve düşük riski vurgular.

Şu durumlarda bir sorunun yaması daha olasıdır:
  * Birden çok müşteriyi etkiliyor
  * Önceki bir sürümden gerileme
  * Hata veri bozulmasına neden oluyor

Şu durumlarda bir soruna düzeltme eki uygulama olasılığınız düşüktür:
  * Makul geçici çözümler mevcuttur
  * Bu düzeltmeyle, başka bir şeyi koparmadan yüksek risk vardır
  * Hata bir köşe durumunda

Bu çubuk, [uzun süreli destek (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) sürümünün ömrü boyunca yavaş bir şekilde arttığında. Bunun nedeni, LTS 'nin kararlılığı vurgularken oluşur.

Microsoft 'ta .NET yöneticileri tarafından bir sorunun düzeltme eki uygulanıp yapılmayacağı konusunda son karar.

### <a name="minor-releases"></a>Küçük yayınlar

Küçük yayınlar yalnızca sürümün "ikincil" kısmını değiştirir.
Örneğin, EF Core 3. **1**0, EF Core 3 ' te geliştiren bir sürümdür. **0**. 0.

Küçük yayınlar:
* , Önceki sürümün kalitesini ve özelliklerini geliştirmek için tasarlanmıştır
* Genellikle hata düzeltmeleri ve yeni özellikler içerir
* Kasıtlı olarak yapılan değişiklikleri dahil etme
* NuGet 'e birkaç yayın öncesi önizleme gönderildi

### <a name="major-releases"></a>Ana yayınlar

Ana yayınlar EF "ana" sürüm numarasını değiştirir.
Örneğin, EF Core **3**. 0,0, EF Core 2.2. x ' in üzerine büyük bir adım ileri getiren önemli bir sürümdür.

Ana yayınlar:
* , Önceki sürümün kalitesini ve özelliklerini geliştirmek için tasarlanmıştır
* Genellikle hata düzeltmeleri ve yeni özellikler içerir
  * Yeni özelliklerden bazıları EF Core çalışması için temel değişiklikler olabilir
* Genellikle kasıtlı olarak yapılan değişiklikleri dahil et
  * Önemli değişiklikler, öğrendiğimiz EF Core gelişen bir parçasıdır
  * Bununla birlikte, olası müşteri etkisi nedeniyle herhangi bir son değişiklik yapma konusunda çok dikkatli bir düşünün. Geçmişteki değişikliklere karşı çok ısruz olmuş olabilir. İleri giderek, uygulamaları kesen değişiklikleri en aza indirmek ve veritabanı sağlayıcılarını ve uzantılarını kesen değişiklikleri azaltmak için çaba duyuyoruz.
* NuGet 'e çok sayıda yayın öncesi önizleme gönderildi

## <a name="planning-for-majorminor-releases"></a>Büyük/küçük yayınlar için planlama

### <a name="github-issue-tracking"></a>GitHub sorun izleme

GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)), tüm EF Core planlama için Truth kaynağıdır.

GitHub 'daki sorunlar şunlardır:

* Bir durum
  * [Açık](https://github.com/dotnet/efcore/issues) sorunlar giderilmemiş.
  * [Kapatılan](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) sorunlar giderilmiştir.
    * Düzeltilen tüm sorunlar [kapalı-sabit ile etiketlenir](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). Kapalı-fixed ile etiketlenmiş bir sorun düzeltildi ve birleştirildi, ancak yayımlanmamış olabilir.
    * Diğer `closed-` Etiketler bir sorunu kapatmayla ilgili diğer nedenleri gösterir. Örneğin, yinelemeler kapalı-yinelenen ile etiketlendi.
* Bir tür
  * [Hatalar hataları temsil eder](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) .
  * [Geliştirmeler](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) , yeni özellikleri veya mevcut özelliklerde daha iyi işlevselliği temsil eder.
* Kilometre taşı
  * [Kilometre Taşsız sorunlar](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) takım tarafından değerlendiriliyor. Sorun ile ne yapacaklarınız henüz yapılmadı veya karardaki bir değişiklik göz önünde bulundurulmaktadır.
  * [Biriktirme listesindeki sorunlar](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) , EF ekibinin sonraki bir sürümde üzerinde çalışmayı düşünecağı öğelerdir
    * Biriktirme listesindeki sorunlar, bu iş öğesinin bir sonraki sürüm için listede yüksek olduğunu belirten, bir [sonraki-for-for-Release ile etiketlenebilir](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) .
  * Sürümlü kilometre taşlarındaki açık sorunlar, takımın o sürümde çalışmayı planlıyor olan öğelerdir. Örneğin, [bunlar EF Core 5,0 ' de çalışmayı planladığımız sorunlardır](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * Sürümlü kilometre taşında kapatılan sorunlar, bu sürüm için tamamlanmış sorunlardır. Sürümün henüz yayımlanmadığını unutmayın. Örneğin, [bunlar EF Core 3,0 için tamamlanan sorunlardır](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Oyları!
  * Oylama, bir sorunun sizin için önemli olduğunu belirtmenin en iyi yoludur.
  * Oylamak için, soruna bir "thumbs-up" 👍 eklemeniz yeterlidir. Örneğin, [bunlar en önemli sorunlardır](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * Bu, değer eklediğinizi düşünüyorsanız, özelliğin ihtiyaç duymadığınız belirli nedenleri açıklayan yorum yapın. "+ 1" veya benzer yorum ekleme değeri eklemez.

### <a name="the-planning-process"></a>Planlama işlemi

Planlama işlemi, biriktirme listesinden yalnızca en çok istenen özelliklerden yararlanılarak daha fazla yer alır.
Bunun nedeni birden çok paydaşdan birden çok farklı şekilde geri bildirim topladığımyız.
Daha sonra aşağıdakileri temel alarak bir yayın şekilliyoruz:

* Müşterilerden giriş
* Diğer paydaşlardan giriş
* Stratejik yön
* Kullanılabilir kaynaklar
* Zamanlama

Sorduğumuz sorulardan bazıları şunlardır:

1. **Kaç geliştirici özelliği kullanacağınızı ve uygulamalarını ya da deneyimlerini ne kadar daha iyi hale getireceğinizi düşünüyorsunuz?** Bu soruyu yanıtlamak için birçok kaynaktan geri bildirim topladık. sorunların açıklamaları ve oyları bu kaynaklardan biridir. Önemli müşterilerle belirli görevlendirmeler başka bir öneme sahiptir.

2. **Bu özelliği henüz uygulamadığımızda kişilerin kullanabileceği geçici çözümler nelerdir?** Örneğin, birçok geliştirici, yerel çoktan çoğa desteğin olmaması için bir JOIN tablosunu eşleyebilir. Kuşkusuz, tüm geliştiriciler bunu yapmak istememiz, ancak pek çok can ve kararımız bir faktör olarak sayılır.

3. **Bu özelliğin uygulanması, diğer özellikleri uygulamaya yaklaştırmak üzere EF Core mimarisini geliştirsin mi?** Diğer özellikler için yapı taşları görevi gören özellikleri tercih ederiz. Örneğin, özellik paketi varlıkları çoktan çoğa destekten ve varlık oluşturucularının yavaş yükleme desteğimizi etkinleştirmemize yardımcı olabilir.

4. **Özellik bir genişletilebilirlik noktası mi?** Geliştiricilerin kendi davranışlarını ve eksik işlevleri dengelamalarını sağladığından normal Özellikler üzerinde genişletilebilirlik noktalarını tercih ederiz.

5. **Diğer ürünlerle birlikte kullanıldığında özelliğin sinerjiden bahsederek denemelerini özelliği nedir?** .NET Core, Visual Studio 'nun en son sürümü, Microsoft Azure vb. gibi diğer ürünlerle EF Core kullanma deneyimini etkinleştiren veya önemli ölçüde iyileştiren Özellikler önerilir.

6. **Bir özellik üzerinde çalışmak için kullanılabilecek kişilerin becerileri nelerdir ve bu kaynaklardan en iyi şekilde faydalanabilirsiniz?** EF ekibinin ve topluluk Katkılarımızın her üyesi farklı alanlarda farklı deneyim düzeylerine sahiptir, bu nedenle uygun şekilde planlamanız gerekir. "Her türlü BT destesi", GroupBy çevirileri veya çoktan çoğa gibi belirli bir özellik üzerinde çalışması için pratik olmasa bile.
