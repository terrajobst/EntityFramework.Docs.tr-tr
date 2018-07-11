---
title: EF Core ve EF6 - sizin için uygun olan
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949146"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core ve EF6: hangisinin sizin için uygun

Aşağıdaki bilgiler, Entity Framework Core ve Entity Framework 6 arasında seçim yardımcı olur.

## <a name="guidance-for-new-applications"></a>Yeni uygulamalar için yönergeler

EF Core için yeni uygulamalar ' ın tüm özelliklerini EF Core yararlanmak istiyorsanız ve uygulamanızı EF Core henüz uygulanmadı herhangi bir özellik gerektirmez kullanmayı düşünün.

EF6 .NET Framework 4.0 (veya sonraki bir sürüm) gerektirir ve yalnızca Windows üzerinde desteklenir (diğer bir deyişle, EF6 şu anda .NET Core üzerinde çalışmaz ve diğer işletim sistemlerinde desteklenmez), ancak uzun bu kısıtlamaları olarak hala yeni uygulamalar için uygun bir seçim kabul edilebilir ve uygulamanın EF Core için EF6 kullanılamaz yeni özellikler gerektirmez.

Gözden geçirme [özellik karşılaştırması](features.md) EF Core uygulamanız için uygun bir seçim olabilir, görmek için.

## <a name="guidance-for-existing-ef6-applications"></a>EF6 uygulamalarınız için yönergeler

EF Core temel yapılan değişiklikler nedeniyle değişiklik yapmak için yeterli bir neden olmadığı sürece EF6 uygulamanın EF core'a taşıma girişiminde önermiyoruz. Taşımak istiyorsanız kullandığınız yeni özellikler sonra emin olmak için EF Core var. onun sınırlamaları dikkate başlamadan önce Gözden geçirme [özellik karşılaştırması](features.md) EF Core uygulamanız için uygun bir seçim olabilir, görmek için.

**EF6'dan yükseltme işlemi yerine bir bağlantı noktası olarak hareket EF core'a görüntülemeniz gerekir.** Bkz: [EF6'dan EF Core'a taşıma](porting/index.md) daha fazla bilgi için.
