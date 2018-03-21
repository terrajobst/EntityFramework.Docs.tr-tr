---
title: "EF çekirdek ve sizin için uygun olan EF6-"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2018
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF çekirdek ve EF6: hangisinin sizin için uygun

Aşağıdaki bilgiler Entity Framework Çekirdek ve Entity Framework 6 arasında seçmenize yardımcı olur.

## <a name="guidance-for-new-applications"></a>Yeni uygulamalar için yönergeler

EF çekirdek yeni uygulamalar için uygulamanın EF çekirdek henüz uygulanmadı herhangi bir özellik gerektirmez ve tüm özelliklerini EF çekirdek yararlanmak isterseniz kullanmayı düşünün.

EF6 .NET Framework 4.0 (veya sonraki bir sürümünü) gerektirir ve yalnızca (yani, .NET Core üzerinde çalışmaz ve diğer işletim sistemlerinde desteklenmez) Windows desteklenir, ancak hala uzun bu kısıtlamalar kabul edilebilir olduğundan yeni uygulamalar için uygun bir seçim değil ve bir uygulama için EF6 bulunmayan yeni EF çekirdek özellikler gerektirmez.

Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.

## <a name="guidance-for-existing-ef6-applications"></a>Mevcut EF6 uygulamalar için kılavuz

EF çekirdek temel değişiklikleri nedeniyle değişiklik yapmak için ilgi çekici bir nedeniniz yoksa EF6 uygulamaya EF çekirdek taşınmaya çalışılırken önermiyoruz. Taşımak istiyorsanız EF yeni özellikler ve ardından emin olun kullandığınız yapmak için çekirdek kendi sınırlamaları kullanan başlamadan önce. Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.

**Taşımak EF6 EF çekirdek için bir yükseltme yerine bir bağlantı noktası olarak görüntülemeniz gerekir.** Bkz: [EF çekirdek EF6 bağlantı noktası oluşturma](porting/index.md) daha fazla bilgi için.
