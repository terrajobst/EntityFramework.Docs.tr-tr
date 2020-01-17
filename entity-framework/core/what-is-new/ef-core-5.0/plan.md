---
title: Entity Framework Core 5,0 planlaması
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125384"
---
# <a name="plan-for-entity-framework-core-50"></a>Entity Framework Core 5,0 planlaması

[Planlama sürecinde](../release-planning.md)açıklandığı gibi, hissedarlardan EF Core 5,0 sürümü için geçici bir plana toplanan girişleri topladık.

> [!IMPORTANT] 
> Bu plan hala devam eden bir çalışmadır. Taahhüt aşağıda verilmiştir. Bu plan, daha fazla öğrendiğimiz için geliştireceğiz bir başlangıç noktasıdır. 5,0 için şu anda planlanmayan bazı şeyler, çekime alabilir. 5,0 için şu anda planlanmış bazı şeyler kullanıma hazır olabilir.

### <a name="version-number-and-release-date"></a>Sürüm numarası ve sürüm tarihi.

EF Core 5,0 şu anda sürüm için [.net 5,0 ile aynı zamanda](https://devblogs.microsoft.com/dotnet/introducing-net-5/)zamanlandı. .NET 5,0 ile hizalamak için "5,0" sürümü seçildi.

### <a name="supported-platforms"></a>Desteklenen platformlar

EF Core 5,0, [Bu platformların .NET Core 'a yakınsamasını](https://devblogs.microsoft.com/dotnet/introducing-net-5/)temel alan herhangi bir .NET 5,0 platformunda çalıştırılmak üzere planlanmaktadır. Bu .NET Standard, ne anlama gelir ve kullanılan gerçek tfd, hala TBD 'dir.

EF Core 5,0 .NET Framework çalıştırmayacak.

### <a name="breaking-changes"></a>Yeni değişiklikler

EF Core 5,0 bazı önemli değişiklikler içerir, ancak bu durum EF Core 3,0 ' den çok daha az önem altına alınır. Hedefimiz, uygulamanın büyük çoğunluğunun bozmadan güncelleştirilmesine izin vermektir.

Veritabanı sağlayıcılarının, özellikle de TPT desteğinin çevresinde bazı önemli değişiklikler olacağı için bu değer beklenmektedir. Bununla birlikte, 5,0 için bir sağlayıcıyı güncelleştirme işinin 3,0 için güncelleştirilmesi gerekenden daha az olacağını umyoruz.

## <a name="themes"></a>Temalar

EF Core 5,0 ' de büyük yatırımların temelini oluşturacak birkaç önemli alanı veya temaları ayıkladık.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Çoka çok gezinti özellikleri (bir. k. a "gezintilerini atla")

Lider geliştiricileri: @smitpatel ve @AndriySvyryd

[#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003) tarafından izleniyor

Tişörlü Boyut: L

Durum: devam ediyor

Birden çok-çok, GitHub biriktirme listesindeki en çok istenen özelliktir (~ 407 oylardır). Çoktan çoğa ilişkiler için destek üç ana alana ayrılabilir:

* Gezinme özelliklerini atlayın. Bunlar, modelin, temel alınan JOIN tablosu varlığına başvurmaksızın sorgular, vb. için kullanılmasına izin verir.
* Özellik paketi varlık türleri. Bunlar, varlık örnekleri için, her varlık türü için açık bir CLR türü gerektirmeyen standart bir CLR türüne (ör. `Dictionary`) olanak tanır.
* Çoka çok ilişkilerin kolay yapılandırması için cukr.

Çok-çok desteği olan en önemli engelleyici, JOIN tablosuna başvurulmadan, sorgular gibi iş mantığından "doğal" ilişkileri kullanmadığımızı düşüntik. JOIN tablosu varlık türü yine de mevcut olabilir, ancak iş mantığı gibi kullanılmamalıdır. Bu nedenle 5,0 için gezinme özelliklerini atla ' nın üstesinden gelmeyi seçtik.

Şu anda, çoktan çoğa diğer bölümleri EF Core 5,0 için bir Esnetme hedefi olarak işlenir. Bu, şu anda 5,0 için planda olmadıkları anlamına gelir, ancak şeyler ' ın içinde çekmesini umuyoruz.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Tablo/tür (TPT) devralma eşleme

Lider geliştiricisi: @AndriySvyryd

[#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266) tarafından izleniyor

Tişörlü Boyut: XL

Durum: başlatılmadı

Yüksek düzeyde istenmiş bir Özellik (~ 254 oy ve 3. Genel) olduğundan ve genel .NET 5 planının temel yapısı için uygun olan bazı düşük düzey değişiklikler gerektirdiğinden TPT yapıyoruz. Bunun, 3,0 için gereken değişikliklerden çok daha az ciddi olmasına rağmen, bu, veritabanı sağlayıcılarının değişikliklere neden olduğunu umyoruz.

## <a name="filtered-include"></a>Filtrelenen ekleme

Lider geliştiricisi: @maumar

[#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833) tarafından izleniyor

Tişörlü Boyut: a

Durum: başlatılmadı

Filtrelenmiş dahil, çok büyük bir iş miktarı olmayan ve şu anda model düzeyi filtreler veya daha karmaşık sorgular gerektiren çok sayıda senaryonun engellemesini veya daha kolay olduğunu düşünmemiz için yüksek düzeyde istenmiş bir özelliktir (~ 317 oy; 2. Genel).

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Dtiontotable, ToQuery, ToView, FromSql vb.

Lider geliştiricileri: @maumar ve @smitpatel

[#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270) tarafından izleniyor

Tişörlü Boyut: L

Durum: başlatılmadı

Ham SQL, anahtarsız türlerini ve ilgili alanı desteklemeye yönelik önceki sürümlerde ilerleme yaptık. Ancak, her şeyin bir bütün olarak birlikte çalıştıkları şekilde hem boşluklar hem de tutarsızlıklar vardır. 5,0 için amaç bunları çözmektir ve farklı varlık türlerini ve bunlarla ilişkili sorguları ve veritabanı yapılarını tanımlamak, geçirmek ve kullanmak için iyi bir deneyim oluşturmaktır. Bu, derlenmiş sorgu API 'SI güncelleştirmelerini de içerebilir.

Bu öğe, şu anda kişilerin hata Pits 'e hızlı bir şekilde yol açabileceğini sağlayan çok fazla izin olduğundan, bu öğenin bazı uygulama düzeyi değişikliklere neden olabileceğini unutmayın. Bunun yerine ne yapacaklarla ilgili rehberlik ile bu işlevlerden bazılarını engellemeyi büyük olasılıkla yapacağız.

## <a name="general-query-enhancements"></a>Genel sorgu geliştirmeleri

Lider geliştiricileri: @smitpatel ve @maumar

[5,0 kilometre taşında `area-query` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+) tarafından izleniyor

Tişörlü Boyut: XL

Durum: devam ediyor

Sorgu çevirisi kodu EF Core 3,0 için kapsamlı olarak yeniden yazıldı. Sorgu kodu genellikle bu nedenle çok daha sağlam durumda. 5,0 için, TPT 'yi desteklemek ve gezinme özelliklerini atlamak için gerekenlerden büyük sorgu değişiklikleri yapmayı planlamadık. Bununla birlikte, 3,0 fazla yerine kalan bazı teknik borcu gidermek için hala önemli bir iş vardır. Ayrıca, genel sorgu deneyimini geliştirmek için birçok hatayı gidermeyi planlıyoruz ve küçük geliştirmeler uygulayacağız.

## <a name="migrations-and-deployment-experience"></a>Geçişler ve dağıtım deneyimi

Lider geliştiricileri: @bricelam

[#19587](https://github.com/dotnet/efcore/issues/19587) tarafından izleniyor

Tişörlü Boyut: L

Durum: devam ediyor

Şu anda birçok geliştirici, veritabanlarını uygulama başlatma zamanına geçirecektir. Bu kolay ancak önerilmez, çünkü:

* Birden çok iş parçacığı/işlem/sunucu veritabanını aynı anda geçirmeye çalışabilir
* Uygulamalar, bu durumdayken tutarsız duruma erişmeye çalışabilir
* Genellikle Şemayı değiştirme veritabanı izinleri uygulama yürütmesi için verilmemelidir
* Bir şeyler yanlış olursa temiz bir duruma geri dönme

Veritabanını dağıtım zamanında geçirmek için kolay bir yol sağlayan daha iyi bir deneyim sunmak istiyoruz. Şunları yapmanız gerekir:

* Linux, Mac ve Windows üzerinde çalışma
* Komut satırında iyi bir deneyim olun
* Kapsayıcılarla destek senaryoları
* Yaygın olarak kullanılan gerçek dünya dağıtım araçlarıyla ve akışlarla çalışma
* En az Visual Studio ile tümleştirin

Sonuçta, yalnızca EF 'in ötesinde çok sayıda küçük EF Core geliştirmeler (örneğin, SQLite üzerinde daha iyi geçişler), diğer ekiplere yönelik kılavuzlarla daha fazla geçiş ve daha uzun süreli İşbirlikten yararlanmaktır.

## <a name="ef-core-platforms-experience"></a>EF Core platformları deneyimi 

Lider geliştiricileri: @roji ve @bricelam

[#19588](https://github.com/dotnet/efcore/issues/19588) tarafından izleniyor

Tişörlü Boyut: L

Durum: başlatılmadı

Geleneksel MVC benzeri Web uygulamalarında EF Core kullanmak için iyi bir kılavuzluk sunuyoruz. Diğer platformlar ve uygulama modelleriyle ilgili rehberlik eksik ya da güncel değil. EF Core 5,0 ' de, ile EF Core kullanma deneyimini araştırmaya, iyileştirmenize ve belgeleyecek şekilde planlıyoruz:

* Blazor
* AOT/bağlayıcı hikayesini kullanma dahil Xamarin
* WinForms/WPF/WinUI ve muhtemelen diğer U.I. çerçeveleri

Bu, EF Core çok küçük geliştirmeler, diğer ekiplere yönelik kılavuz ve uzun süreli ortak çalışmalarla birlikte, yalnızca EF 'in ötesine geçen uçtan uca deneyimler geliştirmeye olanak sağlar.

Göz atadığımız belirli bölgeler şunlardır:

* Geçiş için gibi EF araçları kullanma deneyimini de içeren dağıtım
* Xamarin ve Blazor dahil olmak üzere uygulama modelleri ve büyük olasılıkla diğerleri
* Uzamsal deneyim ve tablo da dahil olmak üzere SQLite deneyimler
* AOT ve bağlama deneyimleri
* Performans sayaçlarını içeren tanılama tümleştirmesi

## <a name="performance"></a>Performans

Lider geliştiricisi: @roji

[5,0 kilometre taşında `area-perf` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+) tarafından izleniyor

Tişörlü Boyut: L

Durum: devam ediyor

EF Core için performans değerlendirmeleri takımımızı geliştirmeyi ve çalışma zamanında yönlendirilmiş performans iyileştirmeleri yapmayı planlıyoruz. Ayrıca, 3,0 yayın çevrimi sırasında prototip oluşturulan yeni ADO.NET toplu işlem API 'sini tamamlamayı planlıyoruz. Ayrıca, ADO.NET katmanında Npgsql sağlayıcısında ek performans geliştirmeleri planlıyoruz.

Bu çalışmanın bir parçası olarak, uygun şekilde ADO.NET/EF Core performans sayaçlarını ve diğer tanılamayı eklemeyi de planlıyoruz.

## <a name="architecturalcontributor-documentation"></a>Mimari/katkıda bulunan belgeleri

Müşteri adayı belge girme: @ajcvickers

[#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920) tarafından izleniyor

Tişörlü Boyut: L

Durum: başlatılmadı

Buradaki fikir, EF Core iç yapıları hakkında daha kolay anlaşılır hale getirmek için. Bu, EF Core kullanan herkes için yararlı olabilir, ancak birincil mosyon, dış kişilerin şunları yapmasını kolaylaştırır:

* EF Core koduna katkıda bulunun
* Veritabanı sağlayıcıları oluşturma
* Diğer uzantıları oluşturma

## <a name="microsoftdatasqlite-documentation"></a>Microsoft. Data. SQLite belgeleri

Müşteri adayı belge girme: @bricelam

[#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675) tarafından izleniyor

Tişörlü Boyut: a

Durum: tamamlandı. Yeni belgeler [Microsoft docs sitesinde canlı](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

EF ekibi, Microsoft. Data. SQLite ADO.NET sağlayıcısına de sahiptir. Bu sağlayıcıyı 5,0 sürümünün bir parçası olarak tam olarak belgelemek planlıyoruz.

## <a name="general-documentation"></a>Genel belgeler

Müşteri adayı belge girme: @ajcvickers

[5,0 kilometre taşında docs deposunda bulunan sorunlara](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+) göre izleniyor

Tişörlü Boyut: L

Durum: devam ediyor

3,0 ve 3,1 sürümleri için belgeleri güncelleştirme sürecimiz zaten var. Ayrıca şu şekilde çalışıyoruz:
  * Daha fazla ulaşılabilir/daha kolay hale getirmek için kullanmaya başlama belgelerine yönelik fazla mesafe
  * Çapraz başvuruları bulmayı ve eklemeyi kolaylaştırmak için belgeleri yeniden düzenleme
  * Mevcut docs için daha fazla ayrıntı ve ayrıntılı açıklamalar ekleme
  * Örnekleri güncelleştirme ve daha fazla örnek ekleme

## <a name="fixing-bugs"></a>Hataları düzeltme

[5,0 kilometre taşında `type-bug` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+) tarafından izleniyor

Geliştiriciler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Tişörlü Boyut: L

Durum: devam ediyor

Yazma sırasında, 5,0 sürümünde (zaten düzeltilmiş olan 62) düzeltilen 135 hata değerlendirildi, ancak yukarıdaki _genel sorgu geliştirmeleri_ bölümünde önemli bir çakışma var.

Gelen ücret (bir kilometre taşında çalışan olarak biten sorunlar) 3,0 sürümü boyunca ayda yaklaşık 23 sorun olmuştur. Bunların tümünün 5,0 ' de düzeltilmesi gerekmez. Kabaca bir tahmin olarak 5,0 zaman çerçevesinde ek 150 sorunları gidermeyi planlıyoruz.

## <a name="small-enhancements"></a>Küçük geliştirmeler

[5,0 kilometre taşında `type-enhancement` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+) tarafından izleniyor

Geliştiriciler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Tişörlü Boyut: L

Durum: devam ediyor

Yukarıda özetlenen daha büyük özelliklere ek olarak, 5,0 "kağıt-keser" düzeltmesini sağlamak için zamanlanan çok sayıda daha küçük geliştirmeler de sunuyoruz. Bu geliştirmelerin birçoğu yukarıda özetlenen daha genel temalar tarafından da ele alınmıştır.

## <a name="below-the-line"></a>Satır altı

[`consider-for-next-release`etiketli sorunlar](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release) tarafından izleniyor

Bunlar, şu anda 5,0 sürümü için zamanlanmamış **olan hata** düzeltmeleri ve geliştirmeleridir, ancak yukarıdaki iş üzerinde yapılan ilerlemeye bağlı olarak, Esnetme hedefleri olarak bakacağız.

Ayrıca, planlama sırasında [en fazla oylanan sorunları](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) her zaman göz önünde bulundurmanız gerekir. Bu sorunlardan herhangi birini bir yayından kesmek her zaman çok önemlidir, ancak sahip olduğumuz kaynaklar için gerçekçi bir plana ihtiyacımız var.

## <a name="feedback"></a>Geribildirim

Planlamaya ilişkin geri bildiriminiz önemlidir. Bir sorunun önemini belirtmenin en iyi yolu GitHub 'da söz konusu sorundan oylanmanız (thumbs-up). Bu veriler daha sonra bir sonraki sürüm için [planlama işlemine](../release-planning.md) akış eklenecektir.
