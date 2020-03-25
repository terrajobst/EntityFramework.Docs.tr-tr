---
title: EF Core yayınları ve planlamayı
author: ajcvickers
ms.date: 03/03/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 7b58b4de0eb8d9575f77e0b147da017eabad1867
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136217"
---
# <a name="ef-core-releases-and-planning"></a>EF Core yayınları ve planlamayı

## <a name="stable-releases"></a>Kararlı yayınlar

| Yayınla | Hedef çerçeve | Destek sonu | Bağlantılar
|:--------|------------------|-----------------|------
| [EF Core 3,1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.2) | .NET Standard 2,0 | 3 Aralık 2022 (LTS) | [Duyur](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3,0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2,1 | 3 Mart 2020 ' de geçen süre | [Duyuru](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [son değişiklikler](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2,2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2,0 | Zaman aşımına uğradı, 23 Aralık 2019 | [Duyur](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2,0 | 21 Ağustos 2021 (LTS) | [Duyur](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2,0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2,0 | 1 Ekim 2018 tarihinde zaman aşımına uğradı | [Duyur](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1,1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1,3 | Süre için 27 2019 Haziran | [Duyur](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1,0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1,3 | Süre için 27 2019 Haziran | [Duyur](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Her EF Core sürümü tarafından desteklenen belirli platformlar hakkında daha fazla bilgi için bkz. [Desteklenen platformlar](../platforms/index.md) .

Destek süre sonu ve uzun süreli destek (LTS) sürümleri hakkında bilgi için bkz. [.net destek ilkesi](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) .

## <a name="guidance-on-updating-to-new-releases"></a>Yeni sürümlere güncelleştirme hakkında rehberlik

* Desteklenen yayınlar güvenlik ve diğer kritik hatalar için düzeltme eki uygulanır. Her zaman belirli bir sürümün en son düzeltme ekini kullanın. Örneğin, EF Core 2,1 için 2.1.14 kullanın.
* Ana sürüm güncelleştirmeleri (örneğin, EF Core 2 ' den EF Core 3 ' e) genellikle son değişiklikler olur. Önemli sürümler üzerinde güncelleştirme yaparken tam test önerilir. Üstteki değişikliklerle ilgili yönergeler için yukarıdaki Son değişiklik bağlantılarını kullanın.
* İkincil sürüm güncelleştirmeleri genellikle son değişiklikleri içermez. Ancak, yeni özelliklerin gerileme getirebilmesinden bu yana kapsamlı test yine de önerilir.

## <a name="release-planning-and-schedules"></a>Yayın planlama ve zamanlamalar

EF Core yayınları [.NET Core sevkiyat zamanlaması](https://github.com/dotnet/core/blob/master/roadmap.md)ile hizalanır.

Düzeltme Eki yayınları genellikle aylık olarak sevk ederken uzun bir sağlama süresine sahiptir.
Bunu geliştirmek için çalışıyoruz.

Her sürümde nelerin yayınlanacağına karar verdiğimiz hakkında daha fazla bilgi için [Sürüm Planlama sürecini](release-planning.md) inceleyin.
Genellikle bir sonraki büyük veya küçük sürümden daha fazla ayrıntı planlaması yapılmazlar.

## <a name="ef-core-50"></a>EF Core 5.0

Sonraki planlı kararlı sürüm, 2020 Kasım 5,0 ' de zamanlanan **EF Core**' dir.

[EF Core 5,0 için yüksek düzey bir plan](ef-core-5.0/plan.md) , belgelenen [yayın planlama işlemi](release-planning.md)sonrasında oluşturulmuştur.

Planlamaya ilişkin geri bildiriminiz önemlidir.
Bir sorunun önemini belirtmenin en iyi yolu GitHub 'daki bu sorun için oy vermektir (👍).
Bu veriler daha sonra bir sonraki sürüm için planlama işlemine akış eklenecektir.

### <a name="get-it-now"></a>Hemen alın!

EF Core 5,0 paketleri **Şu şekilde kullanılabilir**

* [Günlük derlemeler](https://github.com/dotnet/aspnetcore/blob/master/docs/DailyBuilds.md)
  * Tüm en son özellikler ve hata düzeltmeleri. Genel olarak çok kararlı; 57000 + test her bir derlemede çalışır.
* [NuGet önizlemesi](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
  * Günlük derlemelerin arkasına gecikti, ancak karşılık gelen ASP.NET Core ve .NET Core önizlemeleriyle birlikte çalışmak üzere test edilmiştir.

Önizlemeleri veya günlük derlemeler kullanmak, sorunları bulmanın ve mümkün olduğunca erken geri bildirimde bulunmak için harika bir yoldur.
Daha erken bu geri bildirimde bulunduk, daha büyük olasılıkla sonraki resmi yayından önce eyleme geçirilecektir.
