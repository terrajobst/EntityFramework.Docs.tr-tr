---
title: Entity Framework Core yol haritası
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: a12d628a28515f0c6710bfa59bc6dcdf41fcb58b
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688595"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core yol haritası

> [!IMPORTANT]
> Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.

### <a name="ef-core-22"></a>EF Core 2.2

Bu sürümde birçok hata düzeltmeleri ve görece daha az sayıda yeni özellik içerir. Bu sürüm için yıl 2018 sonuna planlanmaktadır. Bu sürüm hakkındaki ayrıntıları dahil edilecek [EF Core 2.2 içinde yenilikler](xref:core/what-is-new/ef-core-2.2). 

### <a name="ef-core-30"></a>EF Core 3.0

.NET Core 3.0 ve ASP.NET 3.0 ile hizalanan ana EF Core sürümüne planlıyoruz, ancak biz tamamlamadıysanız [yayın Planlama işlemi](#release-planning-process) de.

Kullanım [bu bizim sorun İzleyicisi sorguda](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) görmek için iş öğelerini 3.0 için geçici olarak atanmış.

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