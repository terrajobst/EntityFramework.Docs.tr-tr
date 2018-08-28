---
title: Entity Framework Core yol haritası
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: cd4b7ddaafe9501c4bb9f2496e87f619d239ab62
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995266"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core yol haritası

> [!IMPORTANT]
> Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.

EF Core 2.1 kararlı sürümünü 30 Mayıs 2018 tarihinde yayınlanmıştır. Bu sürümde hakkında daha fazla bilgi bulabilirsiniz [EF Core 2.1 yenilikler](xref:core/what-is-new/ef-core-2.1).

Biz tamamlanmamış [yayın Planlama işlemi](#release-planning-process) 2.1 sonraki sürümün için.

## <a name="schedule"></a>Zamanlama

EF Core için zamanlamayı eşitlenmiş ile [.NET Core zamanlama](https://github.com/dotnet/core/blob/master/roadmap.md) ve [ASP.NET Core zamanlama](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Biriktirme

Kullandığımız [biriktirme listesi kilometre](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) müşterilerimize sorun İzleyicisi, sorunları ve özelliklerinin ayrıntılı bir liste korumak için de. Müşteriler açıklama ve bunlar yukarı oy.

Sorunları makul biz belirli bir noktada çalışır veya topluluktan birinin üstesinden gelmek, ancak, değil kapsıyor biz belirli bir aşama bizim birparçasıolarakatanıncayakadarbelirlibirzamançerçevesindeçözümleyinyapmabekliyoruzolduğunuaçıkbırakıneğilimindedir [Planlama işlemi yayın](#release-planning-process).

Biz, hiç bir özelliği uygulamak planlamıyorsanız şu sorun büyük olasılıkla sona erecektir. Biz bunu ilgili yeni bilgiler elde etmek, biz kapalı bir sorun, sonraki bir noktada reconsidered.

Tüm bunları, biz bu özelliği X saat/yayın tarafından Y çözümlenir söyleyin yapabilmek için gelecek hakkında yeterli bilgi yok belirtti. Tüm yazılım projelerinde olduğu gibi herhangi bir noktada öncelikler, yayın ve kullanılabilir kaynaklar değiştirebilirsiniz.

## <a name="release-planning-process"></a>Yayın Planlama işlemi

Biz genellikle belirli bir sürüm Git belirli özellikleri nasıl Seçtiğimiz hakkında sorular alın. Çalışıyoruz kesinlikle otomatik olarak yayın planlarına anlamına gelmez. Bir özellik EF6 varlığını da otomatik olarak özellik EF Core uygulanması gerektiğini anlamına gelmez.

Biz çok miktarda belirli özellikler, fırsatları ve öncelikler kısmen görüştükten olması ve işlemin kendisi genellikle her sürümle geliştikçe kısmen gibi nedenlerle bir yayın plana izleyin tüm işlem aşağıda ayrıntılı olarak zordur. Ancak, sonraki çalışma gerekenler verirken yanıt deneyin sık sorulan sorular özetlemek oldukça kolaydır:

1. **Düşünüyoruz kaç geliştiriciler özelliği kullanır ve uygulamaların/deneyimlerini ne kadar iyi hale getirir?** Biz geri bildirim pek çok kaynaktan bu toplama — yorumlar ve oyları sorunları, bu kaynakları biridir.

2. **Bu özelliği henüz uygulamanız yoksa, geçici çözümler kişilerdir kullanabilir miyim?** Örneğin, birçok geliştiricinin birleştirme tablo yerel çoktan çoğa destek eksikliği geçici olarak çalışması için eşleme olanağına sahip olursunuz. Kuşkusuz, tüm geliştiriciler, bunu yapabilirsiniz ancak çoğu olabilir ve sayan bir faktörüyle budur.

3. **Bu özelliği uygulamak, bize yakın diğer özelliklerini uygulama için taşıdığında, EF Core mimarisi gelişmek mu?** Diğer özellikler için yapı taşları gibi davranan özellikleri favor eğilimindedir — Örneğin, sahip olunan türleri için yapıldığı tablo bölme TPT destek hareket yardımcı olur.

4. **' % S'özelliği bir genişletilebilirlik noktası?** Bunlar daha kolay kendi davranışları bağlama ve eksik işlevlerinden bazıları bu şekilde almak geliştiricilerin çünkü genişletilebilirlik noktaları yerine boyuta ayrıcalık eğilimindedir. Bazı yavaş yükleniyor çalışmak için bir başlangıç olarak bunun yapmak planlıyoruz.

5. **Diğer ürünleri ile birlikte kullanıldığında özelliğinin synergy nedir?** EF Core ile diğer ürünler kullanılacak veya .NET Core, en son sürümünü Visual Studio, Microsoft Azure, vb. gibi diğer ürünleri kullanma deneyimini önemli ölçüde geliştirmek için olanak tanıyan özellikler tanınacağını eğilimindedir.

6. **Bir özellik ve bu kaynakları en iyi şekilde yararlanmak nasıl çalışmak için uygun kişilerin özellikleri nelerdir?** EF takım ve topluluğa katkıda bulunanlar bile bizim her üyesi, farklı alanlarda farklı düzeylerde deneyimi sahip ve uygun şekilde planlamanız gerekir. "Tüm eller deste" olmasını istedik bile belirli bir özelliği çalışma ister GroupBy çevirileri veya çok çok, pratik olmaz mıydı.

Daha önce belirtildiği gibi bu işlem her sürümdeki geliştikçe ve geliştirici Topluluğu'nun üyeleri için önerilen taslakları özelliklerini gözden geçirmek kolay hale getirerek sürüm planlarına, örneğin, giriş sağlamak için daha fazla fırsat eklemek gelecekte istiyoruz ve kendi sürümü planlayın.
