---
title: Oluşturulan değerler-EF Core
description: Entity Framework Core kullanırken özellikler için değer oluşturmayı yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 9c616e157ff1bdb9700f436a7ae2788330fe5d45
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416348"
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

Özelliğe atanmış bir değere sahip bir varlık eklerseniz, EF yeni bir değer oluşturmak yerine bu değeri eklemeye çalışacaktır. Bir özellik, CLR varsayılan değeri (`string`için`null`, `int`için `0` `Guid.Empty`, vb.) atanmadığı takdirde atanan bir değere sahip olarak kabul edilir.`Guid` Daha fazla bilgi için bkz. [oluşturulan Özellikler Için açık değerler](../saving/explicit-values-generated-properties.md).

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

## <a name="value-generated-on-add"></a>Ekleme sırasında oluşturulan değer

Kural gereği, uygulama tarafından bir değer sağlanmadıysa, anahtar, int, Long veya GUID türündeki bileşik olmayan birincil anahtarlar, eklenen varlıklar için oluşturulan değerlere sahip olacak şekilde ayarlanır. Veritabanı sağlayıcınız genellikle gereken yapılandırmayı üstlenir; Örneğin, SQL Server bir sayısal birincil anahtar otomatik olarak bir KIMLIK sütunu olarak ayarlanır.

Herhangi bir özelliği, ekli varlıklar için değeri oluşturulacak şekilde aşağıdaki gibi yapılandırabilirsiniz:

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

> [!WARNING]
> Bu yalnızca AŞV 'nin eklenen varlıklar için değerlerin oluşturulduğunu bilmesini sağlar. Bu, EF 'in değer oluşturmak için gerçek mekanizmayı ayarlayabileceklerini garanti etmez. Daha fazla ayrıntı için bkz. [ekleme bölümünde oluşturulan değer](#value-generated-on-add) .

### <a name="default-values"></a>Varsayılan değerler

İlişkisel veritabanlarında, bir sütun varsayılan bir değerle yapılandırılabilir; bir satır bu sütun için bir değer olmadan eklenirse, varsayılan değer kullanılacaktır.

Bir özellik üzerinde varsayılan bir değer yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

Varsayılan değeri hesaplamak için kullanılan bir SQL parçası da belirtebilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

Varsayılan bir değer belirtmek, özelliği, ekleme sırasında oluşturulan değer olarak örtülü olarak yapılandırır.

## <a name="value-generated-on-add-or-update"></a>Ekleme veya güncelleştirme üzerinde oluşturulan değer

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

***

> [!WARNING]
> Bu yalnızca AŞV 'nin eklenen veya güncelleştirilmiş varlıklar için değerlerin oluşturulduğunu bilmesini sağlar. Bu, EF 'in, değer oluşturmak için gerçek mekanizmayı ayarlayabileceklerini garanti etmez. Daha fazla ayrıntı için bkz. [ekleme veya güncelleştirme üzerinde oluşturulan değer](#value-generated-on-add-or-update) .

### <a name="computed-columns"></a>Hesaplanan sütunlar

Bazı ilişkisel veritabanlarında, bir sütun değeri, genellikle diğer sütunlara başvuran bir ifadeyle birlikte veritabanında hesaplanmalıdır:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=ComputedColumn&highlight=5)]

> [!NOTE]
> Bazı durumlarda, sütunun değeri her getirilirken (bazen *sanal* sütunlar olarak adlandırılır) hesaplanır ve diğer bir deyişle, satırdaki her güncelleştirmede ve depolanan (bazen *depolanan* veya *kalıcı* sütunlar olarak adlandırılır) hesaplanır. Bu, veritabanı sağlayıcılarının tamamında farklılık gösterir.

## <a name="no-value-generation"></a>Değer oluşturma yok

Bir özellik üzerinde değer oluşturmayı devre dışı bırakmak genellikle bir kural değer oluşturma için yapılandırıyorsa gereklidir. Örneğin, int türünde bir birincil anahtarınız varsa, bu, ekleme sırasında oluşturulan değer olarak örtülü olarak ayarlanır; Bunu aşağıdakiler aracılığıyla devre dışı bırakabilirsiniz:

### <a name="data-annotations"></a>[Veri Açıklamaları](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-api"></a>[Akıcı API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
