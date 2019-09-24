---
title: Oluşturulan değerler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6b38fd2e540ec29674f1116e7c204052d06ca1bc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197432"
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

Kullanılan veritabanı sağlayıcısına bağlı olarak, değerler istemci tarafında EF veya veritabanında oluşturulabilir. Değer veritabanı tarafından oluşturulduysa, varlığı bağlama eklediğinizde EF geçici bir değer atayabilir. Daha sonra bu geçici değer sırasında `SaveChanges()`veritabanı tarafından oluşturulan değerle değiştirilirler.

Özelliğe atanmış bir değere sahip bir varlık eklerseniz, EF yeni bir değer oluşturmak yerine bu değeri eklemeye çalışacaktır. Bir özellik, clr varsayılan değeri (`null` `0` `int` `string` için,for`Guid`, vb.) atanmadığı takdirde atanmış bir değere sahip olarak değerlendirilir. `Guid.Empty` Daha fazla bilgi için bkz. [oluşturulan Özellikler Için açık değerler](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Eklenen varlıklar için değerin nasıl oluşturulduğu, kullanılmakta olan veritabanı sağlayıcısına bağlıdır. Veritabanı sağlayıcıları bazı özellik türleri için değer oluşturmayı otomatik olarak oluşturabilir, ancak diğerleri değerin nasıl oluşturulduğunu el ile ayarlamanıza gerek olabilir.
>
> Örneğin, SQL Server kullanırken, özellikler için `GUID` değerler otomatik olarak oluşturulur (SQL Server sıralı GUID algoritması kullanılarak). Ancak, ekleme sırasında bir `DateTime` özelliğin oluşturulduğunu belirtirseniz, değerlerin oluşturulması için bir yol oluşturmanız gerekir. Bunu yapmanın bir yolu, varsayılan değerini `GETDATE()`yapılandırmak, bkz. [varsayılan değerler](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Ekleme veya güncelleştirme üzerinde oluşturulan değer

Ekleme veya güncelleştirme üzerinde oluşturulan değer, kayıt her kaydedildiğinde (ekleme veya güncelleştirme) yeni bir değerin oluşturulduğu anlamına gelir.

Benzer `value generated on add`şekilde, bir varlığın yeni eklenen örneğinde özelliği için bir değer belirtirseniz, bu değer, üretilmekte olan bir değer yerine eklenir. Güncelleştirme sırasında açık bir değer ayarlamak da mümkündür. Daha fazla bilgi için bkz. [oluşturulan Özellikler Için açık değerler](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Değerin eklenen ve güncelleştirilmiş varlıkların nasıl oluşturulduğu, kullanılmakta olan veritabanı sağlayıcısına bağlıdır. Veritabanı sağlayıcıları bazı özellik türleri için değer oluşturmayı otomatik olarak oluşturabilir, diğerleri ise değerin nasıl oluşturulduğunu el ile ayarlamanıza gerek duyar.
> 
> Örneğin, SQL Server kullanırken, `byte[]` ekleme veya güncelleştirme sırasında oluşturulan ve eşzamanlılık belirteçleri olarak işaretlenen özellikler, `rowversion` veri türüyle ayarlanır. böylece değerler veritabanında oluşturulacaktır. Ancak, ekleme veya güncelleştirme üzerinde bir `DateTime` özelliğin oluşturulduğunu belirtirseniz, değerlerin oluşturulması için bir yol oluşturmanız gerekir. Bunu yapmanın bir yolu, varsayılan değeri `GETDATE()` (bkz. [varsayılan değerler](relational/default-values.md)) yeni satırlara yönelik değerler oluşturmak için yapılandırmaktır. Daha sonra güncelleştirmeler sırasında değerler oluşturmak için bir veritabanı tetikleyicisi kullanabilirsiniz (örneğin, aşağıdaki örnek tetikleyici).
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
> `ValueGeneratedOnAdd()`EF 'in, eklenen varlıklar için değerlerin oluşturulduğunu bilmesini sağlar, bu, EF 'in değer oluşturmak için gerçek mekanizmayı ayarlayabileceklerini garanti etmez.  Daha fazla ayrıntı için bkz. [ekleme bölümünde oluşturulan değer](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Ekleme veya güncelleştirme üzerinde oluşturulan değer (akıcı API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Bu yalnızca AŞV 'nin eklenen veya güncelleştirilmiş varlıklar için değerlerin oluşturulduğunu bilmesini sağlar. Bu, EF 'in, değer oluşturmak için gerçek mekanizmayı ayarlayabileceklerini garanti etmez. Daha fazla ayrıntı için bkz. [ekleme veya güncelleştirme üzerinde oluşturulan değer](#value-generated-on-add-or-update) .
