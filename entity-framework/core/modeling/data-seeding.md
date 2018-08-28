---
title: Veri üretme - EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994485"
---
# <a name="data-seeding"></a>Veri çekirdeği oluşturma

> [!NOTE]  
> Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.

Bir veritabanını doldurmak için ilk veri sağlayacak veri üretme sağlar. EF Core içinde EF6 içinde veri dengeli dağıtım modeli yapılandırmasının bir parçası olarak bir varlık türü ile ilişkili benzemez. Ardından EF Core [geçişler](xref:core/managing-schemas/migrations/index) ne ekleme, güncelleştirme veya silme işlemleri veritabanı modelin yeni bir sürüme yükseltirken uygulanmasına gerek işlem otomatik olarak.

Örneğin, bu için çekirdek veri yapılandırmak için kullanabileceğiniz bir `Blog` içinde `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Bir ilişki, yabancı anahtar değerlerine sahip varlıklar eklemek için belirtilmesi gerekir. Sık anonim bir sınıf değerleri ayarlayabilmek için kullanılması gereken şekilde gölge durumunda, yabancı anahtar özellikler şunlardır:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Varlıkları ekledikten sonra kullanmak için önerilir [geçişler](xref:core/managing-schemas/migrations/index) değişiklikleri uygulamak için. 

Alternatif olarak, `context.Database.EnsureCreated()` bellek içi sağlayıcısı kullanırken veya örneğin, bir test veritabanı için çekirdek veri içeren yeni bir veritabanı oluşturmak için. Unutmayın veritabanı zaten var, `EnsureCreated()` ne ya da veritabanında çekirdek veri şemasını güncelleştirir.
