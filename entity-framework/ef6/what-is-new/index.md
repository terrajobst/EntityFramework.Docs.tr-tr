---
title: Yenilikler-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149116"
---
# <a name="whats-new-in-ef6"></a>EF6 'deki yenilikler

En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.
Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.
EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

EF 6.3.0 Runtime, Eylül 2019 ' de NuGet 'e yayımlandı. Bu sürümün ana amacı, EF 6 kullanan mevcut uygulamaların .NET Core 3,0 ' e geçirilmesini kolaylaştırmıştı. Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeleri de katkıda bulunur. Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın. Burada, daha fazla bilgi yer verilmiştir:

- .NET Core 3,0 desteği
  - EntityFramework paketi artık .NET Framework 4. x ' e ek olarak .NET Standard 2,1 ' i hedefliyor
  - Geçişler komutları, işlem dışında yürütülecek ve SDK stili projelerle çalışacak şekilde yeniden yazıldı
- SQL Server HierarchyId desteği
- Roslyn ve NuGet PackageReference ile iyileştirilmiş uyumluluk
- Derlemelerden geçişleri etkinleştirmek, eklemek, betik oluşturmak ve uygulamak için EF6. exe eklendi. Bu, migrate. exe ' nin yerini alır

## <a name="past-releases"></a>Geçmiş yayınlar

[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.
