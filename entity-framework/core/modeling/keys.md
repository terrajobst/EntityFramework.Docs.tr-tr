---
title: Anahtarlar-EF Core
description: Entity Framework Core kullanırken varlık türleri için anahtarlar yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416469"
---
# <a name="keys"></a>Anahtarlar

Bir anahtar, her varlık örneği için benzersiz bir tanımlayıcı işlevi görür. EF 'teki varlıkların çoğu, ilişkisel veritabanlarında *birincil anahtar* kavramıyla eşleşen tek bir anahtara sahiptir (anahtar içermeyen varlıklar için bkz. [Keyless varlıkları](xref:core/modeling/keyless-entity-types)). Varlıklar birincil anahtarın ötesinde ek anahtarlara sahip olabilir (daha fazla bilgi için bkz. [Alternatif anahtarlar](#alternate-keys) ).

Kurala göre, `Id` veya `<type name>Id` adlı bir özellik, bir varlığın birincil anahtarı olarak yapılandırılır.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> [Sahip olan varlık türleri](xref:core/modeling/owned-entities) anahtarları tanımlamak için farklı kurallar kullanır.

Tek bir özelliği, bir varlığın birincil anahtarı olacak şekilde aşağıdaki gibi yapılandırabilirsiniz:

## <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

Ayrıca, birden çok özelliği bir varlığın anahtarı olacak şekilde yapılandırabilirsiniz. Bu bileşik anahtar olarak bilinir. Bileşik anahtarlar yalnızca Floent API 'SI kullanılarak yapılandırılabilir; kurallar hiçbir şekilde bir bileşik anahtar kurmayacaktır ve veri açıklamalarını yapılandırmak için kullanamazsınız.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a>Birincil anahtar adı

Kurala göre, ilişkisel veritabanlarında birincil anahtarlar `PK_<type name>`adıyla oluşturulur. Birincil anahtar kısıtlamasının adını şu şekilde yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a>Anahtar türleri ve değerleri

EF Core, `string`, `Guid`, `byte[]` ve diğerleri dahil olmak üzere herhangi bir ilkel türün özelliklerinin kullanılmasını destekler, çünkü tüm veritabanları anahtar olarak tüm türleri desteklemez. Bazı durumlarda, anahtar değerleri otomatik olarak desteklenen bir türe dönüştürülebilir, aksi takdirde dönüştürmenin [el ile belirtilmesi](xref:core/modeling/value-conversions)gerekir.

Bağlam için yeni bir varlık eklenirken anahtar özellikler her zaman varsayılan olmayan bir değere sahip olmalıdır, ancak bazı türler [veritabanı tarafından oluşturulacaktır](xref:core/modeling/generated-properties). Bu durumda, varlık izleme amacıyla eklendiğinde EF geçici bir değer oluşturmaya çalışır. [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) çağrıldıktan sonra, geçici değer veritabanı tarafından oluşturulan değerle değiştirilmelidir.

> [!Important]
> Bir anahtar özelliğin değeri veritabanı tarafından oluşturulduysa ve bir varlık eklendiğinde varsayılan olmayan bir değer belirtilmişse EF, varlığın veritabanında zaten var olduğunu varsayacaktır ve yenisini eklemek yerine, güncelleştirmeyi deneyebilir. Bu değer oluşturmayı devre dışı bırakmak veya [üretilen özellikler için nasıl açık değerler belirteceğinize](../saving/explicit-values-generated-properties.md)bakın.

## <a name="alternate-keys"></a>Alternatif Anahtarlar

Alternatif bir anahtar, birincil anahtara ek olarak her bir varlık örneği için alternatif benzersiz tanımlayıcı işlevi görür; ilişki hedefi olarak kullanılabilir. İlişkisel bir veritabanı kullanırken, bu, diğer anahtar sütunları üzerinde benzersiz bir dizin/kısıtlama kavramına eşlenir ve sütunlara (ler) başvuran bir veya daha fazla yabancı anahtar kısıtlamasıdır.

> [!TIP]
> Yalnızca bir sütunda benzersizlik zorlamak istiyorsanız, alternatif bir anahtar yerine benzersiz bir dizin tanımlayın (bkz. [dizinler](indexes.md)). EF 'de, alternatif anahtarlar salt okunurdur ve benzersiz dizinler üzerinde ek semantik sağlar çünkü bir yabancı anahtarın hedefi olarak kullanılabilirler.

Diğer anahtarlar genellikle gerektiğinde sizin için sunulmuştur ve bunları el ile yapılandırmanız gerekmez. Kural gereği, bir ilişkinin hedefi olarak birincil anahtar olmayan bir özelliği tanımlarken sizin için alternatif bir anahtar sunulmuştur.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

Ayrıca, tek bir özelliği alternatif bir anahtar olacak şekilde yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

Ayrıca, birden çok özelliği alternatif bir anahtar (bileşik alternatif anahtar olarak bilinir) olarak yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

Son olarak, kurala göre, alternatif bir anahtar için tanıtılan dizin ve kısıtlama `AK_<type name>_<property name>` olarak adlandırılır (`<property name>` bileşik alternatif anahtarlar için, özellik adlarının alt çizgiyle ayrılmış bir listesi olur). Alternatif anahtarın dizin ve benzersiz kısıtlamasının adını yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
