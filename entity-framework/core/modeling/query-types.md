---
title: "Sorgu türleri - EF çekirdek"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 19a371c65da33e8209cc1ab3423a67c34ddae61e
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="query-types"></a>Sorgu türleri
> [!NOTE]
> Bu özellik EF çekirdek 2.1 yenilikler

Sorgu, EF çekirdek modeli eklenebilir salt okunur sorgu sonuç türleri türleridir. Sorgu türleri geçici (anonim türleri gibi) sorgulama yapmayı etkinleştirmek, ancak belirtilen eşleme yapılandırması olduğundan daha esnektir.

Bunlar, varlık türleri için kavramsal olarak benzerdir:

- Model ya da eklenen POCO C# türleri oldukları ```OnModelCreating``` kullanarak ```ModelBuilder.Query``` yöntemi, veya bir DbContext "Ayarla" özelliği aracılığıyla (olarak sorgu böyle bir özellik türleri için yazılan ```DbQuery<T>``` yerine, ```DbSet<T>```).
- Bunlar normal varlık türü olarak aynı eşleme özelliklerinin çoğunu destekler. Örneğin, devralma eşleme, gezintilerini (limitiations aşağıya bakın) ve ilişkisel depoları, hedef veritabanı şema nesnelerindeki aracılığıyla yapılandırma yeteneğini üzerinde ```ToTable```, ```HasColumn``` fluent API yöntemlerini (veya veri ek açıklamaları).

Sorgu türleri varlıktan farklı türleri, bunlar:

- Tanımlanmamış bir anahtarı gerektirmez.
- Hiçbir zaman değişikliği İzleyicisi tarafından izlenir.
- Hiçbir zaman kurala göre bulunur.
- Yalnızca bir alt kümesini Gezinti eşleme özelliklerini - özellikle destek, hiçbir zaman bir ilişkinin asıl ucu çalışabilir.
- Eşlenmiş bir _sorgu tanımlama_ -A tanımlama sorgudur sorgu türü için bir veri kaynağı görevi gören bir ikincil sorgu.

Sorgu türleri için temel kullanım senaryoları bazıları şunlardır:

- Veritabanı görünümlerine eşleme.
- Tanımlı birincil anahtarı olmayan tablolar için eşleme.
- Dönüş türü için geçici hizmet veren ```FromSql()``` sorgular.
- Model içinde tanımlanan sorgulara eşleme.

> [!TIP]
> Bir veritabanı görünümü için bir sorgu türü eşleme elde edilir kullanarak ```ToTable``` fluent API.

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

Ardından, biz sorgu türünde yapılandırın _OnModelCreating_ kullanarak ```modelBuilder.Query<T>``` API.
Sorgu türü eşlemeyi yapılandırmak için standart fluent yapılandırma API'leri kullanın:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Son olarak, biz veritabanı görünümü standart şekilde sorgulama yapabilirsiniz:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Biz de içerik düzeyi sorgu özelliği (DbQuery) bu tür sorguları için bir kök olarak görev yapması için tanımladığınız unutmayın.
