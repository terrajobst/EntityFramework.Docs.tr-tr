---
title: "Alternatif anahtarları (benzersiz kısıtlamalar) - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a>Alternatif anahtarları (benzersiz kısıtlamalar)

> [!NOTE]  
> Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

Model diğer her anahtar için benzersiz bir kısıtlama sunmuştur.

## <a name="conventions"></a>Kurallar

Kurala göre indeks ve için alternatif bir anahtar sunulan kısıtlaması adlandırılacağını `AK_<type name>_<property name>`. Bileşik alternatif anahtarları için `<property name>` özellik adları alt çizgi ayrılmış listesini olur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Benzersiz kısıtlamalar veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API, alternatif bir anahtar dizini ve kısıtlama adı yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
