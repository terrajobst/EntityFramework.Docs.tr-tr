---
title: Veri türleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993527"
---
# <a name="data-types"></a>Veri Türleri

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Veri türü için bir özellik eşlendi sütun veritabanı belirli türüne başvuruyor.

## <a name="conventions"></a>Kurallar

Kural gereği, veritabanı sağlayıcısı özelliğin CLR türüne göre bir veri türünü seçer. Hesaba katar yapılandırılmış gibi diğer meta veriler [en fazla uzunluk](../max-length.md), özelliğin bir birincil anahtar, vb. bir parçası olup.

Örneğin, SQL Server'ı kullanır `datetime2(7)` için `DateTime` özellikleri ve `nvarchar(max)` için `string` özelliklerini (veya `nvarchar(450)` için `string` bir anahtar olarak kullanılan özellikleri).

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir sütun için bir tam veri türünü belirtmek için veri ek açıklamaları kullanabilirsiniz.

Örneğin aşağıdaki kod yapılandırır `Url` uzunluğu en fazla bir unicode olmayan dize olarak `200` ve `Rating` duyarlığını ile ondalık `5` ve, ölçeği `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, sütunlar için aynı veri türlerini belirtmek için de kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
