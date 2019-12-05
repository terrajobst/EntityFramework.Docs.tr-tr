---
title: Anahtarlar (birincil)-EF Core
description: Entity Framework Core kullanırken varlık türleri için anahtarlar yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824614"
---
# <a name="keys-primary"></a>Anahtarlar (birincil)

Bir anahtar, her varlık örneği için birincil benzersiz tanımlayıcı işlevi görür. İlişkisel bir veritabanı kullanılırken bu, *birincil anahtar*kavramıyla eşlenir. Birincil anahtar olmayan benzersiz bir tanımlayıcı da yapılandırabilirsiniz (daha fazla bilgi için bkz. [Alternatif anahtarlar](alternate-keys.md) ).

Aşağıdaki yöntemlerden biri, bir birincil anahtar ayarlamak/oluşturmak için kullanılabilir.

## <a name="conventions"></a>Kurallar

Varsayılan olarak, `Id` veya `<type name>Id` adlı bir özellik bir varlığın anahtarı olarak yapılandırılır.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> [Sahip olan varlık türleri](xref:core/modeling/owned-entities) anahtarları tanımlamak için farklı kurallar kullanır.

## <a name="data-annotations"></a>Veri Açıklamaları

Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için, veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Akıcı API

Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Ayrıca, birden çok özelliği bir varlığın anahtarı olacak şekilde (bileşik anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz. Bileşik anahtarlar yalnızca akıcı API kullanılarak yapılandırılabilir-kurallar hiçbir şekilde bir bileşik anahtar kurmayacaktır ve veri açıklamalarını yapılandırmak için kullanamazsınız.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Anahtar türleri ve değerleri

EF Core, `string`, `Guid`, `byte[]` ve diğerleri dahil olmak üzere herhangi bir ilkel türün özelliklerinin, birincil anahtar olarak kullanılmasını destekler. Ancak tüm veritabanları bunları desteklemez. Bazı durumlarda, anahtar değerleri otomatik olarak desteklenen bir türe dönüştürülebilir, aksi takdirde dönüştürmenin [el ile belirtilmesi](xref:core/modeling/value-conversions)gerekir.

Bağlam için yeni bir varlık eklenirken anahtar özellikler her zaman varsayılan olmayan bir değere sahip olmalıdır, ancak bazı türler [veritabanı tarafından oluşturulacaktır](xref:core/modeling/generated-properties). Bu durumda, varlık izleme amacıyla eklendiğinde EF geçici bir değer oluşturmaya çalışır. [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) çağrıldıktan sonra, geçici değer veritabanı tarafından oluşturulan değerle değiştirilmelidir.

> [!Important]
> Bir anahtar özelliğin veritabanı tarafından oluşturulmuş bir değeri varsa ve bir varlık eklendiğinde varsayılan olmayan bir değer belirtilmişse EF, varlığın veritabanında zaten var olduğunu varsayar ve yeni bir tane eklemek yerine bu alanı güncelleştirmeye çalışır. Bu değer oluşturmayı devre dışı bırakmak veya [üretilen özellikler için nasıl açık değerler belirteceğinize](../saving/explicit-values-generated-properties.md)bakın.