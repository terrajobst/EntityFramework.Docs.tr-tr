---
title: Veri türleri-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149162"
---
# <a name="data-types"></a>Veri Türleri

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Veri türü, bir özelliğin eşlendiği sütunun veritabanına özgü türünü ifade eder.

## <a name="conventions"></a>Kurallar

Kural gereği, veritabanı sağlayıcısı özelliğin .NET türüne göre bir veri türü seçer. Ayrıca, yapılandırılmış [Maksimum uzunluk](../max-length.md), özelliğin birincil bir anahtarın parçası olup olmadığı gibi diğer meta veriler de hesaba girer.

Örneğin, SQL Server `DateTime` özellikler için `datetime2(7)` `nvarchar(max)` ve `string` özellikler için (veya `nvarchar(450)` anahtar olarak kullanılan özellikler `string` için) kullanılır.

## <a name="data-annotations"></a>Veri açıklamaları

Bir sütun için tam bir veri türü belirtmek için veri açıklamalarını kullanabilirsiniz.

Örneğin, aşağıdaki kod, en `Url` fazla `200` uzunluğu olan Unicode olmayan bir dize olarak ve `Rating` kesinliği `5` ve ölçeği `2`ile ondalık olarak yapılandırılır.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Akıcı API

Ayrıca, sütunlar için aynı veri türlerini belirtmek üzere Floent API 'sini de kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
