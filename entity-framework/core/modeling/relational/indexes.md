---
title: Dizinler - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29679005"
---
# <a name="indexes"></a>Dizinler

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

İlişkisel bir veritabanındaki bir dizin aynı kavram çekirdek Entity Framework'ün bir dizin olarak eşleştirir.

## <a name="conventions"></a>Kurallar

Kurala göre dizinleri adlı `IX_<type name>_<property name>`. Bileşik dizinler için `<property name>` özellik adları alt çizgi ayrılmış listesini olur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Dizinleri veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Bir dizinin adını yapılandırmak için Fluent API kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Ayrıca, bir filtre belirtebilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

SQL Server sağlayıcısı EF kullanarak eklediğinde bir 'Is NOT NULL' için benzersiz bir dizinin parçası olan tüm boş değer atanabilir sütunları filtreleyin. Bu kural sağladığınız geçersiz kılmak için bir `null` değeri.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
