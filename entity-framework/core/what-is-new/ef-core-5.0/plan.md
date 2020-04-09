---
title: Varlık Çerçeve Çekirdek 5.0 Planı
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136234"
---
# <a name="plan-for-entity-framework-core-50"></a>Varlık Çerçeve Çekirdek 5.0 Planı

[Planlama sürecinde](../release-planning.md)açıklandığı gibi, paydaşlardan EF Core 5.0 sürümü için geçici bir plan için girdi topladık.

> [!IMPORTANT] 
> Bu plan hala devam etmekte olan bir çalışmadır. Buradaki hiçbir şey bağlılık değildir. Bu plan, biz daha fazla bilgi olarak gelişecek bir başlangıç noktasıdır. Şu anda 5.0 için planlanmamış bazı şeyler çekilebilir. Şu anda 5.0 için planlanan bazı şeyler dışarı punted alabilirsiniz.

### <a name="version-number-and-release-date"></a>Sürüm numarası ve çıkış tarihi.

EF Core 5.0 şu anda [.NET 5.0 ile aynı anda](https://devblogs.microsoft.com/dotnet/introducing-net-5/)piyasaya sürülmesi planlanıyor. "5.0" sürümü .NET 5.0 ile hizalamak için seçildi.

### <a name="supported-platforms"></a>Desteklenen platformlar

EF Core 5.0'ın [bu platformların .NET Core ile yakınsaması](https://devblogs.microsoft.com/dotnet/introducing-net-5/)temel alınabilmek için herhangi bir .NET 5.0 platformunda çalışması planlanmaktadır. Bu .NET Standart ve kullanılan gerçek TFM açısından ne anlama geliyor hala TBD olduğunu.

EF Core 5.0 .NET Framework üzerinde çalışmaz.

### <a name="breaking-changes"></a>Yeni değişiklikler

EF Core 5.0 bazı kırılma değişiklikleri içerecektir, ancak bu EF Core 3.0 için olduğu gibi çok daha az şiddetli olacaktır. Amacımız, uygulamaların büyük çoğunluğunun kırılmadan güncellemesine izin vermektir.

Özellikle TPT desteği çevresinde veritabanı sağlayıcıları için bazı kırılma değişiklikleri olması beklenmektedir. Ancak, 5.0 için bir sağlayıcı güncelleştirmek için çalışma 3.0 için güncelleştirmek için gerekli olandan daha az olacağını bekliyoruz.

## <a name="themes"></a>Temalar

EF Core 5.0'daki büyük yatırımların temelini oluşturacak birkaç ana alan veya tema çıkardık.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Çok-çok navigasyon özellikleri (aka "gezintileri atlamak")

Müşteri adayı @smitpatel geliştiriciler: ve@AndriySvyryd

#19003 [tarafından](https://github.com/aspnet/EntityFrameworkCore/issues/19003) izlenir

T-shirt boyutu: L

Durum: Devam ediyor

Çok-to-çok GitHub biriktirme listesien [çok istenen özellik](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~ 407 oy) olduğunu.

Onların bütünüyle çok-çok ilişkiler için destek [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)olarak izlenir. Bu üç ana alana ayrılabilir:

* Gezinme özelliklerini atlayın. Bunlar, altyapı birleştirme tablosu varlığına başvurmadan, modelin sorgular vb. için kullanılmasına izin verir. ([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))
* Özellik-çanta varlık türleri. Bunlar, standart bir CLR türünün `Dictionary`(örn. ) varlık örnekleri için kullanılmasına izin verir, böylece her varlık türü için açık bir CLR türü gerekmez. (5.0 için stretch: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)
* Çok-çok ilişkilerin kolay yapılandırma için Şeker. (5.0 için streç.)

Çok-çok destek isteyenler için en önemli engelleyicinin, birleştirme tablosuna atıfta bulunmaksızın, sorgular gibi iş mantığıyla "doğal" ilişkileri kullanamamak olduğuna inanıyoruz. Birleştirme tablosu varlık türü hala var olabilir, ancak iş mantığının önüne girmemelidir. Bu nedenle 5.0 için atlama navigasyon özellikleri ele seçtik.

Şu anda çok-çok diğer parçaları EF Core 5.0 için bir streç hedef olarak takip edilmektedir. Bu da şu anda 5.0 için planda olmadıkları anlamına geliyor, ancak işler yolunda giderse onları içeri çekmeyi umuyoruz.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Tablo başına tip (TPT) kalıtım eşleme

Müşteri adayı geliştirici:@AndriySvyryd

İzlenen [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

T-shirt boyutu: XL

Durum: Devam ediyor

Hem çok istenen bir özellik (~254 oy; genel olarak 3. Bunun veritabanı sağlayıcıları için kırılmaya neden olmasını bekliyoruz, ancak bunlar 3.0 için gereken değişikliklerden çok daha az ciddi olmalıdır.

## <a name="filtered-include"></a>Filtrelenmiş Ekle

Müşteri adayı geliştirici:@maumar

#1833 [tarafından](https://github.com/aspnet/EntityFrameworkCore/issues/1833) izlenir

T-shirt boyutu: M

Durum: Devam ediyor

Filtreli Include, çok fazla istenen bir özelliktir (~317 oy; genel olarak 2.) ve şu anda model düzeyinde filtreler veya daha karmaşık sorgular gerektiren birçok senaryonun engelini kaldıracağına veya kolaylaştıracağına inandığımız bir özelliktir.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>ToTable, ToQuery, ToView, FromSql, vb. rasyonalize edin

Müşteri adayı @maumar geliştiriciler: ve@smitpatel

İzlendi [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

T-shirt boyutu: L

Durum: Devam ediyor

Ham SQL, anahtarsız türleri ve ilgili alanları destekleme yolunda önceki sürümlerde ilerleme kaydettik. Ancak, her şeyin bir bütün olarak birlikte çalışması nda hem boşluklar hem de tutarsızlıklar vardır. 5.0 için amaç bunları düzeltmek ve farklı türde varlıkları ve bunların ilişkili sorgularını ve veritabanı yapılarını tanımlamak, geçiş yapmak ve kullanmak için iyi bir deneyim oluşturmaktır. Bu, derlenmiş sorgu API güncelleştirmelerini de içerebilir.

Şu anda sahip olduğumuz bazı işlevler insanları hızlı bir şekilde başarısızlık çukurlarına sokabilecek kadar izin verici olduğundan, bu öğenin bazı uygulama düzeyinde kırılmadeğişikliklerine neden olabileceğini unutmayın. Büyük olasılıkla bu işlevlerin bir kısmını, bunun yerine ne yapacağımıza ilişkin rehberlikle birlikte engelleyeceğiz.

## <a name="general-query-enhancements"></a>Genel sorgu geliştirmeleri

Müşteri adayı @smitpatel geliştiriciler: ve@maumar

[5.0 `area-query` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+) izlenir

T-shirt boyutu: XL

Durum: Devam ediyor

Sorgu çeviri kodu EF Core 3.0 için kapsamlı olarak yeniden yazılmıştır. Sorgu kodu genellikle bu nedenle çok daha sağlam bir durumdadır. 5.0 için, TPT'yi desteklemek ve gezinme özelliklerini atlamak için gerekenler dışında büyük sorgu değişiklikleri yapmayı planlamiyoruz. Ancak 3,0'lık elden geçirmeden geriye kalan bazı teknik borçları düzeltmek için hala önemli çalışmalar gerekiyor. Ayrıca, genel sorgu deneyimini daha da geliştirmek için birçok hatayı düzeltmeyi ve küçük geliştirmeler uygulamayı planlıyoruz.

## <a name="migrations-and-deployment-experience"></a>Geçişler ve dağıtım deneyimi

Müşteri adayı geliştiriciler:@bricelam

[#19587](https://github.com/dotnet/efcore/issues/19587) tarafından izlenir

T-shirt boyutu: L

Durum: Devam ediyor

Şu anda, birçok geliştirici uygulama başlangıç zamanında veritabanlarını geçirin. Bu kolay, ancak önerilmez çünkü:

* Birden çok iş parçacığı/işlem/sunucu veritabanını aynı anda geçirmeye çalışabilir
* Bu olurken uygulamalar tutarsız duruma erişmeye çalışabilir
* Genellikle şema değiştirmek için veritabanı izinleri uygulama yürütme için verilmemelidir
* Bir şeyler ters giderse temiz bir duruma dönmek zordur.

Dağıtım zamanında veritabanını geçirmenin kolay bir yolunu sağlayan daha iyi bir deneyim sunmak istiyoruz. Bu gerekir:

* Linux, Mac ve Windows üzerinde çalışma
* Komut satırında iyi bir deneyim yaşayın
* Kapsayıcılarla destek senaryoları
* Yaygın olarak kullanılan gerçek dünya dağıtım araçları/akışları ile çalışma
* En azından Visual Studio'ya entegre edin

Sonuç, EF Core'da (örneğin, SQLite'deki daha iyi Göçler) ve sadece EF'nin ötesine geçen uçtan uca deneyimleri geliştirmek için diğer ekiplerle daha uzun vadeli işbirlikleri yle birlikte pek çok küçük iyileştirme olacaktır.

## <a name="ef-core-platforms-experience"></a>EF Core platformları deneyimi 

Müşteri adayı @roji geliştiriciler: ve@bricelam

#19588 [tarafından](https://github.com/dotnet/efcore/issues/19588) izlenir

T-shirt boyutu: L

Durum: Başlamadı

Geleneksel MVC benzeri web uygulamalarında EF Core'u kullanmak için iyi bir kılavuzumuz vardır. Diğer platformlar ve uygulama modelleri için kılavuz eksik veya güncel değil. EF Core 5.0 için, EF Core'u aşağıdakilerle birlikte kullanma deneyimini araştırmayı, geliştirmeyi ve belgelemayı planlıyoruz:

* Blazor
* Xamarin, AOT/linker hikayesini kullanmak da dahil olmak üzere
* WinForms/WPF/WinUI ve muhtemelen diğer U.I. Çerçeve

Bu, EF Core'da, sadece EF'nin ötesine geçen uçtan uca deneyimleri geliştirmek için diğer ekiplerle rehberlik ve uzun vadeli işbirlikleriyle birlikte pek çok küçük iyileştirme olması muhtemeldir.

Bakmayı planladığımız belirli alanlar şunlardır:

* Geçişler gibi EF takımlarını kullanma deneyimi de dahil olmak üzere dağıtım
* Xamarin ve Blazor ve muhtemelen diğerleri de dahil olmak üzere uygulama modelleri,
* Mekansal deneyim ve tablo yeniden de dahil olmak üzere SQLite deneyimleri,
* AOT ve bağlantı deneyimleri
* Perf sayaçları da dahil olmak üzere tanılama entegrasyonu

## <a name="performance"></a>Performans

Müşteri adayı geliştirici:@roji

[5.0 `area-perf` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+) izlenir

T-shirt boyutu: L

Durum: Devam ediyor

EF Core için, performans kriterleri paketimizi geliştirmeyi ve çalışma süresine yönelik performans iyileştirmeleri yapmayı planlıyoruz. Buna ek olarak, 3.0 sürüm döngüsü sırasında prototipedildi yeni ADO.NET toplu API tamamlamak için planlıyoruz. Ayrıca ADO.NET katmanında, Npgsql sağlayıcısına ek performans iyileştirmeleri planlıyoruz.

Bu çalışmanın bir parçası olarak, uygun ADO.NET/EF Çekirdek performans sayaçları ve diğer tanılamaeklemeyi planlıyoruz.

## <a name="architecturalcontributor-documentation"></a>Mimari/katkıda bulunan dokümantasyon

Müşteri adayı belgeleyici:@ajcvickers

İzlenen [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

T-shirt boyutu: L

Durum: Devam ediyor

Buradaki fikir, EF Core'un iç lerinde neler olup bittiğini daha kolay anlamaktır. Bu, EF Core kullanan herkes için yararlı olabilir, ancak birincil motivasyon, dış insanların aşağıdakileri yapmasını kolaylaştırmaktır:

* EF Core koduna katkıda bulunun
* Veritabanı sağlayıcıları oluşturma
* Diğer uzantıları oluşturma

## <a name="microsoftdatasqlite-documentation"></a>Microsoft.Data.Sqlite belgeleri

Müşteri adayı belgeleyici:@bricelam

İzlenen [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

T-shirt boyutu: M

Durum: Tamamlandı. Yeni belgeler [Microsoft dokümanlar sitesinde yayında.](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli)

EF Ekibi ayrıca Microsoft.Data.Sqlite ADO.NET sağlayıcısının da sahibidir. Bu sağlayıcıyı 5.0 sürümün bir parçası olarak tam olarak belgeletmayı planlıyoruz.

## <a name="general-documentation"></a>Genel dokümantasyon

Müşteri adayı belgeleyici:@ajcvickers

[5.0 kilometre taşındaki dokümanreposundaki sorunlar](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+) tarafından izlenir

T-shirt boyutu: L

Durum: Devam ediyor

3.0 ve 3.1 sürümleri için belgeleri güncelleme sürecindeyiz. Biz de üzerinde çalışıyoruz:
  * Daha ulaşılabilir/takip edilebilen daha kolay hale getirmek için dokümanların elden geçirilmesi
  * İşleri bulmayı kolaylaştırmak ve çapraz referanslar eklemek için dokümanların yeniden düzenlenmesi
  * Varolan dokümanlara daha fazla ayrıntı ve açıklama ekleme
  * Örnekleri güncelleme ve daha fazla örnek ekleme

## <a name="fixing-bugs"></a>Hataları düzeltme

[5.0 `type-bug` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+) izlenir

Geliştiriciler: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, ,@ajcvickers

T-shirt boyutu: L

Durum: Devam ediyor

Yazma sırasında, 5.0 sürümünde (62'si zaten sabitlenmiş) düzeltilmesi gereken 135 hata var, ancak yukarıdaki _Genel sorgu geliştirmeleri_ bölümüyle önemli bir çakışma söz vardır.

Gelen oran (bir kilometre taşı çalışma olarak sona erer sorunları) 3.0 sürümü boyunca ayda yaklaşık 23 sorunları oldu. Bunların hepsinin 5.0'da düzeltilmesi gerekmez. Kaba bir tahmin olarak, 5,0 zaman diliminde 150 sorunu daha düzeltmeyi planlıyoruz.

## <a name="small-enhancements"></a>Küçük geliştirmeler

[5.0 `type-enhancement` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+) izlenir

Geliştiriciler: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, ,@ajcvickers

T-shirt boyutu: L

Durum: Devam ediyor

Yukarıda özetlenen büyük özelliklere ek olarak, biz de "kağıt kesim" düzeltmek için 5.0 için planlanan birçok küçük iyileştirmeler var. Bu geliştirmelerin çoğunun yukarıda özetlenen daha genel temalar tarafından da karşılandığını unutmayın.

## <a name="below-the-line"></a>Satır Altı

[Etiketli sorunlartarafından `consider-for-next-release` ](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release) izlenen

Bunlar, **not** şu anda 5.0 sürümü için zamanlanmamış hata düzeltmeleri ve geliştirmelerdir, ancak yukarıdaki çalışmada kaydedilen ilerlemeye bağlı olarak streç hedefler olarak bakacağız.

Buna ek olarak, biz her zaman planlama [en çok oy sorunları](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) düşünün. Bu sorunlardan herhangi birini bir sürümden kesmek her zaman acı vericidir, ancak sahip olduğumuz kaynaklar için gerçekçi bir plana ihtiyacımız vardır.

## <a name="feedback"></a>Geri Bildirim

Planlama hakkındaki görüşleriniz önemlidir. Bir sorunun önemini belirtmenin en iyi yolu GitHub'da bu sorun için oy kullanmaktır (thumbs-up). Bu veriler daha sonra bir sonraki sürüm için [planlama sürecine](../release-planning.md) beslenir.
