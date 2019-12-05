---
title: Yabancı anahtar kısıtlamaları-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824582"
---
# <a name="foreign-key-constraints"></a>Yabancı Anahtar Kısıtlamaları

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Modeldeki her ilişki için bir yabancı anahtar kısıtlaması tanıtılmıştır.

## <a name="conventions"></a>Kurallar

Kurala göre, yabancı anahtar kısıtlamaları `FK_<dependent type name>_<principal type name>_<foreign key property name>`olarak adlandırılır. Bileşik yabancı anahtarlar için `<foreign key property name>` yabancı anahtar özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.

## <a name="data-annotations"></a>Veri Açıklamaları

Yabancı anahtar kısıtlama adları, veri açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Bir ilişkinin yabancı anahtar kısıtlama adını yapılandırmak için akıcı API 'YI kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
