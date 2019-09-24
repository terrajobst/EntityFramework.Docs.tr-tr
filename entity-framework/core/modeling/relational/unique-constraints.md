---
title: Alternatif anahtarlar (benzersiz kısıtlamalar)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197608"
---
# <a name="alternate-keys-unique-constraints"></a>Alternatif Anahtarlar (Benzersiz Kısıtlamalar)

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Modeldeki her alternatif anahtar için benzersiz bir kısıtlama getirilmiştir.

## <a name="conventions"></a>Kurallar

Kurala göre, alternatif bir anahtar için tanıtılan dizin ve kısıtlama adlandıralınacaktır `AK_<type name>_<property name>`. Bileşik alternatif anahtarlar `<property name>` için özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.

## <a name="data-annotations"></a>Veri Açıklamaları

Benzersiz kısıtlamalar, veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Farklı bir anahtar için dizin ve kısıtlama adını yapılandırmak üzere Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
