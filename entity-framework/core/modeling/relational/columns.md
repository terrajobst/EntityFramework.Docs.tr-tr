---
title: Sütun eşleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197206"
---
# <a name="column-mapping"></a>Sütun Eşleme

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Sütun eşleme, hangi sütun verilerinin sorgulandığını ve veritabanında veritabanına kaydedileceğini belirler.

## <a name="conventions"></a>Kurallar

Kurala göre, her bir özellik, özelliği ile aynı ada sahip bir sütuna eşlenecek şekilde ayarlanır.

## <a name="data-annotations"></a>Veri Açıklamaları

Bir özelliğin eşlendiği sütunu yapılandırmak için, veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Akıcı API

Bir özelliğin eşlendiği sütunu yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
