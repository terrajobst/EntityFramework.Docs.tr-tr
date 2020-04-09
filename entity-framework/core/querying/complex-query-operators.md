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
# <a name="complex-query-operators"></a><span data-ttu-id="f73a0-102">Karmaşık Sorgu İşleçleri</span><span class="sxs-lookup"><span data-stu-id="f73a0-102">Complex Query Operators</span></span>

<span data-ttu-id="f73a0-103">Dil Tümleşik Sorgusu (LINQ), birden çok veri kaynağını birleştiren veya karmaşık işleme yapan birçok karmaşık işleç içerir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="f73a0-104">Tüm LINQ operatörleri sunucu tarafında uygun çevirilere sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="f73a0-105">Bazen, bir formdaki bir sorgu sunucuya çevirir, ancak farklı bir biçimde yazılmışsa, sonuç aynı olsa bile çevrilemez.</span><span class="sxs-lookup"><span data-stu-id="f73a0-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="f73a0-106">Bu sayfa, bazı karmaşık işleçleri ve desteklenen varyasyonlarını açıklar.</span><span class="sxs-lookup"><span data-stu-id="f73a0-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="f73a0-107">Gelecek sürümlerde, daha fazla desen tanıyabilir ve karşılık gelen çevirileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f73a0-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="f73a0-108">Çeviri desteğinin sağlayıcılar arasında farklılık gösterdiğini de unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f73a0-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="f73a0-109">SqlServer'da çevrilen belirli bir sorgu, SQLite veritabanları için çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="f73a0-110">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f73a0-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="f73a0-111">Birleştir</span><span class="sxs-lookup"><span data-stu-id="f73a0-111">Join</span></span>

<span data-ttu-id="f73a0-112">LINQ Join işleci, her kaynak için anahtar seçiciyi temel alan iki veri kaynağını bağlamanızı sağlar ve anahtar eşleştiğinde bir dizi değer oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="f73a0-113">Doğal olarak ilişkisel `INNER JOIN` veritabanlarına çevirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="f73a0-114">LINQ Join'in dış ve iç anahtar seçicileri olsa da, veritabanı tek bir birleştirme koşulu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="f73a0-115">Böylece EF Core, dış anahtar seçiciyi eşitlik için iç anahtar seçiciyle karşılaştırarak bir birleştirme koşulu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="f73a0-116">Ayrıca, anahtar seçiciler anonim türlerise, EF Core eşitlik bileşenini akıllıca karşılaştırmak için bir birleştirme koşulu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="f73a0-117">Groupjoin</span><span class="sxs-lookup"><span data-stu-id="f73a0-117">GroupJoin</span></span>

<span data-ttu-id="f73a0-118">LINQ GroupJoin işleci Birleştirme'ye benzer iki veri kaynağını bağlamanızı sağlar, ancak dış öğeleri eşleştirmek için bir iç değer grubu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="f73a0-119">Aşağıdaki örnek gibi bir sorgu yürütme bir `Blog`  &  `IEnumerable<Post>`sonucu oluşturur .</span><span class="sxs-lookup"><span data-stu-id="f73a0-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="f73a0-120">Veritabanlarının (özellikle ilişkisel veritabanları) istemci tarafındaki nesnelerin bir koleksiyonunu temsil etmenin bir yolu olmadığından, GroupJoin birçok durumda sunucuya çevrilmez.</span><span class="sxs-lookup"><span data-stu-id="f73a0-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="f73a0-121">Özel bir seçici olmadan GroupJoin yapmak için sunucudan tüm verileri almak için gerektirir (aşağıdaki ilk sorgu).</span><span class="sxs-lookup"><span data-stu-id="f73a0-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="f73a0-122">Ancak seçici seçili verileri sınırlıyorsa, tüm verileri sunucudan almak performans sorunlarına (aşağıdaki ikinci sorgu) neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="f73a0-123">Bu yüzden EF Core GroupJoin'i tercüme etmez.</span><span class="sxs-lookup"><span data-stu-id="f73a0-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="f73a0-124">Selectmany</span><span class="sxs-lookup"><span data-stu-id="f73a0-124">SelectMany</span></span>

