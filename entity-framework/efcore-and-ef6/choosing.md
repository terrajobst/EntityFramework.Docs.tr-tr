---
title: "EF çekirdek ve sizin için uygun olan EF6-"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF çekirdek ve EF6: hangisinin sizin için uygun

Aşağıdaki bilgiler Entity Framework Çekirdek ve Entity Framework 6 arasında seçmenize yardımcı olur.

## <a name="guidance-for-new-applications"></a>Yeni uygulamalar için yönergeler

EF çekirdek yeni bir üründür ve hala bazı önemli O/RM özellikleri eksik olduğundan, EF6 birçok uygulama için en uygun seçim olmaya devam eder.

**EF çekirdek için kullanmanızı öneririz uygulama türleri şunlardır:**

* EF çekirdek henüz uygulanmadı özellikleri gerektirmeyen yeni uygulamalar. Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.

* .NET Core gibi Evrensel Windows Platformu (UWP) ve ASP.NET Core uygulamaları hedefleyen uygulamalar. .NET Framework (yani .NET Framework 4.5) gerektirdiği şekilde bu uygulamaları EF6 kullanamazsınız.

## <a name="guidance-for-existing-ef6-applications"></a>Mevcut EF6 uygulamalar için kılavuz

EF çekirdek temel değişiklikleri nedeniyle değişiklik yapmak için ilgi çekici bir nedeniniz yoksa EF6 uygulamaya EF çekirdek taşınmaya çalışılırken önermiyoruz. Taşımak istiyorsanız EF yeni özellikler ve ardından emin olun kullandığınız yapmak için çekirdek kendi sınırlamaları kullanan başlamadan önce. Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.

**Taşımak EF6 EF çekirdek için bir yükseltme yerine bir bağlantı noktası olarak görüntülemeniz gerekir.** Bkz: [EF çekirdek EF6 bağlantı noktası oluşturma](porting/index.md) daha fazla bilgi için.
