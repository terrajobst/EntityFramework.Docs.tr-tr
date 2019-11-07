---
title: Oluşturulan değerler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6643d3c5c9b3363e450e820793f449a41e2eba80
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655754"
---
# <a name="generated-values"></a>Oluşturulan Değerler

## <a name="value-generation-patterns"></a>Değer oluşturma desenleri

Özellikler için kullanılabilecek üç değer oluşturma deseni vardır:

* Değer oluşturma yok
* Ekleme sırasında oluşturulan değer
* Ekleme veya güncelleştirme üzerinde oluşturulan değer

### <a name="no-value-generation"></a>Değer oluşturma yok

Değer oluşturma, her zaman veritabanına kaydedilecek geçerli bir değer sağlayacaksınız anlamına gelir. Bu geçerli değer, içeriğe eklenmeden önce yeni varlıklara atanmalıdır.

### <a name="value-generated-on-add"></a>Ekleme sırasında oluşturulan değer

Ekleme sırasında oluşturulan değer, yeni varlıklar için bir değerin oluşturulduğu anlamına gelir.

Kullanılan veritabanı sağlayıcısına bağlı olarak, değerler istemci tarafında EF veya veritabanında oluşturulabilir. Değer veritabanı tarafından oluşturulduysa, varlığı bağlama eklediğinizde EF geçici bir değer atayabilir. Bu geçici değer, `SaveChanges()`sırasında veritabanı tarafından oluşturulan değerle değiştirilmelidir.

Özelliğe atanmış bir değere sahip bir varlık eklerseniz, EF yeni bir değer oluşturmak yerine bu değeri eklemeye çalışacaktır. Bir özellik, CLR varsayılan değeri (`string`için`null`, `int`için `0` `Guid.Empty`, vb.) atanmadığı takdirde atanan bir değere sahip olarak kabul edilir. Daha fazla bilgi için bkz. [oluşturulan Özellikler Için açık değerler](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Eklenen varlıklar için değerin nasıl oluşturulduğu, kullanılmakta olan veritabanı sağlayıcısına bağlıdır. Veritabanı sağlayıcıları bazı özellik türleri için değer oluşturmayı otomatik olarak oluşturabilir, ancak diğerleri değerin nasıl oluşturulduğunu el ile ayarlamanıza gerek olabilir.
>
> Örneğin, SQL Server kullanırken, `GUID` özellikler için değerler otomatik olarak oluşturulur (SQL Server sıralı GUID algoritması kullanılarak). Ancak, ekleme sırasında bir `DateTime` özelliğinin oluşturulduğunu belirtirseniz, değerlerin oluşturulması için bir yol oluşturmanız gerekir. Bunu yapmanın bir yolu, `GETDATE()`varsayılan değerini yapılandırmak için [varsayılan değerler](relational/default-values.md)bölümüne bakın.

### <a name="value-generated-on-add-or-update"></a>Ekleme veya güncelleştirme üzerinde oluşturulan değer

Ekleme veya güncelleştirme üzerinde oluşturulan değer, kayıt her kaydedildiğinde (ekleme veya güncelleştirme) yeni bir değerin oluşturulduğu anlamına gelir.

`value generated on add`gibi, bir varlığın yeni eklenen örneğinde özellik için bir değer belirtirseniz, bu değer, üretilmekte olan bir değer yerine eklenir. Güncelleştirme sırasında açık bir değer ayarlamak da mümkündür. Daha fazla bilgi için bkz. [oluşturulan Özellikler Için açık değerler](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Değerin eklenen ve güncelleştirilmiş varlıkların nasıl oluşturulduğu, kullanılmakta olan veritabanı sağlayıcısına bağlıdır. Veritabanı sağlayıcıları bazı özellik türleri için değer oluşturmayı otomatik olarak oluşturabilir, diğerleri ise değerin nasıl oluşturulduğunu el ile ayarlamanıza gerek duyar.
>
> Örneğin, SQL Server kullanırken, ekleme veya güncelleştirme sırasında oluşturulan ve eşzamanlılık belirteçleri olarak işaretlenen `byte[]` özellikler, `rowversion` veri türüyle ayarlanır. böylece değerler veritabanında oluşturulur. Ancak, ekleme veya güncelleştirme üzerinde bir `DateTime` özelliğinin oluşturulduğunu belirtirseniz, değerlerin oluşturulması için bir yol oluşturmanız gerekir. Bunu yapmanın bir yolu, yeni satırların değerlerini oluşturmak için `GETDATE()` varsayılan değerini (bkz. [varsayılan değerler](relational/default-values.md)) yapılandırmak içindir. Daha sonra güncelleştirmeler sırasında değerler oluşturmak için bir veritabanı tetikleyicisi kullanabilirsiniz (örneğin, aşağıdaki örnek tetikleyici).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Kurallar

Kurala göre, Short, int, Long veya GUID türündeki bileşik olmayan birincil anahtarlar, ekleme sırasında oluşturulan değerleri alacak şekilde oluşturulacaktır. Diğer tüm özellikler hiçbir değer üretimi olmadan kuruluma sunulacaktır.

## <a name="data-annotations"></a>Veri Açıklamaları

### <a name="no-value-generation-data-annotations"></a>Değer üretimi yok (veri ek açıklamaları)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Add (veri ek açıklamaları) üzerinde oluşturulan değer

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Bu yalnızca AŞV 'nin eklenen varlıklar için değerlerin oluşturulduğunu bilmesini sağlar. Bu, EF 'in değer oluşturmak için gerçek mekanizmayı ayarlayabileceklerini garanti etmez. Daha fazla ayrıntı için bkz. [ekleme bölümünde oluşturulan değer](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-data-annotations"></a>Ekleme veya güncelleştirme üzerinde oluşturulan değer (veri açıklamaları)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Bu yalnızca AŞV 'nin eklenen veya güncelleştirilmiş varlıklar için değerlerin oluşturulduğunu bilmesini sağlar. Bu, EF 'in, değer oluşturmak için gerçek mekanizmayı ayarlayabileceklerini garanti etmez. Daha fazla ayrıntı için bkz. [ekleme veya güncelleştirme üzerinde oluşturulan değer](#value-generated-on-add-or-update) .

## <a name="fluent-api"></a>Akıcı API

Belirli bir özellik için değer oluşturma modelini değiştirmek üzere Floent API 'sini kullanabilirsiniz.

### <a name="no-value-generation-fluent-api"></a>Değer oluşturma (Floent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Add (Floent API) üzerinde oluşturulan değer

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` yalnızca, eklenen varlıklar için değerlerin oluşturulduğunu bilmesini sağlar, EF 'in değer oluşturmak için gerçek mekanizmayı ayarlayacağının garantisi yoktur.  Daha fazla ayrıntı için bkz. [ekleme bölümünde oluşturulan değer](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Ekleme veya güncelleştirme üzerinde oluşturulan değer (akıcı API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Bu yalnızca AŞV 'nin eklenen veya güncelleştirilmiş varlıklar için değerlerin oluşturulduğunu bilmesini sağlar. Bu, EF 'in, değer oluşturmak için gerçek mekanizmayı ayarlayabileceklerini garanti etmez. Daha fazla ayrıntı için bkz. [ekleme veya güncelleştirme üzerinde oluşturulan değer](#value-generated-on-add-or-update) .
