---
title: Entity Framework Core yol haritası
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: f18de8e8cb4fbe81bb2f983a00c9dd2f46be6073
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182026"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core yol haritası

> [!IMPORTANT]
> Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.

### <a name="ef-core-30"></a>EF Core 3.0

EF Core 2.2 kullanıma ile ana kazanmasının artık .NET Core 3. 0'ile uyumlu hale getirilerek EF Core 3.0 ve ASP.NET 3.0 serbest bırakır.

Şu yeni özellikleri henüz tamamlamadıysanız böylece [EF Core 3.0 Önizleme 1 paketleri için NuGet galerisinde yayımlanan](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1) aralık 2018'de yalnızca içeren [hata düzeltmeleri, küçük iyileştirmeler ve biz yapılan değişiklikler 3.0 iş hazırlığı](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed).

Aslında, yine de iyileştirmek ihtiyacımız bizim [planlama yayın](#release-planning-process) 3.0 için emin olmak için ayrılan süre içinde tamamlanması gereken özellikler doğru ortaklık kümesi sahibiz.
Biz size daha fazla netlik alabilirsiniz, ama bazı üst düzey Temalar şunlardır daha fazla bilgi paylaşır ve biz üzerinde çalışmak için itend özellikleri:

- **LINQ geliştirmeleri ([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: LINQ sayesinde, istediğiniz dilde çıkmadan veritabanı sorguları yazma yararlanarak zengin IntelliSense ve derleme zamanı tür denetimi alma bilgilerini yazın.
  Ancak LINQ, sınırsız sayıda karmaşık sorgular yazmaya olanak tanır ve her zaman LINQ sağlayıcıları için çok zor olmuştur.
  EF Core birkaç ilk sürümlerinde, biz, kısmi çıkış bir sorgu kısımlarını olabilir hesaplayarak Çözüldü SQL ve istemci üzerindeki bellekte yürütülecek sorgu geri kalanı sağlayarak çevrilir.
  Bu istemci-tarafı yürütme, bazı durumlarda istenmez olabilir, ancak olmayabilir verimsiz sorgularda sonuçlanabilir çoğu durumda, bir uygulama üretime dağıtıldığında kadar tanımlanmış.
  EF Core 3.0 sürümünde, çok büyük değişiklikler LINQ kararlılığımızın nasıl çalıştığını ve bunu nasıl test yapmak biz planlıyorsunuz.
  Hedefleridir (örneğin sorguları düzeltme eki sürümlerde bozmayı önlemek için), daha sağlam hale getirmek için doğru SQL, daha fazla durumda verimli sorgular oluşturun ve verimsiz sorguları algılanmayan gitmesini önlemek için daha fazla ifadelere çevirmek için.

- **Cosmos DB desteği ([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: Bir Cosmos DB sağlayıcısı için bir uygulama veritabanının olarak Azure Cosmos DB kolayca hedeflemek geliştiricilerin EF ölçeklenebilirliğinden modeli için EF Core üzerinde çalışıyoruz.
  Hedef, Cosmos DB, küresel dağıtım gibi avantajlarından bazıları "her zaman açık" olmasını sağlamaktır kullanılabilirlik, esnek ölçeklenebilirlik ve düşük gecikme süresi, .NET geliştiricileri için daha erişilebilir.
  EF Core gibi özelliklerin çoğu, Cosmos DB SQL API karşı değer dönüştürmeleri otomatik değişiklik izleme ve LINQ sağlayıcı etkinleştirir. EF Core 2.2 önce bu çalışmaların Başladık ve [bazı yaptık Önizleme sürümleri kullanılabilir sağlayıcısının](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  Yeni plan EF Core 3.0 yanı sıra sağlayıcı geliştirmeye devam sağlamaktır.   

- **C#8.0 desteği ([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: Bazı yararlanmak için müşterilerimizin istiyoruz [gelen yeni özellikler C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) istediğiniz zaman uyumsuz akışlar (dahil olmak üzere await her biri için) ve EF Core kullanırken null başvuru türleri.

- **Mühendislik veritabanı görünümleri ters sorgu türlerine ([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: EF Core 2.1 içinde veritabanından okunan ancak güncelleştirilemiyor verileri temsil etmek sorgu türleri için destek ekledik.
  Sorgu türleri şunlardır: eşleme veritabanı için harika bir uygun görünümleri, bunu EF Core 3. 0'ı, veritabanı görünümleri için sorgu türleri oluşturulmasını otomatik hale getirmek istiyoruz.

- **Özellik paketi varlıklar ([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) ve [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: Dizinli Özellikler normal özellikleri yerine verileri depolayan varlıkları etkinleştirme ve ayrıca aynı .NET sınıf örnekleri kullanabilmek için hakkındaki bu özellik, (büyük olasılıkla bir şey kadar basit bir `Dictionary<string, object>`) farklı varlık türlerini temsil etmek için aynı EF Core modelinde.
  EF Core için en çok istenen geliştirmeleri biri olan bir birleşim varlık olmadan çok-çok ilişkileri desteklemek için bir atlama taşı özelliğidir.

- **.NET core'da EF 6.3 ([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))**: Birçok mevcut uygulamaları EF'ın önceki sürümlerini kullanın ve bunları yalnızca .NET Core yararlanmak için EF Core'a taşıma bazen önemli bir efor gerektirebilir olduğunu biliyoruz.
  Bu nedenle, biz .NET Core 3.0 üzerinde çalıştırılacak EF 6'ın sonraki sürümü adapte.
  Biz bunu çok az değişiklikle taşıma mevcut uygulamaları kolaylaştırmak için yapıyor.
  Var olan giderek bazı sınırlamalar olacak şekilde (örneğin, yeni sağlayıcıları gerekir, SQL Server uzamsal desteğiyle olmaz etkinleştirilmesi) ve EF 6 için planlanan yeni özellik yoktur.

Bu arada, kullanabilirsiniz [müşterilerimize sorun İzleyicisi, bu sorgu](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) görmek için iş öğelerini 3.0 için geçici olarak atanmış.

## <a name="schedule"></a>Zamanlama

EF Core için zamanlamayı eşitlenmiş ile [.NET Core zamanlama](https://github.com/dotnet/core/blob/master/roadmap.md) ve [ASP.NET Core zamanlama](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Biriktirme

[Biriktirme listesi kilometre](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) bizim sorunu gün çalışmak üzere bekliyoruz veya topluluk birinden üstesinden düşünüyoruz sorun İzleyicisi içerir.
Müşteriler, açıklamalar ve oyları bu sorunları göndermek Hoş Geldiniz.
Katkıda Bulunanlar bu sorunların üzerlerinde çalışmak istiyorsanız önce bunları yaklaşımı hakkında bir tartışma başlatın önerilir.

Hiçbir zaman EF Core belirli bir sürümünü verilen herhangi bir özellik üzerinde çalışacak bir garanti yoktur.
Tüm yazılım projelerinde olduğu gibi herhangi bir noktada öncelikler, yayın ve kullanılabilir kaynaklar değiştirebilirsiniz.
Ancak biz, belirli bir zaman çerçevesinde bir sorunu çözmek düşünüyorsanız, biz bir yayın aşama biriktirme listesi kilometre yerine atamanız.
Biz rutin sorunları parçası olarak biriktirme listesi ve yayın kilometre taşları arasındaki taşıma bizim [yayın Planlama işlemi](#release-planning-process).

Biz şimdiye kadar bu sorunları gidermek planlamıyorsanız ki bir sorun büyük olasılıkla kapatın.
Ancak biz size ilgili yeni bilgiler alırsanız, daha önce kapalı bir sorun verme.

## <a name="release-planning-process"></a>Yayın Planlama işlemi

Biz genellikle belirli bir sürüm Git belirli özellikleri nasıl Seçtiğimiz hakkında sorular alın.
Kesinlikle, bizim biriktirme listesi otomatik olarak yayın planlarına Çevir değil.
Bir özellik EF6 varlığını da otomatik olarak özellik EF Core uygulanması gerektiğini anlamına gelmez.

Bir yayın planlama için biz izleyin bu işlem ayrıntılarını zordur.
Çoğu belirli özellikler, fırsatları ve öncelikler tartışmak ve işlemin kendisi de her sürümle geliştikçe.
Ancak biz size ne sonraki çalışmaya verirken yanıtlamayı deneyin sık sorulan sorular özetleyebilir:

1. **Düşünüyoruz kaç geliştiriciler özelliği kullanır ve uygulamaların/deneyimlerini ne kadar iyi hale getirir?** Bu yanıt için geri bildirim pek çok kaynaktan toplarız — yorumlar ve oyları sorunları, bu kaynakları biridir.

2. **Bu özelliği henüz uygulamanız yoksa, geçici çözümler kişilerdir kullanabilir miyim?** Örneğin, birçok geliştiricinin yerel çoktan çoğa destek eksikliği geçici olarak çalışmak için bir birleştirme tablo eşleyebilirsiniz. Kuşkusuz, tüm geliştiriciler yapmak istediğiniz ancak birçok olabilir ve, karar bir faktör olarak sayılmaktadır.

3. **Bu özelliği uygulamak, bize yakın diğer özelliklerini uygulama için taşıdığında, EF Core mimarisi gelişmek mu?** Diğer özellikler için yapı taşları gibi davranan özellikleri yerine boyuta ayrıcalık eğilimindedir. Örneğin, özellik paketi varlıklar çok-çok destek hareket bize yardımcı olabilir ve varlık oluşturucular yavaş yükleniyor desteğimiz etkin. 

4. **' % S'özelliği bir genişletilebilirlik noktası?** Geliştiricilerin kendi davranışları bağlama ve herhangi bir kayıp işlevsellik için telafi tanırlar çünkü genişletilebilirlik noktaları normal özellikleri yerine boyuta ayrıcalık eğilimindedir. 

5. **Diğer ürünleri ile birlikte kullanıldığında özelliğinin synergy nedir?** Biz etkinleştiren veya önemli ölçüde EF Core ile .NET Core, en son sürümünü Visual Studio, Microsoft Azure, vb. gibi diğer ürünleri kullanma deneyimini iyileştirmek özellikleri favor.

6. **Bir özellik ve bu kaynakları en iyi şekilde yararlanmak nasıl çalışmak için uygun kişilerin becerilerini nelerdir?** Bu nedenle uygun şekilde planlamanız gerekir her üyesi EF takım ve topluluğa Katkıda Bulunanlar, farklı alanlarda farklı düzeylerde deneyimi sahiptir. "Tüm eller deste" olmasını istedik bile belirli bir özelliği çalışma ister GroupBy çevirileri veya çok çok, pratik olmaz mıydı.

Daha önce belirtildiği gibi her sürüm üzerinde işlem geliştikçe.
Gelecekte sorundan yayın planlarımızı girişleri sağlamak için daha fazla fırsat topluluk üyeleri eklemek denenir.
Örneğin, Taslak tasarım özellikleri ve yayın planı gözden geçirmek daha kolay hale getirmek istiyoruz.
