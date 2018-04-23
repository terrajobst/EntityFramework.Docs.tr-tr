---
title: Entity Framework Çekirdek yol haritası
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: 6c10e64a4fa3bf26dc0da64bb9e102c8b76d3a6e
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Çekirdek yol haritası

> [!IMPORTANT]
> Özellik kümeleri ve gelecek sürümlerin zamanlamaları her zaman değiştirilebilir ve bu sayfa, son planlarımızı hiç yansıtmayabilir güncelliğini deneyeceğiz rağmen zaman olmadığını unutmayın.

EF çekirdek 2.1 ikinci önizlemesini Nisan 2018 üzerinde yayımlanmıştır. Şimdi bu sürümde hakkında daha fazla bilgi bulabilirsiniz [EF çekirdek 2.1 yenilikler](xref:core/what-is-new/ef-core-2.1).

EF çekirdek aylık 2.1 ve son sürümde 2018 ikinci Takvim çeyreğini üzerinde ek önizlemeleri yayımlamayı düşündüğünüz.

Biz tamamlanmamış [planlama yayın](#release-planning-process) 2.1 sonra bir sonraki sürümü için.

## <a name="schedule"></a>Zamanlama

-Sync EF çekirdek için zamanlama ile [.NET Core zamanlama](https://github.com/dotnet/core/blob/master/roadmap.md) ve [ASP.NET Core zamanlama](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Biriktirme

Kullanırız [biriktirme listesi Kilometre Taşı](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) bizim sorun İzleyicisi, sorunları ve özelliklerinin ayrıntılı bir liste korumak için de. Müşteriler açıklama ve bunlar yukarı oy.

Sorunları makul biz belirli bir noktada çalışır veya topluluktan birinin üstesinden gelmek, ancak, değil kapsıyor biz belirli bir aşama bizim birparçasıolarakatanıncayakadarbelirlibirzamançerçevesindeçözümleyinyapmabekliyoruzolduğunuaçıkbırakıneğilimindedir [Planlama işlemi yayın](#release-planning-process).

Biz, herhangi bir özelliği uygulamak düşünmüyorsanız biz sorun büyük olasılıkla kapanacak. Biz bunu ilgili yeni bilgiler elde etmek, biz kapalı bir sorun sonraki bir noktada reconsidered.

Tüm, gelecekte bu özelliği X Saat/Sürüm Y çözümlenir söyleyin yapabilmek için yeterli bilgi yok mu belirtti. Tüm yazılım projeleri olduğu gibi herhangi bir noktada öncelikleri, yayın zamanlamaları ve kullanılabilir kaynakları değiştirebilirsiniz.

## <a name="release-planning-process"></a>Planlama işleminin sürüm

Biz genellikle belirli bir yayın içinde gitmek için belirli özellikler seçeneğini nasıl belirledik hakkında sorular alın. Bizim biriktirme listesi kesinlikle otomatik olarak sürüm planları tercüme etmez. Bir özellik EF6 varlığını de otomatik olarak özellik EF çekirdek uygulanması gerektiğini anlamına gelmez.

Burada size çok miktarda belirli özellikler, fırsatları ve öncelikler kısmen tartışmak için ve kısmen işlemi her sürümle genellikle dönüşmesi çünkü bir yayın plana izleyin tüm işlem ayrıntı zordur. Ancak, sonraki çalışma gerekenler karar verirken yanıt deneyin sık sorulan sorular özetlemek oldukça kolaydır:

1. **Kaç tane geliştiriciler düşünüyoruz özelliğini kullanacak ve uygulamalar/deneyimlerini ne kadar daha iyi hale getirir?** Biz geri bildirim birçok kaynaktan bu toplama — açıklamaları ve sorunları oylar biridir bu kaynakları.

2. **Bu özelliği henüz uygulamanız yok, geçici çözümler kişilerdir kullanabilir miyim?** Örneğin, çoğu geliştiricinin yerel çoktan çoğa destek eksikliği çalışması için bir birleşim tablosundan eşleyebilir. Belli ki, tüm geliştiriciler, bunu yapabilirsiniz ancak birçok olabilir ve sayıları bir faktörüyle budur.

3. **Bunu bize yakın diğer özelliklerini uygulama için taşır, bu özellik uygulama EF çekirdek mimarisini gelişmesi mu?** Diğer özellikler için yapı taşları gibi davranan özellikleri favor eğilimindedir — Örneğin, tablo ait türleri için yapılan bölme birleştirilmiş TPT destek hareket yardımcı olur.

4. **Genişletilebilirlik noktası özelliğidir?** Biz, geliştiricilerin daha kolay kendi davranışlarının bağlayın ve eksik işlev bazıları bu şekilde alma etkinleştirmek için genişletilebilirlik noktaları favor olma eğilimi gösterir. Biz bu yavaş yükleniyor çalışmak için bir başlangıç olarak bazıları yapmayı planlamıyorsanız.

5. **Diğer ürünleri ile birlikte kullanıldığında özelliği synergy nedir?** Biz EF çekirdek diğer ürünleri ile kullanılmak üzere veya .NET Core, en son sürümünü Visual Studio, Microsoft Azure, vb. gibi diğer ürünler kullanılarak deneyimini önemli ölçüde artırmak için olanak tanıyan özellikler favor eğilimindedir.

6. **Bir özellik ve bu kaynakları en iyi şekilde yararlanmak nasıl çalışmak için uygun kişilerin özellikleri nelerdir?** Her üyesine EF bile bizim topluluğa Katkıda Bulunanlar, farklı düzeylerde deneyimi alanında farklı alanlara sahip ve uygun şekilde planlamanız gerekir. Biz "deste tüm durum" olmasını istediğinizi olsa bile belirli bir özellik çalışmaları ister GroupBy çevirileri veya çok-çok, pratik olmayacaktır.

Önceden belirtildiği gibi her yayın üzerinde bu işlem dönüşmesi ve geliştirici topluluğu üyeleri için daha fazla fırsat girişleri sürüm planları örneğin önerilen Taslaklar Özellikler'in ve gözden geçirmek kolaylaştırarak sağlamak üzere eklemek gelecekte isteriz Yayın Planın kendisi.
