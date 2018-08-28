---
title: Oluşturulan değerler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: a3656eb1d2dc79ceead04e3a142a58e8afb3cbce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996783"
---
# <a name="generated-values"></a>Oluşturulan değerler

## <a name="value-generation-patterns"></a>Değer oluşturma düzenleri

Özellikleri için kullanılabilecek üç değer oluşturma düzenleri vardır.

### <a name="no-value-generation"></a>Hiçbir değer oluşturma

Hiçbir değer oluşturma, veritabanına kaydedilecek geçerli bir değer her zaman sağlayacak anlamına gelir. Bağlamına eklenmesi için yeni varlıklar Bu geçerli bir değer atanmalıdır.

### <a name="value-generated-on-add"></a>Üzerinde oluşturulan değeri Ekle

Üzerinde oluşturulan değeri eklemek anlamına gelir, bir değer için varlıklar oluşturulur.

Veritabanı sağlayıcısı bağlı olarak, kullanılan değerler olabilir istemci tarafı EF veya veritabanında oluşturulmuş. Ardından değeri veritabanı tarafından oluşturulmuşsa, varlık bağlamına eklediğinizde EF geçici bir değer atayabilirsiniz. Bu geçici değer ardından sırasında oluşturulan veritabanı değeriyle değiştirilir `SaveChanges()`.

Özelliğe atanmış bir değere sahip bağlamı bir varlık eklemek, yeni bir tane oluşturmak yerine bu değeri eklemek EF dener. Bir özelliğin CLR varsayılan değer atanmazsa, atanan bir değere sahip olduğu kabul edildiği (`null` için `string`, `0` için `int`, `Guid.Empty` için `Guid`vb..). Daha fazla bilgi için [oluşturulan özellikler için açık değerler](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Değer eklenen varlıklar için nasıl oluşturulacağını kullanılan veritabanı sağlayıcısına bağlıdır. Veritabanı sağlayıcıları bazı özellik türleri için değer oluşturmayı otomatik olarak ayarlayabilir, ancak diğerleri değerin nasıl oluşturulacağını el ile Kurulum gerekebilir.
>
> Örneğin, SQL Server kullanıldığında, değerleri otomatik olarak için oluşturulacak `GUID` özellikleri (SQL Server sıralı GUID algoritması kullanarak). Ancak, belirtirseniz bir `DateTime` özelliği oluşturulan oluşturulması için değerleri için bir yol kurmalısınız ardından, eklemek. Bunu yapmanın bir yolu olan bir varsayılan değerini yapılandırmak için `GETDATE()`, bkz: [varsayılan değerleri](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Oluşturulan değeri Ekle veya güncelleştir

Oluşturulan değeri eklemek veya güncelleştirme, yeni bir değer kaydı (ekleme veya güncelleştirme) her kaydedildiğinde oluşturulur anlamına gelir.

Gibi `value generated on add`, değer oluşturulmasını değer yerine eklenir, bir varlığın yeni eklenen bir örneği üzerinde bir özellik için bir değer belirtin. Güncelleştirirken bir açık değeri ayarlamak da mümkündür. Daha fazla bilgi için [oluşturulan özellikler için açık değerler](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Değeri için eklenen ve güncelleştirilen varlıkların nasıl oluşturulacağını kullanılan veritabanı sağlayıcısına bağlıdır. Başkalarının değerin nasıl oluşturulacağını el ile Kurulum gerekir ancak veritabanı sağlayıcıları bazı özellik türleri için değer oluşturmayı otomatik olarak ayarlayabilir.
> 
> Örneğin, SQL Server kullanırken `byte[]` üzerinde oluşturulan ayarlanan özellikler ekleyin veya güncelleştirin ve eşzamanlılık belirteçleri işaretlenmiş kuruluma olacaktır `rowversion` veri türü - böylece değerleri veritabanında oluşturulur. Ancak, belirtirseniz bir `DateTime` özelliği oluşturulan ekleme veya güncelleştirme sonra oluşturulması için değerleri için bir yol ayarlamanız gerekir. Bunu yapmanın bir yolu olan bir varsayılan değerini yapılandırmak için `GETDATE()` (bkz [varsayılan değerleri](relational/default-values.md)) yeni satırlar için değerler oluşturmak için. Ardından veritabanı tetikleyicisi (örneğin, aşağıdaki örnekte tetikleyici) güncelleştirmeler sırasında değerler oluşturmak için de kullanabilirsiniz.
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Kurallar

Kural olarak, bileşik olmayan birincil anahtar türü short, int, long veya üzerinde oluşturulan değerler eklemek için Kurulum GUID olacaktır. Diğer tüm özellikler kurulumu ile hiçbir değer oluşturma olacaktır.

## <a name="data-annotations"></a>Veri ek açıklamaları

### <a name="no-value-generation-data-annotations"></a>Hiçbir değer oluşturma (veri ek açıklamaları)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>(Veri ek açıklamaları) üzerinde oluşturulan değeri Ekle

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Bu değerleri eklenen varlıkları için oluşturulur, bu değerleri oluşturmak için gerçek bir mekanizma EF Kurulum garanti etmez bildiğinizden EF yalnızca sağlar. Bkz: [üzerinde oluşturulan değeri eklemek](#value-generated-on-add) ayrıntılı bilgi için.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Oluşturuldu: değer ekleme veya güncelleştirme (veri ek açıklamaları)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Bu değerleri, eklenen veya güncelleştirilen varlıkları için oluşturulur, bu değerleri oluşturmak için gerçek bir mekanizma EF Kurulum garanti etmez bildiğinizden EF yalnızca sağlar. Bkz: [üzerinde üretilen değer ekleme veya güncelleştirme](#value-generated-on-add-or-update) ayrıntılı bilgi için.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'sini, belirli bir özellik için değer nesil düzenini değiştirmek için kullanabilirsiniz.

### <a name="no-value-generation-fluent-api"></a>Hiçbir değer oluşturma (Fluent API'si)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Üzerinde oluşturulan değeri ekleyin (Fluent API'si)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` yalnızca bildiğiniz değerleri eklenen varlıkları için oluşturulur, bu değerleri oluşturmak için gerçek bir mekanizma EF Kurulum garanti etmez EF olanak tanır.  Bkz: [üzerinde oluşturulan değeri eklemek](#value-generated-on-add) ayrıntılı bilgi için.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Oluşturuldu: değer ekleme veya güncelleştirme (Fluent API'si)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Bu değerleri, eklenen veya güncelleştirilen varlıkları için oluşturulur, bu değerleri oluşturmak için gerçek bir mekanizma EF Kurulum garanti etmez bildiğinizden EF yalnızca sağlar. Bkz: [üzerinde üretilen değer ekleme veya güncelleştirme](#value-generated-on-add-or-update) ayrıntılı bilgi için.
