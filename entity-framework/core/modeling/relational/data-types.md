---
title: "Veri türleri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a>Veri Türleri

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Veri türü, veritabanının belirli bir özellik eşlenmiş sütununun türüne başvuruyor.

## <a name="conventions"></a>Kurallar

Kurala göre veritabanı sağlayıcısı özelliğin CLR türüne göre bir veri türünü seçer. Ayrıca dikkate alır yapılandırılmış gibi diğer meta verileri [en fazla uzunluk](../max-length.md), özelliğin bir birincil anahtar, vb. bir parçası olup.

Örneğin, SQL Server kullanır `datetime2(7)` için `DateTime` özelliklerini ve `nvarchar(max)` için `string` özellikleri (veya `nvarchar(450)` için `string` bir anahtar olarak kullanılan özellikleri).

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir sütun için bir tam veri türünü belirtmek için veri ek açıklamaları kullanabilirsiniz.

Örneğin aşağıdaki kodu yapılandırır `Url` unicode olmayan dize uzunluğu en fazla ile olarak `200` ve `Rating` kesinliğini ile ondalık `5` ve, ölçeği `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent API'si

Fluent API, sütunları için aynı veri türlerini belirtmek için de kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