<span data-ttu-id="f73a0-125">LINQ SelectMany işleci, her dış öğe için bir koleksiyon seçici üzerinde sayısaloluşturmanıza ve her veri kaynağından değer tuples oluşturmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f73a0-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="f73a0-126">Bir bakıma, bu bir birleştirme ama her dış öğe toplama kaynağından bir öğe ile bağlı böylece herhangi bir koşul olmadan.</span><span class="sxs-lookup"><span data-stu-id="f73a0-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="f73a0-127">Toplama seçicinin dış veri kaynağıyla nasıl ilişkili olduğuna bağlı olarak, SelectMany sunucu tarafında çeşitli sorgulara çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="f73a0-128">Koleksiyon seçici dış başvuru yok</span><span class="sxs-lookup"><span data-stu-id="f73a0-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="f73a0-129">Koleksiyon seçici dış kaynaktan bir şey atıfta bulunmuyorsa, sonuç her iki veri kaynağının kartezyen ürünüdür.</span><span class="sxs-lookup"><span data-stu-id="f73a0-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="f73a0-130">Bu ilişkisel `CROSS JOIN` veritabanları çevirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="f73a0-131">Toplama seçici referansları bir yer yan tümcesi dış</span><span class="sxs-lookup"><span data-stu-id="f73a0-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="f73a0-132">Koleksiyon seçici, dış öğeye başvurulan bir where yan tümcesi olduğunda, EF Core bunu bir veritabanı birleştirmesine çevirir ve yüklemi birleştirme koşulu olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="f73a0-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="f73a0-133">Normalde bu durum, koleksiyon seçici olarak dış öğeüzerinde toplama gezintisi kullanırken ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="f73a0-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="f73a0-134">Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="f73a0-135">Ancak `DefaultIfEmpty` koleksiyon seçiciye uygulanırsa, dış öğe iç öğenin varsayılan değeriyle bağlanır.</span><span class="sxs-lookup"><span data-stu-id="f73a0-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="f73a0-136">Bu ayrım nedeniyle, bu tür sorgular `INNER JOIN` yokluğunda `DefaultIfEmpty` ve `LEFT JOIN` ne `DefaultIfEmpty` zaman uygulanır çevirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="f73a0-137">Koleksiyon seçici, olmayan bir durumda dış başvurular</span><span class="sxs-lookup"><span data-stu-id="f73a0-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="f73a0-138">Koleksiyon seçici, bir where yan tümcesinde olmayan dış öğeye (yukarıdaki gibi) başvurursa, veritabanı birleştirmesine çevrilmez.</span><span class="sxs-lookup"><span data-stu-id="f73a0-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="f73a0-139">Bu nedenle her dış öğe için koleksiyon seçiciyi değerlendirmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="f73a0-140">Birçok ilişkisel `APPLY` veritabanlarındaki işlemlere çevirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="f73a0-141">Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="f73a0-142">Ancak `DefaultIfEmpty` koleksiyon seçiciye uygulanırsa, dış öğe iç öğenin varsayılan değeriyle bağlanır.</span><span class="sxs-lookup"><span data-stu-id="f73a0-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="f73a0-143">Bu ayrım nedeniyle, bu tür sorgular `CROSS APPLY` yokluğunda `DefaultIfEmpty` ve `OUTER APPLY` ne `DefaultIfEmpty` zaman uygulanır çevirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="f73a0-144">SQLite gibi bazı veritabanları operatörleri `APPLY` desteklemez, bu nedenle bu tür bir sorgu çevrilmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="f73a0-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="f73a0-145">GroupBy</span></span>

