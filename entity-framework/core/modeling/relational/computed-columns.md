---
title: Hesaplanan sütunlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655925"
---
# <a name="computed-columns"></a>Hesaplanan Sütunlar

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Hesaplanan sütun, değeri veritabanında hesaplanan bir sütundur. Hesaplanan bir sütun, değerini hesaplamak için tablodaki diğer sütunları kullanabilir.

## <a name="conventions"></a>Kurallar

Kurala göre, hesaplanan sütunlar modelde oluşturulmaz.

## <a name="data-annotations"></a>Veri Açıklamaları

Hesaplanan sütunlar, veri açıklamaları ile yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Bir özelliğin hesaplanan bir sütuna eşlenmesi gerektiğini belirtmek için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
