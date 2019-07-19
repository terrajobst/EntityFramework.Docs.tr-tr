---
title: Sorgu türleri - EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 6f0f860c6a4e619e13d55e6207234a8b5261ee09
ms.sourcegitcommit: d1230e34673b8323a227ab37958dfa77f3684728
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68330800"
---
# <a name="query-types"></a>Sorgu türleri
> [!NOTE]
> Bu EF Core 2.1 içinde yeni bir özelliktir

Varlık türlerine ek olarak, EF Core modeli içerebilir _sorgu türü_, varlık türlerine eşlenmediği verilere karşı veritabanı sorgularını yürütmek için kullanılabilir.

## <a name="compare-query-types-to-entity-types"></a>Sorgu türleri varlık türleri için karşılaştırma

Sorgu türleri gibi varlık türleri, bunlar:

- Modele ya da eklenebilir, `OnModelCreating` veya türetilmiş "set" özellik aracılığıyla _DbContext_.
- Devralma eşleme ve gezinti özellikleri gibi aynı Haritalama özellikleri çoğunu destekler. İlişkisel depolarını, bunlar hedef veritabanı nesneleri ve sütunları fluent API yöntemleri veya veri ek açıklamaları üzerinden yapılandırabilirsiniz.

Ancak, varlıktan farklı türlerden bunlar:

- Tanımlanacak bir anahtarı gerektirmez.
- Üzerinde değişiklikler için hiçbir zaman izlenir _DbContext_ ve bu nedenle hiçbir zaman eklenen, güncelleştirilen veya veritabanında silindi.
- Kural gereği hiçbir zaman bulunur.
- Yalnızca bir alt kümesini Gezinti Haritalama özellikleri - özellikle destekler:
  - Bunlar, hiçbir zaman bir ilişkisinin birincil ucu çalışabilir.
  - Bunlar yalnızca varlıklara işaret eden başvuru Gezinti özellikleri de içerebilir.
  - Varlıklar için sorgu türleri Gezinti özellikleri içeremez.
- Üzerinde ele _ModelBuilder_ kullanarak `Query` yöntemi yerine `Entity` yöntemi.
- Üzerinde eşlenen _DbContext_ türü özellikleri aracılığıyla `DbQuery<T>` yerine `DbSet<T>`
- Kullanarak veritabanı nesneleri için eşlenmiş `ToView` yöntemi yerine `ToTable`.
- Eşlenen bir _sorgu tanımlama_ - bir sorgu tanımlanıyor bir sorgu türü için bir veri kaynağı görevi gören modelinde bildirilen ikincil bir sorgu verilmiştir.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Sorgu türleri için ana kullanım senaryoları bazıları şunlardır:

- Dönüş türü için geçici hizmet `FromSql()` sorgular.
- Veritabanı görünümleriyle eşleme.
- Tanımlı bir birincil anahtarı olmayan tablolar için eşleme.
- Eşleme için modelde tanımlı sorgular.

## <a name="mapping-to-database-objects"></a>Veritabanı nesneleri eşleme

Bir sorgu türü için bir veritabanı nesnesi eşleme gerçekleştirilir kullanarak `ToView` fluent API'si. EF Core açısından bakıldığında, bu yöntemde belirtilen veritabanı nesnesi olan bir _görünümü_, yani bir salt okunur sorgu kaynağı olarak kabul edilir ve güncelleştirme işleminin hedefi, ekleme ya da silme işlemleri. Ancak, bu gelmez veritabanı nesnesi veritabanı görünümü için gerçekten gerekli değildir - alternatif olarak salt okunur olarak kabul edilir bir veritabanınız olabilir. Buna karşılık, varlık türleri için bir veritabanı nesnesi içinde belirtilen EF Core varsayar `ToTable` yöntemi olarak kabul bir _tablo_, sorgu kaynağı olarak kullanılabilir olduğunu bildirir ancak aynı zamanda update tarafından hedeflenen Sil anlamına gelir ve Ekle işlemler. Aslında, bir veritabanı görünümü'nde adını belirtebilirsiniz. `ToTable` ve görünümü veritabanında güncelleştirilebilir olarak yapılandırılmış olduğu sürece her şeyin düzgün çalışmalıdır.

## <a name="example"></a>Örnek

Aşağıdaki örnek bir veritabanı görünümü sorgulamak için sorgu türünü kullanmayı gösterir.

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) GitHub üzerinde.

İlk olarak, basit bir Blog ve gönderi modeli tanımlayın:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

Ardından, bize her blog ile ilişkili gönderi sayısı sorgulamaya izin verecek basit veritabanı görünümü tanımlayın:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

Ardından, veritabanı görünümü sonucu için bir sınıf tanımlayın:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

Ardından, sorgu türünde yapılandırıyoruz _OnModelCreating_ kullanarak `modelBuilder.Query<T>` API.
Sorgu türü için eşlemeyi yapılandırmak için standart fluent yapılandırma API'leri kullanın:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

Ardından, öğesini `DbQuery<T>`şunları içerecek `DbContext` şekilde yapılandıracağız:[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]

Son olarak, biz veritabanı görünümü standart şekilde sorgulayabilirsiniz:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> Biz de içerik düzeyi sorgu özelliği (Bu tür sorgu için bir kök olarak görev yapacak DbQuery) tanımladığınız unutmayın.
