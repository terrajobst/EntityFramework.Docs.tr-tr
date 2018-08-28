---
title: EF Core için alternatif anahtarlar (benzersiz kısıtlamalar)-
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994197"
---
# <a name="alternate-keys-unique-constraints"></a>Alternatif anahtarlar (benzersiz kısıtlamalar)

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Modeldeki her alternatif anahtar için benzersiz kısıtlama tanıtılmaktadır.

## <a name="conventions"></a>Kurallar

Kural gereği, dizin ve alternatif anahtar için sunulan kısıtlaması adlandırılacağını `AK_<type name>_<property name>`. Bileşik alternatif anahtarlar için `<property name>` özellik adlarının bir alt çizgi ayrılmış listesi olur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Benzersiz kısıtlamalar veri ek açıklamalarını kullanma yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si alternatif anahtar dizini ve kısıtlama adı yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
