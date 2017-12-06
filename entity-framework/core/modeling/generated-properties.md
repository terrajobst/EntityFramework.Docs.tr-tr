---
title: "Oluşturulan değerler - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 2d79bf1339ebe522c39fe8971d908c30e1f4dca0
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="generated-values"></a>Oluşturulan değerleri

## <a name="value-generation-patterns"></a>Değer üretme deseni

Özellikler için kullanılabilir üç değeri üretme deseni vardır.

### <a name="no-value-generation"></a>Hiçbir değer oluşturma

Hiçbir değer oluşturma, veritabanına kaydedilmesi için geçerli bir değer her zaman sağlayacak anlamına gelir. Bağlamına eklenmeden önce yeni varlıklar için bu geçerli bir değer atanmalıdır.

### <a name="value-generated-on-add"></a>Üzerinde üretilen değer ekleme

Üzerinde üretilen değer Ekle anlamına gelir, yeni varlıklar için bir değer oluşturulur.

Veritabanı sağlayıcısı bağlı olarak kullanılan değerler olabilir istemci tarafı EF ya da veritabanında oluşturulmuş. Ardından değeri veritabanı tarafından oluşturulmuşsa bağlamına varlık eklediğinizde EF geçici bir değeri atayabilir. Bu geçici bir değerinin sırasında oluşturulan veritabanı değeriyle sonra değiştirilecek `SaveChanges()`.

Bir varlık özelliğine atanan bir değere sahip bağlam eklerseniz, yeni bir tane oluşturmak yerine bu değeri eklemek EF deneyecek. Bir özelliğin CLR varsayılan değer atanmamışsa atanmış bir değere sahip olduğu kabul edildiği (`null` için `string`, `0` için `int`, `Guid.Empty` için `Guid`, vb..). Daha fazla bilgi için bkz: [oluşturulan özellikler için açık değerler](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Değer eklenen varlıklar için nasıl oluşturulacağını kullanılan veritabanı sağlayıcısı bağlıdır. Veritabanı sağlayıcıları otomatik olarak değer oluşturma bazı özellik türleri için Kurulum, ancak diğer el ile değer nasıl oluşturulacağını Kurulum gerektirebilir.
>
> Örneğin, SQL Server kullanırken, değerleri otomatik olarak için oluşturulacak `GUID` özellikleri (SQL Server sıralı GUID algoritmasını kullanarak). Ancak, belirtirseniz bir `DateTime` özelliği oluşturulur oluşturulması için değerleri için bir yol kurulumu daha sonra eklemek. Bunu yapmanın bir yolu olan bir varsayılan değerini yapılandırmak için `GETDATE()`, bkz: [varsayılan değerleri](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Üretilen değer ekleme veya güncelleştirme

Oluşturulan değeri eklemek veya güncelleştirme anlamına gelir (INSERT veya update) kaydı kaydedilen her zaman yeni bir değer oluşturulur.

Gibi `value generated on add`, değer oluşturulan bir değer yerine eklenir, bir varlığın yeni eklenen bir örnek özelliği için bir değer belirtirseniz. Açık bir değer güncelleştirilirken ayarlamak da mümkündür. Daha fazla bilgi için bkz: [oluşturulan özellikler için açık değerler](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Değer eklenen ve güncelleştirilen varlıklar için nasıl oluşturulacağını kullanılan veritabanı sağlayıcısı bağlıdır. Başkalarının el ile değer nasıl oluşturulacağını Kurulum gerekir sırasında veritabanı sağlayıcıları otomatik olarak değer oluşturma bazı özellik türleri için Kurulum.
>
> Örneğin, SQL Server'ı kullanırken `byte[]` üretilir olarak ayarlanmış olan özellikler ekleme veya güncelleştirme ve eşzamanlılık belirteçleri işaretlenmiş Kurulum'a olacaktır `rowversion` veri türü - böylece değerleri veritabanında oluşturulur. Ancak, belirtirseniz bir `DateTime` özelliği oluşturulur üzerinde ekleyebilir ya da bir yol oluşturulacak değerleri için Kurulum sonra güncelleştirebilirsiniz. Bunu yapmanın bir yolu olan bir varsayılan değerini yapılandırmak için `GETDATE()` (bkz [varsayılan değerleri](relational/default-values.md)) yeni satırlar için değerlerini oluşturmak için. Veritabanı tetikleyicisi sonra (örneğin, aşağıdaki örnek tetikleyici) güncelleştirilmesi sırasında değerlerini oluşturmak için de kullanabilirsiniz.
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Kurallar

Kurala göre bir tamsayı ya da GUID veri türüyle birincil anahtarlar üzerinde oluşturulan değerler eklemek için Kur olacaktır. Diğer tüm özellikleri Kurulum hiçbir değer nesil olacaktır.

## <a name="data-annotations"></a>Veri ek açıklamaları

### <a name="no-value-generation-data-annotations"></a>Hiçbir değer oluşturma (veri ek açıklamaları)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Üzerinde üretilen değer ekleme (veri ek açıklamaları)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Bu yalnızca değerleri eklenen varlıklar için oluşturulur, bu EF değerlerini oluşturmak için gerçek mekanizmasını Kurulum garanti etmez bildiğinizden EF olanak tanır. Bkz: [üzerinde üretilen değer eklemek](#value-generated-on-add) daha fazla ayrıntı için bölüm.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Üzerinde üretilen değer ekleme veya güncelleştirme (veri ek açıklamaları)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Bu yalnızca değerleri eklenen veya güncelleştirilen varlıklar için oluşturulur, bu EF değerlerini oluşturmak için gerçek mekanizmasını Kurulum garanti etmez bildiğinizden EF olanak tanır. Bkz: [üzerinde üretilen değer ekleme veya güncelleştirme](#value-generated-on-add-or-update) daha fazla ayrıntı için bölüm.

## <a name="fluent-api"></a>Fluent API'si

Belirli bir özellik için değer oluşturma düzenini değiştirmek için Fluent API'sini kullanın.

### <a name="no-value-generation-fluent-api"></a>Hiçbir değer oluşturma (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Üzerinde üretilen değer ekleme (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`yalnızca bildiğiniz değerleri eklenen varlıklar için oluşturulur, bu EF değerlerini oluşturmak için gerçek mekanizmasını Kurulum garanti etmez EF sağlar.  Bkz: [üzerinde üretilen değer eklemek](#value-generated-on-add) daha fazla ayrıntı için bölüm.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Üzerinde üretilen değer ekleme veya güncelleştirme (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Bu yalnızca değerleri eklenen veya güncelleştirilen varlıklar için oluşturulur, bu EF değerlerini oluşturmak için gerçek mekanizmasını Kurulum garanti etmez bildiğinizden EF olanak tanır. Bkz: [üzerinde üretilen değer ekleme veya güncelleştirme](#value-generated-on-add-or-update) daha fazla ayrıntı için bölüm.
