---
title: Karmaşık sorgu Işleçleri-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417745"
---
# <a name="complex-query-operators"></a>Karmaşık Sorgu İşleçleri

Dil ile tümleşik sorgu (LINQ), birden çok veri kaynağını birleştiren veya karmaşık işleme olan birçok karmaşık işleç içerir. Tüm LINQ işleçleri sunucu tarafında uygun Çeviriler içermez. Bazen, tek bir formdaki bir sorgu sunucuya çevrilir ancak farklı bir biçimde yazılmışsa, sonuç aynı olsa bile bu şekilde çevrilmez. Bu sayfada bazı karmaşık işleçler ve bunların desteklenen çeşitlemeleri açıklanmaktadır. Gelecek sürümlerde, daha fazla desen tanıyabilir ve bunlara karşılık gelen çevirileri ekleyebiliriz. Ayrıca, çeviri desteğinin sağlayıcılar arasında değiştiğini aklınızda bulundurmanız önemlidir. SqlServer içinde çevrilen belirli bir sorgu, SQLite veritabanları için çalışmayabilir.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub ' da görebilirsiniz.

## <a name="join"></a>Birleştir

LINQ JOIN işleci, her bir kaynak için anahtar seçicisine göre iki veri kaynağını bağlamanıza olanak tanır ve anahtar eşleştiğinde bir değer grubu oluşturabilirsiniz. İlişkisel veritabanlarında `INNER JOIN` doğal olarak çevirir. LINQ birleşimi dış ve iç anahtar seçicileri içerdiğinden, veritabanı tek bir JOIN koşulu gerektirir. EF Core, dış anahtar seçiciyi eşitlik için iç anahtar seçiciyle karşılaştırarak bir birleşim koşulu oluşturur. Ayrıca, anahtar seçicileri anonim türse EF Core eşitlik bileşeni yönlerini karşılaştırmak için bir JOIN koşulu oluşturur.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin

LINQ Groupjoın işleci, birleşime benzer iki veri kaynağına bağlanmanızı sağlar, ancak eşleşen dış öğeler için bir iç değer grubu oluşturur. Aşağıdaki örnek gibi bir sorgu yürütmek `IEnumerable<Post>``Blog` & sonucunu üretir. Veritabanları (özellikle de ilişkisel veritabanları), istemci tarafı nesneler koleksiyonunu temsil etmek için bir yol içermediğinden, Groupjoın birçok durumda sunucuya çevrilmez. Özel bir seçici olmadan (aşağıdaki ilk sorgu), sunucudan tüm verileri Groupjoın yapmak üzere almanızı gerektirir. Ancak seçici verilerin seçili olduğunu sınırlandırırken, sunucudaki tüm verilerin getirilmesi performans sorunlarına neden olabilir (aşağıdaki ikinci sorgu aşağıda verilmiştir). EF Core Groupjoın 'i çevirmez.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>SelectMany

LINQ SelectMany işleci her bir dış öğe için bir koleksiyon seçicisi üzerinde listeleme ve her bir veri kaynağından değer başlıkları oluşturma olanağı sağlar. Bir şekilde, her dış öğenin koleksiyon kaynağından bir öğe ile bağlanması için herhangi bir koşul olmadan bir birleşimdir. Koleksiyon seçicinin dış veri kaynağıyla nasıl ilişkili olduğuna bağlı olarak, SelectMany sunucu tarafında çeşitli farklı sorgulara çevrilebilir.

### <a name="collection-selector-doesnt-reference-outer"></a>Koleksiyon Seçicisi dış başvuruya başvurmuyor

Koleksiyon seçici dış kaynaktan herhangi bir şeye başvurmazsa, sonuç her iki veri kaynağına ait bir Kartezyen ürünüdür. İlişkisel veritabanlarında `CROSS JOIN` çevirir.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Koleksiyon Seçicisi bir where yan tümcesinde Outer öğesine başvuruyor

Koleksiyon seçicisinin dış öğeye başvuran bir where yan tümcesi olduğunda EF Core, bunu bir veritabanı JOIN 'e çevirir ve koşul birleşim koşulu olarak kullanır. Normalde bu durum, koleksiyon seçici olarak dış öğe üzerinde koleksiyon gezintisi kullanılırken ortaya çıkar. Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulmaz. Ancak koleksiyon seçicisine `DefaultIfEmpty` uygulanmışsa dıştaki öğe, iç öğenin varsayılan bir değeriyle bağlanır. Bu ayrım nedeniyle, bu tür sorgular `DefaultIfEmpty` ve `DefaultIfEmpty` uygulandığında `LEFT JOIN` `INNER JOIN` dönüştürür.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Koleksiyon seçici, bir yerde olmayan bir dış durum başvurusu

