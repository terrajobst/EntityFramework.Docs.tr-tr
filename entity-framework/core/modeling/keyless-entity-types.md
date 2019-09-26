---
title: Keyless varlık türleri-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: e78b9f91fd2505de300ced7b5e73291b5d1ad3b4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266770"
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
- `.HasNoKey()` Metot çağrısıyla yapılandırılması gerekir.
- , _Tanımlayan bir sorguyla_eşleştirilebilir. Tanımlama sorgusu, modelde tanımlanan ve anahtarsız varlık türü için veri kaynağı olarak davranan bir sorgudur.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Anahtarsız varlık türlerine yönelik ana kullanım senaryolarından bazıları şunlardır:

- [Ham SQL sorguları](xref:core/querying/raw-sql)için dönüş türü olarak hizmet sunma.
- Birincil anahtar içermeyen veritabanı görünümlerine eşleme.
- Tanımlı bir birincil anahtarı olmayan tablolar için eşleme.
- Eşleme için modelde tanımlı sorgular.

## <a name="mapping-to-database-objects"></a>Veritabanı nesneleri eşleme

Anahtarsız varlık türünü bir veritabanı nesnesiyle eşlemek `ToTable` veya `ToView` Fluent API kullanılarak elde edilir. EF Core açısından bakıldığında, bu yöntemde belirtilen veritabanı nesnesi olan bir _görünümü_, yani bir salt okunur sorgu kaynağı olarak kabul edilir ve güncelleştirme işleminin hedefi, ekleme ya da silme işlemleri. Ancak bu, veritabanı nesnesinin gerçekten bir veritabanı görünümü olması gerektiği anlamına gelmez. Alternatif olarak, salt okunurdur olarak değerlendirilecek bir veritabanı tablosu olabilir. Buna karşılık, normal varlık türleri için EF Core, `ToTable` yöntemde belirtilen bir veritabanı nesnesinin _tablo_olarak değerlendirilebileceği, yani bir sorgu kaynağı olarak kullanılabileceği, ancak aynı zamanda Update, DELETE ve INSERT işlemlerine hedeflenmiş olduğunu varsayar. Aslında, bir veritabanı görünümü'nde adını belirtebilirsiniz. `ToTable` ve görünümü veritabanında güncelleştirilebilir olarak yapılandırılmış olduğu sürece her şeyin düzgün çalışmalıdır.

> [!NOTE]
> `ToView`nesnenin veritabanında zaten var olduğunu varsayar ve geçişler tarafından oluşturulmaz.

## <a name="example"></a>Örnek

Aşağıdaki örnek, bir veritabanı görünümünü sorgulamak için anahtarsız varlık türlerinin nasıl kullanılacağını gösterir.

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) GitHub üzerinde.

İlk olarak, basit bir Blog ve gönderi modeli tanımlayın:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Ardından, bize her blog ile ilişkili gönderi sayısı sorgulamaya izin verecek basit veritabanı görünümü tanımlayın:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Ardından, veritabanı görünümü sonucu için bir sınıf tanımlayın:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Daha sonra, `HasNoKey` API kullanarak _onmodelyaratırken_ anahtarsız varlık türünü yapılandıracağız.
Anahtarsız varlık türü için eşlemeyi yapılandırmak üzere floent Yapılandırma API 'sini kullanıyoruz:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Ardından, öğesini `DbSet<T>`şunları içerecek `DbContext` şekilde yapılandıracağız:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Son olarak, biz veritabanı görünümü standart şekilde sorgulayabilirsiniz:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Ayrıca, bu tür bir sorgu için kök görevi gören bir bağlam düzeyi sorgu özelliği (DbSet) tanımladık.
