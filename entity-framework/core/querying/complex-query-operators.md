---
title: Karmaşık Sorgu Operatörleri - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417745"
---
# <a name="complex-query-operators"></a>Karmaşık Sorgu İşleçleri

Dil Tümleşik Sorgusu (LINQ), birden çok veri kaynağını birleştiren veya karmaşık işleme yapan birçok karmaşık işleç içerir. Tüm LINQ operatörleri sunucu tarafında uygun çevirilere sahip değildir. Bazen, bir formdaki bir sorgu sunucuya çevirir, ancak farklı bir biçimde yazılmışsa, sonuç aynı olsa bile çevrilemez. Bu sayfa, bazı karmaşık işleçleri ve desteklenen varyasyonlarını açıklar. Gelecek sürümlerde, daha fazla desen tanıyabilir ve karşılık gelen çevirileri ekleyebilirsiniz. Çeviri desteğinin sağlayıcılar arasında farklılık gösterdiğini de unutmayın. SqlServer'da çevrilen belirli bir sorgu, SQLite veritabanları için çalışmayabilir.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.

## <a name="join"></a>Birleştir

LINQ Join işleci, her kaynak için anahtar seçiciyi temel alan iki veri kaynağını bağlamanızı sağlar ve anahtar eşleştiğinde bir dizi değer oluşturur. Doğal olarak ilişkisel `INNER JOIN` veritabanlarına çevirir. LINQ Join'in dış ve iç anahtar seçicileri olsa da, veritabanı tek bir birleştirme koşulu gerektirir. Böylece EF Core, dış anahtar seçiciyi eşitlik için iç anahtar seçiciyle karşılaştırarak bir birleştirme koşulu oluşturur. Ayrıca, anahtar seçiciler anonim türlerise, EF Core eşitlik bileşenini akıllıca karşılaştırmak için bir birleştirme koşulu oluşturur.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>Groupjoin

LINQ GroupJoin işleci Birleştirme'ye benzer iki veri kaynağını bağlamanızı sağlar, ancak dış öğeleri eşleştirmek için bir iç değer grubu oluşturur. Aşağıdaki örnek gibi bir sorgu yürütme bir `Blog`  &  `IEnumerable<Post>`sonucu oluşturur . Veritabanlarının (özellikle ilişkisel veritabanları) istemci tarafındaki nesnelerin bir koleksiyonunu temsil etmenin bir yolu olmadığından, GroupJoin birçok durumda sunucuya çevrilmez. Özel bir seçici olmadan GroupJoin yapmak için sunucudan tüm verileri almak için gerektirir (aşağıdaki ilk sorgu). Ancak seçici seçili verileri sınırlıyorsa, tüm verileri sunucudan almak performans sorunlarına (aşağıdaki ikinci sorgu) neden olabilir. Bu yüzden EF Core GroupJoin'i tercüme etmez.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>Selectmany

LINQ SelectMany işleci, her dış öğe için bir koleksiyon seçici üzerinde sayısaloluşturmanıza ve her veri kaynağından değer tuples oluşturmanıza olanak tanır. Bir bakıma, bu bir birleştirme ama her dış öğe toplama kaynağından bir öğe ile bağlı böylece herhangi bir koşul olmadan. Toplama seçicinin dış veri kaynağıyla nasıl ilişkili olduğuna bağlı olarak, SelectMany sunucu tarafında çeşitli sorgulara çevrilebilir.

### <a name="collection-selector-doesnt-reference-outer"></a>Koleksiyon seçici dış başvuru yok

Koleksiyon seçici dış kaynaktan bir şey atıfta bulunmuyorsa, sonuç her iki veri kaynağının kartezyen ürünüdür. Bu ilişkisel `CROSS JOIN` veritabanları çevirir.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Toplama seçici referansları bir yer yan tümcesi dış

Koleksiyon seçici, dış öğeye başvurulan bir where yan tümcesi olduğunda, EF Core bunu bir veritabanı birleştirmesine çevirir ve yüklemi birleştirme koşulu olarak kullanır. Normalde bu durum, koleksiyon seçici olarak dış öğeüzerinde toplama gezintisi kullanırken ortaya çıkar. Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulur. Ancak `DefaultIfEmpty` koleksiyon seçiciye uygulanırsa, dış öğe iç öğenin varsayılan değeriyle bağlanır. Bu ayrım nedeniyle, bu tür sorgular `INNER JOIN` yokluğunda `DefaultIfEmpty` ve `LEFT JOIN` ne `DefaultIfEmpty` zaman uygulanır çevirir.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Koleksiyon seçici, olmayan bir durumda dış başvurular

