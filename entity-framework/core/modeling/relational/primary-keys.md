---
title: Birincil anahtarlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656044"
---
# <a name="primary-keys"></a>Birincil Anahtarlar

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Her varlık türünün anahtarı için bir birincil anahtar kısıtlaması tanıtılmıştır.

## <a name="conventions"></a>Kurallar

Kurala göre, veritabanındaki birincil anahtar `PK_<type name>`olarak adlandırılır.

## <a name="data-annotations"></a>Veri Açıklamaları

Birincil anahtarın ilişkisel veritabanı özel yönleri, veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Akıcı API 'YI, veritabanında birincil anahtar kısıtlamasının adını yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
