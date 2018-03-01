---
title: "Verileri dengeli - EF çekirdek"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a>Dengeli veri

> [!NOTE]  
> Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.

Bir veritabanını doldurmak için ilk veri sağlamak için verileri dengeli sağlar. Farklı olarak EF çekirdek içinde EF6 içinde verileri dengeli bir varlık türü, model yapılandırmasının bir parçası olarak ilişkilendirilir. Ardından EF çekirdek geçişler otomatik olarak ne ekleme, güncelleştirme veya silme işlemleri veritabanı modeli yeni bir sürüme yükseltirken uygulanması gerek hesaplayabilirsiniz.

Örnek olarak, bu çekirdek verileri için yapılandırmak için kullanabileceğiniz bir `Blog` içinde `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Bir ilişki yabancı anahtar değerlere sahip varlıklar ekleyebilir belirtilmesi gerekir. Sık anonim bir sınıf değerlerini ayarlamak için kullanılması gereken şekilde gölge durumunda, yabancı anahtar özellikleri şunlardır:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
