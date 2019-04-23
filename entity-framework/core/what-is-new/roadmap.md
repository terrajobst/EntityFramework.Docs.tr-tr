---
title: Entity Framework Core yol haritası
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 937bfff4cbc3ec0b2235a78cb2f1f246697128d5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929790"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core yol haritası

> [!IMPORTANT]
> Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.

## <a name="ef-core-30"></a>EF Core 3.0

Kullanıma EF Core 2.2 ile ana kazanmasının EF Core 3.0 sunulmuştur.
Bkz: [EF Core 3. 0'yenilikler](xref:core/what-is-new/ef-core-3.0/index) hakkında bilgi için planlanan [yeni özellikler](xref:core/what-is-new/ef-core-3.0/features) ve kasıtlı [bozucu değişiklikler](xref:core/what-is-new/ef-core-3.0/breaking-changes) bu sürüm şunları sunar.

## <a name="schedule"></a>Zamanlama

EF Core için yayın zamanlaması eşitlenmiş ile [.NET Core yayın zamanlaması](https://github.com/dotnet/core/blob/master/roadmap.md).

## <a name="backlog"></a>Biriktirme

[Biriktirme listesi kilometre](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) müşterilerimize sorun İzleyicisi ya da gün çalışmak üzere bekliyoruz sorunları içerir veya topluluk birinden üstesinden düşünüyoruz.
Müşteriler, açıklamalar ve oyları bu sorunları göndermek Hoş Geldiniz.
Katkıda Bulunanlar bu sorunların üzerlerinde çalışmak istiyorsanız önce bunları yaklaşımı hakkında bir tartışma başlatın önerilir.

Hiçbir zaman EF Core belirli bir sürümünü verilen herhangi bir özellik üzerinde çalışıyoruz bir garanti yoktur.
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

1. **Düşünüyoruz kaç geliştiriciler özelliği kullanır ve ne kadar iyi devam uygulamalarını olun veya deneyimi?** Bu soruyu cevaplamak için geri bildirim pek çok kaynaktan topladığımız — yorumlar ve oyları sorunları, bu kaynakları biridir.

2. **Bu özelliği henüz uygulamanız yoksa, geçici çözümler kişilerdir kullanabilir miyim?** Örneğin, birçok geliştiricinin yerel çoktan çoğa destek eksikliği geçici olarak çalışmak için bir birleştirme tablo eşleyebilirsiniz. Kuşkusuz, tüm geliştiriciler yapmak istediğiniz ancak birçok olabilir ve, karar bir faktör olarak sayılmaktadır.

3. **Bu özelliği uygulamak, bize yakın diğer özelliklerini uygulama için taşıdığında, EF Core mimarisi gelişmek mu?** Diğer özellikler için yapı taşları gibi davranan özellikleri yerine boyuta ayrıcalık eğilimindedir. Örneğin, özellik paketi varlıklar çok-çok destek hareket bize yardımcı olabilir ve varlık oluşturucular yavaş yükleniyor desteğimiz etkinleştirilmiş.

4. **' % S'özelliği bir genişletilebilirlik noktası?** Geliştiricilerin kendi davranışları bağlama ve herhangi bir kayıp işlevsellik için telafi tanırlar çünkü genişletilebilirlik noktaları normal özellikleri yerine boyuta ayrıcalık eğilimindedir.

5. **Diğer ürünleri ile birlikte kullanıldığında özelliğinin synergy nedir?** Biz favor özellikleri etkinleştiren veya önemli ölçüde EF Core ile .NET Core, Visual Studio, Microsoft Azure, en son sürümünü gibi diğer ürünleri kullanma deneyimini iyileştirmek ve benzeri.

6. **Bir özellik ve bu kaynakları en iyi şekilde yararlanmak nasıl çalışmak için uygun kişilerin becerilerini nelerdir?** Bu nedenle uygun şekilde planlamanız gerekir her üyesi EF takım ve topluluğa Katkıda Bulunanlar, farklı alanlarda farklı düzeylerde deneyimi sahiptir. "Tüm eller deste" olmasını istedik bile belirli bir özelliği çalışma ister GroupBy çevirileri veya çok çok, pratik olmaz mıydı.

Daha önce belirtildiği gibi her sürüm üzerinde işlem geliştikçe.
Gelecekte, biz yayın planlarımızı girişleri sağlamak için daha fazla fırsat topluluk üyeleri eklemek denenir.
Örneğin, Taslak tasarım özellikleri ve yayın planı gözden geçirmek daha kolay hale getirmek istiyoruz.