Koleksiyon Seçicisi bir where yan tümcesinde olmayan dış öğeye başvurduğunda (Yukarıdaki durum gibi), bir veritabanı katılımı çevirmez. Bu nedenle her bir dış öğe için koleksiyon seçicisini değerlendirmemiz gerekiyor. Birçok ilişkisel veritabanında `APPLY` işlemlerine çevirir. Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulmaz. Ancak koleksiyon seçicisine `DefaultIfEmpty` uygulanmışsa dıştaki öğe, iç öğenin varsayılan bir değeriyle bağlanır. Bu ayrım nedeniyle, bu tür sorgular `DefaultIfEmpty` ve `DefaultIfEmpty` uygulandığında `OUTER APPLY` `CROSS APPLY` dönüştürür. SQLite gibi bazı veritabanları `APPLY` işleçlerini desteklemediğinden, bu tür bir sorgu çevrilemeyebilir.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>Ölçütü

LINQ GroupBy işleçleri, `TKey` ve `TElement` herhangi bir rastgele tür olabilecek `IGrouping<TKey, TElement>` türünde bir sonuç oluşturur. Ayrıca, `IGrouping` `IEnumerable<TElement>`uygular, bu da Gruplamadan sonra herhangi bir LINQ işlecini kullanarak oluşturabileceğiniz anlamına gelir. Hiçbir veritabanı yapısı bir `IGrouping`temsil etmediği için, GroupBy işleçleri çoğu durumda çeviri içermez. Bir skaler döndüren her gruba bir toplama işleci uygulandığında, ilişkisel veritabanlarında SQL `GROUP BY` çevrilebilir. SQL `GROUP BY` de kısıtlayıcıdır. Yalnızca skaler değerlere göre gruplandırma yapmanızı gerektirir. Projeksiyon yalnızca gruplandırma anahtarı sütunlarını veya bir sütun üzerinde uygulanan herhangi bir toplamı içerebilir. EF Core, bu kalıbı tanımlar ve aşağıdaki örnekte olduğu gibi sunucuya çevirir:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core Ayrıca, gruplandırmadaki bir toplama işlecinin WHERE veya OrderBy (ya da başka bir sıralama) LINQ işlecinde göründüğü sorguları çevirir. WHERE yan tümcesi için SQL 'de `HAVING` yan tümcesini kullanır. Bir sorgu, GroupBy işlecini uygulamadan önce, sunucuya çevrilebilmesi gerektiği sürece herhangi bir karmaşık sorgu olabilir. Ayrıca, sonuçta elde edilen kaynaktaki gruplandırmaları kaldırmak için bir gruplandırma sorgusuna toplama işleçleri uyguladıktan sonra, bunun üzerine diğer sorgular gibi oluşturabilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

EF Core tarafından desteklenen toplama işleçleri şunlardır

- Ortalama
- Sayı
- LongCount
- Maks
- Min
- Toplam

## <a name="left-join"></a>Sola ekleme

LEFT JOIN bir LINQ işleci olmadığından, ilişkisel veritabanları sorgularda sık kullanılan bir LEFT JOIN kavramıdır. LINQ sorgularında belirli bir model, sunucudaki `LEFT JOIN` aynı sonucu verir. EF Core bu desenleri tanımlar ve sunucu tarafında eşdeğer `LEFT JOIN` oluşturur. Bu model, her iki veri kaynağı arasında bir Groupjoın oluşturmayı ve sonra, iç ilgili bir öğe olmadığında null ile eşleştirmek için gruplandırma kaynağında Defaultıempty ile Selectınempty olan selectıempty işlecini kullanarak gruplamayı düzleştirmeyi içerir. Aşağıdaki örnek, bu düzenin neye benzemediğini ve ne yaptığını gösterir.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Yukarıdaki model ifade ağacında karmaşık bir yapı oluşturur. Bu nedenle, EF Core, bir adım içindeki Groupjoın işlecinin gruplandırma sonuçlarını, işlecin hemen sonrasında bir adımla düzleştirmeniz gerekir. Groupjoın-Defaultıempty-SelectMany çok fazla kullanılıyor olsa da, farklı bir düzende bunu bir LEFT JOIN olarak belirleyemeyebilirsiniz.
