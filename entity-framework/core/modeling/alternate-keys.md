---
title: Alternatif anahtarlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655770"
---
# <a name="alternate-keys"></a>Alternatif Anahtarlar

Alternatif bir anahtar, birincil anahtara ek olarak her bir varlık örneği için alternatif benzersiz bir tanımlayıcı işlevi görür. Alternatif anahtarlar bir ilişkinin hedefi olarak kullanılabilir. İlişkisel bir veritabanı kullanırken, bu, diğer anahtar sütunları üzerinde benzersiz bir dizin/kısıtlama kavramına eşlenir ve sütunlara (ler) başvuran bir veya daha fazla yabancı anahtar kısıtlamasıdır.

> [!TIP]  
> Yalnızca bir sütunun benzersizlik düzeyini zorlamak isterseniz, alternatif bir anahtar yerine benzersiz bir dizin isterseniz bkz. [dizinler](indexes.md). EF 'de, alternatif anahtarlar yabancı bir anahtarın hedefi olarak kullanılabildiğinden benzersiz dizinlerden daha fazla işlevsellik sağlar.

Diğer anahtarlar genellikle gerektiğinde sizin için sunulmuştur ve bunları el ile yapılandırmanız gerekmez. Daha fazla ayrıntı için bkz. [kurallar](#conventions) .

## <a name="conventions"></a>Kurallar

Kural gereği, bir ilişkinin hedefi olarak birincil anahtar olmayan bir özelliği tanımlarken sizin için alternatif bir anahtar sunulmuştur.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>Veri Açıklamaları

Alternatif anahtarlar, veri ek açıklamaları kullanılarak yapılandırılamaz.

## <a name="fluent-api"></a>Akıcı API

Tek bir özelliği alternatif bir anahtar olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

Ayrıca, birden çok özelliği farklı bir anahtar olacak şekilde (bileşik alternatif anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
