---
title: EF Core bültenleri ve planlama
description: Gelecekteki sürümler için geçerli EF Core sürümleri ve zamanlama/planlama ayrıntıları
author: ajcvickers
ms.date: 03/03/2020
uid: core/what-is-new/index
ms.openlocfilehash: 89687417685f291b44dcb250c96c5c9fa57da80f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634261"
---
# <a name="ef-core-releases-and-planning"></a>EF Core bültenleri ve planlama

## <a name="stable-releases"></a>Kararlı sürümler

| Yayınla | Hedef çerçeve | Destek sonu | Bağlantılar
|:--------|------------------|-----------------|------
| [EF Çekirdek 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.3) | .NET Standart 2.0 | 3 Aralık 2022 (LTS) | [Duyuru](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standart 2.1 | Süresi dolan 3 Mart 2020 | [Duyuru](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Son dakika değişiklikleri](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standart 2.0 | Son 23 Aralık 2019 | [Duyuru](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standart 2.0 | 21 Ağustos 2021 (LTS) | [Duyuru](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standart 2.0 | Süresi 1 Ekim 2018 | [Duyuru](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standart 1.3 | Süresi Dolan 27 Haziran 2019 | [Duyuru](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standart 1.3 | Süresi Dolan 27 Haziran 2019 | [Duyuru](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Her EF Core sürümü tarafından desteklenen belirli platformlar hakkında bilgi almak için [desteklenen platformlara](../platforms/index.md) bakın.

Destek süresi ve uzun vadeli destek (LTS) sürümleri hakkında bilgi için [.NET destek politikasına](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) bakın.

## <a name="guidance-on-updating-to-new-releases"></a>Yeni sürümlere güncelleştirme kılavuzu

* Desteklenen sürümler güvenlik ve diğer kritik hatalar için yamalı. Her zaman belirli bir sürümün en son yama kullanın. Örneğin, EF Core 2.1 için 2.1.14 kullanın.
* Ana sürüm güncelleştirmeleri (örneğin, EF Core 2'den EF Core 3'e kadar) genellikle kırılma değişiklikleri vardır. Ana sürümler arasında güncellenirken kapsamlı testler önerilir. Kesme değişiklikleri yle ilgili rehberlik için yukarıdaki son kesme değişiklikleri bağlantılarını kullanın.
* Küçük sürüm güncelleştirmeleri genellikle kesme değişiklikleri içermez. Ancak, yeni özellikler gerilemelere neden olabileceğinden, kapsamlı testler yine de önerilir.

## <a name="release-planning-and-schedules"></a>Yayın planlama ve zamanlamaları

EF Core bültenleri [.NET Core gönderi mataziyle](https://github.com/dotnet/core/blob/master/roadmap.md)uyumluhale geliyor.

Yama bültenleri genellikle aylık gemi, ancak uzun bir kurşun süresi var.
Bunu geliştirmek için çalışıyoruz.

Her [sürümde](release-planning.md) ne leri sevk edeceğimiz hakkında daha fazla bilgi için sürüm planlama sürecine bakın.
Biz genellikle bir sonraki büyük veya küçük sürüm daha fazla dışarı ayrıntılı planlama yapmayız.

## <a name="ef-core-50"></a>EF Core 5.0

Bir sonraki planlanan kararlı sürüm **EF Core 5.0**, Kasım 2020 için planlanan.

[EF Core 5.0 için üst düzey](ef-core-5.0/plan.md) bir plan, belgelenmiş sürüm planlama [işlemini](release-planning.md)izleyerek oluşturulmuştur.

Planlama hakkındaki görüşleriniz önemlidir.
Bir sorunun önemini belirtmenin en iyi yolu GitHub'daki bu sorun için oy kullanmaktır (thumbs-up). 👍
Bu veriler daha sonra bir sonraki sürüm için planlama sürecine beslenir.

### <a name="get-it-now"></a>Hemen alın!

EF Core 5.0 paketleri **artık**

* [Günlük yapılar](https://github.com/dotnet/aspnetcore/blob/master/docs/DailyBuilds.md)
  * En son özellikler ve hata düzeltmeleri. Genellikle çok kararlı; 57.000' den fazla test her yapıya karşı çalışır.
* [NuGet'de Önizlemeler](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
  * Günlük yapıların gerisinde kalır, ancak ilgili ASP.NET Core ve .NET Core önizlemeleriyle çalışmak üzere test edilir.

Önizlemeleri veya günlük yapıları kullanmak, sorunları bulmak ve mümkün olduğunca erken geri bildirim sağlamak için harika bir yoldur.
Bu tür geri bildirimleri ne kadar erken alırsak, bir sonraki resmi yayından önce işlem yapılabilir olma olasılığı o kadar artar.