<span data-ttu-id="f73a0-146">LINQ GroupBy işleçleri, `IGrouping<TKey, TElement>` `TKey` rasgele `TElement` bir tür olabileceği ve olabileceği türden bir sonuç oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="f73a0-147">Ayrıca, `IGrouping` uygular `IEnumerable<TElement>`, gruplama sonra herhangi bir LINQ işleci kullanarak üzerine oluşturabilirsiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="f73a0-148">Hiçbir veritabanı yapısı bir `IGrouping`temsil edemediğinden, GroupBy işleçleri çoğu durumda çeviriye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="f73a0-149">Bir skaler döndürür her gruba bir toplam işleç uygulandığında, `GROUP BY` ilişkisel veritabanlarında SQL'e çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="f73a0-150">SQL `GROUP BY` de kısıtlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="f73a0-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="f73a0-151">Yalnızca skaler değerlere göre gruplandırmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="f73a0-152">Projeksiyon yalnızca anahtar sütunları veya bir sütun üzerinde uygulanan herhangi bir toplamı gruplandırma içerebilir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="f73a0-153">EF Core bu deseni tanımlar ve aşağıdaki örnekte olduğu gibi sunucuya çevirir:</span><span class="sxs-lookup"><span data-stu-id="f73a0-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="f73a0-154">EF Core ayrıca, gruplandırmadaki toplam işlecinin bir Where veya OrderBy (veya diğer sipariş) LINQ işlecinde göründüğü sorguları da çevirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="f73a0-155">Nerede `HAVING` yan tümcesi için SQL'deki yan tümceyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="f73a0-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="f73a0-156">GroupBy işleci uygulamadan önce sorgunun bölümü, sunucuya çevrilebildiği sürece karmaşık bir sorgu olabilir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="f73a0-157">Ayrıca, gruplandırma ları elde edilen kaynaktan kaldırmak için gruplandırma sorgusuna toplu işleçler uyguladığınız da, diğer sorgular gibi bunun üzerine oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f73a0-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="f73a0-158">EF Core desteklerinin toplamı aşağıdaki gibidir</span><span class="sxs-lookup"><span data-stu-id="f73a0-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="f73a0-159">Ortalama</span><span class="sxs-lookup"><span data-stu-id="f73a0-159">Average</span></span>
- <span data-ttu-id="f73a0-160">Sayı</span><span class="sxs-lookup"><span data-stu-id="f73a0-160">Count</span></span>
- <span data-ttu-id="f73a0-161">Longcount</span><span class="sxs-lookup"><span data-stu-id="f73a0-161">LongCount</span></span>
- <span data-ttu-id="f73a0-162">Maks</span><span class="sxs-lookup"><span data-stu-id="f73a0-162">Max</span></span>
- <span data-ttu-id="f73a0-163">Min</span><span class="sxs-lookup"><span data-stu-id="f73a0-163">Min</span></span>
- <span data-ttu-id="f73a0-164">Toplam</span><span class="sxs-lookup"><span data-stu-id="f73a0-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="f73a0-165">Sola Katılma</span><span class="sxs-lookup"><span data-stu-id="f73a0-165">Left Join</span></span>

<span data-ttu-id="f73a0-166">Sol Birleştirme bir LINQ işleci olmasa da, ilişkisel veritabanları sorgularda sık lıkla kullanılan Sol Birleştirme kavramına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="f73a0-167">LINQ sorgularında belirli bir desen sunucuda `LEFT JOIN` aynı sonucu verir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="f73a0-168">EF Core bu tür desenleri `LEFT JOIN` tanımlar ve sunucu tarafında eşdeğer oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="f73a0-169">Desen, hem veri kaynakları arasında bir GroupJoin oluşturmayı ve sonra selectmany işlecinin içinin ilgili bir öğesi olmadığında null'u eşleştirmek için gruplandırma kaynağında DefaultIfEmpty ile birlikte gruplandırmayı düzleştirmekten içerir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="f73a0-170">Aşağıdaki örnek, bu örünteğin neye benzediğini ve ne ürettiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="f73a0-171">Yukarıdaki desen ifade ağacında karmaşık bir yapı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f73a0-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="f73a0-172">Bu nedenle, EF Core, GroupJoin işlecinin gruplandırma sonuçlarını operatörü hemen izleyen bir adımda düzleştirir.</span><span class="sxs-lookup"><span data-stu-id="f73a0-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="f73a0-173">GroupJoin-DefaultIfEmpty-SelectMany kullanılsa bile, ancak farklı bir desende, onu Sol Birleştirme olarak tanımlayamayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="f73a0-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
