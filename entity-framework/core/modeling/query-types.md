---
title: Sorgu türleri - EF çekirdek
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163180"
---
# <a name="query-types"></a>Sorgu türleri
> [!NOTE]
> Bu özellik EF çekirdek 2.1 yenilikler

EF çekirdek modeli varlık türlerine ek olarak içerebilir _sorgu türü_, varlık türleri eşlenmediği veri veritabanı sorguları yürütmek için kullanılabilir.

Sorgu türleri varlık türleriyle birçok benzerlikler vardır:

- Bunlar ayrıca modeline ya da eklenebilir, `OnModelCreating`, veya türetilmiş bir "Ayarla" özellik aracılığıyla _DbContext_.
- Bunlar aynı eşleme özelliklerini, devralma eşleme, gezinti özellikleri (sınırlamalar aşağıya bakın) gibi ve ilişkisel depoları, hedef veritabanı nesneleri ve sütunları fluent API yöntemlerini veya veri ek açıklamaları aracılığıyla yapılandırma yeteneğini destekler.

Ancak varlıktan farklı türleri, bunlar:

- Tanımlanmamış bir anahtarı gerektirmez.
- Üzerinde değişiklikler için hiçbir zaman izlenir _DbContext_ ve bu nedenle hiçbir zaman eklenir, güncelleştirilmiş veya veritabanında silindi.
- Hiçbir zaman kurala göre bulunur.
- Yalnızca bir alt kümesini Gezinti eşleme özelliklerini - özellikle destekler:
  - Bunlar, hiçbir zaman bir ilişkinin asıl ucu çalışabilir.
  - Bunlar yalnızca varlıklara işaret eden başvuru Gezinti özellikleri de içerebilir.
  - Varlık Gezinti özellikleri için sorgu türleri içeremez.
- Üzerinde ele _ModelBuilder_ kullanarak `Query` yöntemi yerine `Entity` yöntemi.
- Üzerinde eşlenen _DbContext_ türünün özelliklerini aracılığıyla `DbQuery<T>` yerine `DbSet<T>`
- Kullanarak veritabanı nesneleri eşlenen `ToView` yöntemi yerine `ToTable`.
- Eşlenmiş bir _sorgu tanımlama_ - A olan bir sorgu türü için bir veri kaynağı görevi gören modelinde bildirilen ikincil bir sorgu sorgu tanımlama.

Sorgu türleri için temel kullanım senaryoları bazıları şunlardır:

- Dönüş türü için geçici hizmet veren `FromSql()` sorgular.
- Veritabanı görünümlerine eşleme.
- Tanımlı birincil anahtarı olmayan tablolar için eşleme.
- Model içinde tanımlanan sorgulara eşleme.

> [!TIP]
> Bir veritabanı nesnesi için bir sorgu türü eşleme elde edilir kullanarak `ToView` fluent API. EF çekirdek açısından bakıldığında, bu yöntemi, belirtilen veritabanı nesnesidir bir _Görünüm_, yani bir salt okunur sorgu kaynağı olarak kabul edilir ve güncelleştirme hedefi, eklenemiyor veya silme işlemleri. Ancak, bu gelmez veritabanı nesne bir veritabanı görünümü olması gerektiğine - alternatif olarak salt okunur olarak kabul edilecek bir veritabanı tablosu olabilir. Buna karşılık, varlık türleri için bir veritabanı nesnesi içinde belirtilen EF çekirdek varsayar `ToTable` yöntemi kabul bir _tablo_, bir sorgu kaynağı olarak kullanılabilir ancak aynı zamanda güncelleştirme tarafından hedeflenmiş silme anlamına gelir ve Ekle işlemler. Aslında, bir veritabanı görünümünde adını belirtebilirsiniz `ToTable` ve görünüm veritabanında güncelleştirilebilir için yapılandırılmış olduğu sürece her şeyi sorunsuz çalışması gerekir.

## <a name="example"></a>Örnek

Aşağıdaki örnek sorgu türü bir veritabanı görünümü sorgulamak için nasıl kullanılacağını gösterir.

> [!TIP]
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) github'da.

İlk olarak, basit bir Blog ve Post modelinin tanımlayın:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

Ardından, bize her blog ile ilişkili gönderileri sayısı sorgulamaya izin veren basit veritabanı görünümü tanımlayın:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

Ardından, veritabanı görünümü sonucundan tutmak için bir sınıf tanımlayın:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

Ardından, biz sorgu türünde yapılandırın _OnModelCreating_ kullanarak `modelBuilder.Query<T>` API.
Sorgu türü eşlemeyi yapılandırmak için standart fluent yapılandırma API'leri kullanın:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Son olarak, biz veritabanı görünümü standart şekilde sorgulama yapabilirsiniz:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Biz de içerik düzeyi sorgu özelliği (DbQuery) bu tür sorguları için bir kök olarak görev yapması için tanımladığınız unutmayın.