Koleksiyon seçici, bir where yan tümcesinde olmayan dış öğeye (yukarıdaki gibi) başvurursa, veritabanı birleştirmesine çevrilmez. Bu nedenle her dış öğe için koleksiyon seçiciyi değerlendirmemiz gerekir. Birçok ilişkisel `APPLY` veritabanlarındaki işlemlere çevirir. Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulur. Ancak `DefaultIfEmpty` koleksiyon seçiciye uygulanırsa, dış öğe iç öğenin varsayılan değeriyle bağlanır. Bu ayrım nedeniyle, bu tür sorgular `CROSS APPLY` yokluğunda `DefaultIfEmpty` ve `OUTER APPLY` ne `DefaultIfEmpty` zaman uygulanır çevirir. SQLite gibi bazı veritabanları operatörleri `APPLY` desteklemez, bu nedenle bu tür bir sorgu çevrilmeyebilir.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>GroupBy

LINQ GroupBy işleçleri, `IGrouping<TKey, TElement>` `TKey` rasgele `TElement` bir tür olabileceği ve olabileceği türden bir sonuç oluşturur. Ayrıca, `IGrouping` uygular `IEnumerable<TElement>`, gruplama sonra herhangi bir LINQ işleci kullanarak üzerine oluşturabilirsiniz anlamına gelir. Hiçbir veritabanı yapısı bir `IGrouping`temsil edemediğinden, GroupBy işleçleri çoğu durumda çeviriye sahip değildir. Bir skaler döndürür her gruba bir toplam işleç uygulandığında, `GROUP BY` ilişkisel veritabanlarında SQL'e çevrilebilir. SQL `GROUP BY` de kısıtlayıcıdır. Yalnızca skaler değerlere göre gruplandırmanızı gerektirir. Projeksiyon yalnızca anahtar sütunları veya bir sütun üzerinde uygulanan herhangi bir toplamı gruplandırma içerebilir. EF Core bu deseni tanımlar ve aşağıdaki örnekte olduğu gibi sunucuya çevirir:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core ayrıca, gruplandırmadaki toplam işlecinin bir Where veya OrderBy (veya diğer sipariş) LINQ işlecinde göründüğü sorguları da çevirir. Nerede `HAVING` yan tümcesi için SQL'deki yan tümceyi kullanır. GroupBy işleci uygulamadan önce sorgunun bölümü, sunucuya çevrilebildiği sürece karmaşık bir sorgu olabilir. Ayrıca, gruplandırma ları elde edilen kaynaktan kaldırmak için gruplandırma sorgusuna toplu işleçler uyguladığınız da, diğer sorgular gibi bunun üzerine oluşturabilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

EF Core desteklerinin toplamı aşağıdaki gibidir

- Ortalama
- Sayı
- Longcount
- Maks
- Min
- Toplam

## <a name="left-join"></a>Sola Katılma

Sol Birleştirme bir LINQ işleci olmasa da, ilişkisel veritabanları sorgularda sık lıkla kullanılan Sol Birleştirme kavramına sahiptir. LINQ sorgularında belirli bir desen sunucuda `LEFT JOIN` aynı sonucu verir. EF Core bu tür desenleri `LEFT JOIN` tanımlar ve sunucu tarafında eşdeğer oluşturur. Desen, hem veri kaynakları arasında bir GroupJoin oluşturmayı ve sonra selectmany işlecinin içinin ilgili bir öğesi olmadığında null'u eşleştirmek için gruplandırma kaynağında DefaultIfEmpty ile birlikte gruplandırmayı düzleştirmekten içerir. Aşağıdaki örnek, bu örünteğin neye benzediğini ve ne ürettiğini gösterir.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Yukarıdaki desen ifade ağacında karmaşık bir yapı oluşturur. Bu nedenle, EF Core, GroupJoin işlecinin gruplandırma sonuçlarını operatörü hemen izleyen bir adımda düzleştirir. GroupJoin-DefaultIfEmpty-SelectMany kullanılsa bile, ancak farklı bir desende, onu Sol Birleştirme olarak tanımlayamayabiliriz.
