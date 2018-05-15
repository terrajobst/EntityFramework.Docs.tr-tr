---
title: Verileri dengeli - EF çekirdek
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="data-seeding"></a>Dengeli veri

> [!NOTE]  
> Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.

Bir veritabanını doldurmak için ilk veri sağlamak için verileri dengeli sağlar. Farklı olarak EF çekirdek içinde EF6 içinde verileri dengeli bir varlık türü, model yapılandırmasının bir parçası olarak ilişkilendirilir. Ardından EF çekirdek [geçişler](xref:core/managing-schemas/migrations/index) ne ekleme, güncelleştirme veya silme işlemleri veritabanı modeli yeni bir sürüme yükseltirken uygulanması gerek işlem otomatik olarak.

Örnek olarak, bu çekirdek verileri için yapılandırmak için kullanabileceğiniz bir `Blog` içinde `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Bir ilişki yabancı anahtar değerlere sahip varlıklar ekleyebilir belirtilmesi gerekir. Sık anonim bir sınıf değerlerini ayarlamak için kullanılması gereken şekilde gölge durumunda, yabancı anahtar özellikleri şunlardır:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Varlıkları eklendikten sonra kullanmak için önerilir [geçişler](xref:core/managing-schemas/migrations/index) değişiklikleri uygulamak için. 

Alternatif olarak, kullanabileceğiniz `context.Database.EnsureCreated()` çekirdek verileri, örneğin, bir test veritabanı için veya bellek içi sağlayıcı kullanırken içeren yeni bir veritabanı oluşturmak için. Unutmayın veritabanı zaten var., `EnsureCreated()` hiçbiri şemayı ya da veritabanındaki çekirdek verileri güncelleştirir.
