---
title: Keyless varlık türleri-EF Core
description: Entity Framework Core kullanarak anahtarsız varlık türlerini yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 520c9ed93240c05deee36fa527a3757490fd7082
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417318"
---
# <a name="keyless-entity-types"></a>Anahtarsız Varlık Türleri

> [!NOTE]
> Bu özellik, sorgu türlerinin adı altında EF Core 2,1 ' ye eklenmiştir. EF Core 3,0 ' de kavram, anahtarsız varlık türleri olarak yeniden adlandırıldı.

Normal varlık türlerine ek olarak, bir EF Core modeli, anahtar değerleri içermeyen verilere karşı veritabanı sorgularını yürütmek için kullanılabilen, _daha seyrek varlık türleri_içerebilir.

## <a name="keyless-entity-types-characteristics"></a>Keyless varlık türleri özellikleri

Keyless varlık türleri, devralma eşlemesi ve gezinti özellikleri gibi normal varlık türleriyle aynı eşleme özelliklerinin çoğunu destekler. İlişkisel depolarını, bunlar hedef veritabanı nesneleri ve sütunları fluent API yöntemleri veya veri ek açıklamaları üzerinden yapılandırabilirsiniz.

Ancak bunlar, normal varlık türlerinden farklıdır:

- Tanımlı bir anahtar olamaz.
- , _DbContext_ 'teki değişiklikler için hiçbir şekilde izlenmez ve bu nedenle veritabanında hiçbir şekilde eklenmemiş, güncellenmez veya silinmez.
- Kural gereği hiçbir zaman bulunur.
- Yalnızca bir gezinti eşleme özellikleri alt kümesini destekler, özellikle:
  - Bunlar, hiçbir zaman bir ilişkisinin birincil ucu çalışabilir.
  - Sahip oldukları varlıkların gezginlerine sahip olmayabilir
  - Yalnızca normal varlıkların işaret eden başvuru gezinti özelliklerini içerebilir.
  - Varlıklar, anahtarsız varlık türlerine gezinti özellikleri içeremez.
- `.HasNoKey()` yöntemi çağrısıyla birlikte yapılandırılması gerekir.
- , _Tanımlayan bir sorguyla_eşleştirilebilir. Tanımlama sorgusu, modelde tanımlanan ve anahtarsız varlık türü için veri kaynağı olarak davranan bir sorgudur.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Anahtarsız varlık türlerine yönelik ana kullanım senaryolarından bazıları şunlardır:

- [Ham SQL sorguları](xref:core/querying/raw-sql)için dönüş türü olarak hizmet sunma.
- Birincil anahtar içermeyen veritabanı görünümlerine eşleme.
- Tanımlı bir birincil anahtarı olmayan tablolar için eşleme.
- Eşleme için modelde tanımlı sorgular.

## <a name="mapping-to-database-objects"></a>Veritabanı nesneleri eşleme

`ToTable` veya `ToView` Fluent API kullanılarak, bir anahtarsız varlık türünü bir veritabanı nesnesiyle eşleme elde edilir. EF Core perspektifinden, bu yöntemde belirtilen veritabanı nesnesi bir _görünümdir_, yani salt okunurdur bir sorgu kaynağı olarak kabul edilir ve güncelleştirme, ekleme veya silme işlemlerinin hedefi olamaz. Ancak bu, veritabanı nesnesinin gerçekten bir veritabanı görünümü olması gerektiği anlamına gelmez. Alternatif olarak, salt okunurdur olarak değerlendirilecek bir veritabanı tablosu olabilir. Buna karşılık, normal varlık türleri için EF Core, `ToTable` yönteminde belirtilen bir veritabanı nesnesinin _tablo_olarak değerlendirilebileceği anlamına gelir, yani bir sorgu kaynağı olarak kullanılabilecek ancak aynı zamanda Update, DELETE ve INSERT işlemleri tarafından hedeflenebilir. Aslında, `ToTable` ' de bir veritabanı görünümünün adını belirtebilir ve görünümün veritabanında güncelleştirimek üzere yapılandırıldığı sürece her şey iyi çalışmalıdır.

> [!NOTE]
> `ToView`, nesnenin veritabanında zaten var olduğunu ve geçişler tarafından oluşturulmayacak olduğunu varsayar.

## <a name="example"></a>Örnek

Aşağıdaki örnek, bir veritabanı görünümünü sorgulamak için anahtarsız varlık türlerinin nasıl kullanılacağını gösterir.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) GitHub ' da görebilirsiniz.

İlk olarak, basit bir Blog ve gönderi modeli tanımlayın:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Ardından, bize her blog ile ilişkili gönderi sayısı sorgulamaya izin verecek basit veritabanı görünümü tanımlayın:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Ardından, veritabanı görünümü sonucu için bir sınıf tanımlayın:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Daha sonra, `HasNoKey` API kullanarak _onmodelyaratırken_ anahtarsız varlık türünü yapılandıracağız.
Anahtarsız varlık türü için eşlemeyi yapılandırmak üzere floent Yapılandırma API 'sini kullanıyoruz:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Sonra, `DbContext` `DbSet<T>`içerecek şekilde yapılandırdık:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Son olarak, biz veritabanı görünümü standart şekilde sorgulayabilirsiniz:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Ayrıca, bu tür bir sorgu için kök görevi gören bir bağlam düzeyi sorgu özelliği (DbSet) tanımladık.
