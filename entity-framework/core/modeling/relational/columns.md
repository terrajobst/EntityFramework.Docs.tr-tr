---
title: Sütun eşlemesi - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929868"
---
# <a name="column-mapping"></a>Sütun eşleme

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Sütun eşlemesi, hangi sütunun veri gelen sorgulanabilen ve veritabanında kaydedilmiş tanımlar.

## <a name="conventions"></a>Kurallar

Kural gereği, her bir özellik özelliğiyle aynı ada sahip bir sütun eşlemek için ayarlanır.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özellik için eşlenen sütun yapılandırmak için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir özellik için eşlenen sütun yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
