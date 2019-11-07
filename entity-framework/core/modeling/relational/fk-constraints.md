---
title: Yabancı anahtar kısıtlamaları-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655992"
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
