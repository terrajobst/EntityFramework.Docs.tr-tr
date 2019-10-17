---
title: Keyless varlık türleri-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 3dbc2700fc9bb277eb90885dfc2506c250ae21f1
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445938"
---
# <a name="keyless-entity-types"></a>Anahtarsız Varlık Türleri

> [!NOTE]
> Bu özellik, sorgu türlerinin adı altında EF Core 2,1 ' ye eklenmiştir. EF Core 3,0 ' de kavram, anahtarsız varlık türleri olarak yeniden adlandırıldı.

Normal varlık türlerine ek olarak, bir EF Core modeli, anahtar değerleri içermeyen verilere karşı veritabanı sorgularını yürütmek için kullanılabilen, _daha seyrek varlık türleri_içerebilir.

## <a name="keyless-entity-types-characteristics"></a>Keyless varlık türleri özellikleri

Keyless varlık türleri, devralma eşlemesi ve gezinti özellikleri gibi normal varlık türleriyle aynı eşleme özelliklerinin çoğunu destekler. İlişkisel depolarda, hedef veritabanı nesnelerini ve sütunları Fluent API Yöntemler veya veri ek açıklamaları aracılığıyla yapılandırabilirler.

Ancak bunlar, normal varlık türlerinden farklıdır:

- Tanımlı bir anahtar olamaz.
- , _DbContext_ 'teki değişiklikler için hiçbir şekilde izlenmez ve bu nedenle veritabanında hiçbir şekilde eklenmemiş, güncellenmez veya silinmez.
- Hiçbir şekilde kural tarafından keşfedilir.
- Yalnızca bir gezinti eşleme özellikleri alt kümesini destekler, özellikle:
  - Bir ilişkinin asıl ucu olarak hiçbir şekilde davranmayabilir.
  - Sahip oldukları varlıkların gezginlerine sahip olmayabilir
  - Yalnızca normal varlıkların işaret eden başvuru gezinti özelliklerini içerebilir.
  - Varlıklar, anahtarsız varlık türlerine gezinti özellikleri içeremez.
- @No__t-0 yöntem çağrısıyla yapılandırılması gerekir.
- , _Tanımlayan bir sorguyla_eşleştirilebilir. Tanımlama sorgusu, modelde tanımlanan ve anahtarsız varlık türü için veri kaynağı olarak davranan bir sorgudur.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Anahtarsız varlık türlerine yönelik ana kullanım senaryolarından bazıları şunlardır:

- [Ham SQL sorguları](xref:core/querying/raw-sql)için dönüş türü olarak hizmet sunma.
- Birincil anahtar içermeyen veritabanı görünümlerine eşleme.
- Birincil anahtarı tanımlanmış olmayan tablolarla eşleme.
- Modelde tanımlanan sorgularla eşleme.

## <a name="mapping-to-database-objects"></a>Veritabanı nesneleriyle eşleme

@No__t-0 veya `ToView` Fluent API kullanılarak bir anahtarsız varlık türünü bir veritabanı nesnesiyle eşleme yapılır. EF Core perspektifinden, bu yöntemde belirtilen veritabanı nesnesi bir _görünümdir_, yani salt okunurdur bir sorgu kaynağı olarak kabul edilir ve güncelleştirme, ekleme veya silme işlemlerinin hedefi olamaz. Ancak bu, veritabanı nesnesinin gerçekten bir veritabanı görünümü olması gerektiği anlamına gelmez. Alternatif olarak, salt okunurdur olarak değerlendirilecek bir veritabanı tablosu olabilir. Buna karşılık, normal varlık türleri için EF Core, `ToTable` yönteminde belirtilen bir veritabanı nesnesinin _tablo_olarak değerlendirilebileceği anlamına gelir, yani bir sorgu kaynağı olarak kullanılabilecek ancak aynı zamanda Update, DELETE ve INSERT işlemleri tarafından hedeflenebilir. Aslında `ToTable` ' da bir veritabanı görünümü adı belirtebilirsiniz ve görünümün veritabanında güncelleştirimek üzere yapılandırıldığı sürece her şey iyi çalışmalıdır.

> [!NOTE]
> `ToView` nesnenin veritabanında zaten var olduğunu ve geçişler tarafından oluşturulmayacak olduğunu varsayar.

## <a name="example"></a>Örnek

Aşağıdaki örnek, bir veritabanı görünümünü sorgulamak için anahtarsız varlık türlerinin nasıl kullanılacağını gösterir.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) GitHub ' da görebilirsiniz.

İlk olarak, basit bir blog ve gönderi modeli tanımladık:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Ardından, her bloga ait gönderi sayısını sorgulamanızı sağlayacak basit bir veritabanı görünümü tanımladık:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Sonra, veritabanı görünümünden elde edilen sonucu barındıracak bir sınıf tanımlayacağız:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Daha sonra, `HasNoKey` API kullanarak _onmodelyaratırken_ anahtarsız varlık türünü yapılandıracağız.
Anahtarsız varlık türü için eşlemeyi yapılandırmak üzere floent Yapılandırma API 'sini kullanıyoruz:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Sonra, `DbContext` `DbSet<T>` ' i içerecek şekilde yapılandırdık:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Son olarak, veritabanı görünümünü standart şekilde sorgulayabilir:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Ayrıca, bu tür bir sorgu için kök görevi gören bir bağlam düzeyi sorgu özelliği (DbSet) tanımladık.
