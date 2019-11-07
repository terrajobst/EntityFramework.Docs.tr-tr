---
title: Anahtarlar (birincil)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655941"
---
# <a name="keys-primary"></a>Anahtarlar (birincil)

Bir anahtar, her varlık örneği için birincil benzersiz tanımlayıcı işlevi görür. İlişkisel bir veritabanı kullanılırken bu, *birincil anahtar*kavramıyla eşlenir. Birincil anahtar olmayan benzersiz bir tanımlayıcı da yapılandırabilirsiniz (daha fazla bilgi için bkz. [Alternatif anahtarlar](alternate-keys.md) ).

Aşağıdaki yöntemlerden biri, bir birincil anahtar ayarlamak/oluşturmak için kullanılabilir.

## <a name="conventions"></a>Kurallar

Kurala göre, `Id` veya `<type name>Id` adlı bir özellik bir varlığın anahtarı olarak yapılandırılır.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Veri Açıklamaları

Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için, veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Akıcı API

Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Ayrıca, birden çok özelliği bir varlığın anahtarı olacak şekilde (bileşik anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz. Bileşik anahtarlar yalnızca akıcı API kullanılarak yapılandırılabilir-kurallar hiçbir şekilde bir bileşik anahtar kurmayacaktır ve veri açıklamalarını yapılandırmak için kullanamazsınız.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
