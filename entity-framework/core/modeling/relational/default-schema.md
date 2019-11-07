---
title: Varsayılan şema-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655977"
---
# <a name="default-schema"></a>Varsayılan Şema

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa nesnelerin oluşturulacağı veritabanı şemadır.

## <a name="conventions"></a>Kurallar

Kural gereği, veritabanı sağlayıcısı en uygun varsayılan şemayı seçer. Örneğin, Microsoft SQL Server `dbo` şemasını kullanır ve SQLite bir şema kullanmaz (çünkü şemalar SQLite ' de desteklenmez).

## <a name="data-annotations"></a>Veri Açıklamaları

Veri ek açıklamalarını kullanarak varsayılan şemayı ayarlayamazsınız.

## <a name="fluent-api"></a>Akıcı API

Varsayılan bir şema belirtmek için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
